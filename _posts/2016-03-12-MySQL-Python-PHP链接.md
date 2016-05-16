---
---
1. 复习Python爬虫
	1. 复习静觅的python[Python爬虫学习系列教程](http://cuiqingcai.com/1052.html)
	2. 结合另一篇对python爬虫经验总结的文章:[用python爬虫抓站的一些技巧总结 zz](http://www.pythonclub.org/python-network-application/observer-spider)
	3. Python解析json
	
2. 复习MySQL 
	1. 学习Mysql语法
	2. Mysql远程登录
	3. 连接: [Mysql远程登录](http://www.cnblogs.com/good_hans/archive/2010/03/29/1700046.html) [Mysql建表](http://www.runoob.com/mysql/mysql-create-tables.html)
	4. 修改表结构: [MySQL更改表结构的添加与删除](http://database.51cto.com/art/201005/201148.htm)
	5. [数据库的导入导出](https://dev.mysql.com/doc/refman/5.5/en/copying-databases.html)
			
		
			shell> mkdir DUMPDIR
			shell> mysqldump --tab=DUMPDIR db_name
			shell> mysqladmin create db_name           # create database
			shell> cat DUMPDIR/*.sql | mysql db_name   # create tables in database
			shell> mysqlimport db_name DUMPDIR/*.txt   # load data into tables
	
	 6. php7 mysql_connect 不能用 的解决办法: [PHP7连接MySql错误”Fatal error: ](http://www.happy3w.com/2016/01/11/php7%E8%BF%9E%E6%8E%A5mysql%E9%94%99%E8%AF%AFfatal-error-uncaught-error-call-to-undefined-function-mysql_connect/)
	 7. [Mysql中基本数据类型](http://www.cnblogs.com/zbseoag/archive/2013/03/19/2970004.html)
	 8. 远程登录Mysql问题
	 	1. 没有开启其他IP的监听 [设置Ubuntu上的MySQL可以远程访问](http://blog.csdn.net/mydeman/article/details/3847695)
	 	2. 没有grant用户权限

	 其他链接：[Linux下安装与使用MySQL详细介绍](http://www.jb51.net/article/40975.htm)
	 9. [MYSQL中无重复插入更新几种方法](http://www.360doc.com/content/14/0621/19/9200790_388653458.shtml)

3. Python Mysql交互
	1. 用了[PurePython](https://github.com/PyMySQL/PyMySQL)

4. 安装Nginx和PHP
	[ubuntu 14.04安装nginx+php+mysql](http://www.cnblogs.com/helinfeng/p/4219051.html)
	[Nginx从入门到精通](http://tengine.taobao.org/book/)


5. Mysql安装
	[Uninstall Mysql on MacOSX](http://community.jaspersoft.com/wiki/uninstall-mysql-mac-os-x)

