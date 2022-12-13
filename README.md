# 数据库实验平台OpenEuler的安装、OpenGauss数据库的安装即配置实验

## 实验目的

* 掌握 VirtualBox 软件的安装方法以及在 virtualBox 上创建 linux 虚拟机并安装 openEuler 系统的方法

* 掌握基本的 openEuler 系统的使用和配置方法

* 掌握 PuTTY 的基本使用方法

* 掌握 XML 配置文件的创建和书写方法

* 学会下载解压 OpenGauss 数据库

* 经行初始化等简单的配置

## 实验环境

虚拟机软件使用的是 VirtualBox6.1.14 版本，虚拟机上操作系统是 openEuler-20.03-LTS-x86_64。电脑建议使用 windows10 系统，内存建议 8GB 以上。

## 实验内容

* 安装 VirtualBox

* 安装 OpenEuler

* 安装 PuTTY

* 安装 OpenGauss

## 实验步骤（包括实验过程、实验结果、查询语句等等）

第一步，进入VirtualBox官网下载并安装VirtualBox

第二步，进入华为开源镜像站下载openEuler-20.03-LTS镜像文件

第三步，依据实验指导书中设置新建虚拟电脑

第四步，依据实验指导书中步骤安装openEuler

第五步，检查完网络后，安装PuTTY并连接虚拟机，之后可在PuTTY上按照实验指导书对openEuler进行相关配置

第六步，使用wget下载数据库安装包到安装包目录，并解压安装包

第七步，创建XML文件，用于数据安装，并将该文件作为参数，执行初始化脚本

最后，依据实验指导书初始化数据库完成数据库的安装

## 实验总结（包括遇到的问题、解决方案等）

该实验主要是完成openGauss数据的安装，为后续的实验做准备。实验指导书上有详细的步骤，只要细心的按照步骤一步一步来，就能较为容易的实现数据库的安装。

由于直接在虚拟机上无法复制粘贴命令，因此使用PuTTY工具远程连接虚拟机，直接将命令复粘贴到PuTTY上执行，能够减少输入错误的概率。唯一需要注意的一点是XML文件中的配置项需要按照自己虚拟机的数据库名和网络IP设置。

为了完整体验一次数据库的安装，因此本次试验并未使用数据库安装脚本。若要提高部署数据库的效率，可以将安装步骤写入shell脚本，实现一键式配置、下载、安装。

# OpenGauss数据库建表及数据导入实验

## 实验目的

* 了解 openGauss 的基本数据类型

* 学会根据现实场景中表中的数据，在 openGauss 创建对应的关系表

* 学会使用 gsql 元命令批量导入数据

## 实验环境

实验环境为 virtualBOX 虚拟机 openEuler20.03 系统上的 openGauss1.1.0 数据库，实验数据采用 TPC-H 数据库的八张表，实验过程会用到 WinSCP，Putty 软件。

## 实验内容

* 创建关系表

* 数据导入

## 实验步骤

输入`gsql -d postgres -p 26000 -r`以编辑模式连接数据库。然后使用 CREATE TABLE 命令创建各所需表

创建订单表 ORDERS

    CREATE TABLE ORDERS ( O_ORDERKEY INTEGER NOT NULL,
    O_CUSTKEY INTEGER NOT NULL,
    O_ORDERSTATUS CHAR(1) NOT NULL,
    O_TOTALPRICE DECIMAL(15,2) NOT NULL,
    O_ORDERDATE DATE NOT NULL,
    O_ORDERPRIORITY CHAR(15) NOT NULL,
    O_CLERK CHAR(15) NOT NULL,
    O_SHIPPRIORITY INTEGER NOT NULL,
    O_COMMENT VARCHAR(79) NOT NULL);

创建区域表 REGION

    CREATE TABLE REGION ( R_REGIONKEY INTEGER NOT NULL,
    R_NAME CHAR(25) NOT NULL,
    R_COMMENT VARCHAR(152));

创建国家表 NATION

    CREATE TABLE NATION ( N_NATIONKEY INTEGER NOT NULL,
    N_NAME CHAR(25) NOT NULL,
    N_REGIONKEY INTEGER NOT NULL,
    N_COMMENT VARCHAR(152));

创建供应商表 SUPPLIER

    CREATE TABLE SUPPLIER ( S_SUPPKEY INTEGER NOT NULL,
    S_NAME CHAR(25) NOT NULL,
    S_ADDRESS VARCHAR(40) NOT NULL,
    S_NATIONKEY INTEGER NOT NULL,
    S_PHONE CHAR(15) NOT NULL,
    S_ACCTBAL DECIMAL(15,2) NOT NULL,
    S_COMMENT VARCHAR(101) NOT NULL);

创建零部件表 PART

    CREATE TABLE PART ( P_PARTKEY INTEGER NOT NULL,
    P_NAME VARCHAR(55) NOT NULL,
    P_MFGR CHAR(25) NOT NULL,
    P_BRAND CHAR(10) NOT NULL,
    P_TYPE VARCHAR(25) NOT NULL,
    P_SIZE INTEGER NOT NULL,
    P_CONTAINER CHAR(10) NOT NULL,
    P_RETAILPRICE DECIMAL(15,2) NOT NULL,
    P_COMMENT VARCHAR(23) NOT NULL );

创建零部件供应商表 PARTSUPP

    CREATE TABLE PARTSUPP ( PS_PARTKEY INTEGER NOT NULL,
    PS_SUPPKEY INTEGER NOT NULL,
    PS_AVAILQTY INTEGER NOT NULL,
    PS_SUPPLYCOST DECIMAL(15,2) NOT NULL,
    PS_COMMENT VARCHAR(199) NOT NULL );

创建客户表 CUSTOMER

    CREATE TABLE CUSTOMER ( C_CUSTKEY INTEGER NOT NULL,
    C_NAME VARCHAR(25) NOT NULL,
    C_ADDRESS VARCHAR(40) NOT NULL,
    C_NATIONKEY INTEGER NOT NULL,
    C_PHONE CHAR(15) NOT NULL,
    C_ACCTBAL DECIMAL(15,2) NOT NULL,
    postgres(# C_MKTSEGMENT CHAR(10) NOT NULL,
    C_COMMENT VARCHAR(117) NOT NULL);

创建订单明细表 LINEITEM

    CREATE TABLE LINEITEM ( L_ORDERKEY INTEGER NOT NULL,
    L_PARTKEY INTEGER NOT NULL,
    L_SUPPKEY INTEGER NOT NULL,
    L_LINENUMBER INTEGER NOT NULL,
    L_QUANTITY DECIMAL(15,2) NOT NULL,
    L_EXTENDEDPRICE DECIMAL(15,2) NOT NULL,
    L_DISCOUNT DECIMAL(15,2) NOT NULL,
    L_TAX DECIMAL(15,2) NOT NULL,
    L_RETURNFLAG CHAR(1) NOT NULL,
    L_LINESTATUS CHAR(1) NOT NULL,
    L_SHIPDATE DATE NOT NULL,
    L_COMMITDATE DATE NOT NULL,
    L_RECEIPTDATE DATE NOT NULL,
    L_SHIPINSTRUCT CHAR(25) NOT NULL,
    L_SHIPMODE CHAR(10) NOT NULL,
    L_COMMENT VARCHAR(44) NOT NULL);

以上操作完成了8张表的创建，输入`\d`查看所有已创建表。

![创建表格](img/创建表格.png)

接着使用WinSCP连接上虚拟机，以omm用户身份登录，将tpc-h的数据上传到`home/omm/tpc-h/data`目录下

![数据上传](img/数据上传.png)

使用 \copy 命令将上传的数据导入之前创建的表


    postgres=# copy region FROM '/home/omm/tpc-h/data/region.txt' with delimiter as '|';
    COPY 5
    postgres=# copy nation FROM '/home/omm/tpc-h/data/nation.txt' with delimiter as '|';
    COPY 25
    postgres=# copy part FROM '/home/omm/tpc-h/data/part.txt' with delimiter as '|';
    COPY 40000
    postgres=# copy supplier FROM '/home/omm/tpc-h/data/supplier.txt' with delimiter as '|';
    COPY 2000
    postgres=# copy customer FROM '/home/omm/tpc-h/data/customer.txt' with delimiter as '|';
    COPY 30000
    postgres=# copy lineitem FROM '/home/omm/tpc-h/data/lineitem.txt' with delimiter as '|';
    COPY 1199969
    postgres=# copy partsupp FROM '/home/omm/tpc-h/data/partsupp.txt' with delimiter as '|';
    COPY 160000
    postgres=# copy orders FROM '/home/omm/tpc-h/data/orders.txt' with delimiter as '|';
    COPY 300000

使用`select * from tablename`语句一一检查每个表数据是否导入正确，完成数据导入。

最后为各表添加主键约束和外键约束

    postgres=# ALTER TABLE REGION ADD PRIMARY KEY (R_REGIONKEY);
    NOTICE:  ALTER TABLE / ADD PRIMARY KEY will create implicit index "region_pkey" for table "region"
    ALTER TABLE
    postgres=# ALTER TABLE NATION ADD PRIMARY KEY (N_NATIONKEY);
    NOTICE:  ALTER TABLE / ADD PRIMARY KEY will create implicit index "nation_pkey" for table "nation"
    ALTER TABLE
    postgres=# ALTER TABLE NATION ADD FOREIGN KEY (N_REGIONKEY) references REGION;
    ALTER TABLE
    postgres=# ALTER TABLE PART ADD PRIMARY KEY (P_PARTKEY);
    NOTICE:  ALTER TABLE / ADD PRIMARY KEY will create implicit index "part_pkey" for table "part"
    ALTER TABLE
    postgres=# ALTER TABLE SUPPLIER ADD PRIMARY KEY (S_SUPPKEY);
    NOTICE:  ALTER TABLE / ADD PRIMARY KEY will create implicit index "supplier_pkey" for table "supplier"
    ALTER TABLE
    postgres=# ALTER TABLE SUPPLIER ADD FOREIGN KEY (S_NATIONKEY) references NATION;
    ALTER TABLE
    postgres=# ALTER TABLE PARTSUPP ADD PRIMARY KEY (PS_PARTKEY,PS_SUPPKEY);
    NOTICE:  ALTER TABLE / ADD PRIMARY KEY will create implicit index "partsupp_pkey" for table "partsupp"
    ALTER TABLE
    postgres=# ALTER TABLE CUSTOMER ADD PRIMARY KEY (C_CUSTKEY);
    NOTICE:  ALTER TABLE / ADD PRIMARY KEY will create implicit index "customer_pkey" for table "customer"
    ALTER TABLE
    postgres=# ALTER TABLE CUSTOMER ADD FOREIGN KEY (C_NATIONKEY) references NATION;
    ALTER TABLE
    postgres=# ALTER TABLE LINEITEM ADD PRIMARY KEY (L_ORDERKEY,L_LINENUMBER);
    NOTICE:  ALTER TABLE / ADD PRIMARY KEY will create implicit index "lineitem_pkey" for table "lineitem"
    ALTER TABLE
    postgres=# ALTER TABLE ORDERS ADD PRIMARY KEY (O_ORDERKEY);
    NOTICE:  ALTER TABLE / ADD PRIMARY KEY will create implicit index "orders_pkey" for table "orders"
    ALTER TABLE
    postgres=# ALTER TABLE PARTSUPP ADD FOREIGN KEY (PS_SUPPKEY) references SUPPLIER;
    ALTER TABLE
    postgres=# ALTER TABLE PARTSUPP ADD FOREIGN KEY (PS_PARTKEY) references PART;
    ALTER TABLE
    postgres=# ALTER TABLE ORDERS ADD FOREIGN KEY (O_CUSTKEY) references CUSTOMER;
    ALTER TABLE
    postgres=# ALTER TABLE LINEITEM ADD FOREIGN KEY (L_ORDERKEY) references ORDERS;
    ALTER TABLE
    postgres=# ALTER TABLE LINEITEM ADD FOREIGN KEY (L_PARTKEY,L_SUPPKEY) references PARTSUPP;
    ALTER TABLE


通过命令`\d TABLENAME`依次查看所创建表的属性，确认各约束正确加入。

以下为查看orders表格的结果

![orders表格](img/orders表格.png)

可以看到o_orderkey已经被成功设置为主键，o_curskey已经被成功设置为引用customer的外键，lineitem的l_orderkey也被成功设置为引用o_orderkey的外键。

## 实验总结

本次试验主要完成了表的创建和数据的导入，为后续对表格做各种操作的实验做准备。通过实践，本人掌握了数据库OpenGauss创建表格语句create的使用方法以及设置主键和外键约束的方法，了解该如何进行数据导入。

按照实验指导书的步骤一步一步完成基本没有遇到问题，只需要注意每完成一个步骤都需要检查一下这一步是否正确完成。每一步骤都有不同的检查方式。


# 数据查询与修改实验

## 实验目的

对前面实验建立的电商数据库关系表进行各种类型的查询操作和修改操作，加深对 SQL 语言中 DML的了解，掌握相关查询语句和数据修改语句的使用方法。

## 实验环境

本实验环境为 virtualBOX 虚拟机 openEuler20.03 系统上的 openGauss1.1.0/openGauss2.0.0 数据库和华为云GaussDB(openGauss)数据库，实验数据采用电商数据库的八张表

## 实验内容

* 单表简单查询，包括复合选择条件、结果排序、结果去重、结果重命名查询

* 多表查询，包括等值连接、自然连接、元组变量查询

* 统计查询，包括带有分组、聚集函数的查询

* 嵌套查询，包括带有 in/some/all、 exists、unique 的嵌套查询，from 中子查询

* with 临时视图查询

* 键/函数依赖分析

* 表的插入、删除、更新

## 实验步骤

**查询1：从订单表ORDERS表中，找出由收银员Clerk#000000951处理的满足下列条件的所有订单o_orderkey：**

* 订单总价位于[10000, 50000]
* 下单日期在'2020-01-01'至'2020-12-31'之间
* 订单状态 O_ORDERSTATUS 不为空

列出这些订单的订单 key（O_ORDERKEY）、客户 key、订单状态、订单总价、下单日期、订单优先级和发货优先级；

要求：对查询结果，按照订单优先级从高到低、发货优先级从高到低排序，并且将 O_ORDERDATE 重新命名为 O_DATE。

查询语句输入如下：

    select o_orderkey, o_custkey, o_orderstatus, o_totalprice, o_orderdate as o_date, o_orderpriority, o_shippriority
    from orders
    where o_clerk='Clerk#000000951'
    and o_totalprice between 10000 and 50000
    and o_orderdate between '2020-01-01'::date and '2020-12-31'::date
    and o_orderstatus is not null
    order by o_orderpriority desc, o_shippriority desc;

使用`EXPLAIN ANALYSE`语句可以获取查询所需时间和查询结果数量。一共获取8行数据，耗时105ms。


**查询2：从订单明细表 LINEITEM 表中，找出满足下列条件的所有订单 L_ORDERKEY：**
* 数量位于[10, 100]，
* 退货标志为‘N’的订单中，价格不小于10000

列出这些订单的 key 和零件供应商 key、价格；
要求：对查询结果，按照价格从高到低排序，并且对查询结果使用 distinct 去重。

比较对查询结果去重和不去重，在查询时间和查询结果上的差异。

去重的查询语句输入如下：

    select distinct l_orderkey, l_suppkey, l_extendedprice
    from lineitem
    where l_quantity between 10 and 100
    and l_returnflag='N'
    and l_extendedprice >= 10000
    order by l_extendedprice desc;


使用`EXPLAIN ANALYSE`语句对比去重和不去重的查询结果，发现去重和不去重的数量分别为498300和498304，仅仅有4行的差距。查询时间分别为1104ms和951ms，产生了150ms的差距，相当于不去重耗时的16%。


**查询3：从客户表 CUSTOMER 表中，找出满足下列条件的客户：**
* 客户电话开头部分包含‘10’，或者客户市场领域中包含“BUILDING”
* 客户电话结尾不为‘8’。

查询语句输入如下：

    select c_custkey
    from customer
    where (c_phone like '10%' or c_mktsegment like '%BUILDING%')
    and c_phone not like '%8';

使用`EXPLAIN ANALYSE`语句获得查询结果数为26915，查询时间为29ms。

**查询4：从客户表 CUSTOMER 表中，找出满足下列条件的客户姓名**
* 客户 key 由 2 个字符组成
* 客户地址至少包括 18 个字符，即地址字符串的长度不小于 18。

查询语句输入如下：

    select c_name
    from customer
    where c_custkey like '__'
    and length(c_address) >= 18;

使用`EXPLAIN ANALYSE`语句获得查询结果数为64，查询时间为41ms。


**查询5：使用集合并操作 union、union all，从订单明细表 LINEITEM 查询满足下列条件的订单 key**
* 订单发货日期早于‘2016-01-01’或者订单数量大于 100

对比 union all、union 操作在查询结果、执行时间上的差异

使用union all查询语句如下：

    select l_orderkey 
    from lineitem
    where l_shipdate < '2016-01-01'::date
    union all
    select l_orderkey
    from lineitem
    where l_quantity > 100;

使用union查询语句如下：

    select l_orderkey 
    from lineitem
    where l_shipdate < '2016-01-01'::date
    union
    select l_orderkey
    from lineitem
    where l_quantity > 100;

使用`EXPLAIN ANALYSE`语句获取到了两个查询语句的查询结果数分别为151587和41621，查询时间分别为916ms和667ms。由于union all保留了大量重复元祖，因此查询结果数和查询时间都要明显多于去重的union。

**查询6：结合教材 3.4.1 节元组变量样例，使用集合操作 except、except all，从供应商表 SUPPLIER 中，查询账户余额最大的供应商。**

对比使用 except、except all、聚集函数 max，对比完成此查询在执行时间、查询结果上的异同。

使用except查询语句如下：

    select s_suppkey
    from supplier 
    except(
      select t1.s_suppkey
      from supplier t1, supplier t2
      where t1.s_acctbal < t2.s_acctbal
    );

使用except all查询语句如下：

    select s_suppkey
    from supplier 
    except all(
      select t1.s_suppkey
      from supplier t1, supplier t2
      where t1.s_acctbal < t2.s_acctbal
    );

使用max查询语句如下：

    select s_suppkey, s_name
    from supplier
    where s_acctbal=(
      select max(s_acctbal)
      from supplier
    );

使用`EXPLAIN ANALYSE`语句获取到了三个查询语句的查询结果数分别为1、1和1，查询时间分别为2908ms、2583ms和1ms。三者的查询结果都为1个，但使用max语句的查询时间要远远小于使用except语句耗费的时间。

**查询7：选取两张数据量比较小的表 T1 和 T2，如地区表 REGION、国家表 NATION、供应商表 SUPPLIER，执行如下无连接条件的笛卡尔积操作，观察数据库系统的反应和查询结果：**

查询语句如下：

    select * from region, nation;

使用`EXPLAIN ANALYSE`语句获得查询结果数为125，查询时间为0.1ms。两个行数分别为5和25的表，产生了一张125行的表，表项依旧较少，因此耗时很短。

**查询8：使用多表连接操作（3.3.3 join/natural join，4.1.1 join），从订单表 ORDERS、供应商表 SUPPLIER、订单明细表 LINEITEM 中，查询实际到达日期小于预计到达日期的订单，列出这些订单的订单 key、订单总价、下单日期以及该供应商的姓名、地址和手机号。**

查询语句如下：

    select o_orderkey, o_totalprice, o_orderdate, s_name, s_address, s_phone
    from lineitem join orders on l_orderkey = o_orderkey join supplier on s_suppkey = l_suppkey
    where l_receiptdate < l_commitdate;

使用`EXPLAIN ANALYSE`语句获得查询结果数为133613，查询时间为790ms。

**查询9：使用多表连接操作，从供应商表 SUPPLIER、零部件表 PART、零部件供应表 PARTSUPP 中，查询供应零件品牌为‘Brand#13’的供应商信息，列出零件供应数量与成本，以及供应商的姓名与手机号。**

查询语句如下：

    select ps_availqty, ps_supplycost, s_name, s_phone
    from partsupp join supplier on ps_suppkey = s_suppkey join part on ps_partkey = p_partkey
    where part.p_brand = 'Brand#13';

使用`EXPLAIN ANALYSE`语句获得查询结果数为6424，查询时间为87ms。

**查询10：利用订单明细表 LINEITEM，使用教材 3.4.1 节元组变量 as/rename 方式，查询所有比流水号为“1”，订单号为“1”的折扣高的订单 key 和流水号，列出这些订单的零件、折扣，结果按照折扣的降序排列。**

查询语句如下：

    select l_orderkey as orderkey, 
    l_linenumber as linenumber, 
    l_partkey as partkey, 
    l_discount as discount
    from lineitem
    where l_discount > (
      select l_discount
      from lineitem
      where l_orderkey = '1'
      and l_linenumber = '1'
    )
    order by l_discount desc;

使用`EXPLAIN ANALYSE`语句获得查询结果数为655380，查询时间为1356ms。

**查询11：从订单明细表 LINEITEM、订单表 ORDERS、客户表 CUSTOMER、国家表 NATION，查询客户来自ALGERIA，下单日期为'2015-01-01'到'2015-02-02'的订单下列信息：**

* 满足条件订单的最大数量、最小数量和平均数量。
* 具有最大数量且满足上述条件的订单，列出该订单的发货日期、下单日期。

获取满足条件订单的最大数量、最小数量和平均数量的查询语句为：

    select max(l_quantity), min(l_quantity), avg(l_quantity)
    from lineitem join orders on l_orderkey = o_orderkey 
    join customer on o_custkey = c_custkey
    join nation on c_nationkey = n_nationkey
    where nation.n_name = 'ALGERIA'
    and orders.o_orderdate between '2015-01-01'::date and '2015-02-02'::date;

获得最大数量为50，最小数量为1，平均值约为25

具有最大数量且满足上述条件的订单，查询该订单的发货日期、下单日期的语句如下：

    select l_shipdate, o_orderdate
    from lineitem join orders on l_orderkey = o_orderkey 
    join customer on o_custkey = c_custkey
    join nation on c_nationkey = n_nationkey
    where nation.n_name = 'ALGERIA'
    and orders.o_orderdate between '2015-01-01'::date and '2015-02-02'::date
    order by l_quantity desc
    limit 1;

获得发货时间为2015-01-20，下单日期为2015-01-14

**查询12：根据零部件表 PART 和零部件供应表 PARTSUPP 和供应商表 SUPPLIER，查询有多少零件厂商提供了品牌为 Brand#13 的零件，给出这些零件的类型、零售价和供应商数量，并将查询结果按照零售价降序排列。**

查询语句如下：

    select p_type, p_retailprice, count(s_suppkey) as supplier_number
    from part join partsupp on ps_partkey = p_partkey 
    join supplier on ps_suppkey = s_suppkey
    where part.p_brand='Brand#13'
    group by p_type, p_retailprice
    order by p_retailprice desc;

使用`EXPLAIN ANALYSE`语句获得查询结果数为1349，查询时间为65ms。

**查询13：从零部件表 PART 和零部件供应表 PARTSUPP 中，查询所有零件大小在[7,14]之间的零件的平均零售价，给出零件 key，供应成本，平均零售价，结果按照零售价降序排列。**

查询语句如下：

    select p_partkey, ps_supplycost, avg(p_retailprice) as avgretailprice
    from part join partsupp on p_partkey = ps_partkey
    where part.p_size between 7 and 14
    group by p_partkey, ps_supplycost
    order by avgretailprice desc;

使用`EXPLAIN ANALYSE`语句获得查询结果数为26084，查询时间为173ms。

**查询14：：从订单明细表 LINEITEM、订单表 ORDERS、客户表 CUSTOMER 中，使用 set membership 运算符 in，查询明细折扣小于 0.01 的订单，列出这些订单的 key 和采购订单的客户姓名。**

对比使用多表连接、嵌套查询在执行时间、查询结果上的异同。

嵌套查询的查询语句如下：

    select o_orderkey, c_name
    from orders join customer on o_custkey = c_custkey
    where o_orderkey in(
        select l_orderkey 
        from lineitem
        where l_discount < 0.01);

多表查询查询语句如下：

    select distinct l_orderkey, c_name
    from orders join customer on c_custkey = o_custkey
    join lineitem on o_orderkey = l_orderkey
    where l_discount < 0.01;

使用`EXPLAIN ANALYSE`语句获取到了两个查询语句的查询结果数分别为90939和90939，查询时间分别为960ms和994ms。两者在耗时方面的差距并不是很大。

**查询15-1：从订单明细表 LINEITEM，使用 Set Comparison 运算符 some，查询满足下列条件的订单：该订单的数量大于发货日期在['2015-01-01','2015-02-02']之间的部分（至少一个）订单的数量，列出这些订单的流水号、key 和税。**

查询语句如下：

    select l_orderkey, l_linenumber, l_tax
    from lineitem
    where l_quantity > some (
        select l_quantity
        from lineitem
        where l_shipdate > '2015-01-01'::date
        and l_shipdate < '2015-02-02'::date
    );

使用`EXPLAIN ANALYSE`语句获得查询结果数为1176049，查询时间为17525ms。

**查询15-2：从订单表 ORDERS，使用 Set Comparison 运算符 some，查询满足下列条件的订单：订单状态为‘O’，订单总价大于部分在 2020 年之后下单的订单。列出这些订单的 key、客户 key、收银员。**

查询语句如下：

    select o_orderkey, o_custkey, o_clerk
    from orders
    where o_totalprice > some (
        select o_totalprice 
        from orders
        where o_orderdate >= '2020-01-01'::date)
        and o_orderstatus = 'O';

使用`EXPLAIN ANALYSE`语句获得查询结果数为146319，查询时间为847ms。

**查询16-1：从订单明细表 LINEITEM 中，使用 Set Comparison 运算符>=all，查询满足下列条件的供应商：该供应商在 2019 年出货量大于等于同时段其他供应商的出货量，即 2019 年该供应商的出货量最高。**

查询语句如下：

    select l_suppkey
    from lineitem
    where l_shipdate >= '2019-01-01'::date
    and l_shipdate <= '2019-12-30'::date
    group by l_suppkey
    having sum(l_quantity) >= all(
        select sum(l_quantity)
        from lineitem
        where l_shipdate >= '2019-01-01'::date
        and l_shipdate <= '2019-12-30'::date
        group by l_suppkey
    );

得到供应商key为370。

**查询 16-2：供应商表 SUPPLIER，使用 Set Comparison 运算符 all，查询账户余额大于等于其他供应商的供应商。列出该供应商的姓名、key、手机号。**

查询语句如下：

    select s_suppkey, s_name, s_phone
    from supplier
    where s_acctbal >= all(
        select s_acctbal
        from supplier
    );

得到该供应商的姓名为 Supplier#000000892 ，key为
892， 手机号为 18-893-665-3629


**查询 17-1：从供应商表 SUPPLIER、国家表 NATION，使用 Test for Empty Relations 运算符“exists”，查询国家为日本，账户余额大于 5000 的供应商。**

查询语句如下：

    select s_suppkey
    from supplier
    where exists(
        select * 
        from nation
        where n_nationkey = s_nationkey
        and n_name = 'JAPAN'
        and s_acctbal > 5000
    );

使用`EXPLAIN ANALYSE`语句获得查询结果数为42，查询时间为1.4ms。

**查询 17-2：从客户表 CUSTOMER、国家表 NATION、订单表 ORDERS、订单明细表 LINEITEM、供应商表SUPPLIER 中，使用 Test for Empty Relations 运算符“not exists except”，查询满足下列条件的供应商：该供应商不能供应所有的零件。**

查询语句如下：

    select s_suppkey
    from supplier
    except(
        select s_suppkey
        from supplier
        where not exists(
            select p_partkey
            from part
            except(
                select ps_partkey 
                from partsupp
                where ps_suppkey = s_suppkey
            )
        )
    );

使用`EXPLAIN ANALYSE`语句获得查询结果数为2000，查询时间为90578ms。


**查询 18：从国家表 NATION、客户表 CUSTOMER 中，使用“count”，查询满足下列条件的国家：至少有 3个客户来自这个国家，并列出该国家的国家 key 和国家名。**

查询语句如下：

    select n_nationkey, n_name
    from nation join customer on n_nationkey = c_nationkey
    group by n_nationkey, n_name
    having count(c_custkey) >= 3;

只有一个查询结果，该国家key为0，国家名为ALGERIA


**查询 19：从零部件表 PART 和零部件供应表 PARTSUPP 中，使用 Subqueries in the From Clause 方法，查询满足下列条件的零件：零件由 2 个以上的供应商供应，且零件大小在 20 以上。**

查询语句如下：

    select ps_partkey
    from (
        select ps_partkey
        from partsupp
        group by ps_partkey
        having count(ps_suppkey) > 2
    ) 
    join part on p_partkey = ps_partkey
    where p_size > 20;

使用`EXPLAIN ANALYSE`语句获得查询结果数为23757，查询时间为141ms。

**查询 20：用 with 临时视图方式，实现查询 19 中查询要求。**

查询语句如下：

    with temp as(
        select ps_partkey
            from partsupp
            group by ps_partkey
            having count(ps_suppkey) > 2)
    select ps_partkey
    from temp join part on p_partkey = ps_partkey
    where p_size > 20;

使用`EXPLAIN ANALYSE`语句获得查询结果数为23757，查询时间为179ms。

**查询 21：从零部件供应表 PARTSUPP 中，用 with 临时视图方式，查询零件供应数量最多的供应商 key 和其供应的数量。**

查询语句如下：

    with temp as(
        select ps_suppkey, sum(ps_availqty) as sum_num
        from partsupp
        group by ps_suppkey
    )
    select ps_suppkey, sum_num
    from temp
    where sum_num = (
        select max(sum_num)
        from temp
    );

使用`EXPLAIN ANALYSE`语句获得查询结果数为2000和供应商数量一样多，所有供应商供应任意零件数量的总合都为80，查询时间为76ms。


**查询 22：在订单明细表 LINEITEM 中，检查订单 key、零件 key、供应商 key、流水号是否组成超键。**

查询语句如下：

    select l_orderkey, l_partkey, l_suppkey, l_linenumber
    from lineitem
    group by l_orderkey, l_partkey, l_suppkey, l_linenumber
    having count(*) > 1;


结果为空，因此订单 key、零件 key、供应商 key、流水号组成超键。


**查询 23：在订单明细表 LINEITEM 中，利用 SQL 语句检查函数依赖零件 key→价格是否成立；如果不成立，利用 SQL 语句找出导致函数依赖不成立的元组。**

以下为查询语句：

    select *
    from lineitem
    where l_partkey in(
    select l_partkey
    from lineitem
    group by l_partkey
    having count(distinct l_extendedprice) > 1);

使用`EXPLAIN ANALYSE`语句获得查询结果数为1199969，查询时间为3887.795ms。因此key→价格不成立。


**查询 24：向订单表 ORDERS 中插入一条订单数据；**

插入语句如下：

    insert into orders values('1200001', '20045', 'F', 61365.24, '2017-03-19'::date, '2-HIGH', 
    'Clerk#000000098', 0, 'furiously special f');

使用查询语句`select * from orders where o_orderkey = 1200001;`可以查询这条插入的元组

![插入语句](img/插入语句.png)


**查询 25：将零件 32 的全部供应商，作为零件 20 的供应商，加入到零部件供应表 PARTSUPP 中。**

插入语句如下：

    insert into partsupp(
        select '20', ps_suppkey, ps_availqty, ps_supplycost, ps_comment
        from partsupp
        where ps_partkey = '32'
        and ps_suppkey not in(
            select ps_suppkey
            from partsupp
            where ps_partkey = '20'
        )
    );

在插入前后使用以下查询语句

    select * 
    from partsupp
    where ps_partkey = '20';

发现插入前查询结果行数为4，插入后的行数为8。

对比以下查询语句的结果：

    EXPLAIN ANALYZE select * 
    from partsupp
    where ps_partkey = '32';

查询结果行数为4，除了ps_partkey其他属性刚好与插入后比插入前的差值，证明插入成功。


**查询 26：在订单明细表 LINEITEM 中，删除已退货的订单记录。(returnflag='R')**

以下为删除语句：

    delete from lineitem
    where l_returnflag = 'R';

使用以下查询语句

    select * 
    from lineitem
    where l_returnflag = 'R';

查询结果为0，说明删除成功。

**查询 27：用订单明细表 LINEITEM 中在 2019 年之后交易中的预计到达日期，替换表中的实际到达日期。**

以下为更新语句：

TODU

    update lineitem
    set l_receiptdate = l_commitdate
    from orders
    where l_orderkey = o_orderkey
    and o_orderdate >= '2020-01-01'::date;

使用以下查询语句

    select * 
    from lineitem join orders on l_orderkey = o_orderkey
    where l_receiptdate != l_commitdate 
    and o_orderdate >= '2020-01-01'::date;

查询结果为0，说明更新成功。

**查询 28：针对订单明细表 LINEITEM、订单表 ORDERS，使用 update/case 语句做出如下修改：如果订单的订单优先级低于 medium，则其在订单明细表中的预计到达日期推后 2 天, 否则推迟一天。**

以下为更新语句：

    update lineitem
    set l_commitdate = case 
        when l_orderkey in (
            select o_orderkey 
            from orders
            where o_orderpriority = '4-NOT SPECIFIED' 
            or o_orderpriority = '5-LOW'
        ) 
        then l_commitdate + '2 day'
        else l_commitdate + '1 day'
    end;

查询使用更新语句前后的lineitem表格，发现订单优先级为not specified或low的订单l_commitdate多了两天，其余订单多了一天，说明更新成功。

**查询 29：在订单表 ORDERS 中，按照订单总价对订单进行降序排序，并输出订单 key 和排名。**

以下为查询语句：

select o_orderkey, o_totalprice, rank() over(order by o_totalprice desc) as ‘Rank’
from ORDERS;

查询结果按o_totalprice降序排列，新增Rank列显示订单的排名。

## 实验总结

通过本次实践，本人基本掌握了查询语句的各种使用方式和场景，基本掌握了插入、删除和更新操作，学会通过查询语句判断函数依赖关系。

实验中遇到的一个问题就是由于查询结果一般数量巨大，实验结果难以表示。因此，实验结果主要通过使用`EXPLAIN ANALYZE`语句获取查询结果行数和查询所需时间来展现。同时，对于需要对比的查询语句，`EXPLAIN ANALYZE`语句也可用于对比不同查询语句的表现。

# 数据库完整性约束实验

## 实验目的

了解 SQL 语言和 openGauss 数据库提供的完整性（integrity）机制，通过实验掌握面向实际数据库建立实体完整性、参照完整性、断言、函数依赖等各种完整性约束的方法，验证各类完整性保障措施。

## 实验环境

本实验环境为 virtualBOX 虚拟机 openEuler20.03 系统上的openGauss1.1.0/openGauss2.0.0 数据库和华为云 GaussDB(openGauss)数据库，实验数据采用电商数据库的八张表。

## 实验内容

在前面完成的实验中已建立了本实验所需的 8 张表。本实验将针对这 8 张表，采用 create table、alter table 等语句，添加主键、候选键、外键、check 约束、默认/缺省值约束，并观察当用户对数据库进行增、删、改操作时，DBMS 如何维护完整性约束。

* 建立完整性约束
* 主键/候选键/空值/check/默认值约束验证
* 外键/参照完整性验证分析
* 函数依赖
* 触发器

## 实验步骤

### 利用 Create table/Alter table 语句建立完整性约束

创建lineitem表格副本，并使用`Create table`建立完整性约束。以下是创建语句：

    CREATE TABLE LINEITEMcopy1(
    L_ORDERKEY integer NOT NULL,
    L_PARTKEY integer NOT NULL,
    L_SUPPKEY integer NOT NULL,
    L_LINENUMBER integer NOT NULL,
    L_QUANTITY DECIMAL(15,2) NOT NULL,
    L_EXTENDEDPRICE DECIMAL(15,2) NOT NULL,
    L_DISCOUNT DECIMAL(15,2) NOT NULL,
    L_TAX DECIMAL(15,2) NOT NULL,
    L_RETURNFLAG CHAR(1) NOT NULL,
    L_LINESTATUS CHAR(1) NOT NULL,
    L_SHIPDATE DATE NOT NULL,
    L_COMMITDATE DATE NOT NULL,
    L_RECEIPTDATE DATE NOT NULL,
    L_SHIPINSTRUCT CHAR(25) NOT NULL,
    L_SHIPMODE CHAR(10) NOT NULL,
    L_COMMENT VARCHAR(44) NOT NULL,
    PRIMARY KEY (L_ORDERKEY, L_LINENUMBER),
    FOREIGN KEY (L_PARTKEY) REFERENCES PART(P_PARTKEY),
    FOREIGN KEY (L_SUPPKEY) REFERENCES SUPPLIER(S_SUPPKEY)
    );

![create建立约束](/img/create建立约束.png)

可以看到完整性约束建立成功。

接着在创建另一个不带约束的lineitem表格副本LINEITEMcopy2，改用`alter table`添加约束。

设置主键为L_ORDERKEY和L_LINENUMBER：

    alter table LINEITEMcopy2 add constraint LINEITEMcopy2_PKEY primary key (L_ORDERKEY, L_LINENUMBER);

设置外键L_PARTKEY引用自PART：

    alter table LINEITEMcopy2 add constraint LINEITEMcopy2_L_PARTKEY_FKEY foreign key (L_PARTKEY) references PART;

设置外键L_SUPPKEY引用自SUPPLIER：

    alter table LINEITEMcopy2 add constraint LINEITEMcopy2_L_SUPPKEY_FKEY foreign key (L_SUPPKEY) references SUPPLIER;

![alter建立约束](/img/alter建立约束.png)

可以看到成功建立了和LINEITEMcopy1一样的完整性约束

### 主键约束验证

将LINEITEM数据全部插入到表LINEITEMcopy1后，使用查询语句判断LINEITEMcopy1是否满足主键约束，以下为查询语句：

    Select l_orderkey, count(*)
    From lineitemcopy1
    Group by (l_orderkey, l_linenumber)
    Having count(*)>1;

返回结果为0行，说明表中无重复主键的数据。

使用查询语句判断是否含有主键为空的数据：

    select *
    from lineitemcopy1
    where l_orderkey is null
    and l_linenumber is null;

返回结果为0行，说明表中无主键为空的数据。

尝试插入主键为空的数据：

    INSERT INTO lineitemcopy1
    values(null,0,0,null,0,0,0,0, 'a', 'b', '2020-01-01'::date,'2020-01-12'::date, '2020-01-15'::date,'name3', 'name4', 'name5');

主键约束会自动为l_orderkey添加非空值约束，因此发生错误：
   
    ERROR:  null value in column "l_orderkey" violates not-null constraint

尝试修改原有数据行 l_orderkey, l_linenumber 字段为空

    UPDATE lineitemcopy1
    SET l_orderkey=null, l_linenumber=null
    WHERE l_orderkey =1 and l_linenumber=5;

发生错误：
   
    ERROR:  null value in column "l_orderkey" violates not-null constraint

尝试修改原有数据行 l_orderkey 字段和 l_linenumber 字段与表中已有其它元组的主属性取值相同：

    UPDATE lineitemcopy1
    SET l_linenumber =2
    WHERE l_orderkey=1 and l_linenumber=1;

由表中已存在l_orderkey=1 and l_linenumber=2的数据行，产生主键相同的数据行，因此发生错误：
    
    ERROR:  duplicate key value violates unique constraint "lineitemcopy1_pkey"

尝试插入主键重复的数据：

    INSERT INTO lineitemcopy1
    values(1,0,0,2,0,0,0,0, 'a', 'b', '2020-01-01'::date,
    '2020-01-01'::date, '2020-01-01'::date,'name3', 'name4', 'name5');

发生错误：

    ERROR:  duplicate key value violates unique constraint "lineitemcopy1_pkey"

### 空值约束验证

尝试插入一行l_extendedprice字段为空的数据

    INSERT INTO lineitemcopy1
    values(1,0,0,2,0,null,0,0, 'a', 'b', '2020-01-01'::date,
    '2020-01-01'::date, '2020-01-01'::date,'name3', 'name4', 'name5');

发生错误，l_extendedprice字段为空违反非空约束

    ERROR:  null value in column "l_extendedprice" violates not-null constraint

尝试修改原有数据行l_extendedprice字段为空

同样发生错误

    ERROR:  null value in column "l_extendedprice" violates not-null constraint

### 外键完整性约束

为方便起见，和前文中lineitem一样，分别创建关系表orders和customer的副本orderscopy和customercopy，并仅创建这两个表之间的外键约束，即`FOREIGN KEY (o_custkey) REFERENCES customercopy1(c_custkey)`，然后将数据导入进去

使用查询语句判断两表间是否满足参照完整性约束：

    select count(O_CUSTKEY)
    from orderscopy1
    where O_CUSTKEY not in (
        select C_CUSTKEY
        from customercopy1
    );

输出count为0，因此两表满足参照完整性约束。

尝试向orderscopy表中插入一行数据，其O_CUSTKEY值设为0

    insert into orderscopy1
    values(1200001,0,'O',181580, '2019-01-02'::date,'5-LOW','Clerk#000000406',0,'special f');

由于customercopy表中不存在C_CUSTKEY值为0的数据行，违反了外键约束，因此插入失败：

    ERROR:  insert or update on table "orderscopy1" violates foreign key constraint "orderscopy1_o_custkey_fkey"

尝试将orderscopy表中一行O_CUSTKEY值为7828的数据的O_CUSTKEY字段值修改为31000

    UPDATE orderscopy1
    SET O_CUSTKEY=31000
    WHERE O_CUSTKEY=7828;

由而customercopy表中并没有C_CUSTKEY值为31000的数据行，因此违反了外键约束，更新失败

    ERROR:  insert or update on table "orderscopy1" violates foreign key constraint "orderscopy1_o_custkey_fkey"

**在非级联外键约束条件下**

尝试将customercopy表中一行C_CUSTKEY值为29980的数据的 C_CUSTKEY字段值修改为31001：

    UPDATE customercopy1
    SET C_CUSTKEY=31001
    WHERE C_CUSTKEY=29980;

由于orderscopy中存在O_CUSTKEY值为29980的数据行，违反外键约束，执行出错：

    ERROR:  update or delete on table "customercopy1" violates foreign key constraint "orderscopy1_o_custkey_fkey" on table "orderscopy1"

从customercopy表中删除C_CUSTKEY值为29980的数据行：

    delete from customercopy1
    where C_CUSTKEY=29980;

由于orderscopy中存在O_CUSTKEY值为29980的数据行，违反外键约束，执行出错

    ERROR:  update or delete on table "customercopy1" violates foreign key constraint "orderscopy1_o_custkey_fkey" on table "orderscopy1"

**在级联外键约束条件下**

为了重新定义级联的外键约束，首先删除原来的外键约束

    alter table orderscopy1
    drop constraint orderscopy1_o_custkey_fkey;

重新定义orderscopy和customercopy之间的级联关联如下：

    alter table orderscopy1
    add constraint FK_O_CUSTKEY
    foreign key(O_CUSTKEY) references customercopy1(C_CUSTKEY)
    on delete cascade
    on update cascade;

再次尝试将customercopy表中一行C_CUSTKEY值为29980的数据的C_CUSTKEY字段值修改为31001，此时就更新成功了。

查看ORDERS中O_CUSTKEY值为29980和31001的数据行：

    select *
    from orderscopy1
    where O_CUSTKEY=29980;
    select *
    from orderscopy1
    where O_CUSTKEY=31001;

由于ORDERS中O_CUSTKEY值为29980的数据行的值也跟着改为31001，因此两个查询分别返回0和37个数据项。

再次从customercopy表中删除C_CUSTKEY值为31001的数据行，也同样删除成功。

查看ORDERS中O_CUSTKEY值为31001的数据行，返回0项数据，说明ORDERS中O_CUSTKEY值为31001的数据行也跟着被删除了。

这就是级联和非级联的区别，级联外键关联下，当被参照关系中的主键发生修改，删除时，参照关系中的外键会跟着进行相应地修改，删除。

### 函数依赖分析验证

可以使用查询语句判断函数依赖P_BRAND→P_MFGR是否满足：

    select max(a) as a_MFGR
    from(
        select count(DISTINCT P_MFGR) as a
        from part
        group by P_BRAND
    );

返回值为1，因此对于P_BRAND值相同的所有数据行，其P_MFGR值最多只有一个。满足函数依赖P_BRAND→P_MFGR。

### 触发器约束

由于触发器中禁止增删改操作的嵌套使用，因此为了完成实验需求，再对lineitemcopy表进行一个备份，新表为lineitemcopy_new。为了验证触发器正确性，删除新表上的相关约束。在实际应用中需保持两表的数据一致性，本实验仅验证触发器效果。


首先需要创建将新数据插入到lineitemcopy中的触发器函数：

    CREATE OR REPLACE FUNCTION tri_insert_func() RETURNS TRIGGER AS 
    $$ DECLARE BEGIN 
    INSERT INTO lineitemcopy1
    VALUES (new.L_ORDERKEY, new.L_PARTKEY,new.L_SUPPKEY,new.L_LINENUMBER,new.L_QUANTITY,new.L_EXTENDEDPRICE,
    new.L_DISCOUNT, new.L_TAX, new.L_RETURNFLAG, new.L_LINESTATUS, new.L_SHIPDATE, new.L_COMMITDATE,
    new.L_RECEIPTDATE, new.L_SHIPINSTRUCT, new.L_SHIPMODE, new.L_COMMENT);
    RETURN NEW; 
    END $$ LANGUAGE PLPGSQL;

然后在lineitemcopy_new上定义插入触发器，如果发货日期满足发货日期必须在预计到达日期和实际到达日期之前，则插入到 lineitemcopy中，若不满足条件，则不进行插入操作：

    CREATE TRIGGER insert_trig_before BEFORE INSERT ON lineitemcopy1_new for EACH ROW
    WHEN(new.l_shipdate <= new.l_commitdate and new.l_shipdate <= new.l_receiptdate)
    EXECUTE PROCEDURE tri_insert_func();

此时尝试向lineitemcopy_new插入一行数据，发货日期为2020年1月2日，预计到达日期和实际到达日期均为2020年1月1日。

    INSERT INTO lineitemcopy1_new
    values(4,0,0,2,0,0,0,0, 'a', 'b', '2020-01-02'::date,
    '2020-01-01'::date, '2020-01-01'::date,'name3', 'name4', 'name5');

使用查询语句分别检查表lineitemcopy1_new和表lineitemcopy：

    select *
    from lineitemcopy1_new
    where l_orderkey=4 and l_linenumber=2;
    select *
    from lineitemcopy1
    where l_orderkey=4 and l_linenumber=2;


发现语句成功插入到表lineitemcopy1_new，但由于不满足约束条件，没有插入到lineitemcopy中。

再插入一行发货日期为2020年1月2日，预计到达日期为2020年1 月3日，实际到达日期均为2020年1月4日的数据：

    INSERT INTO lineitemcopy1_new
    values(4,1,1,3,0,0,0,0, 'a', 'b', '2020-01-02'::date,'2020-01-03'::date, '2020-01-04'::date,'name3', 'name4', 'name5');

再次使用查询语句分别检查表lineitemcopy1_new和表lineitemcopy，由于满足约束，数据成功插入到两个表中。

再创建一个触发器函数用于修改改发货日期：

    CREATE OR REPLACE FUNCTION tri_update_func() RETURNS TRIGGER AS 
    $$ DECLARE BEGIN
    UPDATE lineitemcopy1 
    SET L_SHIPDATE =new. L_SHIPDATE
    WHERE old. L_ORDERKEY = new. L_ORDERKEY and old. L_LINENUMBER = new. L_LINENUMBER;
    RETURN NEW; 
    END $$ LANGUAGE PLPGSQL;

同样对lineitemcopy_new设置一个更新触发器，插入前比较发货日期与预计到达日期、实际到达日期，当发货日期满足约束时，将新值更新到lineitemcopy对应行中，否则不进行更新：

    CREATE TRIGGER updata_trig_before BEFORE UPDATE ON lineitemcopy1_new for EACH ROW
    WHEN(new.l_shipdate <= new.l_commitdate and new.l_shipdate <= new.l_receiptdate)
    EXECUTE PROCEDURE tri_update_func();

将刚才插入的订单 key 为 4，流水号为 3 的数据进行修改，发货日期新值为 2019-02-01：

    UPDATE lineitemcopy1_new
    SET l_shipdate='2019-02-01'::date
    WHERE l_orderkey=4 and l_linenumber=3;

由于满足条件，使用之前的查询语句分别检查表lineitemcopy1_new和表lineitemcopy，发现在两张表中数据都同样更新成功了。

再将订单key为4，流水号为3的数据进行修改，PCI新值为 2020-02-01：

    UPDATE lineitemcopy1_new
    SET l_shipdate='2020-02-01'::date
    WHERE l_orderkey=4 and l_linenumber=3;

使用之前的查询语句分别检查表lineitemcopy1_new和表lineitemcopy，由于不满足条件，只有lineitemcopy1_new的数据项更新成功。

## 实验总结

通过本次实践，本人基本掌握了为表添加完整性约束的方法，学会通过查询语句判断完整性约束的正确性。通过实践，进一步了解了级联和非级联的区别。最后还尝试使用了触发器实现条件检测与过程的触发。


# 数据库接口实验

# 事务及其并发控制实验