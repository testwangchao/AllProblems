### 问题描述：Jenkins配置GitHub时，会出现被拒绝的现象
##### 解决方式：
1. Jenkins在服务器是会单独启动一个jenkins用户
2. 生成公钥和私钥时，先切换为Jenkins，即su jenkins（此时可能会遇到无法切换的情况，/etc/passwd文件中的/bin/bash被yum安装的时候变成了/bin/false，,把false改为bash，然后再切换，当我切换到jenkins用户后，命令提示符的用户名不是jenkins而变成了 
-bash-4.1，原因是在安装jenkins时，jenkins只是创建了jenkins用户，并没有为其创建home目录。所以系统就不会在创建用户的时候，自动拷贝/etc/skel目录下的用户环境变量文件到用户家目录，也就导致这些文件不存在，出现-bash-4.1#的问题了 
以下命令是在切换到jenkins用户下执行的！（只是用户现在显示的是-bash-4.1），通过vim ~/.bash_profile，没有的话创建即可，然后再添加这句
export PS1='[\u@\h \W]\$'， PS1：命令行提示符环境变量，最后刷新，source ~/.bash_profile ）
3. 然后把生成的公钥(/var/lib/jenkins/.ssh)复制到需要集成的项目内，![项目下的Deploy keys](http://192.168.16.101:8844/test.png)
4.  ssh -v git@github.com尝试连接
![](http://i4.buimg.com/567571/d8d0c4dfa3067e37.png)
