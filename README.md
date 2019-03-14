# find
    find命令
    常用选项
            -name
            -type
            -user
            -nouser
            -group
            -nogroup
            -mtime
            -size
    可以使用 -o 或者 -a 连接多个条件,可以使用-exec或者-ok来执行shell命令 
    find /etc/ -name hosts -exec cp {} /tmp/ \;
    find /var/logs -type f -mtime +7 -exec rm {} \;

# xargs
    [root@manager home]#  find / -name "core" -exec file {} \;
    [root@manager home]#  find / -name "core" |xargs  file
    
# nohup
    nohup xclock -update 1 &
    
# echo
    echo命令
    -e 使转义符生效 如:  解释\t \n含义
    -n 不换行输出

    字颜色：30—–37 
    echo -e “\033[30m 黑色字 \033[0m” 
    echo -e "\033[31m 红色字 \033[0m"
    echo -e “\033[32m 绿色字 \033[0m” 
    echo -e “\033[33m 黄色字 \033[0m” 
    echo -e “\033[34m 蓝色字 \033[0m” 
    echo -e “\033[35m 紫色字 \033[0m” 
    echo -e “\033[36m 天蓝字 \033[0m” 
    echo -e “\033[37m 白色字 \033[0m”

    字背景颜色范围：40—–47 
    echo -e “\033[40;37m 黑底白字 \033[0m” 
    echo -e “\033[41;37m 红底白字 \033[0m” 
    echo -e “\033[42;37m 绿底白字 \033[0m” 
    echo -e “\033[43;37m 黄底白字 \033[0m” 
    echo -e "\033[44;37m 蓝底白字 \033[0m" 
    echo -e “\033[45;37m 紫底白字 \033[0m” 
    echo -e “\033[46;37m 天蓝底白字 \033[0m” 
    echo -e “\033[47;30m 白底黑字 \033[0m”

    改变提示符文件的颜色
    [root@manager home]# echo -e "\033[40;32m"

    报警声音
    [root@manager home]# echo -e "\007 the bell ring"
   
# read
    赋值
    [root@manager home]# read name
    zhangsan
    [root@manager home]# echo $name
    zhangsan

    赋多值
    [root@manager home]# read firstname lastname
    huibin zhang
    [root@manager home]# echo $firstname $lastname
    huibin zhang

    交互式:
    [root@manager home]# read -p "input a num: " var
    input a num: 123
    [root@manager home]# echo $var
    123

# 重定向(文件描述符)
    文件描述符:进程连接到文件时,获得的一个标记.当进程操作文件时,首先
    打开文件 获得打开文件的一个状态,给它一个标记 成为文件描述符
    0标准输入
    1标准输出
    2标准错误输出

    > >> 定向符(重定向) >覆盖  >>追加
    1> 标准正确输出,文件存在则覆盖,不存在则创建
    1>> 标准正确输出,文件存在则追加,不存在则创建
    2> 标准错误输出,文件存在则覆盖,不存在则创建
    2>> 标准错误输出,文件存在则追加,不存在则创建
    &> 标准正确和标准错误输出,文件存在则覆盖,不存在则创建

    cat < /dev/sda > /dev/null 测试改变文件描述符

    ls >cleanup.out 2>&1
    在上面的例子中，我们将 ls命令的输出重定向到 c l e a n u p . o u t文件中，而且其错误也 被重定向到相同的文件中。
    2>&1 标准错误输出定向到标准正确输出

    cat > x.txt << EOF 
    > sdfsadlkf
    > asdfsadhf
    > asfdhkasfd
    > EOF  ------------直到遇到EOF结束

# tee
    tee命令命令作用可以用字母 T来形象地表示。它把输出的一个副本输送到标准输出，另一个 副本拷贝到相应的文件中。
    如果希望在看到输出的同时，也将其存入一个文件, 这种情况可以使用tee命令
    如:
    [root@manager home]# who | tee who.out
    root     pts/1        2016-09-18 07:50 (192.168.10.102)
    [root@manager home]# cat who.out 
    root     pts/1        2016-09-18 07:50 (192.168.10.102)
