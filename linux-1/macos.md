# macOS



锁屏 ctrl + cmd + q

窗口最大话 shift + option +绿色图标

光标移动到行首ctrl + a

光标移动到行尾ctrl + e

切换应用 ctrl + tab

切换同一应用不同窗口 ctrl + \`

Reverse lookup反向查找命令  ctrl + r

chrome强制刷新 cmd+shift+r

grep -r “word” .  

截屏保存到桌面  Command+Shift+4

截全屏保存到桌面 Command+Shift+3

快捷显示桌面 f11

ShadowsocksX-NG

brew 类似apt

cheatsheet

git

Jdk

maven 

Idea

switchhosts

sourcetree

chrome

commander one

vs code

bear markdown 

typora markdown

the unarhiver

nodejs

Charles fiddler alternative

破解Charles

Registered Name: https://zhile.io

License Key: 48891cf209c6d32bf4

export http\_proxy=http://127.0.0.1:1080;export https\_proxy=http://127.0.0.1:1080;

安装

* **.app** 文件：从习惯上来说，安装软件只需要把 .app 文件放在「应用程序」这个文件夹下（实际上放在别的目录也可以运行），直接双击就可以运行了。.app 从作用上来说和 Windows 下的 .exe 很像，直接运行这个文件就可以启动程序，然而它们的实现是非常不一样的：.exe 文件实际上是一个可执行文件，在运行时去动态加载其它所需要的文件，而 .app 实际上是一个打包文件，里面封装了这个程序的相关文件，而如右图所示，通过一定方法可以查看这些 .app 文件封装的程序具体文件。
* **.dmg** 文件：你可以理解 .dmg 只是对 .app 文件做了一次压缩，从网上下回来的一些 OS X 软件安装包，大多都是这个格式的。双击运行后 .dmg 文件会自动解压出 .app 文件，你要做的只是把 **.app** 文件拖动到「应用程序」文件夹就行了。
* **.pkg** 文件：一般比较大型的软件会用这种安装方式，如 Office 等。这个就很类似于 Windows 上的各类程序安装包了，它不仅仅包含了 .app 文件，还有一定数量其它文件，往往会需要你选择安装位置等步骤来完成安装。
* 
安装目录 /usr/local

Idea

On Mac OS X IDEA uses the following directories:

Config: ~/Library/Preferences/IntelliJIdeaXX

System: ~/Library/Caches/IntelliJIdeaXX

Plugins: ~/Library/Application Support/IntelliJIdeaXX

Logs: ~/Library/Logs/IntelliJIdeaXX \(starting from IntelliJ IDEA 9.0, older versions keep logs under System location\)

shell 走代理

export http\_proxy=http://127.0.0.1:1080;export https\_proxy=http://127.0.0.1:1080;

Finder 显示隐藏文件

defaults write com.apple.finder AppleShowAllFiles -boolean true ; killall Finder

Finder 不显示隐藏文件

defaults write com.apple.finder AppleShowAllFiles -boolean false ; killall Finder

