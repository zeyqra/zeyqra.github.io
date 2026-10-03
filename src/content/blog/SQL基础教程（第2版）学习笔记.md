---
title: 'SQL基础教程（第2版）学习笔记'
description: 'Learn SQL'
pubDate: '2026-09-29'
# heroImage: '../../assets/blog-placeholder-3.jpg'
---

## 准备和概述
安装和连接 `PostgreSQL`
```
cd D:\soft\PostgreSQL\18\bin
./psql.exe -U postgres -d shop
./psql.exe -U postgres -d
\q
``` 
> 关系数据库必须以行为单位进行数据读写。  
> 一个单元格中只能输入一个数据。  
> 字符串和日期常数需要使用单引号括起来。
```SQL
CREATE DATABASE shop;

CREATE TABLE Product (
  product_id      CHAR(4)       not null,
  product_name    varchar(100)  not null,
  product_type    varchar(32)   not null,
  sale_price      integer,
  purchase_price  integer,
  regist_date     date,
  primary key (product_id)
);

alter table Product add column product_name_py varchar(100);
alter table Product drop column product_name_py;

begin transaction;
insert into Product values (
  '0001', 'T恤衫', '衣服', 1000, 500, '2009-09-20');
insert into Product values (
  '0002', '打孔器', '办公用品', 500, 320, '2009-09-11');
insert into Product values (
  '0003', '运动T恤', '衣服', 4000, 2800, NULL);
insert into Product values (
  '0004', '菜刀', '厨房用具', 3000, 2800, '2009-09-20');
insert into Product values (
  '0005', '高压锅', '厨房用具', 6800, 5000, '2009-01-15');
insert into Product values (
  '0006', '叉子', '厨房用具', 500, NULL, '2009-09-20');
insert into Product values (
  '0007', '擦菜板', '厨房用具', 880, 790, '2008-04-28');
insert into Product values (
  '0008', '圆珠笔', '办公用品', 100, NULL,'2009-11-11');
commit;
```

#### 数据类型
`integer` 整数  
`char`    定长字符串  
`varchar` 可变长字符串  
`date `   日期


### 基础查询
```SQL
select product_id, product_name, purchase_price
  from Product;
```
#### 查询所有列
```SQL
select * from Product;
```
#### 为列设定别名
```SQL
select product_id as id, product_name as name, purchase_price as price
  from Product;
```
> 设定汉语别名时需要使用双引号括起来。

#### 常数的查询
```SQL
select '商品' as string, 38 as number, '2009-02-24' as date, product_id, product_name
  from Product;
```
#### 从结果中删除重复行
```SQL
select distinct product_type from Product;
```
> NULL 的数据也被合并为一条。 
```SQL
select distinct product_type, regist_date from Product;
```
> product_type 和 regist_date 同时相同。
#### WHERE语句
```SQL
select product_name, product_type
  from Product
  where product_type = '衣服';
```
> WHERE 是 SELECT 查询块的一部分,WHERE 修饰其所在的 SELECT 查询块  
> 在 WHERE 子句中不能使用聚合函数
#### 注释
```
select product_name, product_type 
-- 单行注释
/* 多行注释 */
from Product where product_type = '衣服';
```
### 运算符
#### 算术运算符
```
select product_name, sale_price, sale_price * 2 as "sale_price_x2" from Product;
```
> +、-、*、/
> 所有包含 NULL 的计算，结果肯定是 NULL。

#### 比较运算符
```SQL
select product_name, product_type from Product where sale_price = 500;
select product_name, product_type from Product where sale_price <> 500;
select product_name, product_type, sale_price from Product where sale_price >= 1000;
select product_name, product_type, regist_date from Product where regist_date < '2009-09-27';
select product_name, sale_price, purchase_price from Product where sale_price - purchase_price >= 500;
```
> 字符串类型的数据按照字典顺序进行排序。  
> 筛选不出值为 NULL 的记录。

#### NULL运算符
```SQL
select product_name, purchase_price from Product where purchase_price is null;
select product_name, purchase_price from Product where purchase_price is not null;
```
#### NOT运算符
```SQL
select product_name, product_type, sale_price from Product where not sale_price >= 1000;
```
#### AND运算符和OR运算符
```SQL
select product_name, purchase_price from Product where product_type = '厨房用具' and sale_price >= 3000;
select product_name, purchase_price from Product where product_type = '厨房用具' or sale_price >= 3000;
select product_name, product_type, regist_date from Product
  where product_type = '办公用品' and (regist_date = '2009-09-11' or regist_date = '2009-09-20');
```
> AND 运算符优先于 OR 运算符

#### 含有NULL时的真值
> 除真假之外的第三种值——不确定（UNKNOWN）。  
> 真 and 不确定 -> 不确定  
> 假 or 不确定 -> 不确定

#### 聚合和排序
#### 数据的行数
```SQL
select count(*) from Product;
select count(purchase_price) from Product;
```
COUNT函数的结果根据参数的不同而不同。COUNT(*)会得到包含NULL的数据行数，而COUNT(<列名>)会得到NULL之外的数据行数。

#### sum avg max min
```SQL
select sum(sale_price), sum(purchase_price) from Product;
```
聚合函数会将NULL排除在外。COUNT(*)例外。  
```SQL
select avg(sale_price), avg(purchase_price) from Product; -- 分母也排除了null
select max(sale_price), min(purchase_price) from Product;
select max(regist_date), min(regist_date) from Product;
```
MAX/MIN函数几乎适用于所有数据类型的列。SUM/AVG函数只适用于数值类型的列。

#### 在聚合函数的参数中使用DISTINCT 删除重复数据
```SQL
select count(product_type) from Product; 8
select count(distinct product_type) from Product; 3
select distinct count (product_type) from Product; 8
select sum(sale_price), sum(distinct sale_price) from Product;
```
#### 分组（每组展示一行）
```SQL
select product_type, count(*) from Product group by product_type;
select purchase_price, count(*) from Product group by purchase_price;
```
NULL会单独分为一组  
```SQL
select product_type, purchase_price, count(*) from Product group by product_type, purchase_price;
select purchase_price, count(*) from Product where product_type = '衣服' group by purchase_price;
```
> 使用聚合函数时，SELECT 子句中只能存在以下三种元素。  
>   ●	常数  
>   ●	聚合函数  
>   ● GROUP BY子句中指定的列名（也就是聚合键）  
> 使用GROUP BY子句时，SELECT子句中不能出现聚合键之外的列名。  
> GROUP BY子句结果的显示是无序的。  
> 只有SELECT子句和HAVING子句（以及ORDER BY子句）中能够使用聚合函数。

#### 为聚合结果指定条件
WHERE子句用来指定数据行的条件，HAVING子句用来指定分组的条件。
```SQL
select product_type, count(*)
from Product
group by product_type
having count(*) = 2;
-- 组的 count(*)

select product_type, avg(sale_price)
from Product
group by product_type
having avg(sale_price) >= 2500;
-- 组的 avg(sale_price)
```
>HAVING 子句中能够使用的 3 种要素如下所示。  
>  ●	常数  
>  ●	聚合函数  
>  ● GROUP BY子句中指定的列名（即聚合键）

#### 对查询结果进行排序
```SQL
select product_id, product_name, sale_price, purchase_price from Product order by sale_price;
select product_id, product_name, sale_price, purchase_price from Product order by sale_price desc;
```
> 默认使用升序 asc 进行排列。  
```SQL
select product_id, product_name, sale_price, purchase_price from Product order by sale_price, product_id;
```
> 排序键中包含NULL时，会在开头或末尾进行汇总。  
```SQL
select product_type, count(*) from Product group by product_type order by product_id,  count(*);
```
> 在ORDER BY子句中可以使用SELECT子句中未使用的列和聚合函数。  
> 书写顺序: select from where group by having order by  
> 执行顺序: from where group by having select order by


## 数据更新
```SQL
create table ProductIns(
  product_id      char(4)       not null,
  product_name    varchar(100)  not null,
  product_type    varchar(32)   not null,
  sale_price      integer       default 0, -- 销售单价的默认值设定为0;
  purchase_price  integer,
  regist_date     date,
  primary key (product_id)
);
```
#### INSERT语句
```SQL
insert into ProductIns (product_id, product_name, product_type, sale_price, purchase_price, regist_date) 
  values ('0001', 'T恤衫', '衣服', 1000, 500, '2009-09-20');
```
#### 多行INSERT
```SQL
INSERT INTO ProductIns VALUES
  ('0002', '打孔器', '办公用品', 500, 320, '2009-09-11'),
  ('0003', '运动T恤', '衣服', 4000, 2800, NULL),
  ('0004', '菜刀', '厨房用具', 3000, 2800, '2009-09-20');
```
> 对表进行全列 INSERT 时，可以省略表名后的列清单。
```SQL
insert into ProductIns
  values ('0005', '高压锅', '厨房用具', 6800, 5000, '2009-01-15');
```
#### 插入NULL
```SQL
INSERT INTO ProductIns (product_id, product_name, product_type, sale_price, purchase_price, regist_date) 
  VALUES ('0006', '叉子', '厨房用具', 500, NULL, '2009-09-20');
```
#### 插入默认值
```SQL
INSERT INTO ProductIns (product_id, product_name, product_type, sale_price, purchase_price, regist_date) 
  VALUES ('0007', '擦菜板', '厨房用具', DEFAULT, 790, '2009-04-28');
```
> 省略INSERT语句中的列名，就会自动设定为该列的默认值（没有默认值时会设定为NULL）。

#### 从其他表中复制数据
```SQL
CREATE TABLE ProductCopy
(product_id     CHAR(4)       NOT NULL,
 product_name   VARCHAR(100)  NOT NULL,
 product_type   VARCHAR(32)   NOT NULL,
 sale_price     INTEGER,
 purchase_price INTEGER,
 regist_date    DATE,
 PRIMARY KEY (product_id));

insert into ProductCopy (product_id, product_name, product_type, sale_price, purchase_price, regist_date) 
  select product_id, product_name, product_type, sale_price, purchase_price, regist_date
    from Product;
```
#### INSERT语句的SELECT语句中，使用 WHERE 子句或者 GROUP BY 子句
```SQL
create table ProductType (
  product_type        varchar(32) not null,
  sum_sale_price      integer,
  sum_purchase_price  integer,
  primary key (product_type)
);
insert into ProductType (product_type, sum_sale_price, sum_purchase_price)
  select product_type, sum(sale_price), sum(purchase_price)
    from Product
    group by product_type;
```
  
### 数据的删除
#### 删除整张表
```SQL
drop table Product;
```
#### 删除表中的全部数据
```SQL
delete from Product;
```
#### 删除表中的部分数据
```SQL
delete from Product where sale_price >= 4000;
```

### 数据的更新
#### 更新单列数据
```SQL
update Product set regist_date = '2009-10-10';
update Product set sale_price = sale_price * 10
  where product_type = '厨房用具';
update Product set regist_date = null
  where product_id = '0008';
```
#### 更新多列数据
```SQL
update Product set sale_price = sale_price * 10, purchase_price = purchase_price / 2
  where product_type = '厨房用具';
update Product set (sale_price, purchase_price) = (sale_price * 10, purchase_price / 2)
  where product_type = '厨房用具';
```

### 事务
```SQL
begin transaction;
update Product set sale_price = sale_price - 1000 where product_name = '运动T恤';
update Product set sale_price = sale_price + 1000 where product_name = 'T恤衫';
commit 或 rollback;
```
> 两种模式：
> A 每条SQL语句就是一个事务（自动提交模式）
> B 直到用户执行COMMIT或者ROLLBACK为止算作一个事务


### 复杂查询
#### 创建视图
```SQL
create view ProductSum (product_type, cnt_product) as
select product_type, count(*) from Product group by product_type;

select product_type, cnt_product from ProductSum;
```
> 视图就是保存好的 SELECT 语句。  
> 定义视图时可以使用任何 SELECT 语句。  
#### 创建多重视图
```SQL
create view ProductSumJim (product_type, cnt_product) as
select product_type, cnt_product from ProductSum where product_type = '办公用品';
```
> 应该避免在视图的基础上创建视图。多重视图会降低 SQL 的性能。   

> 视图的限制: 
> 定义视图时不能使用ORDER BY子句  

#### 对视图进行更新
> 视图需要满足以下条件:
> SELECT 子句中未使用 DISTINCT  
> FROM 子句中只有一张表  
> 未使用 GROUP BY 子句  
> 未使用 HAVING 子句  
```SQL
create view ProductJim (product_id, product_name, product_type, sale_price, purchase_price, regist_date) as
select * from Product where product_type = '办公用品';

insert into ProductJim values ('0009', '印章', '办公用品', 95, 10, '2009-11-30');
```
#### 删除视图
```SQL
drop view ProductSum;
```

### 子查询
子查询就是一次性视图（SELECT语句）。子查询在SELECT语句执行完毕之后就会消失。
```SQL
select product_type, cnt_product from (
select product_type, count(*) as cnt_product from Product group by product_type
) as ProductSum;
```
> 子查询作为内层查询会首先执行。

#### 嵌套的子查询
```SQL
select product_type, cnt_product from (
  select * from (
    select product_type, count(*) as cnt_product from Product group by product_type
  ) as ProductSum where cnt_product = 4
)as ProductSum2;
```

#### 标量子查询
必须而且只能返回 1 行 1 列的结果
```SQL
select product_id, product_name, sale_price from Product
where sale_price > (select avg(sale_price) from Product);
```
> 几乎所有的地方都可以使用标量子查询。
```SQL
select product_id, product_name, sale_price, 
  (select avg(sale_price) from Product as avg_price)
from Product;

select product_type, avg(sale_price)
from Product
group by product_type
having avg(sale_price) > (select avg(sale_price) from Product);
```
#### **关联子查询**
> 在细分的组内进行比较时，需要使用关联子查询。
> 关联子查询也是用来对集合进行切分的，实际只能返回 1 行结果。
```SQL
select product_type, product_name, sale_price
from Product as P1
where sale_price > (
  select avg(sale_price)
  from Product as P2
  where P1.product_type = P2.product_type
);
```

### 六、函数、谓词、CASE表达式

#### 算术函数
> + - * /  
> 绝对值: ABS(数值)  
> 求余: MOD(被除数，除数)  
> 舍入: ROUND(对象数值，保留小数的位数)  

#### 字符串函数
> 拼接: 字符串1 || 字符串2 || 字符串3  
> 长度: LENGTH(字符串)  
> 小写: LOWER(字符串)  
> 大写: UPPER(字符串)  
> 替换: REPLACE(对象字符串, 替换前的字符串, 替换后的字符串)  
> 截取: SUBSTRING(对象字符串 FROM 截取的起始位置 FOR 截取的字符数)  

#### 日期函数
> 当前日期: CURRENT_DATE  
> 当前时间: CURRENT_TIME  
> 当前日期和时间: CURRENT_TIMESTAMP  
> 截取:   
> EXTRACT(日期元素 FROM 日期)   
> EXTRACT(YEAR FROM CURRENT_TIMESTAMP)  

#### 转换函数
> 类型转换: cast(转换前的值 AS 想要转换的数据类型)  
> 左侧开始第1个不是 NULL 的值: coalesce(数据1, 数据2, 数据3……)  

#### 谓词
#### LIKE
```SQL
-- create table SampleLike (
--   strcol varchar(6) not null,
--   primary key (strcol)
-- );
-- begin transaction;
-- insert into SampleLike (strcol) values ('abcddd');
-- INSERT INTO SampleLike (strcol) VALUES ('dddabc');
-- INSERT INTO SampleLike (strcol) VALUES ('abdddc');
-- INSERT INTO SampleLike (strcol) VALUES ('abcdd');
-- INSERT INTO SampleLike (strcol) VALUES ('ddabc');
-- INSERT INTO SampleLike (strcol) VALUES ('abddc');
-- commit;
```
**前方一致查询**
```SQL
select * from SampleLike where strcol like 'ddd%';
```
**中间一致查询**
```SQL
select * from SampleLike where strcol like '%ddd%';
```
**后方一致查询**
```SQL
select * from SampleLike where strcol like '%ddd';
```
**自定义字符数量和位置**
```SQL
select * from SampleLike where strcol like 'abc__';
select * from SampleLike where strcol like '__abc';
select * from SampleLike where strcol like 'a_c%';
```
#### BETWEEN
```SQL
select product_name, sale_price 
  from Product 
  where sale_price between 100 and 1000;
```
> 会包含 100 和 1000 这两个临界值。  

#### IS NULL、IS NOT NULL
```SQL
select product_name, purchase_price
  from Product
  where purchase_price is not null;
```

#### IN
```SQL
select product_name, purchase_price
  from Product
  where purchase_price in (320, 500, 5000);

select product_name, purchase_price
  from Product
  where purchase_price not in (320, 500, 5000);
```
> 在使用IN 和 NOT IN 时是无法选取出 NULL 数据的。  

#### **_使用子查询作为IN的参数_**
```SQL
-- CREATE TABLE ShopProduct
-- (shop_id CHAR(4) NOT NULL,
--  shop_name VARCHAR(200) NOT NULL,
--  product_id CHAR(4) NOT NULL,
--  quantity INTEGER NOT NULL,
--  PRIMARY KEY (shop_id, product_id));
-- BEGIN TRANSACTION;
-- INSERT INTO ShopProduct (shop_id, shop_name, product_id, quantity) VALUES ('000A', '东京', '0001', 30);
-- INSERT INTO ShopProduct (shop_id, shop_name, product_id, quantity) VALUES ('000A', '东京', '0002', 50);
-- INSERT INTO ShopProduct (shop_id, shop_name, product_id, quantity) VALUES ('000A', '东京', '0003', 15);
-- INSERT INTO ShopProduct (shop_id, shop_name, product_id, quantity) VALUES ('000B', '名古屋', '0002', 30);
-- INSERT INTO ShopProduct (shop_id, shop_name, product_id, quantity) VALUES ('000B', '名古屋', '0003', 120);
-- INSERT INTO ShopProduct (shop_id, shop_name, product_id, quantity) VALUES ('000B', '名古屋', '0004', 20);
-- INSERT INTO ShopProduct (shop_id, shop_name, product_id, quantity) VALUES ('000B', '名古屋', '0006', 10);
-- INSERT INTO ShopProduct (shop_id, shop_name, product_id, quantity) VALUES ('000B', '名古屋', '0007', 40);
-- INSERT INTO ShopProduct (shop_id, shop_name, product_id, quantity) VALUES ('000C', '大阪', '0003', 20);
-- INSERT INTO ShopProduct (shop_id, shop_name, product_id, quantity) VALUES ('000C', '大阪', '0004', 50);
-- INSERT INTO ShopProduct (shop_id, shop_name, product_id, quantity) VALUES ('000C', '大阪', '0006', 90);
-- INSERT INTO ShopProduct (shop_id, shop_name, product_id, quantity) VALUES ('000C', '大阪', '0007', 70);
-- INSERT INTO ShopProduct (shop_id, shop_name, product_id, quantity) VALUES ('000D', '福冈', '0001', 100);
-- COMMIT;

select product_name, sale_price
  from Product
  where product_id in 
    (select product_id from ShopProduct where shop_id = '000C');

select product_name, sale_price
  from Product
  where product_id not in
    (select product_id from ShopProduct where shop_id = '000A');
```
**子查询展开后的结果**
```SQL
SELECT product_name, sale_price
  FROM Product
  WHERE product_id IN ('0003', '0004', '0006', '0007');
```

#### **_EXIST_**
```SQL
select product_name, sale_price
  from Product as P
  where exists
    (select * from ShopProduct as SP where shop_id = '000C' and P.product_id = SP.product_id);

select product_name, sale_price
  from Product as P
  where not exists (select * from ShopProduct as SP where shop_id = '000C' and P.product_id = SP.product_id);
```
> EXIST 通常都会使用关联子查询作为参数。  
> 理解 WHERE EXISTS (子查询):  
>   对于当前外层这一行（Product），子查询有没有返回至少一行？  
>   EXISTS是一个断言(TRUE/FALSE)。  
> 选出 商店 000C 的 **product_ids** 之中的（以内的）。  

#### CASE表达式
```SQL
select product_name, case
  when product_type = '衣服' then 'A: ' || product_type
  when product_type = '办公用品' then 'B: ' || product_type
  when product_type = '厨房用具' then 'C: ' || product_type
  else null
  end as abc_product_type
  from Product;

-- product_name | abc_product_type
-- ---------------+------------------
-- T恤衫         | A ：衣服
-- 打孔器        | B ：办公用品
-- 运动T恤       | A ：衣服

select
  sum(case when product_type = '衣服' then sale_price else 0 end) as sum_price_clothes,
  sum(case when product_type = '厨房用具' then sale_price else 0 end) as sum_price_kitchen,
  sum(case when product_type = '办公用品' then sale_price else 0 end) as sum_price_office
  from Product;

-- sum_price_clothes | sum_price_kitchen | sum_price_office
-- ------------------+-------------------+-----------------
--  5000             | 11180             | 600
```

#### 简单CASE表达式
```SQL
SELECT product_name,
  CASE product_type
    WHEN '衣服' THEN 'A ：' || product_type
    WHEN '办公用品' THEN 'B ：' || product_type
    WHEN '厨房用具' THEN 'C ：' || product_type
    ELSE NULL
  END AS abc_product_type
  FROM Product;
```


