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
    
# 流程控制
