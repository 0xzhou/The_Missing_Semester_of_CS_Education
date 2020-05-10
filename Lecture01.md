### The Shell

`date`—print out the current date

`echo`—print sth.



**Environment parameter:**



`$PATH`: 一个文件列表，在shell执行指令时，在这个列表中搜索可执行的程序。

`which xxx`—查看某个指令在执行什么程序，like: `which echo`.

`pwd`: print working directory



`..`: parent directory 

`.`: current directory

`~`: home directory

`-`: previous directory

`/`: 

经常和`cd`搭配使用。



`ls`: read and list all the files

`--help`: 查看命令的帮助

`ls -l`: 查看具体的信息(use a long listing format):

返回信息，



**permission** for user owner, group owner, everyone else.

```shell
missing:~$ ls -l /home
drwxr-xr-x 1 missing  users  4096 Jun 15  2019 missing
```

- [x] 这里可以**总结**

`d`说明这是一个directory, 接下来的9个字符表示该文件(夹)对于the owners of the file, the owning group and everyone else所属的permission. `-`表示没有拥有权力, 如上述例子中：

只有owner可以modify(w)这个文件夹,例如添加或者删除其中的文件；如果要进入该文件夹，必须拥有search(execute='x')的权力；如果要查看里面的内容，必须拥有read(r)的权力。



`mv`—rename file

`cp`—copy from x to y

`rm`—remove the file

`rdmir`—remove a **empty** dic.

`man`—similar with `--help`



`cat`—print the content of a file

`|`—'chain' programs such that the output of one is the input of another



'root user' = administration：

You will not usually log into your system as the root user though, since it’s too easy to accidentally break something.

`sudo`—do as superuser(root)

`cd sys`—change to the  **kernel** of the computers

`sudo su`—change the terminal to the root user

- [x] 这里有两种sudo的方法，一种不work:

  1. 切换用户到root user: `sudo su`
  2. `xx | sudo xxx `: 注意 `|,>,<`这些都是是由shell本身执行的，若将`sudo`放在前面不能起到sudo的效果！！ 

  

`xdg-open`—open a file in a appropriate form



- [ ] Exercise: