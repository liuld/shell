# 元字符
    . 匹配除换行符之外的任意单个字符，awk中可以匹配换行符
    *匹配0到多个前边字符
    [...] 匹配方括号中的任意一个字符，^为否定匹配， -表示字符的范围
    ^ 作为正则表达式的第一个字符，匹配行的开始。在awk中可以嵌入换行符
    $ 作为正则表达式的最后一个字符，匹配行的结尾。在awk中可以嵌入换行符
    \{n,m\} 匹配出现的n到m次数， \{n\}匹配出现n次。\{n,\}匹配至少出现
    + 匹配前面的正则表达式的一次出现或多次出现
    ? 匹配前面的正则表达式的零次出现或一次出现
    | 可以匹配前面的或后面的正则表达式（替代方案）
    () 替换方案 对正则表达式分组 
    
# 寻址上的全局透视（定址）
    sed '/Beijing/s/CN/China/' file2.txt   
    sed '2s/CN/China/' file2.txt   替换第二行
    sed '/^\.TS/,/^\.TE/s/CN/China/' file2.txt    将以.Ts开头和以.TE开头之间的行用China替换CN
    sed '4,6s/CN/China/' file2.txt         从第四行到第六行进行替换
    sed '4,$s/CN/China/' file2.txt         从第四行到结尾进行替换
    sed '4,/guangzhou/s/CN/China/' file2.txt   从第四行到匹配到guangzhou的行之间进行替换
    
# 删除命令d
    删除所有的行 d
    只删除第一行 1d
    使用寻址符号$，删除最后一行  $d
    删除空行，正则表达式必须封闭在斜杠//当中   /^$/d
    删除.TS 和.TE 标记的tbl 输入   /^\.TS/,/^\.TE/d
    删除第五行到结尾所有的行5,$d
    混合使用行地址和模式地址    sed '1,/^$/d' file2.txt
    删除除了那些行以外的行    1,5!d

# 替换
    n 可以是1-512，表示第n次出现的情况进行替换 如:
        ababab
        sed ‘s/ab/oo/2’ file
    g 全局更改 如:
        apple apple apple
        sed ‘s/apple/ooo/g’file
    p 打印模式空间的内容(完成替换后)如
        apple apple apple
        123 123 123
        sed 's/apple/123/p' file.txt
        sed -n '起始行~行距（每次跳几行）p' 文件名
        只打印apple行 所以能用-n参数
    w file 写入到一个文件file中(只保存替换后行) 如:  sed  's/ab/OOO/w a.txt' test
    & 用正则表达式匹配的内容进行替换  sed 's/apple/&&/' a.txt   &表示前边的正则表达式 2个&代表输出2次apple
    \n 回调参数 (前边的分组再拿回来)   sed 's/\(.*\):\(.*\)/\2:\1/' test1
    sed -n '/Jose/{=;p}' /tmp/1.txt
    
# 追加、插入和更改
    sed '/xx/a hello' test.txt        在匹配行后面追加一行hello
    sed '/xx/i hello' test.txt        在匹配行前面插入一行hello
    sed '/xx/c hello' test.txt        把匹配的行替换成hello

# 分组命令
    不使用分组  sed '/^\.TS/,/^\.TE/s/,/:/;/^\.TS/,/^\.TE/s/CN/China/' file2.txt
    使用分组    sed '/^\.TS/,/^\.TE/{s/,/:/;s/CN/china/}' file2.txt
    
# 实例
    1.把Jon's的名字改成Jonathan.
    [root@manager test6]# sed -n "s/Jon's/Jonathan/p" test6.txt
    2.删除头三行
    [root@manager test6]# cat -n test6.txt  | sed '1,3d'
    3.显示5-10行
    [root@manager test6]# cat -n test6.txt | sed -n '5,10p'
    [root@manager test6]# cat -n test6.txt | sed '5,10!d'
    4.删除包含Lane的行.
    [root@manager test6]# sed '/Lane/d' test6.txt | grep Lane
    5.显示所有生日在November-December之间的行
    [root@manager test6]# sed -n '/:1[12]\//p' test6.txt
    6.把三个星号(***)添加到也Fred开头的行
    [root@manager test6]# grep Fred test6.txt | sed '/^Fred/s/^/***/'
    [root@manager test6]# grep Fred test6.txt | sed 's/^Fred/***Fred/'
    7.用JOSE HAS RETIRED取代包含Jose的行
    [root@manager test6]# grep Jose test6.txt | sed '/Jose/s/.*/JOSE HAS RETIRED/'
    8.把Popeye的生日改成11/14/46
    [root@manager test6]# grep Popeye test6.txt | sed -r '/Popeye/s@[1]?[0-9]/[1-3]?[0-9]/[0-9][0-9]@11/14/46@'
    9.删除所有空白行
    [root@manager test6]# sed '/^$/d' test6.txt 

# n命令
    输出模式空间的内容，然后读取输入的下一行，而不用返回脚本的顶端   
    test aa bb 
    test aa bb
    sed '/test/{n;s/aa/xx}' test.txt    只替换掉下面的aa
# 读和写文件
    [line-address]r file
    [address]w file
    sed -e '/<mail list>/r maillist' -e '/<mail list>/d' file4.txt
    sed -n '/MA$/w region.MA' file4.txt
# 高级sed 命令
    高级命令分成3个组：
    1 处理多行模式空间（N、D、P）
    2 采用保持空间来保存模式空间的内容并使它可用于后续的命令
    （H、h、G、g）
    3 编写使用分支和条件指令的脚本来更改控制流（:、b、t）
    高级脚本都做一件共同的事，那就是他们改变了执行或控制的流程顺序。

    多行模式空间  多行Next（N）命令通过读取新的输入行，并将它添加到模式空间的现有内容  之后来创建多行模式。(相当一次读两行输出)
    sed 'N;' /tmp/2.txt
    
    多行删除 D命令删除模式空间中直到第一个换行符的内容。它不会导致读入新的输入行，相反，它返回到脚本的顶端，将这些指令应用与模式空间剩余的内容。
    多行删除命令完成工作的原因是，当遇到两个空行时，D命令只删除两个空行中的第一个 换句话说，当模式空间中有两个空行时，只有第一个空行被删除。
    当一个空行后面跟有文本时，模式空间可以正常输出。
    sed '/^$/{N;/^\n$/D}' user.txt
    
# 练习
    1，删除文件每行的第一个字符。
    [root@manager zuoye]# sed 's/.//' aa.txt  
    [root@manager zuoye]# sed -r 's/(.)(.*)/\2/' aa.txt
    2，删除文件每行的第二个字符。
    [root@manager zuoye]# sed -r 's/(.)(.)(.*)/\1\3/' aa.txt
    3，删除文件每行的最后一个字符。
    [root@manager zuoye]# sed -r 's/(.*)(.)$/\1/' aa.txt
    4，删除文件每行的倒数第二个字符。
    [root@manager zuoye]# sed -r 's/(.*)(.)(.)$/\1\3/' aa.txt 
    5，删除文件每行的第二个单词。
    [root@manager zuoye]# sed -r 's/([a-Z]+)([^a-Z]+)([a-Z]+)/\1\2/' aa.txt 
    6，删除文件每行的倒数第二个单词。
    [root@manager zuoye]# sed -r 's/(.*)([^a-Z]+)([a-Z]+)([^a-Z]+)([a-Z]+)/\1\2\4\5/' aa.txt
    7，删除文件每行的最后一个单词。
    [root@manager zuoye]# sed -r 's/(.*)([^a-Z]+)([a-Z]+)/\1\2/' aa.txt 
    8，交换每行的第一个字符和第二个字符。
    [root@manager zuoye]# sed -r 's/(.)(.)(.*)/\2\1\3/' aa.txt 
    9，交换每行的第一个字符和第二个单词。
    [root@manager zuoye]# sed -r 's/(.)([a-Z]+)?([^a-Z]+)?([a-Z]+)/\4\2\3\1/' aa.txt
    10，交换每行的第一个单词和最后一个单词。
    [root@manager zuoye]# sed -r 's/([a-Z]+)([^a-Z]+)(.*)([^a-Z]+)([a-Z]+)/\5\2\3\4\1/' aa.txt  
    11，删除一个文件中所有的数字。
    [root@manager zuoye]# sed 's/[0-9]//g' aa.txt
    12，删除每行开头的所有空格。
    [root@manager zuoye]# sed 's/^ *//g' aa.txt
    13，用制表符替换文件中出现的所有空格。
    [root@manager zuoye]# sed 's/ /\t/g' aa.txt 
    14，把所有大写字母用括号（）括起来。
    [root@manager zuoye]# sed 's/[A-Z]/(&)/g' /etc/passwd
    15，打印每行3次。
    [root@manager zuoye]# sed -n 'p;p;p' aa.txt 
    16，隔行删除
    [root@manager zuoye]# cat -n  /etc/passwd | sed 'n;d'。
    [root@manager zuoye]# cat -n  /etc/passwd | sed '1d;n;d'
    [root@manager zuoye]# cat -n  /etc/passwd | sed '0~2d'
    [root@manager zuoye]# cat -n  /etc/passwd | sed '1~2d'

    17，把文件从第22行到第33行复制到第56行后面。
    [root@manager zuoye]# cat -n /etc/passwd | sed '22h;23,33H;56G'
    18，把文件从第22行到第33行移动到第56行后面。
    [root@manager zuoye]# cat -n /etc/passwd | sed '22{h;d};23,33{H;d};56G'
    19，只显示每行的第一个单词
    [root@manager zuoye]# sed -r 's/([a-Z]+)([^a-Z]+)(.*)/\1/' aa.txt
    20，打印每行的第一个单词和第三个单词。
    [root@manager zuoye]# sed -r 's/([a-Z]+)([^a-Z]+)([a-Z]+)([^a-Z]+)([a-Z]+)(.*)/\1:\5/' aa.txt
    21，将格式为    mm/yy/dd    的日期格式换成   mm；yy；dd
    [root@manager zuoye]# echo "mm/yy/dd " | sed 's@/@:@g'

    
    
