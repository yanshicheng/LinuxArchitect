#Mysql安装搭建
```
yum -y remove mariadb* boost-*
yum install -y cmake make gcc gcc-c++ bison ncurses ncurses-devel bison-devel ncurses-devel perl-Data-Dumper net-tools
mkdir /tools
cd /tools/
wget https://cdn.mysql.com/archives/mysql-5.7/mysql-boost-5.7.20.tar.gz
tar xf mysql-boost-5.7.20.tar.gz 
cd mysql-5.7.20/
cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql-5.7.20 -DMYSQL_DATADIR=/usr/local/mysql-5.7.20/data -DDOWNLOAD_BOOST=1 -DWITH_BOOST=/tools/mysql-5.7.20/boost/boost_1_59_0 -DSYSCONFDIR=/etc -DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_MEMORY_STORAGE_ENGINE=1 -DWITH_ARCHIVE_STORAGE_ENGINE=1 -DWITH_FEDERATED_STORAGE_ENGINE=1 -DWITH_BLACKHOLE_STORAGE_ENGINE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DWITH_READLINE=1 -DMYSQL_UNIX_ADDR=/usr/local/mysql-5.7.20/mysql.sock -DMYSQL_TCP_PORT=3306 -DENABLED_LOCAL_INFILE=1 -DENABLE_DTRACE=0 -DEXTRA_CHARSETS=all -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DMYSQL_USER=mysql
echo $?
make -j 4
echo $?
make install
ln -s /usr/local/mysql-5.7.20/ /usr/local/mysql
useradd -M -s /sbin/nologin -r mysql 
mkdir -p /usr/local/mysql/data
chown -R mysql.mysql /usr/local/mysql/

vim /etc/my.cnf                            #修改my.cnf文件
[client]
port = 3306
socket = /usr/local/mysql/mysql.sock
default-character-set = utf8mb4
[mysqld]
port =  3306
socket = /usr/local/mysql/mysql.sock
basedir = /usr/local/mysql
datadir = /usr/local/mysql/data
pid-file = /var/log/mysql/run/mysql.pid
user = mysql
bind-address = 0.0.0.0
server-id = 1
init-connect = 'SET NAMES utf8mb4'
character-set-server = utf8mb4
#skip-name-resolve
#skip-networking
back_log = 300
max_connections = 1000
max_connect_errors = 6000
open_files_limit = 65535
table_open_cache = 128
max_allowed_packet = 4M
binlog_cache_size = 1M
max_heap_table_size = 8M
tmp_table_size = 16M
read_buffer_size = 2M
read_rnd_buffer_size = 8M
sort_buffer_size = 8M
join_buffer_size = 8M
key_buffer_size = 4M
thread_cache_size = 8
query_cache_type = 1
query_cache_size = 8M
query_cache_limit = 2M
ft_min_word_len = 4
log_bin = mysql-bin
binlog_format = mixed
expire_logs_days = 30
log_error = /var/log/mysql/mysql-error.log
slow_query_log = 1
long_query_time = 1
slow_query_log_file = /var/log/mysql/mysql-slow.log
performance_schema = 0
explicit_defaults_for_timestamp
lower_case_table_names = 1
skip-external-locking
default_storage_engine = InnoDB
expire_logs_days = 30
log_error = /var/log/mysql/mysql-error.log
slow_query_log = 1
long_query_time = 1
slow_query_log_file = /var/log/mysql/mysql-slow.log
performance_schema = 0
explicit_defaults_for_timestamp
lower_case_table_names = 1
skip-external-locking
default_storage_engine = InnoDB
#default-storage-engine = MyISAM
innodb_file_per_table = 1
innodb_open_files = 500
innodb_buffer_pool_size = 64M
innodb_write_io_threads = 4
innodb_read_io_threads = 4
innodb_thread_concurrency = 0
innodb_purge_threads = 1
innodb_flush_log_at_trx_commit = 2
innodb_log_buffer_size = 2M
innodb_log_file_size = 32M
innodb_log_files_in_group = 3
innodb_max_dirty_pages_pct = 90
innodb_lock_wait_timeout = 120
bulk_insert_buffer_size = 8M
myisam_sort_buffer_size = 8M
myisam_max_sort_file_size = 10G
myisam_repair_threads = 1
interactive_timeout = 28800
wait_timeout = 28800
[mysqldump]
quick
max_allowed_packet = 16M
[myisamchk]
key_buffer_size = 8M
sort_buffer_size = 8M
read_buffer = 4M
write_buffer = 4M

    
cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld 
chmod +x /etc/init.d/mysqld 
chkconfig --add mysqld
chkconfig mysqld on

vim /etc/init.d/mysqld                   #修改启动文件路径
    basedir=/usr/local/mysql
    datadir=/usr/local/mysql/data 
    
vim /etc/profile                        #设置环境变量
   export PATH=$PATH:/usr/local/mysql/bin
source /etc/profile 
/usr/local/mysql/bin/mysqld --initialize-insecure --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data
/etc/init.d/mysqld start    

mysql -uroot                            #设置root密码
    mysql> alter user 'root'@'localhost' identified by '123456';
    Query OK, 0 rows affected (0.00 sec)
    mysql> flush privileges;
    Query OK, 0 rows affected (0.00 sec)

mysql -uroot -p
```