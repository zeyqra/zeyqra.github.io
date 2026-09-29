---
title: 'SQL基础教程（第2版）学习笔记'
description: 'Learn SQL'
pubDate: '2026-09-29'
# heroImage: '../../assets/blog-placeholder-3.jpg'
---

## 环境搭建
安装和连接 `PostgreSQL`
```
cd D:\soft\PostgreSQL\18\bin
./psql.exe -U postgres -d
CREATE DATABASE shop;
\q
./psql.exe -U postgres -d shop
``` 

### 概要
> 关系数据库必须以行为单位进行数据读写
> 一个单元格中只能输入一个数据。
> 字符串和日期常数需要使用单引号括起来

## 表的创建
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
```
### 数据类型
`integer` 整数
`char`    定长字符串
`varchar` 可变长字符串
`date `   日期
### 
```SQL
DROP TABLE Product;
alter table Product add column product_name_py varchar(100);
alter table Product drop column product_name_py;
```
### 向Product表中插入数据
```SQL
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
