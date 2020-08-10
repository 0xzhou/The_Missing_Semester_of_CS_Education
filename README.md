# The Missing Semester of CS Education

This is a note and summary of the course: https://missing.csail.mit.edu/

### Lecture1. Course overview + the shell

##### What is the Shell?

- a textual interface(customized): allow you run programs, give input and inspect output

- Bourne Again SHell = "bash"

##### Using the shell

- `date`—print out the current date

- `echo`—prints out its arguments
  - whitespace: 语义解析分隔符
  - quote the argument: "My photos"(在Ubuntu18上不用quote)
- *environment parameter:*  a list where the shell search for program when it is given a command
  - `echo $PATH`: 一个文件列表，在shell执行指令时，在这个列表中搜索可执行的程序。
  - `which xxx`—查看某个指令在执行什么程序，e.g. `which echo`.
- Navigating in the shell
  - `pwd`: print working directory
  - `cd`: change directory
    - `..`: parent directory 
    - `.`: current directory
    - `~`: home directory
    - `-`: previous directory
  - `ls`: read and list all the files
    - `--help` or `-h`: 查看命令的帮助
    - `ls -l`: 查看具体的信息(use a long listing format):

返回信息分析：

**permission** for user owner, group owner, everyone else.

```shell
missing:~$ ls -l /home
drwxr-xr-x 1 missing  users  4096 Jun 15  2019 missing
```

- [x] **总结**

`d`说明`missing`一个directory, 

接下来三个三字符组表示该文件(夹)对于the owners of the file(`missing`), the owning group(`users`) and everyone else所属的permission. `-`表示没有拥有权力, 其中：只有owner可以modify(w)这个文件夹,例如添加或者删除其中的文件；如果要进入该文件夹，必须拥有search(execute='x')的权力；如果要查看里面的内容，必须拥有read(r)的权力。

- other functions
  - `mv`—rename file
  - `cp`—copy from x to y
  - `rm`—remove the file
  - `rdmir`—remove a **empty** directory.
  - `man`—similar with `--help`

- Connecting programs: manage the input stream and output stream

  - `>` and `<` : **rewire** the in-/output streams

  - `cat`—print the content of a file

    - interesting of copy 

      ```shell
      cat < hello.txt > hello2.text
      ```

  - `>>` : append 

  - pipe character `|`—'chain' programs such that the output of one is the input of another

- A versatile and powerful tool
  - 'root user' = administration: You will not usually log into your system as the root user though, since it’s too easy to accidentally break something.
  - `sudo`—do as superuser(root)
  - `cd sys`—change to the  **kernel** of the computers
  - `sudo su`—change the terminal to the root user

- [x] 这里有两种sudo的尝试，一种不work:

  1. 切换用户到root user: `sudo su`
  2. `xx | sudo xxx `: 注意 `|,>,<`这些都是是由shell本身执行的，若将`sudo`放在前面不能起到sudo的效果！！ 

  
