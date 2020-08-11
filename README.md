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
  
2. `xx | sudo xxx `: 注意 `|,>,<`这些都是是由shell本身执行的，若将`sudo`放在前面不能起到sudo的效果!!
  
     
  

### Lecture2. Shell Tools and Scripting

##### Shell Scripting

- assign variable: (no whitespace!)

  ```shell
  foo=bar
  ```

- access variable:

  ```shell
  $foo	
  ```

- define string: strings with `''` are literal string and will not substitute(替换) variables

  ```shell
  foo=bar
  echo "$foo"
  # prints bar
  echo '$foo'
  # prints $foo
  ```

- functions: 

  ```shell
  mcd () {
      mkdir -p "$1"
      cd "$1"
  }
  ```

  - `$0`: name of the script

  - `$1` to `$9` : arguments to the script

  - `$@`: all arguments

  - `$#`: number of arguments

  - `$?`: return code of the previous command.  The return code or exit status is the way scripts/commands have to communicate how execution went. A value of 0 usually means everything went OK; anything different from 0 means an error occurred.

    ```shell
    echo "Hello"
    # print Hello
    echo $?
    # print 0
    false
    echo $?
    # print 1
    ```

  - `$$`: process identification number for the current script

  - `!!`: entire last command. `sudo !!` 前一指令无权限时，使用

  - `$_`: last argument from the last command

- `||`(or) and `&&`(and) operators: *short-circuiting* operators(偷懒!)

  `;`: semicolon

  ```shell
  false || echo "Oops, fail"
  # Oops, fail
  
  true || echo "Will not be printed"
  #
  
  true && echo "Things went well"
  # Things went well
  
  false && echo "Will not be printed"
  #
  
  true ; echo "This will always run"
  # This will always run
  
  false ; echo "This will always run"
  # This will always run
  ```

- command substitution: `$(CMD)` it will execute `CMD`, get the output if the command and substitute it in place, *e.g.* `for file in $(ls)`

- process substitution: `< ( CMD )`会执行`CMD`并将结果输出到一个临时文件中，并将`< ( CMD )`替换为临时文件名。例如`diff < (ls foo) <(ls bar)`

- **example** 

  ```shell
  #!/bin/bash
  
  echo "Starting program at $(date)" # Date will be substituted
  
  echo "Running program $0 with $# arguments with pid $$"
  
  for file in "$@"; do
      grep foobar "$file" > /dev/null 2> /dev/null
      # When pattern is not found, grep has exit status 1
      # We redirect STDOUT and STDERR to a null register since we do not care about them
      if [[ $? -ne 0 ]]; then #-ne = not equal
          echo "File $file does not have any foobar, adding one"
          echo "# foobar" >> "$file"
      fi
  done
  ```

  - `grep` 用于查找文件里符合条件的字符串 
  - the standard output stream and standard error stream are separated, you need to tell bash what to do with each one of them
    - `>`: redirect the standard output
    - `2>`: redirect the standard error
  - `/dev/null`: a special device where you can write and it will be discarded

- shell globbing(通配)

  - wildcards(通配符)

    - `?` 匹配一个字符
    - `*` 匹配任意个字符

  - curly braces `{}`: common substring in a series of commands

    ```shell
    convert image.{png,jpg}
    # Will expand to
    convert image.png image.jpg
    
    cp /path/to/project/{foo,bar,baz}.sh /newpath
    # Will expand to
    cp /path/to/project/foo.sh /path/to/project/bar.sh /path/to/project/baz.sh /newpath
    
    # Globbing techniques can also be combined
    mv *{.py,.sh} folder
    # Will move all *.py and *.sh files
    
    
    mkdir foo bar
    # This creates files foo/a, foo/b, ... foo/h, bar/a, bar/b, ... bar/h
    touch {foo,bar}/{a..h}
    touch foo/x bar/y
    # Show differences between files in foo and bar
    diff <(ls foo) <(ls bar)
    # Outputs
    # < x
    # ---
    # > y
    ```

    

