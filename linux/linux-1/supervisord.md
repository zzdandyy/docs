# supervisord

配置后台服务/常驻进程的进程管家工具

## 安装

```text
# 安装 supervisord
apt-get install supervisor
```

## 实例

生成配置文件 `/etc/supervisord.conf`

```text
[program:app]
command=/usr/bin/gunicorn -w 1 wsgiapp:application
directory=/srv/www
user=www-data
```

supervisord: 启动 supervisor 服务

```text
supervisorctl start app
supervisorctl stop app
supervisorctl reload # 修改/添加配置文件需要执行这个
```

## 下载地址

[https://pypi.python.org/pypi/meld3](https://pypi.python.org/pypi/meld3)  
[https://pypi.python.org/pypi/supervisor](https://pypi.python.org/pypi/supervisor)

