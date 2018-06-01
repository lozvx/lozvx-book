# 熔断机制 译文

[https://www.jianshu.com/p/e4a735c90ca8](https://www.jianshu.com/p/e4a735c90ca8)

软件系统向其它运行于不同进程，或是网络中不同机器上的软件发起远程调用是很常见的。内存调用和远程调用的一个比较大的差异是，远程调用可能失败或者挂起直到某一超时时限。而对于一个不能响应的服务提供方，如果有很多调用方依赖于它，情况会更糟糕，因为你将会耗尽关键资源，然后引发多个系统的级联奔溃。在作者的神书 《Release It》中， Michael Nygard 推广 Circuit Breaker 模式来防止这种可怕的级联影响。

断路器的基本思路很简单。通过将待保护的函数调用包裹在断路器对象中，让断路器对象来监控失败。当失败次数达到特定的阈值时，断路器打开，后续对此断路器对象的访问将直接返回 error，根本不会调用受保护的函数。通常，你会想在断路器打开的时候得到某种监控预警。  
![](//upload-images.jianshu.io/upload_images/2732904-52343dad94711839.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/558)断路器执行过程

这边是个 ruby 写的描述断路器这种行为（对超时调用的保护）的简单示例。（虽然是 ruby 的，但是基本代码逻辑还是可以看懂的）

首先新建一个断路器，传入需保护的函数调用，这是个 lambda 表达式  
 `cb = CircuitBreaker.new {|arg| @supplier.func arg}`

断路器保存函数块，初始化一些参数（阈值、超时时间和监控），并重置断路器为关闭状态。

```text
class CircuitBreaker

  attr_accessor :invocation_timeout, :failure_threshold, :monitor
  def initialize &block
    @circuit = block
    @invocation_timeout = 0.01
    @failure_threshold = 5
    @monitor = acquire_monitor
    reset
  end
```

如果断路器处于关闭状态，调用会通过断路器访问内部的函数；如果处于开启状态的话，则会直接返回错误。

```text
# client code
    aCircuitBreaker.call(5)

class CircuitBreaker...
  def call args
    case state
    when :closed
      begin
        do_call args              // 断路器关闭时，调用内部函数
      rescue Timeout::Error
        record_failure
        raise $!
      end
    when :open then raise CircuitBreaker::Open  // 断路器开启，直接抛出异常 
    else raise "Unreachable Code"
    end
  end
  def do_call args
    result = Timeout::timeout(@invocation_timeout) do
      @circuit.call args
    end
    reset
    return result
  end
```

当调用超时时，递增失败计数器；调用成功的话，再把它重置为0。

```text
class CircuitBreaker...

  def record_failure
    @failure_count += 1
    @monitor.alert(:open_circuit) if :open == state
  end
  def reset
    @failure_count = 0
    @monitor.alert :reset_circuit
  end
```

断路器的状态由一个阈值和失败次数的对比决定

```text
class CircuitBreaker...

  def state
     (@failure_count >= @failure_threshold) ? :open : :closed
  end
```

这个简易的断路器在开启时可以避免对保护函数的调用，不过在系统恢复的时候需要额外的干预来重置断路器。在建筑物中的电路断路器是合理的，不过对于软件断路器，我们可以让断路器自身检测内部调用是否恢复。为了实现这个，我们可以间隔一个合适时间段后尝试调用，如果调用成功，则重置断路器。![](//upload-images.jianshu.io/upload_images/2732904-992404e807e83288.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/450)断路器的开启和恢复逻辑

这种断路器，需要增加一个阈值来重置断路器，还需要一个变量来存储上次失败发生的时间

```text
class ResetCircuitBreaker...

  def initialize &block
    @circuit = block
    @invocation_timeout = 0.01
    @failure_threshold = 5
    @monitor = BreakerMonitor.new
    @reset_timeout = 0.1
    reset
  end
  def reset   // 重置
    @failure_count = 0
    @last_failure_time = nil
    @monitor.alert :reset_circuit
  end
```

现在就出现了第三种状态 “半开”，这时，断路器准备好发起一个真实的调用来检验问题是否已经修复。

```text
class ResetCircuitBreaker...

  def state
    case
// 失败次数大于阈值（开启） 并且开启一段时间后，为“半开”状态， 这种状态会去检验
    when (@failure_count >= @failure_threshold) && 
        (Time.now - @last_failure_time) > @reset_timeout
      :half_open
    when (@failure_count >= @failure_threshold)
      :open
    else
      :closed
    end
  end
```

“半开” 状态下的调用是一个测探，调用成功就重置断路器，否则重置超时

```text
class ResetCircuitBreaker...

  def call args
    case state
    when :closed, :half_open
      begin
        do_call args
      rescue Timeout::Error
        record_failure
        raise $!
      end
    when :open
      raise CircuitBreaker::Open
    else
      raise "Unreachable"
    end
  end
  def record_failure
    @failure_count += 1
    @last_failure_time = Time.now   // 重置超时，重新计算下个半开的时间窗口
    @monitor.alert(:open_circuit) if :open == state
  end
```

这个例子用以解释原理的，相对简单，真实的断路器会提供更多的功能和参数。通常它们会将受保护的调用可能引发的异常（如网络连接失败）给隔离开来。并不是所有的错误都会造成短路，有些错误是反应正常的失败的，应该作为正常逻辑的一部分处理。

访问量大的时候，我们的很多请求会遇到一直等待响应超时的问题。由于远程调用通常都比较慢，将每个请求放置在不同的线程里，并使用 [future or promise](https://link.jianshu.com?t=http%3A%2F%2Fen.wikipedia.org%2Fwiki%2FFutures_and_promises) 的方式来处理响应会是个好主意。这些线程从线程池中获取，这样，你就可以在线程池过载的时候将断路器断开。

例子展示了断路跳闸的一种简单方式，在调用成功的时候重置计数器。复杂点的方式可能会观察错误频率，比如说在 50% 失败率的时候跳闸。你也可以对不同的错误设置不同的阈值，比如设置超时的阈值为 10% ，而将连接失败的阈值设置为 3% 。

上面断路器的例子只是针对同步调用的，其实断路器在异步通信时也是有用的。常用的技术有，将所有请求推入到一个队列中，服务提供方根据自身频率来消费，有效的避免了服务器超载。这时，断路器则在队列满的时候断开。

断路器能减少在操作过程中容易失败的资源捆绑在一起。你不再需要等待客户端超时，断路器避免了再向压力过大的系统增加负载。这边讲的远程调用是断路器常见的使用场景，它们也能用于任何你想将系统的一些部分隔离于其他部分的失败的场景。  
 监控断路器是很有价值的。断路器状态的任何变化都应该记录于日志中，并且披露出这些状态的详细细节以供更深入的监控。断路器的行为通常预示着更深层次的环境问题。操作员应该能打开和重置断路器。

断路器本身是很有价值的，但是客户端使用它的时候需要对断路失败做相应的处理。比如，对于远程调用，你需要思考调用失败的时候怎么处理。是你执行的操作失败了，或是有什么你可以补救的？信用卡授权可以放于队列中后续再处理，获取数据失败可以通过显示某些固定的数据，这些数据足够丰富，不会影响展示。

  
  
作者：holysu  
链接：https://www.jianshu.com/p/e4a735c90ca8  
來源：简书  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

