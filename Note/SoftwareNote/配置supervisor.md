##### 5. 配置supervisor

1. 安装

```shell
# 在当前crab的微服务体系下，supervisor是必选项，负责监控微服务进程，并在进程crash时自动重启。
# 新机器安装supervisor的流程如下：
# 如果机器上没有jumbo，则首先安装jumbo，否则跳过
bash -c "$( curl http://jumbo.baidu.com/install_jumbo.sh )"; source ~/.bashrc

# 设置系统环境变量，比如生产环境需要设置 ENV=PRO，测试环境设置 ENV=FAT
vim ~/.bashrc

# 在合适的位置加上 export ENV=PRO
export ENV=PRO

# 加载环境变量
source ~/.bashrc

# 使用jumbo安装supervisor
jumbo install supervisor

# 生成配置文件
cd && mkdir -p etc/supervisor.d/conf && cd etc/supervisor.d && echo_supervisord_conf > supervisord.conf

# 修改包含的配置文件
vi supervisord.conf  # 然后G到最文件尾部

# 移除最后【两】行的注释，然后修改为：
[include]
files = conf/*.ini

cd ~/etc/supervisor.d/conf
vim processName.ini # 写项目的配置参数，有几个项目写几个配置文件

例子：
    [program:q-apm]
    command     = /home/batsdk/thirdparty/php-7.3.11/bin/php /home/batsdk/webroot/qapm-api/bin/server start
    directory   = /home/batsdk/webroot/qapm-api

    autostart   = true
    autorestart = true

# 启动supervisor.d
supervisord -c supervisord.conf

ps -ef|grep q-apm|grep -v grep|awk '{print $2}'|xargs kill -9
rm /home/batsdk/webroot/qapm-api/runtime/run/q-apm.pid
```

2. 操作

```shell
# 通过supervisorctl查看和操作当前运行的进程
# 启动supervisorctl时必须加上conf文件的地址，否则会找不到进程
cd ~/etc/supervisor.d
supervisorctl -c supervisord.conf

# 打印当前状态
status

# 重新加载配置，如果有新的ini文件，会启动里面配置的进程
update

# 当修改$PATH后，需要kill掉项目和supervisor进程，然后source ~/.bashrc，再重启supervisor
```

