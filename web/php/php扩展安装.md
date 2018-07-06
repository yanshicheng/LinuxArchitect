#PHP优化
###安装zendopcache
```
PHP编译时增加--enable-opcache可自动添加扩展

cd /tools/php-7.2.6/ext/opcache  
/usr/local/php/bin/phpize
./configure --with-php-config=/usr/local/php/bin/php-config --enable=opcache
```

###安装缓存软件memcached和redis安装
```
#memcached
下载地址：http://pecl.php.net/package/memcached
#安装libmemcached库支持
wget https://launchpad.net/libmemcached/1.0/1.0.18/+download/libmemcached-1.0.18.tar.gz
tar xf libmemcached-1.0.18.tar.gz
cd libmemcached-1.0.18/
./configure --prefix=/usr/local/libmemcached
echo $?
make && make install



wget http://pecl.php.net/get/memcached-3.0.2.tgz
tar xf memcached-3.0.2.tgz 
cd memcached-3.0.2/
/usr/local/php/bin/phpize
./configure --with-php-config=/usr/local/php/bin/php-config --with-libmemcached-dir=/usr/local/libmemcached --disable-memcached-sasl
echo $?
make 
make install
```
```
#安装redis
下载地址：http://pecl.php.net/package/redis
wget http://101.110.118.57/pecl.php.net/get/redis-4.0.2.tgz
tar xf redis-4.0.2.tgz 
cd redis-4.0.2/
/usr/local/php/bin/phpize 
./configure --with-php-config=/usr/local/php/bin/php-config
echo $?
make
make install
```
###安装DPO_MYSQL扩展
```
编译PHP指定参数：
--enable-pdo 
--with-pdo-mysql
--enable-mysqlnd 
--with-mysqli
--with-pdo-mysql=mysqlnd
下载地址:http://pecl.php.net/package/PDO_MYSQL
php源码包路径：cd /tools/php-7.2.6/ext/pdo_mysql/
/usr/local/php/bin/phpize 
./configure --with-php-config=/usr/local/php/bin/php-config 
make
make install
```
###安装ImageMagick图像软件
```
下载地址：http://download.chinaunix.net/download/0001000/95.shtml
tar xf ImageMagick-6.7.9-9.tar.xz 
cd ImageMagick-6.7.9-9/
./configure
echo $?
make && make install
```
###安装imagick php扩展插件

```
#下载地址：http://pecl.php.net/package/imagick
#需安装ImageMagick图像软件，不然报错
wget http://101.96.10.63/pecl.php.net/get/imagick-3.4.3.tgz
tar xf imagick-3.4.3.tgz 
cd imagick-3.4.3/
/usr/local/php/bin/phpize 
./configure --with-php-config=/usr/local/php/bin/php-config 
echo $?
make
make install
```

###PHP.ini设置
```
#文件末尾增加以下内容
vim /usr/local/php/etc/php.ini 
extension = memcached.so
extension = imagick.so
extension = redis.so

zend_extension= /usr/local/php/lib/php/extensions/no-debug-zts-20170718/opcache.so


```






























