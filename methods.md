[http://blog.csdn.net/ljianhui/article/details/11100625](http://blog.csdn.net/ljianhui/article/details/11100625)

[http://gywbd.github.io/posts/2014/8/50-linux-commands.html](http://gywbd.github.io/posts/2014/8/50-linux-commands.html)



根据端口号查看进程

```bash
netstat –apn | grep 8080
```

后台运行

```
nohup aa.sh &
```

检查linux kernal版本

```
uname -a
cat   /proc/version
```

APT

```
sudo apt-get install maven
```

递归创建多个目录

```
mkdir -p dir1/dir2
```

查看空间

```
df -h
du -sh * | grep G
free -h
top
```

压缩解压缩

```
man tar

#这条命令是将所有.jpg的文件打成一个名为all.tar的包。-c是表示产生新的包
，-f指定包的文件名。 
tar -cf all.tar *.jpg

#tar调用gzip
tar -czf all.tar.gz *.jpg
tar -xzf all.tar.gz

#tar调用bzip2
tar -cjf all.tar.bz2 *.jpg 
tar -xjf all.tar.bz2 

 
 zip all.zip *.jpg
 unzip all.zip
 
  gzip all. 
```



