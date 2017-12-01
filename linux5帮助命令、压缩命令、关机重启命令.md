# linux5帮助命令、压缩命令、关机重启命令

[TOC]

## 帮助命令

### 1.man
man的作用就是寻找帮助，当你对某个命令不熟悉就可以使用man ls之类的进入ls的帮助文档查看帮助，注意linux中命令可以有多个等级，从1-9，并且每个命令都可能有多个等级，也就是如果我们直接调用man ls只会返回最小等级命令的帮助文档，这时你可以调用man -f ls（和 whatis ls作用相同）来查询ls命令有哪些帮助等级，随后man 1 ls用来找到第一级的帮助文档，从而实现查看帮助文档。man -k passwd会返回和包含passwd的一切帮助文档，不仅仅只是命令，你可以在忘掉某个命令时候用这个方法查看，这个方式与apropos完全一样。

### 2.其他帮助命令
1.ls --help表示只查看ls的选项帮助文件，这个相对于man功能较少，但是更集中。当使用pwd --help时会发现pwd无有效的选项。
2.shell就是linux用户和计算机交互的接口，linux内核是不能识别英文字母的，只知道机器语言，这时就要shell进行翻译。
3.help是man的补充命令，man只能查看外部命令ls类的帮助文档，但是help只能查看cd之类的shell内置命令帮助文档。
4.info实际上比man更详细，但是info实际上是返回一个整个的帮助文档，也就是所有的帮助文档都在里面，他只是帮你定位到一个正确的位置。


## 压缩命令

### zip和unzip
1.在linux中zip和Windows的zip没有任何区别，你可以在在Windows下的zip文件在linux下用，用法很easy，直接zip 压缩后的文件名 被压缩的文件。注意没有加选项的时候既可以压缩文件，也能压缩目录，但是存在巨大的漏洞，因为如果目录下有文件，你这样操作完全无法压缩目录下的文件，你只是把目录压缩了而已，所以如果要压缩目录，必须将-r选项加入。在linux中原则上是可以不区分文件后缀的，但是为我们的zip文件命名的时候要加上zip，以免歧义。
2.unzip是解压缩命令，就是直接加压缩文件名，这里注意如果我们解压缩文件的目录下存在完全相同的文件名，你解压缩也无意义，并且当你解压缩成功后，原压缩文件还是存在。

### gzip与gunzip
1.gzip相对zip更为简单，一般直接加源文件就行，而不是先压缩后的文件名再加源文件名，但是gzip有点怪，你直接gzip file他会将file直接压缩为file.gz，删除源文件；当你用gzip -r directory你会发现directory还是在，但是directory下所有文件都被压缩了，所以gzip不支持压缩原目录。
2.gunzip file.gz解压缩文件，gunzip directory解压缩目录，但是注意一旦解压源压缩文件不存在。

### bzip2
1.bzip2相对zip和gzip压根不能操作目录，而且压缩后默认源文件删除，但是可以bzip2 -k file来保存源文件。
2.和gunzip一样bunzip2解压缩文件，但是源文件消失，不过加-k就保存原压缩文件。

### tar
1.linux为了解决上述gzip和bzip2无法压缩目录，他提供的一种解决方式将源目录首先打包：tar -cvf file.tar file来得到file的打包，再然后使用压缩命令。（-c打包、-v显示过程、-f指定打包后的文件名）。最后接打包，恢复成原来的样（tar -xvf file.tar，这里-x解压缩）。
2.tar也能直接压缩解压缩文件，通过tar -zcvf file.tar.gz file压缩文件；tar -xcvf file.tar.gz解压缩；而对于bzip2文件就是tar -jcvf file.tar.bz2 file；解压缩就是tar -jxvf file.tar.bz2；如果只想查看不解压tar -ztvf file.tar.gz或tar -jtvf file.tar.bz2，这里的-t是测试。


## 关机重启

### shutdown
shutdown -r now/时间 就是现在活着特定时间重启，shutdown -c就是取消上一次的重启操作，而shutdown -h就是直接关机，注意这几个操作一定要小心，因为我们在进行服务器操作的时候直接重启或关机，可能出现灾难，由其当别人都在使用的时候，千万注意不要关机。shutdown很安全，关机前会保存。

### runlevel
查询当前运行级别，一般3是字符界面，5是图形假面，可以使用init 5来切换

### logout
注意在使用完成之后必须logout，linux一次最多256个管理员，所以用完一定logout以免占用他人的空间。