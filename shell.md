# 每个用户家目录下的环境变量配置文件:
    .bash_history  保存用户执行过来历史命令,当用户退出时保存
    .bash_logout  保存用户退出时执行的命令

    .bash_profile  保存用户定义环境和启动项目,用户执行命令时的搜索路径
    .bashrc  保存用户别名和函数

    .bash_profile  登录级别环境配置文件
    .bashrc  shell级别的环境配置文件

    /etc/bashrc 全局shell级别环境配置文件
    /etc/profile 全局登录级别环境配置文件

    登录时加载的配置文件顺序
    /etc/profile
    .bash_profile
    .bashrc
    /etc/bashrc

    su - robin  和 su robin 切换帐号的区别
    su - robin 登录级别切换
    su robin shell级别切换
    
    1.当robin用户退出时,清除自己所有的历史命令
    [root@manager robin]# vim /home/robin/.bash_logout
     rm -rf /home/robin/.bash_history
    2.设置别名myip 要求:所有用户可以调用这个别名
    [root@manager robin]# vim /etc/bashrc 
    alias myip="ifconfig eth2 | grep Bcast | cut -d':' -f 2 | cut -d' ' -f 1"
    3.当一个用户登录时将这个用户登录的用户名,时间写入到/tmp/login.txt文件
    [root@manager robin]# vim /etc/profile
    echo "$USER" 'login' `date`  >> /tmp/login.txt
    
# 参数变量
    $0 进程名(如:/etc/init.d/network)
    $$ 进程号(/var/run 模拟系统结束进程)
    $# 位置参数的数量
    $* 所有位置参数的内容
    $? 命令执行后的返回状态.0为执行正确，非0为执行错误

# 算式运算符
    +、-、*、/、()
    1.$((5+3))
    2.$[ 5+3 ]
    3.expr操作符：+、-、\*、/、%取余（取模）  expr 1 + 2
    4.a=1;b=2;let c=$a+$b;echo $c
# 通配符
    *匹配任所有字符
    ?匹配一个字符
    []匹配一个范围
    {}如touch abc{a,b,c}{1..3}.txt
# test命令的使用
  语法:test EXPRESSION 或者 [ EXPRESSION ]
  -n 字符段长度是否非零的 如果结果为真值 返回值为0 如果结果为假值,返回值非0   test -n "$var"  
  -z 判断变量是否为空，空返回0真   非空返回1假     test -z $var
  test 整数
    eq 等于
    ge 大于等于
    gt 大于
    le 小于等于
    lt 小于
    ne 不等于
  test 文件
    ef 两个文件有相同的设备编号和inode编号 (判断硬链接)
    touch aa 
    ln aa bb
    ls -i 
    456733 aa  456733 bb

    根据文件类型判断
    -d 文件存在必须是个目录
    -e 有文件名存在就行不管类型
    -f 文件存在而且是个标准普通文件
    -h 文件存在并且是否为符号链接文件
    -r 判断文件权限是否有r权限
    -w 写权限
    -x 执行权限
# 条件判断语句
    if cmd;如为真值
    then
    fi   执行
    if语句的完整写法
    if cmd1
    then
                       run cmd1-1
                       run cmd1-2
    elif cmd2
    then
                       run cmd2-1
                       run cmd2-2
    elif cmd3
    then
                       run cmd3-1
                       run cmd3-2
    else 
                       run cmd4-1
                       run cmd4-2
    fi
# 循环语句:for
    #!/bin/bash
    for i in 1 3 5 7
    do
             echo $i
             echo ok
    done 

    seq 1 2 100 产生1-100个数字 步长为2  脚本就可以变得更简单了

    for循环的另一种写法 模拟c语言的写法  
    for ((i=0;i<10;i++))
    do
             echo $i
    done

    break 和continue 跳出循环
    break 跳出循环 脚本继续执行
    continue 跳出本次循环,脚本继续执行
    exit 退出脚本, 但是exit可以设置脚本返回值

# select循环
    select i in ls pwd whoami
    do
             $i
    done
# while和until循环
    while 后边跟命令 条件为真值时循环
    until 后边跟命令 条件为假值时循环

    #!/bin/bash
    #15.sh
    sum=0
    while [ $sum -lt 10 ]
    do
    sum=`expr $sum + 1`
    useradd user$sum
    echo "123456" | passwd --stdin user$sum
    done
    
    #!/bin/bash
    #16.sh
    x=1
    until [ $x -ge 10 ]
    do
    echo $x
    x=`expr $x + 1`
    done
    
# case语法结构
    case word in
    pattern1)
    list1
    ;;
    pattern2)
    list2
    ;;
    ... ...
    patternN)
    listN
    ;;
    *)
    list*
    ;;
    esac

# 变量替换
    var=${parameter:-word}   若 parameter 为空或未设置，则var=word，parameter 的值不变,否则var=$parameter

    var=${parameter:=3}   若 parameter 为空或未设置，则$var=$parameter=3   如果parameter设置了值，则$var=$pparameter

    var=${p:+3}     若$p设置了值，则$var=3 $p不变   否则$var $p都为空

# 字符串切片,替换
    ${var:5}    从第五个字符串开始切片到结尾
    ${var:3:5}  从第三个开始，切5个
    ${#var}     表示$var的长度
    ${var/str/}  第一次匹配的被替换(去掉)
    ${var//str/}  全局的匹配被替换
    ${var/str/newstr} 使用newstr替换str
    ${var//str/newstr}  替换所有str

    $ a=123456123789
    $ echo ${a#1*3}                     最短匹配截取
    456123789
    $ echo ${a##1*3}       最长匹配截取
    789

# shell的数组
    [root@manager ~]# ary=(a b c)
    数组取值
    [root@manager ~]# echo ${ary[0]}
    a
    [root@manager ~]# echo ${ary[1]}
    b
    [root@manager ~]# echo ${ary[2]}
    c
    [root@manager ~]# echo ${ary[3]}

    [root@manager ~]# echo $ary
    a

    或者设置数组的值为字符串
    [root@manager ~]# ary=("robin" "zorro" "lucy")
    [root@manager ~]# echo ${ary[0]}
    robin
    [root@manager ~]# echo ${ary[1]}
    zorro
    [root@manager ~]# echo ${ary[2]}
    lucy

    取数组所有值:
    [root@manager ~]# ary=("robin" "zorro" "lucy")
    [root@manager ~]# echo ${ary[@]}
    robin zorro lucy
    或者
    [root@manager ~]# echo ${ary[*]}
    robin zorro lucy

    数组的重新赋值
    [root@manager ~]# ary[0]="jerry"
    [root@manager ~]# echo ${ary[0]}
    jerry

    删除数组赋值
    [root@manager ~]# unset ary[0]
    [root@manager ~]# echo ${ary[0]}

    删除数组
    [root@manager ~]# unset ary
    [root@manager ~]# echo ${ary[@]}

    统计数组成员个数
    [root@manager ~]# ary=("robin" "zorro" "lucy")
    [root@manager ~]# echo ${#ary[@]}
    3

    统计数组成员字符个数 
    [root@manager ~]# ary=("robin" "zorro" "lucy")
    [root@manager ~]# echo ${#ary[1]}
    5
    [root@manager ~]# echo ${#ary[2]}
    4

    数组切片
    [root@manager ~]# ary=("robin" "zorro" "lucy")
    [root@manager ~]# ary=("robin" "zorro" "lucy" "tom" "jerry")
    [root@manager ~]# echo ${ary[@]:1:2}
    zorro lucy
    [root@manager ~]# echo ${ary[@]:1:3}
    zorro lucy tom

    数组成员切片
    [root@manager ~]# echo ${ary[0]:1:2}
    ob
    [root@manager ~]# echo ${ary[0]:1:3}
    obi
    [root@manager ~]# echo ${ary[0]:1:4}
    obin
    [root@manager ~]# echo ${ary[1]:2:2}
    rr

    遍历数组所有的值
    [root@manager ~]# for i in ${ary[@]}; do echo $i; done
    a
    b
    c

    关联数组
    Bash支持关联数组，它可以使用字符串作为数组索引，有时候采用字符串索引更容易理解。

    1.利用内嵌索引-值列表的方法
    [root@manager ~]# declare -A ary
    [root@manager ~]# ary=([robin]=beijing [zorro]=shanghai)
    [root@manager ~]# echo ${ary[robin]}
    beijing
    [root@manager ~]# echo ${ary[zorro]}
    shanghai

    2.使用独立的索引-值进行赋值
    [root@manager ~]# ary[jack]=hebei
    [root@manager ~]# ary[rose]=henan
    [root@manager ~]# echo ${ary[jack]}
    hebei
    [root@manager ~]# echo ${ary[rose]}
    henan

    取数组值
    [root@manager ~]# echo ${ary[*]}
    shanghai beijing
    取数组的键
    [root@manager ~]# echo ${!ary[*]}
    zorro robin

    获取所有键值对
    [root@manager ~]# for key in ${!ary[@]}
    do
        echo "$key = ${ary[$key]}"
    done
    zorro = shanghai
    robin = beijing
    
