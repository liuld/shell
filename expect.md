# expect
    #!/usr/bin/expect  
  
    set timeout 30  

    spawn ssh -l username 192.168.1.1  

    expect "password:"  

    send "ispass\r"  

    interact  

    1. ［#!/usr/bin/expect］  
    这一行告诉操作系统脚本里的代码使用那一个shell来执行。这里的expect其实和linux下的bash、windows下的cmd是一类东西。  
    注意：这一行需要在脚本的第一行。  

    2. ［set timeout 30］  
    基本上认识英文的都知道这是设置超时时间的，现在你只要记住他的计时单位是：秒  

    3. ［spawn ssh -l username 192.168.1.1］  
    spawn是进入expect环境后才可以执行的expect内部命令，如果没有装expect或者直接在默认的SHELL下执行是找不到spawn命令的。所以不要用 “which spawn“之类的命令去找spawn命令。好比windows里的dir就是一个内部命令，这个命令由shell自带，你无法找到一个dir.com 或 dir.exe 的可执行文件。  
    它主要的功能是给ssh运行进程加个壳，用来传递交互指令。  

    4. ［expect "password:"］  
    这里的expect也是expect的一个内部命令，有点晕吧，expect的shell命令和内部命令是一样的，但不是一个功能，习惯就好了。这个命令的意思是判断上次输出结果里是否包含“password:”的字符串，如果有则立即返回，否则就等待一段时间后返回，这里等待时长就是前面设置的30秒  

    5. ［send "ispass\r"］   
    这里就是执行交互动作，与手工输入密码的动作等效。  
    温馨提示： 命令字符串结尾别忘记加上 “\r”，如果出现异常等待的状态可以核查一下。  

    6. ［interact］  
    执行完成后保持交互状态，把控制权交给控制台，这个时候就可以手工操作了。如果没有这一句登录完成后会退出，而不是留在远程终端上。如果你只是登录过去执行一段命令就退出，可改为［expect eof］ 

# example
    #!/bin/bash

    expect << EOF
    spawn ssh root@192.168.0.51
    expect {
        "(yes/no)" {
            send "yes\n"
            expect "password:" {send "linchqd\n"}
        }
        "password:" { send "linchqd930520\n" }
    }
    expect end
    EOF
    
# shell脚本嵌套expect脚本
    cat ssh.sh
    #!/bin/bash
    pass=123
    /usr/bin/expect <<EOF
    set timeout 30
    spawn scp /root/install.log 192.168.1.1:/home
    expect "password:"
    send "$pass\r"
    expect eof
    EOF

