---
layout: wiki
title: SQL
categories: SQL常用命令以及语法提示
description: 常用命令和语法备份
keywords: SQL，mysql，SQL Server
---

## 查看数据库版本

提示：个人认为此命令可间接判断当前数据库采用哪种类型的

类型  | 命令
--|--
sql server| Select @@Version
mysql | select version()
oralce |select * from v$version
MySQL 数据库常用命令

## MySQL常用命令

  命令|功能  
--|--
create database name|创建数据库
use databasename|选择数据库
drop database name| 直接删除数据库，不提醒
show tables| 显示表
desc tablename|表的数据类型详细描述
select distinct|去除重复字段
mysqladmin drop databasename |删除数据库前，有提示

## SQL语法格式提示
![SQL](/images/blog/2018-03-11_0.png)
