# fixhub
This project has been renamed to Piplin

### Fixhub升级到Piplin的方法

假设：你的Fixhub安装在 /var/www/fixhub，数据库使用的是mysql，数据库名是：fixhub，访问地址是fixhub.example.com


1. 备份数据库(！一定要记住备份！)
```shell
$  mysqldump -uroot -p654321 fixhub > fixhub-20171211.sql 
```
2.创建一个临时数据库，名为piplin_tmp
```shell
create database piplin_tmp;
Query OK, 1 row affected (0.00 sec)
```

3.安装最新版piplin，请先忘记升级这回事，先当一次全新的安装
```shell
$ cd /var/www/
$ git clone https://github.com/Piplin/Piplin.git piplin
$ make
$ make install
```
> 注意：提示数据的时候请使用piplin_tmp

4. 创建一个名为piplin的数据库
```shell
create database piplin;
Query OK, 1 row affected (0.00 sec)
```
5.将fixhub-20171211.sql导入到piplin
```shell
mysql -uroot -p654321 piplin < fixhub-20171211.sql
```
5、修改/var/www/piplin/.env文件，将
**DB_DATABASE=piplin_tmp** 修改为 **DB_DATABASE=piplin**

6、让配置文件生效 
```shell
php artisan config:cache
```

7. 升级
```shell
make update
```

8. 修改supervisord的配置文件，全局替换fixhub为piplin
