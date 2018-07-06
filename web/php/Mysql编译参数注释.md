#Mysql编译参数注释
```
cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql \               #MySQL的安装目录
-DMYSQL_DATADIR=/usr/local/mysql-5.7.20/data \                #MySQL的数据目录
-DDOWNLOAD_BOOST=1 \                                          #下载boost
-DWITH_BOOST=/tools/mysql-5.7.20/boost/boost_1_59_0 \         #指定boost的路径
-DSYSCONFDIR=/etc \                                           #my.cnf路径
-DWITH_MYISAM_STORAGE_ENGINE=1 \                              #启用MySQL的myisam引擎
-DWITH_INNOBASE_STORAGE_ENGINE=1 \                            #启用MySQL的innobase引擎
-DWITH_MEMORY_STORAGE_ENGINE=1 \                              #启用MySQL的memory引擎
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
-DWITH_FEDERATED_STORAGE_ENGINE=1 \
-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \
-DWITH_PARTITION_STORAGE_ENGINE=1 \                           #安装支持数据库分区
-DWITH_READLINE=1 \                                           #启用readline库支持（提供可编辑的命令行）
-DMYSQL_UNIX_ADDR=/usr/local/mysql-5.7.20/mysql.sock \        #连接数据库socket路径
-DMYSQL_TCP_PORT=3306 \                                       #MySQL端口
-DENABLED_LOCAL_INFILE=1 \                                    #允许从本地导入数据
-DENABLE_DTRACE=0 \
-DEXTRA_CHARSETS=all \                                        #安装所有的字符集
-DDEFAULT_CHARSET=utf8 \                                      #设置默认字符集为utf-8
-DDEFAULT_COLLATION=utf8_general_ci \                         #设定默认排序规则（utf8_general_ci快速/utf8_unicode_ci准确）
-DMYSQL_USER=mysql \
```