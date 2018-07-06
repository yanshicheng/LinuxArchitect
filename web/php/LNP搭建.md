#Lnp搭建
###搭建nginx
```
yum -y install gcc gcc-c++ autoconf automake zlib zlib-devel openssl openssl-devel pcre*
useradd -M -s /sbin/nologin nginx
mkdir /tools
cd /tools
wget http://nginx.org/download/nginx-1.14.0.tar.gz
tar zxf nginx-1.14.0.tar.gz
cd nginx-1.14.0/
./configure --user=nginx --group=nginx   --prefix=/usr/local/nginx-1.14.3/ --with-http_ssl_module --with-http_stub_status_module --error-log-path=/var/log/nginx/nginx_error.log --pid-path=/var/log/nginx/run/nginx.pid --with-http_realip_module --with-http_sub_module --with-mail_ssl_module --with-http_dav_module --with-http_addition_module --with-http_sub_module  --with-http_flv_module  --with-http_mp4_module   
echo $?
make
echo $?
make install

#添加环境变量 ln -s 到usr/bin也可
ln -s /usr/local/nginx-1.14.3/ /usr/local/nginx
/usr/local/nginx/sbin/nginx -t                    #检查nginx语法是否正确
nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful

/usr/local/nginx/sbin/nginx                       #安装好的启动路径
vim /etc/profile                   　　           #添加环境变量
export PATH=$PATH:/usr/local/nginx/sbin
source /etc/profile
nginx

netstat -antup|grep nginx
tcp    0    0 0.0.0.0:80        0.0.0.0:*       LISTEN      7417/nginx: master 
```

###搭建php
```
yum -y install php-mcrypt libmcrypt libmcrypt-devel  autoconf  freetype gd libmcrypt libpng libpng-devel libjpeg libxml2 libxml2-devel zlib curl curl-devel re2c net-snmp-devel libjpeg-devel php-ldap openldap-devel openldap-servers openldap-clients freetype-devel gmp-devel
cd /tools
wget http://cn2.php.net/distributions/php-7.2.6.tar.gz
tar xf php-7.2.6.tar.gz 
cd php-7.2.6/
./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --with-config-file-scan-dir=/usr/local/php/etc/php.d --with-mysqli --with-pdo-mysql --with-iconv-dir --with-freetype-dir --with-jpeg-dir --with-png-dir --with-curl --with-gd --with-gmp --with-zlib --with-xmlrpc --with-openssl --without-pear --with-snmp --with-gettext --with-mhash --with-libxml-dir=/usr --with-ldap --with-ldap-sasl --with-fpm-user=nginx --with-fpm-group=nginx --enable-xml --enable-fpm  --enable-ftp --enable-bcmath --enable-soap --enable-shmop --enable-sysvsem --enable-sockets --enable-inline-optimization --enable-maintainer-zts --enable-mbregex --enable-mbstring --enable-pcntl --enable-zip --disable-fileinfo --disable-rpath --enable-libxml --enable-opcache --disable-fileinfo --enable-mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd
echo $?
 make -j 8
echo $?
make install
/usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.conf
/tools/php-7.2.6/php.ini-production /usr/local/php/etc/php.ini
/tools/php-7.2.6/sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
chmod +x /etc/init.d/php-fpm
chkconfig --add php-fpm
chkconfig php-fpm on
/etc/init.d/php-fpm start
netstat -lnutp
vim /usr/local/nginx/conf/nginx.conf
    location / {                  
        root   html;
        index  index.php index.html index.htm;                                               #添加index.php
    }
    location ~ \.php$ {
        root           html;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  /data/html$fastcgi_script_name;　　　　#修改路径
        include        fastcgi_params;
        }


/usr/local/nginx/sbin/nginx -t   
netstat -lnutp
/usr/local/nginx/sbin/nginx -s reload
```

###PHP报错处理
```
configure: error: Cannot find ldap libraries in /usr/lib. 　　　　　　  #解决方法
[root@centos7_4 php-7.2.6]# cp -frp /usr/lib64/libldap* /usr/lib/  　　#在重新配置


###make 报错如下
/usr/bin/ld: ext/ldap/.libs/ldap.o: undefined reference to symbol 'ber_strdup'

/usr/lib64/liblber-2.4.so.2: error adding symbols: DSO missing from command line

collect2: error: ld returned 1 exit status

make: *** [sapi/cli/php] Error 1
            解决办法：在以EXTRA_LIBS开头的一行结尾添加‘-llber’
```