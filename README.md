<<<<<<< HEAD
﻿---
date:
status: public
title: '                               HiKari使用说明'
---

# 程序初始化
  使用配置类
```
      HikariConfig hikariConfig = new HikariConfig();
=======
﻿                # HiKari使用说明
   程序初始化
   -----------------------------------------------------------------------------------------------------------------
  使用配置类
 ···
   HikariConfig hikariConfig = new HikariConfig();
>>>>>>> 58415d3cef2665b03d8a8ee8252ca6af7ebc0be0
                hikariConfig.DBType = "PostgreSQL";
                hikariConfig.ConnectString = "Server = 127.0.0.1; Port = 5432; User Id = postgres; Password = 1234; Database =      postgres;Pooling=true; ";
            hikariConfig.DriverDir = "DBDrivers";
            hikariConfig.DriverDLL = "XXXX.dll";
            hikariConfig.DBTypeXml = "DBType.xml";
<<<<<<< HEAD
           HikariDataSource hikariDataSource = new HikariDataSource(hikariConfig);
```
直接使用：
```
=======

            HikariDataSource hikariDataSource = new HikariDataSource(hikariConfig);
  
···
直接使用
···
>>>>>>> 58415d3cef2665b03d8a8ee8252ca6af7ebc0be0
  HikariDataSource hikariDataSource = new HikariDataSource();
                   hikariDataSource.DBType = "PostgreSQL";
            hikariDataSource.ConnectString = "Server = 127.0.0.1; Port = 5432; User Id = postgres; Password = 1234; Database = postgres;Pooling=true; ";
            hikariDataSource.DriverDir = "DBDrivers";
            hikariDataSource.DriverDLL = "XXXX.dll";
            hikariDataSource.DBTypeXml = "DBType.xml";
<<<<<<< HEAD
```
配置文件（推荐方式):
```
HikariConfig hikariConfig = new HikariConfig();
hikariConfig.LoadConfig("Hikari.txt");
HikariDataSource hikariDataSource = new HikariDataSource(hikariConfig);
```
使用连接
=======
···

配置文件（推荐方式）
···
HikariConfig hikariConfig = new HikariConfig();
hikariConfig.LoadConfig("Hikari.txt");
HikariDataSource hikariDataSource = new HikariDataSource(hikariConfig);
···
使用l连接
>>>>>>> 58415d3cef2665b03d8a8ee8252ca6af7ebc0be0
hikariDataSource.GetConnection();
# 配置文件说明
1.连接字符串必须要
2.驱动目录DriverDir项不配置则使用默认drivers目录
3.关于驱动dll有2种：
  （1）直接在配置文件中配置DriverDLL项，这个时候不需要DBType配置项
  （2）DBType项和DBType.xml结合使用，这时不需要单独在每个连接配置中配置DriverDLL。
      这个时候连接池查找dll的方式：
      1>DBType配置是默认的4类，则无需DBType.xml来查找dll文件名称。
      2>DBType配置不是默认的4类，则需要BType.xml中的配置来获取DLL文件名称。因为程序不知道dll.
       其实这2种就是分散与汇总的区别。有的习惯每个配置文件配置dll,方便监测；有的习惯写在一起，方便管理。看自己情况。
       DBType程序中写死了4种，见附录。
<<<<<<< HEAD
 DBType.xml我称为全局配置文件，读取时的配置路径和名称可以设置HikariConfig或者HikariDataSource的DBTypeXml属性进行修改；默认就是DBType.xml
# 升级内容
=======
 DBType.xml我称为全局配置文件，读取时的配置路径和名称可以设置HikariConfig或者HikariDataSource的DBTypeXml属性进行修改；默认就是D     BType.xml
 ------------------------------------------------------------------------------------------------------------------------------
 升级内容：
>>>>>>> 58415d3cef2665b03d8a8ee8252ca6af7ebc0be0
 2018-12-13
 1.优化加载，对全局配置文件DBType.xml和默认项进行检查，已经加载过的就不加载了。
 2.新增连接池管理。可以同时启用多个连接池。连接池使用和以前一样。但是可以同时使用多个。根据配置文件名称来获取不同的连接池连接。只是这个时候只能能够使用配置文件，而不能在使用配置类了。
  连接池管理类：ManagerPool
  ManagerPool获取连接时传入一个名称（配置文件名称）来获取连接，这时不同的配置名称就意味着不同的连接池。
  另外该类提供了驱动目录和全局配置设置。方便把多个连接池使用到的所以DLL放在一起方便管理。连接池管理类不会覆盖已经在各个连接池配置文件配置的  DriverDir目录，但是DBTypeXml属性会覆盖，只能有个，并且在连接池管理类中设置。
  同时提供了使用的连接管理，该管理根据获取连接的线程保存连接对象。如果设置了线程关闭，同一个线程获取新连接，则以前的连接将会关闭。你也可以在获取的连接的线程中再次获取该对象。
  --------------------------------------------------------------------------------------------------------

连接池管理类：连接池管理中的配置文件名称进行了修改。除非是默认枚举名称Hikari没有变，如果你使用了名称，则会自动添加_Hikari。文件类型变成了cfg。例如：使用MySql,那么配置文件应该是MySql_Hikari.cfg.添加后缀的主要原因是其它一些数据库配置组件也使用配置文件，能够全部放在一个目录下。毕竟使用连接池管理，配置文件需要一定规律查找。如果不愿意只需改变管理类中的文件名称查找，很简单。
   -------------------------------------------------------------------------------------------------------


# 源码简说
## 基本内容
HikariDataSource 对外提供连接
HikariConfig 对外配置
HikariPool 管理操作集合，连接来源
PoolBase 操作驱动连接，是HikariPool父类
##新增扩展内容
2018-12-13
ManagerPool 线程池管理类，根据名称提供多线程连接

# 附录
|数据库	|Dll名称|说明|
|:-------:|:------:|:-----:|
|Oracle	|Oracle.ManagedDataAccess	|以前不是这个dll|
|MySql	|MySql.Data| 
|SqlServer|System| |
|PostgreSQL|Npgsql||
