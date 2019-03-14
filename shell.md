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
