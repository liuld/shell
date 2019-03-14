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
