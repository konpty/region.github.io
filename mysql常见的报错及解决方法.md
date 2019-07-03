# 1.Can't connect to MySQL server on 'localhost'(10061)?
翻译：不能连接到localhost 上的mysql?

分析：这说明“localhost”计算机是存在的，但在这台机器上却没提供MySQL服务。?需要启动这台机器上的MySQL服务,如果机子负载太高没空相应请求也会产生这个错误。?解决:既然没有启动那就去启动这台机子的mysql。如果启动不成功，多数是因为你的my.ini配置的有问题。重新配置其即可。?如果觉得mysql负载异常，可以到mysql/bin 的目录下执行mysqladmin-uroot -p123 processlist来查看mysql当前的进程。

# 2.Unknown MySQL ServerHost 'localhosadst' (11001)
翻译：未知的MySQL服务器localhosadst?

分析：服务器localhosasdst 不存在。或者根本无法连接?解决：仔细检查自己论坛下面的./config.inc.php找到$dbhost重新设置为正确的mysql 服务器地址。

# 3.Access denied for user:'roota@localhost' (Using password: YES)
翻译：用户roota 访问 localhost 被拒绝（没有允许通过）

分析：造成这个错误一般数据库用户名和密码相对mysql服务器不正确?解决：仔细检查自己论坛下面的 ./config.inc.php 找到$dbuser、$dbpw核实后重新设置保存即可。

# 4.Access denied for user:'red@localhost' to database 'newbbs'?
翻译：用户 red在localhost 服务器上没有权限操作数据库newbbs?

分析：这个提示和问题三是不同的。那个是在连接数据库的时候就被阻止了，而这个错误是在对数据库进行操作时引起的。比如在selectupdate等等。这个是因为该用户没有操作数据库相应的权力。比如select 这个操作在mysql.user.Select_priv里记录 Y 可以操作N 不可以操作。?解决：如果是自己的独立主机那么更新mysql.user 的相应用户记录，比如这里要更新的用户为red 。或者直接修改./config.inc.php 为其配置一个具有对数据库操作权限的用户?或者通过如下的命令来更新授权grantall privileges on dbname.* to 'user'@'localhost' identified by 'password'?提示：更新了mysql库中的记录一定要重启mysql服务器才能使更新生效?FLUSH PRIVILEGES;

# 5.No Database Selected
翻译：没有数据库被选择上?

分析：产生的原因有两种?config.inc.php 里面$dbname设置的不对。致使数据库根本不存在，所以在$db->select_db($dbname); 时返回了false?和上面问题四是一样的，数据库用户没有select权限，同样会导致这样的错误。当你发现config.inc.php的设置没有任何问题，但还是提示这个错误，那一定就是这种情况了。?解决：对症下药?打开config.inc.php找到$dbname核实重新配置并保存?同问题四的解决方法

# 6.Can't open file:'xxx_forums.MYI'. (errno: 145)
翻译：不能打开xxx_forums.MYI?

问题分析：?这种情况是不能打开cdb_forums.MYI 造成的，引起这种情况可能的原因有：?1、服务器非正常关机，数据库所在空间已满，或一些其它未知的原因，对数据库表造成了损坏。?2、类 unix 操作系统下直接将数据库文件拷贝移动会因为文件的属组问题而产生这个错误。?解决方法：?1、修复数据表?可以使用下面的两种方式修复数据表：（第一种方法仅适合独立主机用户）?1）使用 myisamchk ，MySQL自带了专门用户数据表检查和修复的工具 —— myisamchk 。更改当前目录到MySQL/bin 下面，一般情况下只有在这个下面才能运行 myisamchk 命令。常用的修复命令为：myisamchk-r 数据文件目录/数据表名.MYI；?2）通过 phpMyAdmin 修复，phpMyAdmin 带有修复数据表的功能，进入到某一个表中后，点击“操作”，在下方的“表维护”中点击“修复表”即可。?注意：以上两种修复方式在执行前一定要备份数据库。?2、修改文件的属组（仅适合独立主机用户）?1）复制数据库文件的过程中没有将数据库文件设置为MySQL 运行的帐号可读写（一般适用于 Linux和 FreeBSD 用户）。

# 7.Table'test.xxx_sessions' doesn't exist
翻译：xxxxx表不存在?

分析：在执行sql语句时没有找到表，比如：SELECT * FROMxxx_members WHERE uid='XX' 这里如果表xxx_members不存在于$dbname库里，那么就会提示这个错误。具体可分为以下三种情况来讨论：?安装插件或者hack时修改了程序文件，而忘记了对数据库作相应的升级。?后台使用了不完全备份，导入数据时没有导入到已经安装了相应版本的论坛的数据库中。?解决： 同样对症下药，不同的原因不同的处理方法。?仔细对照插件作者提供的安装说明，把遗漏的对数据库的操作补上，如果仍然不能解决问题，那么应该怀疑该插件的可用性了。去咨询一下插件作者，或者将其卸载。?不要张冠李戴，多大的脚就穿多大的鞋。总之使得程序文件和数据库配套即可.

# 8.Unknown column'column_name' in 'field list'
翻译：未知的字段名column_name?

分析：在执行sql语句是出现了指定表中没有的字段名称，就会出现这个错误。具体导致的原因可分为以下两种?安装插件或者hack时修改了程序文件，而忘记了对数据库作相应的升级。?程序文件和数据库不配套，比如d2.5的数据库配置给d4.1的程序来用肯定会出现这个错误。?解决： 导致的原因和问题八的1和 3是相同的，所以解决方法也一样。

# 9.You have an error in yourSQL syntax
翻译：有一个语法错误在你的sql中?

分析：论坛标准的程序是没有sql语法错误的。所以造成这个错误的原因一般就两类?安装插件或擅自修改程序。?不同的数据库版本数据库导出导入，比如MySQL4.1的数据在导出的语句包含了MySQL4.0没有的功能，像字符集的设定，这时如果将这些sql导入到MySQL4.0的时候就会产生sql语法错误。?解决：?仔细检查看到底是哪里的错误，将其修正，实在不行就用标准程序把出错的程序替换。?在数据库备份的时候要留意，如果不打算倒入到其他版本的mysql中则不用特殊考虑，反之要特殊的设定。使用DZ4.1的后台数据备份，可以按照提示去设定想要的格式。独立主机的也可以在到处的时候将其导出为mysql4.0的格式。?mysqldump -uroot -p--default-character-set=latin1 --set-charset=gbk --skip-opt databse >test.sql

# 10.Duplicate entry 'xxx'for key 1
翻译：插入 xxx使索引1重复?

分析：索引如果是primaryunique这两两种，那么数据表的数据对应的这个字段就必须保证其每条记录的唯一性。否则就会产生这个错误。?一般发生在对数据库写操作的时候，例如Discuz!4.1论坛程序要求所有会员的用户名username必须唯一，即username的索引是unique，这时如果强行往cdb_members表里插入一个已有的username的记录就会发上这个错误，或者将一条记录的username更新 为已有的一个username。?改变表结构的时候也有可能导致这个错误。例如 Discuz!4.0论坛的数据库中cdb_members.username的索引类型是index这个时候是允许有相同username的记录存在的，在升级到4.1的时候，因为要将username的索引由原来的index变 为unique。如果这时cdb_members里存在有相同的username的记录，那么就会引发这个错误。?导出数据据时有时会因为一些原因（作者目前还不清楚）导致同一条记录被重复导出，那么这个备份数据在导入的时候出现这个错误是在所难免的了。?修改了auto_increment的值，致使“下一个Autoindex”为一条已经存在的记录?解决： 两种思路，一是破坏掉唯一性的索引。二是把重复的数据记录干掉，只保留一条。很显然第一种思路是不可取的。那么按照二的思路我们得出以下几种解决方法，对应上面的i iiiii?略?按照错误提示里的信息到数据库中将重复的记录删除，仅保留一条即可。之后继续执行升级操作。?这种情况发生的概率很小，可以用文本编辑器打开备份文档，查找重复的信息。将其多余的拿掉，仅保留一条即可。?查询出表中auto_increment最大的一条记录，设置auto_incerment比其大一即可。?PS：repaire table "表名“，可以暂时解决问题。

# 11.Duplicate key name'xxx'
翻译：字段名xxx重复?

分析：要创建的索引已经存在了，就会引发这个错误，这个错误多发生在升级的时候。可能是已经升级过的，重复升级引起的错误。也有可能是之前用户擅自加的索引，刚好与升级文件中的所以相同了。?解决： 看看已经存在的索引和要添加的索引是否一样，一样的话可以跳过这条sql语句，如果不一样那么现删除已存在的所以，之后再执行。

# 12.Duplicate column name'xxx'
翻译：字段名xxx重复?

分析：添加的字段xxx已经存在，多发生在升级过程中，与问题十二的产生是一样的。?解决： 看一下已经存在的字段是否和将要添加的字段属性完全相同，如果相同则可以跳过不执行这句sql，如果不一样则删除掉这个字段。之后继续执行升级程序。

# 13. Table 'xxx' alreadyexists
翻译：数据表xxx已经存在?

分析：xxx表已经存在于库中，再次试图创建这个名字的表就会引发这个错误。同样多发生在论坛的升级中。类似于问题十二。?解决： 看看已经存在的表是否和将要创建的表完全一样，一样的话可以跳过不执行这个sql，否则请将存在的表先删除，之后继续执行升级文件。

# 14.Can't create database'xxx'. Database exists'
翻译：不能创建数据库xxx，数据库已经存在?

分析：一个mysql下面的数据库名称必须保证唯一性，否则就会有这个错误。?解决：把已经存在的数据库改名或者把将要创建的数据库改名，总之不让他们的名称冲突。

# 15.小结（针对问题11\12\13\14\15）
翻译：此类问题错误提示中都暗藏一个关键词duplicate（重复）?

分析：那么对于mysql数据库来说什么东西是不能重复的呢？?数据库 database?同一个数据库下数据表table?同一个数据表下字段 column?同一个数据表下索引 key?同一个数据表在索引唯一（UNIQUEPRIMARY）的情况下记录中的这些字段不可以重复

# 16.Unknown system variable'NAMES'
翻译：未知的系统变量NAMES?

分析：Mysql版本不支持字符集设定，此时强行设定字符集就会出现这个错误。?解决： 将sql语句中的SET NAMES ‘xxx' 语句去掉

# 17.Lost connection toMySQL server during query?
翻译：MySQL服务器失去连接在查询期间?

分析：远程连接数据库是有时会有这个问题。MySQL服务器在执行一条sql语句的时候失去了连接造成的。?解决： 一般不需要怎么去处理，如果频繁的出现那么考虑改善硬件环境。

# 18.User 'red' has exceededthe 'max_updates' resource (current value: 500)
翻译：msql用户red已经超过了'max_updates'（最大更新次数），'max_questions'（最大查询次数），'max_connections'（最大连接数），当前设定为500?

分析：在mysql数据库的下有一个库为mysql，它其中有一个表为user这里面的纪录每一条都对应为一个mysql用户的授权。其中字段max_questions max_updates max_connections分别记录着最大查询次数 最大更新数 最大连接数，当目前的任何一个参数大于任何一个设定的值就会产生这个错误。?解决： 独立主机用户可以直接修改授权表。修改完之后重启mysql或者跟新授权表，进入mysql提示符下执行?FLUSH PRIVILEGES;?记得后面要有分号';'?虚拟主机的用户如果总是出现这个问题可找空间商协商解决。
 
# 19.Too many connections(1040)链接过多?
翻译：达到最大连接数?

问题分析：?连接数超过了mysql设置的值，与max_connections和wait_timeout 都有关系。wait_timeout的值越大，连接的空闲等待就越长，这样就会造成当前连接数越大?解决方法：?1.虚拟主机用户请联系空间商优化MySQL 服务器的配置；?2.独立主机用户请联系服务器管理员优化MySQL 服务器的配置，可参考：?修改MySQL 配置文件 my.ini 或者 my.cnf 中的参数：?max_connections= 1000?wait_timeout = 10?修改后重启 MySQL ，如果经常性的报此错误，请做一下服务器的整体优化。
 
# 20.There is no such grantdefined for user '%s' on host '%s'?
翻译：错误编号：1141?

问题分析：?MySQL当前用户无权访问数据库。?解决方法：?1、虚拟主机用户请联系空间商，确认给你提供的帐号是否有授权数据库的权限。?2、独立主机用户请联系服务器管理员，确认给您提供的数据库帐号是否有管理此数据库的权限。

# 21.Error on rename of '%s'to '%s' (errno: %d)?error.:1025?
翻译：请检查一下您的程序是否有修改数据库表名的语句。?

分析：1.请检查您的程序中哪些地方需要修改数据库表名；?2.如果您的实际应用确实需要修改到数据库表名的话，请联系空间商或者服务器管理员给您开放修改库名的权限和服务器本身是否正常。

# 22.Error reading file '%s'(errno: %d)?error.:1023?
问题分析：?数据库文件不能被读取。?

解决方法：?1.虚拟主机用户请联系空间商查看数据库是否完好。?2.独立主机用户请联系服务器管理员检查一下MySQL 本身是否正常， MySQL 是否可以读取文件，Linux 用户可以检查一下MySQL 的数据库文件的属主是否正确以及本身的文件是否损坏。

# 23.Host '*****' is blockedbecause of many connection errors; unblock with 'mysqladmin flush-hosts'?error.:1129
问题分析：?数据库出现异常，请重启数据库。?

解决方法：?1. 由于存在很多连接错误，主机'****'被屏蔽，虚拟主机用户请联系空间商处理，独立主机用户请联系服务器管理员，在MySQL 的命令控制台下执行'mysqladmin flush-hosts'解除屏蔽即可，或者重启MySQL 数据库‘

# 24.dropping database (can'tdelete '%s', errno: %d)?error.:1009?
问题分析：?不能删除数据库文件，导致删除数据库失败。?

解决方法：?1.检查您使用的数据库管理帐号是否有权限删除数据。?2.检查数据库是否存在。

# 25.Got error 28 from tablehandler?error.:1030?
问题分析：?数据库所在磁盘空间已满。?

解决方法：?1.虚拟主机用户请联系空间商增加MySQL 所在的磁盘空间或者清理一些无用文件；?2.独立主机用户请联系服务器管理员增加MySQL 所在的磁盘空间或者清理一些无用文件

# 26.Can't create a newthread; if you are not out of available memory, you can consult the manual fora possible OS-dependent bug。?error.:11/35?
问题分析：?数据库服务器问题，数据库操作无法创建新线程。一般是两个原因：?1.服务器系统内存溢出。?2.环境软件损坏或系统损坏。?

解决方法：?1.虚拟主机用户请联系下空间商数据库服务器的内存和系统是否正常。?2.独立主机用户请联系服务器管理员检查服务器的内存和系统是否正常，如果服务器内存紧张，请检查一下哪些进程消耗了服务器的内存，同时考虑是否增加服务器的内存来提高整个的负载能力。

# 27.Error: Client does notsupport authentication protocol requested by server; consider upgrading MySQLclient?error.:1251?
问题分析：?如果你升级MySQL 到 4.1 以上版本后遇到以上问题,请先确定你的MySQL Client 是 4.1 或者更高版本(Windows下有问题你就直接跳到下面看解决方法了，因为 MySQL 在Windows 是 client 和 server 一起装上了的)。?

解决方法：?1.Windows 平台?主要是改变连接MySQL 的帐户的加密方式，MySQL 4.1/5.0 是通过PASSWORD 这种方式加密的。可以通过以下两种方法得到解决：?1) mysql->SET PASSWORD FOR'some_user'@'some_host'=OLD_PASSWORD('new_password');?2) mysql->UPDATE mysql.user SETPassword=OLD_PASSWORD('new_password') WHERE Host='some_host' ANDUser='some_user';?2.Linux/Unix 平台?Linux平台下首先确定是否安装过 MySQL 的客户端，这个用 rpm安装很简单，Linux 代码为：?rpm -ivh MySQL-client-4.1.15-0.i386.rpm?然后在编译 php 的时候要加上：?--with-mysql=/your/path/to/mysql?一般情况下都可以解决。如果还出现这种错误，可以按照下面的方法来做：?mysql->SET PASSWORD FOR'some_user'@'some_host'=OLD_PASSWORD('new_password');?mysql->UPDATE mysql.user SET Password=OLD_PASSWORD('new_password')WHERE Host='some_host' AND User='some_user';

# 28.Error: Can't connect tolocal MySQL server through socket '/var/lib/mysql/mysql.sock'?error.:2002?
问题分析：?出现这个错误一般情况下是因为下面两个原因：?1.MySQL 服务器没有开启。?2.MySQL 服务器开启了，但不能找到 socket 文件。?
    
解决方法：?1.虚拟主机用户，请联系空间商确认数据库是否正常启动。?2.独立主机用户，请检查一下 MySQL 服务是否已经开启，没有开启，请启动MySQL 服务；如果已经开启，并且是 Linux 系统，请检查一下MySQL 的 socket 的路径，然后打开 config.inc.php 找到?$dbhost = 'localhost'; 在hostname 后面加冒号‘:'和 MySQL 的socket 的路径。?比如MySQL 服务器为 localhost?MySQL的 socket 的路径为 /tmp/mysql.sock?那么就改成如下：?$dbhost= 'localhost:/temp/mysql.sock';

# 29.Can't connect to MySQLserver on 'localhost'?error.:2003?
问题分析：?MySQL服务没有启动，一般是在异常的情况下 MySQL 无法启动导致的，比如无可用的磁盘空间，my.ini里 MySQL 的 basedir 路径设置错误等。?
    
解决方法：?1.检查磁盘空间是否还有剩余可用空间，尽量保持有足够的磁盘空间可用。?2.检查 my.ini 里的basedir 等参数设置是否正确，然后重新启动下 MySQL 服务。

# 30.Lost connection to MySQLserver during query?error.:2013
问题分析：?数据库查询过程中丢失了与MySQL 服务器的连接。?

解决方法：?1.请确认您的程序中是否有效率很低的程序，比如某些插件，可以卸载掉插件，检查一下服务器是否正常；?2.服务器本身资源紧张，虚拟主机用户请联系空间商确认，独立主机用户请联系服务器管理员，检查一下服务器是否正常。

# 31.Got a packet bigger than\'max_allowed_packet\' bytes?
问题分析：错误编号：1153?问题分析：调整了 Mantis 的上传附件的大小却没有调整 MySQL 的配置文件。?

解决办法：?1、独立主机用户请按照以下方法调整：?查找 MySQL 的配置文件（my.cnf 或者my.ini）?在[mysqld] 部分添加一句（如果存在，调整其值就可以）：?max_allowed_packet=10M?重启 MySQL 服务就可以了。这里设置的是 10MB。
