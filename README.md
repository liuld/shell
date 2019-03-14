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
