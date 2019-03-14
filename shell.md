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
