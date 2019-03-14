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

# sort
    sort -n -r aa.txt    按完整数字排序 降序
    sort -u aa.txt  去掉重复值 
    sort -t: -k3nr /etc/passwd  以:分割按第三字段降序排序
# uniq
    uniq aa.txt  连续的去掉
    uniq -u aa.txt  显示未连续重复值
    sort -n aa.txt  | uniq -u 排序去重
    sort -n aa.txt  | uniq -d  显示连续重复行
    sort -n aa.txt  | uniq -d -c  统计重复次数
# grep
    --color 
    -i -v -r -n -A -B -C -l -x
    grep -c root /etc/passwd 统计行数
    grep -A/B/C 2 root /etc/passwd  显示匹配行和匹配行的下/上/上下二行
    grep -n root /etc/passwd     显示行号
    grep -r root /etc/    过滤出etc目录下所有文件内容包含root的文件
    grep -rl root /etc/   过滤出etc目录下所有文件内容包含root的文件名
    grep -x root:x:0:0:root:/root:/bin/bash /etc/passwd    完全匹配，相当于^root:x:0:0:root:/root:/bin/bash$

# cut
    cut -c 1/1,3/1-5 /etc/passwd    截取文件每行第1/1和3/1到5个字符串
    cut -d: -f1/1,3/1-5 /etc/passwd         以:分割取第一/1和3/1到5分割域
# tr
    • 大小写转换。 
    • 去除控制字符。 
    • 删除空行。
    - s选项去掉重复字符
        echo "hellooooooo worlddddddd" | tr -s "[a-z]"          #helo world
    删除空行
        tr -s "[\n]" < filename
    大小写转换
        tr "[a-z]" "[A-Z]" < filename
    如果需要删除文件中^M，并代之以换行。使用命令：
        tr -s "[\r]" "[\n]" < file.txt
# wc
    -c 统计字节数。
    -l 统计行数。
    -m 统计字符数。这个标志不能与 -c 标志一起使用。
    -w 统计字数。一个字被定义为由空白、跳格或换行字符分隔的字符串。
    -L 打印最长行的长度。
    -help 显示帮助信息
    --version 显示版本信息

# date
    date -s "20161015 10:10:10"
    date +%Y-%m-%d-%H-%M-%S
# logger
    [root@manager tmp]# logger "hello i am robin"
    自定日志保存位置
    [root@manager tmp]# vim /etc/rsyslog.conf
    local5.*                                                /tmp/test.log
    [root@manager tmp]# service rsyslog restart

    [root@manager tmp]# logger -p local5.err -t test  -f /tmp/test.log  hhhhhhh
    -p日志级别
    -t 标记
    -f 日志位置
# bc
    [root@manager ~]# echo "1+2" | bc
    3
    [root@manager ~]# echo "scale=3;3/2" | bc
    1.500
    [root@manager ~]# echo "ibase=10;obase=2;7" | bc
    111

    [root@manager tmp]# echo "obase=8;19" | bc
    23
    [root@manager tmp]# echo "obase=2;F" | bc
    1111

    [root@manager tmp]# echo "2^10" | bc
    1024
    计算平方根
    [root@manager ~]# echo "sqrt(100)" | bc
    10

