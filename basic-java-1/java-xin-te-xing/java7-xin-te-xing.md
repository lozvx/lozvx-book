# Java7新特性

* 集合类的语法支持

Before：

```java
List<String> list = new ArrayList<String>();
         list.add("item");
         String item = list.get(0);

         Set<String> set = new HashSet<String>();
         set.add("item");
         Map<String, Integer> map = new HashMap<String, Integer>();
         map.put("key", 1);
         int value = map.get("key");
```

Java7：

```java
List<String> list = ["item"];
         String item = list[0];

         Set<String> set = {"item"};

         Map<String, Integer> map = {"key" : 1};
         int value = map["key"];
```

* 自动资源管理

Java中某些资源是需要手动关闭的，如InputStream，Sockets等。这个新的语言特性允许多的资源，这些资源作用于try代码块，并自动关闭。

需要写在try（）块里.

Before：

```java
BufferedReader br = new BufferedReader(new FileReader(path));
          try {
          return br.readLine();
                } finally {
                    br.close();
          }
```

Java7:

```java
 try (BufferedReader br = new BufferedReader(new FileReader(path)) {
              return br.readLine();
          }
```

* 类型判断

Before:

```java
Map<String, List<String>> anagrams = new HashMap<String, List<String>>();
```

Java7:

```java
 Map<String, List<String>> anagrams = new HashMap<>();
```

* 数值字面量下划线支持

```java
int one_million = 1_000_000;
```

* switch中可以使用string

```java
        String s = "test";
        switch (s) {
            case "test" :
                System.out.println("test");
            case "test1" :
                System.out.println("test1");
                break ;
            default :
                System.out.println("break");
                break ;
        }
```

* 新增一些取环境信息的工具方法

```java
File System.getJavaIoTempDir() // IO临时文件夹  
File System.getJavaHomeDir() // JRE的安装目录  
File System.getUserHomeDir() // 当前用户目录  
File System.getUserDir() // 启动java进程时所在的目录5
```

