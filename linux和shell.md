## 硬连接和软连接的区别

**什么是链接？**

链接简单说实际上是一种文件共享的方式，是 POSIX 中的概念，主流文件系统都支持链接文件。

**它是用来干什么的？**

你可以将链接简单地理解为 Windows 中常见的快捷方式（或是 OS X 中的替身），Linux 中常用它来解决一些库版本的问题，通常也会将一些目录层次较深的文件链接到一个更易访问的目录中。在这些用途上，我们通常会使用到软链接（也称符号链接）。

##### 软硬连接的区别

硬链接： 与普通文件没什么不同，inode 都指向同一个文件在硬盘中的区块，删除源文件硬连接不失效。
软链接： 保存了其代表的文件的绝对路径，是另外一种文件，在硬盘上有独立的区块，访问时替换自身路径，删除源文件软连接失效（非常像windows系统下的快捷方式）。

## awk命令使用

awk就是把文件逐行的读入，以空格或tab键为默认分隔符将每行切片，切开的部分再进行各种分析处理

awk工作流程是这样的：读入有'\n'换行符分割的一条记录，然后将记录按指定的域分隔符划分域，填充域，$0则表示所有域,$1表示第一个域,$n表示第n个域。默认域分隔符是"空白键" 或 "[tab]键"

## 系统管理命令

#### 一、目录指令

#### 1、创建目录make directory

```
mkdir  目录名称                 //mkdir spring,创建一个spring文件夹
mkdir  -p  file/file/file            //递归创建多级别关系目录
mkdir      dir/newdir            //不使用递归
mkdir  -p  dir/newdir/newdir      //使用递归
```

#### 2、移动目录 move

```
mv  dir1  dir2                  //把dir1目录移动到dir2目录下
mv  dir1/dir2  dir3              //把dir2目录移动到dir3目录下
mv  dir1/dir2  dir3/dir4          //把dir2目录移动到dir4目录下
mv  dir1/dir2  ./                //把dir2移动到当前目录下
mv  dir1/dir2  dir3/dir4          //dir2移到dir4目录下，并改名字为“原名”
mv  dir1/dir2  dir3/dir4/newdir   //dir2移到dir4目录下， 并改名字为“newdir”
```

#### 3、修改文件名

```
mv  dir1  newdir1              //修改dir1的名称为newdir1
```

#### 4、复制 copy

复制文件

```
cp  file1  dir/file2      //把file1拷贝到dir目录下，并改名为file2
cp  file1  dir            //file1被复制到dir目录下，并改名字为“原名”
cp  dir1/file1  dir2/newfile  //file1被复制到dir2目录下，并改名字为“newfile”
复制目录：-r忽略目录的层级关系
cp  -r  dir1  dir2           //dir1被复制到dir2目录下,并改名字为"原名"
cp  -r  dir1/dir2 dir3/newdir  //dir2被复制到dir3目录下,并改名字为"newdir"
cp  -r  dir1/dir2 dir3/dir4    //dir2被复制到dir4目录下,并改名字为"原名"
cp  -r  dir1/dir2 dir3/dir4/newdir //dir2被制到dir4目录下,并改名字为"newdir"
cp  -r  dir1 ../../newdir      //dir1被复制到上两级目录下,并改名字为"newdir"
```

## 5、删除 remove

```
rm  文件                  //删除文件
remove  regular  file  “”  //y:删除,n:取消操作
rm  -r  层级目录           //递归删除目录
rm  -rf  目录/文件         //强制删除文件或者目录
```

#### 二、文件指令

#### 1、查看文件内容

```
   cat  filename       //打印文件内容到输出终端
   more  filename     //通过敲回车方式逐行查看文件的各个行内容
                      //默认从第一行开始查看
                      //不支持回看
                      //输入q 退出查看       
   less  filename       //通过“上下左右”键查看文件的各个部分内容
                      //支持回看
                      //输入q 退出查看     
   head  -n  filename  //查看文件的前n行内容
   tail  -n  filename    //查看文件的后n行内容       
   wc  filename        //查看文件的行数
```

#### 2、创建文件

```
touch  dir1/filename       //指定路径下创建文件
touch  filename           //当前目录下创建文件
```

#### 3、给文件追加内容

如果文件不存在会创建文件

```
echo content >  filename     //把“内容”以[覆盖]方式追加给“文件”
echo content>>  filename     //把“内容”以[追加]形式写给“文件”
```

#### 三、用户指令

用户操作：需要系统的root登录

#### 1、创建用户user add

配置文件：/etc/passwd

```
useradd
useradd  liming       //创建liming用户，同时会创建一个同名的组出来
useradd  -g 组别编号  username  
//把用户的组别设置好，避免创建同名的组出来
useradd  -g 组编号  -u 用户编号  -d 家目录   username
```

### 2、修改用户 usermodify

```
usermod  -g 组编号  -u 用户编号  -d 家目录  -l 新名字  username
```

#### 3、删除用户 userdelete

```
userdel  username
userdel -r  username    //删除用户同时删除其家目录
```

#### 4、给用户设置密码，使其登录系统

```
passwd  用户名
```

#### 四、组别操作

需要系统root登录

#### 1、创建组 group add

```
配置文件：/etc/group
groupadd  music
```

#### 2、修改组 groupmodify

```
groupmod  -g  gid  -n 新名字  groupname
```

#### 3、删除组 groupdelete

```
groupdel  groupname   //组下边如果有用户存在，就禁止删除
```

#### 五、权限指令

Linux中定义了3种访问权限，分别是r、w、x。

```
r：表示对象是可读的，八进制表示为4
w：表示对象是可写的，八进制表示为2
x：表示对象是可执行的，八进制表示为1
-rwx           rwx       rwx    
文件所属者权限   用户组权限	 其他用户权限
```

功能：修改目录或文件的权限
u:user(所有者)　　g:group(所属组)　　
o:other(其他人)　　a:all(所有人)　　
r:read（读）　　w:write(写)　　x:execute（执行）

```
chmod 460 test.txt ： test文件所有者：r，所属组：rw,其他用户无权限
chmod u+w dest_file：给目标文件的所属者增加w权限。
chmod u+wx,g+x,o+w dest_file：给目标文件的所属者增加w权限，所属组增加x权限，系统其他用户增加w权限。
chmod o-w dest_file：给目标文件的其他用户移除w权限。
chmod u=rwx dest_file：给所属者赋予rwx权限。
chmod -R 777 /tmp/a  ,目录a包括目录下的所有文件或目录的所有用户权限都变为rwx。
```

#### 六、记事本指令

![img](https://img2018.cnblogs.com/blog/1691717/201910/1691717-20191007164428152-1668367992.png)

编辑模式下面显示：- -INSERT- -
命令模式下面显示：（默认什么都不显示）
尾行模式下面显示：:wq(退出并保存)

#### 1、编辑模式操作

```
   a: 光标向后移动一位
   i: 光标和 所在字符 不发生任何变化
   o: 给新起一行
   s: 删除光标所在字符
```

#### 2、尾行模式操作

```
   :q          //quit 退出编辑器
   :w          //write 对修改后的内容进行保存
   :wq         //write quit 保存修改并退出编辑器
   :q!         //(不保存)强制退出编辑器
   :w!         //强制保存
   :wq!        //强制保存并退出编辑
 
   :set number  或 nu          //设置行号
   :set nonumber  或 nonu      //设置行号
 
   :/内容/  或 /内容           //查找指定内容
                                小写n（next）下一个
                                大写N（next）上一个
 
    :数字               //跳转到数字所在行
 
    字符串替换cont1被替换为cont2
   :s/cont1/cont2/         //替换光标所在行的第一个cont1
   :s/cont1/cont2/g        //替换光标所在行的全部的cont1
   :%s/cont1/cont2/g       //替换整个文档的cont1
```

#### 3、命令模式操作

```
u  光标移动
       字符级
           上(k)  下(j)  左(h)  右(l) 键
       单词级
           w:  word移动到下个单词的首字母
           e:  end移动到下个(本)单词的尾字母
           b:  before移动到上个(本)单词的首字母
       行级
           $:  行尾
           0:  行首
       段落级(翻屏)
           {:  上个(本)段落首部
           }:  下个(本)段落尾部
       屏幕级(不翻屏)
           H:  屏幕首部
           L:  屏幕尾部
       文档级
           G:  文档尾部
           1G: 文档第1行
           nG: 文档第n行
 
u  内容删除
       dd:     删除光标当前行
       2dd:    包括当前行在内，向后删除2行内容
       ndd:    包括当前行在内，删除后边n行内容
       x:      删除光标所在字符
       c+w:    从光标所在位置删除至单词结尾，并进入编辑模式
       
u  内容复制
       yy:     复制光标当前行
       2yy:    包括当前行在内，向后复制2行内容
       nyy:    包括当前行在内，复制后边n行内容
       p:      对(删除)复制好的内容进行粘贴操作
 
u  相关快捷操作
       u:      undo撤销
       J:      合并上下两行
       r:      单个字符替换
       .点:    重复执行上次最近的指令
```

#### 七、解和压指令

```
gzip a.txt ：gzip压缩
gzip -d a.txt.gz或gunzip a.txt.gz：解压
bzip2 a：bzip2压缩
bunzip2 a.bz2 或bzip2 -d a.bz2：解压
tar -cvf bak.tar .：将当前目录的文件打包
tar -rvf bak.tar /etc/password：将/etc/password追加文件到bak.tar中(r)
tar -xvf bak.tar：解压
tar -zcvf a.tar.gz：打包并压缩gzip
tar -zxvf a.tar.gz：解压缩
tar -zxvf a.tar.gz -C /usr：解压到/usr/下
tar -ztvf a.tar.gz或zip/unzip：查看压缩包内容
tar -jcvf a.tar.bz2：打包并压缩成bz2
tar -jxvf a.tar.bz2：解压bz2
```

#### 八、防火墙指令

#### 1、重启后生效

```
开启： chkconfig iptables on 
关闭： chkconfig iptables off 
```

#### 2、即时生效，重启后失效

```
开启： service iptables start 
关闭： service iptables stop 
```

#### 九、系统指令

```
shell > # grep  关键字 路径名    将文本中指定的信息匹配出来
shell ># which 指令             查找指令对应的二进制文件
shell > # ps –A                查看系统活跃进程process
shell > # du –h 目标           
以K，M，G为单位显示目录或文件占据磁盘空间的大小 （block块默认=4k）
shell > # date –s “2013-09-13 19:42:30”   给系统设置时间
shell > # date      查看系统时间
shell > # df –lh     查看系统分区情况
shell > # kill -9  pid   杀死指定进程号的进程
```

#### 十、快捷键指令

```
ctrl + c：停止进程
ctrl + l：清屏
ctrl + r：搜索历史命令
ctrl + q：退出
help “指令” 或者 man “指令”：查看内部命令帮助
cd ~ 或 cd：进入到用户根目录
pwd：查看当前所在目录
cd ~cl：进入到cl用户根目录
cd -：返回到原来目录
cd ..：返回到上一级目录
ls –la：查看la用户根目录下的所有文件
```