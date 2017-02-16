#SNMP管理站的设置

Cacti是一套使用PHP开发、B/S的SNMP实现，其需要PHP、MySQL、Nginx/Apache/Lighttpd等WEB服务器运行环境，此外还需要RRD、cacti-spine等软件包，故应先安装这些所需的软件包，再部署常用的PHP运行环境。

注意以下两点：

1. PHP的`session.auto_start`要设置为`1`，同时`cgi.fix_pathinfo`也要设置为`1`。
2. 对`cacti`的上级目录，应执行`sudo chown -R www-data:www-data ../`，因为nginx是以用户`www-data`运行的。

此外，还需使用以下命令：

```
crontab -u www-data -e
```

为用户`www-data`加入下面的计划任务：

```
*/5 * * * * /usr/bin/php /var/www/cacti/poller.php
```

