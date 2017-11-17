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

sed

```
#当你将Dos系统中的文件复制到Unix/Linux后，这个文件每行都会以\r\n结尾，
#sed可以轻易将其转换为Unix格式的文件，使用\n结尾的文件
sed 's/.$//' filename

#反转文件内容并输出
sed -n '1!G; h; p' filename
#为非空行添加行号
sed '/./=' thegeekstuff.txt | sed 'N; s/\n/ /'
```

awk

```
#删除重复行
awk '!($0 in array) { array[$0]; print}' temp

#打印 /etc/password中所有包含同样uid和gid的行
awk -F ':' '$3=$4' /etc/passwd
```



```
awk '!($0 in array) { array[$0]; print}' tempa

#打印 /etc/password中所有包含同样uid和gid的行
awk -F ':' '$3=$4' /etc/passwd
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

```

```
# 当你不知道某个命令到位置
whereis ls
# whatis 显示某个命令的描述信息
whatis ls
```

```
# 查看linux到网络接口
ifconfig -a

```

```
# chmod用于改变文件和目录的权限
#给指定文件的属主和属组所有权限(包括读、写、执行) 
chmod ug+rwx file.txt

#删除指定文件的属组的所有权限 
chmod g-rwx file.txt
#修改目录的权限，以及递归修改目录下面所有文件和子目录的权限 
chmod -R ug+rwx file.txt
```

 如果要挂载一个文件系统，需要先创建一个目录，然后将这个文件系统挂载到这个目录上

```
mkdir /u01
mount /dev/sdb1 /u01
```



```
#删除文件前先确认
rm -i file.txt
#删除文件前先打印文件名确认
rm -i file*
递归删除文件夹下所有文件，并删除该文件夹
rm -r adir
```

```
scp aa@192.168.1.10:/home/text.file ./

```



