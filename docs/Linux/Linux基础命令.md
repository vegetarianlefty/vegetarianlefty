### Linux基础命令

#### 目录的相关操作

- cd : 变换目录
- pwd : 显示目前的目录
- mkdir : 创建一个新的目录
- rmdir : 删除一个空的目录
- ls : 查看目录文件详情
- cp : 复制文件或目录
- mv : 移动文件或目录，或更名
- rm : 移除文件或目录

##### cd 命令介绍:

![img](http://image.wangxiaohuan.com/blog/image/20210511230351.png)

##### pwd 命令介绍:

![img](http://image.wangxiaohuan.com/blog/image/20210511230552.png)

##### mkdir 命令介绍:

![img](http://image.wangxiaohuan.com/blog/image/20210511230712.png)

##### rmdir 命令介绍:

![img](http://image.wangxiaohuan.com/blog/image/20210511230816.png)

##### ls 命令介绍:

```
ls -l 显示当前目录下的长格式文件（包含：权限、文件个数、文件创建的用户、权限组、大小、最后更新时间，以及文件名）
root@node-01 ~]# ls -l     
total 8
-rw-------. 1 root root 1249 Jun  8  2020 anaconda-ks.cfg
-rwxr--r--. 1 root root 3024 Oct 15  2021 k8s-install-check.sh
drwxr-xr-x. 5 root root   58 Nov 27  2021 logs
drwxr-xr-x. 4 root root   34 Nov 27  2021 nacos
ls -a		显示所有文件，包含隐藏文件(以.开头的就是隐藏文件)
[root@node-01 ~]# ls -a
.   anaconda-ks.cfg  .bash_logout   .bashrc  .config  .dubbo  .jenkins              .kube  .m2    .npm  .tcshrc   .wget-hsts
..  .bash_history    .bash_profile  .cache   .cshrc   .java   k8s-install-check.sh  logs   nacos  .ssh  .viminfo
ls -r 	文件逆向排序（默认按照文件名首字母）
[root@node-01 ~]# ls -r
nacos  logs  k8s-install-check.sh  anaconda-ks.cfg
ls -l -r   按长格式、逆向排序文件。多个命令之间可以合并--，等同于ls -lr
[root@node-01 ~]# ls -l -r
total 8
drwxr-xr-x. 4 root root   34 Nov 27  2021 nacos
drwxr-xr-x. 5 root root   58 Nov 27  2021 logs
-rwxr--r--. 1 root root 3025 Aug 29 15:05 k8s-install-check.sh
-rw-------. 1 root root 1249 Jun  8  2020 anaconda-ks.cfg
ls -t		按照时间顺序显示
[root@node-01 ~]# ls -t
k8s-install-check.sh  nacos  logs  anaconda-ks.cfg
ls -R	递归显示，即显示所有文件夹及文件夹下的文件
[root@node-01 ~]# ls -R
```

第一个字段的第一个字符是文件类型

- 如果第一个字段是 '-', 表示是普通文件
- 如果第一个字段是 d, 表示是目录

第一个字段后9位字符是权限位，每三个一组，每一组***rwx***表示“读（read）”“写（write）”“执行（execute）”,如果是字母，就说明有这个权限；如果是横线，就是没有这个权限。这三组分别表示文件所属的用户权限、文件所属的组权限以及其他用户的权限

第二个字段是硬链接（hard link）数目.

第三个字段是所属用户.

第四个字段是所属组.

第五个字段是文件的大小.

第六个字段是文件被修改的日期.

第七个是文件名。

![img](http://image.wangxiaohuan.com/blog/image/20210512151655.png)

![img](http://image.wangxiaohuan.com/blog/image/20210512151740.png)

![img](http://image.wangxiaohuan.com/blog/image/20210512151830.png)

![img](http://image.wangxiaohuan.com/blog/image/20210512151945.png)

##### cp 命令介绍: 

用于将一个或多个文件或目录复制到指定位置，亦常用于文件的备份工作。-r参数用于递归操作，复制目录时若忘记加则会直接报错，而-f参数则用于当目标文件已存在时会直接覆盖不再询问，这两个参数尤为常用。

##### **语法格式：**cp [参数] 源文件 目标文件 



| f    | 若目标文件已存在，则会直接覆盖原文件                         |
| ---- | ------------------------------------------------------------ |
| -i   | 若目标文件已存在，则会询问是否覆盖                           |
| -p   | 保留源文件或目录的所有属性                                   |
| -r   | 递归复制文件和目录                                           |
| -d   | 当复制符号连接时，把目标文件或目录也建立为符号连接，并指向与源文件或目录连接的原始文件或目录 |
| -l   | 对源文件建立硬连接，而非复制文件                             |
| -s   | 对源文件建立符号连接，而非复制文件                           |
| -b   | 覆盖已存在的文件目标前将目标文件备份                         |
| -v   | 详细显示cp命令执行的操作过程                                 |
| -a   | 等价于“pdr”选项                                              |

```
在当前工作目录中，将某个文件复制一份，并定义新文件名称：
[root@node-03 ~]# cp anaconda-ks.cfg copy-anaconda-ks.cfg

在当前工作目录中，将某个目录复制一份，并定义新目录名称： 
[root@node-03 ~]# cp -r logs logs-test

复制某个文件时，保留其原始权限及用户归属信息：
[root@node-03 ~]# cp -a anaconda-ks.cfg copy-anaconda-ks.cfg

将某个文件复制到/etc目录中，并覆盖已有文件，不进行询问：
[root@node-03 ~]# cp -f anaconda-ks.cfg /etc
```

