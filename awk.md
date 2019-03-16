# awk语法 
    awk [options] 'commands' files
# option和内置变量
    -F 定义字段分隔符,默认的分隔符是连续的空格或制表符,用$1,$2,$3等的顺序表示files中每行以间隔符号分隔的各列不同域.$0代表整行
        awk -F: '{print $1}' /etc/passwd
    NF 变量表示当前记录的字段总数 $NF表示最后一个字段的值
    -v 可以借用此方式从shell变量中引入
        a=root;awk –v var=$a –F':' '$1==var{print $0}' /etc/passwd
    FS 定义字段分隔符,默认为一个空格,相当于awk -F
    OFS 输出的字段分隔符，默认为一个空格,相当与把FS替换成OFS
        awk 'BEGIN{FS=":";OFS="---";}{print $1,$2}' /etc/passwd
    RS 记录分隔符,默认为一个换行符. 以RS为分隔符每个字段换行输出
        head -2 /etc/passwd| awk 'BEGIN{RS=":"}{print $0}'
    ORS 输出的记录分隔符，默认为一个换行符.RS的反向：就是把多行以ORS为分隔符连在一起当一行输出
    NR  行数    awk 'NR<3{print $0}' /etc/passwd   输出前两行
    FNR 行数，多文件操作时会重新排序
    FILENAME 文件名
    ARGC 命令行参数个数
    ARGV 命令行参数排列
    ENVIRON 输出系统环境变量    awk 'BEGIN{print ENVIRON["USER"]}'
# 读前处理  行处理   读后处理
    读前处理BEGIN{awk_cmd1;awk_cmd2}      读后处理END {awk_cmd1;awk_cmd2;}
        awk 'BEGIN{print "start handle file"}{print $0}END{print"stop handle file"}' /etc/passwd
    行处理: 定址命令  
        定址方法: 正则,变量,比较和关系运算   正则需要用//包围起来
            awk -F: '/root/{print}' /etc/passwd
            awk -F: '$1~/root/{print}' /etc/passwd
            awk -F: '$1!~/root/{print}' /etc/passwd
            ^ 行首
            $ 行尾
            . 除了换行符以外的任意单个字符
            * 前导字符的零个或多个
            .* 所有字符
            [] 字符组内的任一字符
            [^]对字符组内的每个字符取反(不匹配字符组内的每个字符)
            ^[^] 非字符组内的字符开头的行
            [a-z] 小写字母
            [A-Z] 大写字母
            [a-Z] 小写和大写字母
            [0-9] 数字
            \< 单词头单词一般以空格或特殊字符做分隔,连续的字符串被当做单词
            \> 单词尾
            == >= <= != > <
            awk 'NR==1 {print}' /etc/passwd
# 操作符
    赋值= ++ -- += -= /= *= 
    ++var 先var+1狗后执行命令   var++ 先执行命令后再var+1
    || Logical OR  逻辑 两边任意一边成立
    && Logical AND 逻辑与 两边成立  前边如果假值后边就不做了
    ! Logical NOT  逻辑取反 原本真值变成假值
    匹配正则或不匹配,正则需要用/正则/ 包围住
    匹配正则或不匹配,正则需要用/正则/ 包围住    ~     !~
    关系比较字符串时要把字符串用双引号引起来    < <= > >= != ==
    
# 练习题:
    1打印uid在30~40范围内的用户名
    awk -F: '$3>=30&&$3<=40{print $1,$3}' /etc/passwd
    2打印第5-10行的行号和用户名
    awk -F: 'NR>=5&&NR<=10{print $1,NR}' /etc/passwd
    3 打印奇数行
    awk -F: 'NR%2==1{print NR,$0}' /etc/passwd
    4 打印偶数行
    awk -F: 'NR%2==0{print NR,$0}' /etc/passwd
    5 打印字段数大于5的行
    awk -F: 'NF>5{print $0}' /etc/passwd
    6打印UID不等于GID的用户名
    awk -F: '$3!=$4{print $0}' /etc/passwd
    7.打印1..100以内的7的倍数和包含7的数
    seq 1 100 | awk '$1~/7/||$1%7==0{print $1}'
    8.计算UID相加的总和;计算GID相加的总和
    awk -F: '{uid+=$3;gid+=$4}END{print uid;print gid}' /etc/passwd
    9.计算VSZ和RSS各自的和 并以M单位显示
    ps aux|awk 'NR>1{vsz+=$5;rss+=$6}END{print vsz/1024"M";print rss/1024"M"}'
    10.统计普通用户个数
    awk 'BEGIN{sum=0;FS=":"}{if($3>=500){sum+=1}}END{print sum}' /etc/passwd
    11.将系统用户按UID分组标记 0 admin; 1-499 sysuser; 500+ users   用户名  uid   权限
    awk 'BEGIN{FS=":";OFS="\t";print "用户名","uid","权限"}{if($3==0){print $1,$3,"admin"}else if($3>0&&$3<500){print $1,$3,"sysuser"}else{print $1,$3,"users"}}' /etc/passwd
    12.倒序输出各字段
    awk 'BEGIN{FS=":";}NR==1{for(i=NF;i>0;i--){print $i}}' /etc/passwd
# 流程控制
    if (条件) 动作,若有多个动作,则要用大括号将动作体包含起来if (条件) {动作1;动作2}else if(条件){动作1;动作2}else{动作1;动作2}
        awk 'BEGIN{FS=":"}{if($1=="root"){print $1;print$2}}' /etc/passwd
        awk -F: '{if($1=="root"){print $1,$NF}else{print $1}}' /etc/passwd
# 循环语句
    while(条件) { 动作;条件运算}
        awk 'BEGIN{while(i<100){print i;i++}}'
        awk 'BEGIN{i=0;while(i<=100){if(i%2==0){print i};i++}}'
    
    for(预置;条件;递增) {动作}
        awk 'BEGIN{for(i=0;i<=100;i++){print i}}'
# 跳转语句  
    break 跳出循环   
        awk 'BEGIN{for(x=1;x<5;x++){if(x==3){break}print x}}'
    next 读入下一行同时返回脚本顶部这样可以避免对当前行执行其他操作
        awk -F: 'NR > 5 {next} {print $1} END {print NR}' /etc/passwd
# 数组
    自定义数组
        # awk 'BEGIN {ary[1]="seker";ary[2]="zorro";print ary[1],ary[2]}'
            seker zorro
        # awk 'BEGIN {ary[1]="seker";ary[2]="zorro";for(i in ary) print ary[i]}'
            seker
            zorro
    循环产生数组和取出数组
        # awk 'BEGIN{n=5;for (i=1;i<=n;i++) ary[i]=i+100;for(m in ary) print m,ary[m]}'
        awk 'BEGIN{FS=":"}{arr[NR]=$1;}END{for(i in arr){print i,arr[i]}}' /etc/passwd
# awk的内置函数
    算数函数 sqrt函数(求平方根) int
        awk 'BEGIN{x=sqrt(100);print x}'           10
        awk 'BEGIN{x=int(3.88);print x}'           3
    rand函数(内置函数 srand 使用参数awk 'BEGIN{x=rand();print x}'作为随机数种子。当参数缺省的时候，使用当前时间作为随机数种子。)
    srand函数(内置函数 rand 的伪随机函数，其返回值范围为  0 >= result <= 1。在实际使用时，一般先使用 srand 函数生成随机数种子，
    然后再使用 rand 函数生成随机数。否则每次得到的值会一样。)
        awk 'BEGIN{srand();x=int(100*rand());print x}'
        
    字符串函数: sub和gsub函数使用(替换字符)
        [root@manager ~]# awk 'BEGIN{info="this is a test2010test2010test!";sub(2010,"!",info);print info}'
        this is a test!test2010test!
        [root@manager ~]# awk 'BEGIN{info="this is a test2010test2010test!";gsub(2010,"!",info);print info}'
        this is a test!test!test!
    index函数(查找字符)
        [root@manager ~]# awk 'BEGIN{info="this is test2010test!";print index(info,"test")?"ok":"not found";}'
        ok
    length函数(计算长度)
        [root@manager ~]# awk 'BEGIN{info="hello";print length(info)}'
        5
    substr函数(截取字符串)
        [root@manager ~]# awk 'BEGIN{info="hello world";print substr(info,4,10)}'
        lo world
    match函数(正则匹配查找)
        [root@manager ~]# awk 'BEGIN{info="hello 2016world";print match(info,/[0-9]+/)?"ok":"none";}'
        ok
    split函数(字符分割)
        [root@manager ~]# awk 'BEGIN{info="hello world";split(info,test);print length(test);for(k in test){print k,test[k];}}'
        2
        1 hello
        2 world
    close函数
        [root@manager ~]# awk 'BEGIN{while("head -5 /etc/passwd"|getline){print $0;};close("/etc/passwd");}'
        root:x:0:0:root:/root:/bin/bash
        bin:x:1:1:bin:/bin:/sbin/nologin
        daemon:x:2:2:daemon:/sbin:/sbin/nologin
        adm:x:3:4:adm:/var/adm:/sbin/nologin
        lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
    getline函数
        [root@manager ~]#  awk 'BEGIN{while(getline < "/etc/passwd"){print $0;};close("/etc/passwd");}'
    
    system函数
        [root@manager ~]#  awk 'BEGIN{b=system("ls -l /root");print b;}'
        总用量 20
        -rw-r--r--  1 root root  135 9月  18 16:14 aa.txt
        -rw-------. 1 root root  905 9月   9 06:30 anaconda-ks.cfg
        -rw-r--r--. 1 root root 8003 9月   9 06:30 install.log
        -rw-r--r--. 1 root root 3384 9月   9 06:30 install.log.syslog
        0
        awk -F: 'BEGIN{x=system("id robin &>/dev/null");if(x==0){while(getline < "/etc/passwd")if($1=="robin")print $1,$6}}'
        robin /home/robin
# 时间函数
    mktime( YYYY MM DD HH MM SS[ DST]) 生成时间格式 
    strftime([format [, timestamp]])  格式化时间输出，将时间戳转为时间字符串 
    systime() 得到时间戳,返回从1970年1月1日开始到当前时间(不计闰年)的整秒数 
    创建时间格式
        awk 'BEGIN{tstamp=mktime("2001 01 01 12 12 12");print strftime("%c",tstamp);}'
        2001年01月01日 星期一 12时12分12秒
        awk 'BEGIN{tstamp1=mktime("2001 01 01 12 12 12");tstamp2=mktime("2001 02 01 0 0 0");print tstamp2-tstamp1;}'
        2634468
        awk 'BEGIN{tstamp1=mktime("2001 01 01 12 12 12");tstamp2=systime();print tstamp2-tstamp1;}' 
        495876393
    strftime日期和时间格式说明符
        | 格式 | 描述 |
        | %a | 星期几的缩写(Sun) |
        | %A | 星期几的完整写法(Sunday) |
        | %b | 月名的缩写(Oct) |
        | %B | 月名的完整写法(October) |
        | %c | 本地日期和时间 |
        | %d | 十进制日期 |
        | %D | 日期 08/20/99 |
        | %e | 日期，如果只有一位会补上一个空格 |
        | %H | 用十进制表示24小时格式的小时 |
        | %I | 用十进制表示12小时格式的小时 |
        | %j | 从1月1日起一年中的第几天 |
        | %m | 十进制表示的月份 |
        | %M | 十进制表示的分钟 |
        | %p | 12小时表示法(AM/PM) |
        | %S | 十进制表示的秒 |
        | %U | 十进制表示的一年中的第几个星期(星期天作为一个星期的开始) |
        | %w | 十进制表示的星期几(星期天是0) |
        | %W | 十进制表示的一年中的第几个星期(星期一作为一个星期的开始) |
        | %x | 重新设置本地日期(08/20/99) |
        | %X | 重新设置本地时间(12：00：00) |
        | %y | 两位数字表示的年(99) |
        | %Y | 当前月份 |
        | %Z | 时区(PDT) |
        | %% | 百分号(%) |

# printf函数
    是格式化输出函数, 一般用于向标准输出设备按规定格式输出信息。在编写程序时经常会用到此函数。printf()函数的调用格式为:
        %d 十进制有符号整数
        %u 十进制无符号整数
        %f 浮点数
        %s 字符串
        %c 单个字符
        %p 指针的值
        %e 指数形式的浮点数
        %x, %X 无符号以十六进制表示的整数
        %o 无符号以八进制表示的整数
        %g 自动选择合适的表示法
        \n 换行
        \f 清屏并换页
        \r 回车
        \t Tab符
        \xhh 表示一个ASCII码用16进表示,其中hh是1到2个16进制数

    输出样式
        %s是字符类型,%d数值类型
        printf默认是不输出换行的所以要加\n
        10和7是偏移量
        默认是右对齐,所有加个- 就是左对齐,就是把不足的位数用空格填充
        注意:格式与输出列之间要有逗号

    awk 'BEGIN{a=16;printf "%-10o\n",a}'
    20
