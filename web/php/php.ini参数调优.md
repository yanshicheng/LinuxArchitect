#php.ini参数调优
```
#关闭危险函数
disable_functions = system,passthru,exec,shell_exec,popen,phpinfo
#关闭PHP版本信息
expose_php = Off

#避免暴露php调用mysql的错误信息
display_errors = Off
#在关闭display_errors后开启PHP错误日志（路径在php-fpm.conf中配置）
log_errors = On
#设置PHP的扩展库路径
extension_dir = "/usr/local/php/lib/php/extensions/no-debug-zts-20170718/"
#设置PHP的时区
date.timezone = PRC
 
#开启opcache
[opcache]
opcache.enable=1
#设置PHP脚本允许访问的目录（需要根据实际情况配置）
open_basedir = /etc/nginx/html;
脚本运行时间
max_execution_time = 30
#日志
error_reporting = E_WARNING & E_ERROR #日志级别
error_log = /var/logs/php_errors.log #错误日志路径
#错误信息控制
display_errors = Off
log_errors = On #开启错误日志
log_errors_max_len = 1024     ;设置每个日志项的最大长
调整php sesson 会话保持信息存放类型和位置
session.save_handler = memcache
session.save_path = "tcp://192.168.94.37:11211"

#配置php-fpm.conf

php-fpm.conf是 php-fpm 进程服务的配置文件：

#设置错误日志的路径
error_log = /var/log/php-fpm/error.log
#引入www.conf文件中的配置
include=/usr/local/php7/etc/php-fpm.d/*.conf

www.conf这是 php-fpm 进程服务的扩展配置文件：

#设置用户和用户组
user = nginx
group = nginx


#开启慢日志
slowlog = /var/log/php-fpm/$pool-slow.log
request_slowlog_timeout = 10s






```