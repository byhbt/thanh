---
layout: post
title: "MySQL Index"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - mysql
---

### What is Index?

> Indexes are used to find rows with specific column values quickly. Without an index, MySQL must begin with the first row and > then read through the entire table to find the relevant rows.

### What is B-Tree?

B-Tree is a self-balancing search tree.

### When use index?

- WHERE clause quickly.
- When have multiple indexes, MySQL use the index that finds the smallest number of rows.
- Index won't work if incorrect data type, for example, you index email column but when you do the query like that

```sql
select * from users where email=123
```
- (sure, many other benefits)

[https://dev.mysql.com/doc/refman/8.0/en/mysql-indexes.html](https://dev.mysql.com/doc/refman/8.0/en/mysql-indexes.html)

### Which fields should be indexed?
- WHERE
- JOIN
- SEARCH (WHERE last_name LIKE "a%")

### What happen to field when it's indexed?
- The field data will be put on RAM, so its faster to access.
- PRIMARY KEY, UNIQUE, INDEX, FULLTEXT are stored in B-Tree.

### Why we don't index all the columns?

If the row is updated then index need to be updated.
- Indexing makes **reading** *faster* but **writing** **slower!**.
- Data < 1000 rows, index won't help.
- Indexes eat ram, disk space.

### Common pitfalls
- If we use function like YEAR() then the index column won't be used.
Use range of full datetime instead

- Multi index column:
The order of columns that matters, index(BA) != index(AB)
Inequality of operation


### Sample databases

When learning some concept you need a sample databases.

- [https://dev.mysql.com/doc/sakila/en/](https://dev.mysql.com/doc/sakila/en/)
- [https://github.com/byhbt/laravel-database-indexes](https://github.com/byhbt/laravel-database-indexes)


### Reference:
- https://www.youtube.com/watch?v=HubezKbFL7E
- https://enqueuezero.com/sql-index.html
- https://benjaminwhx.com/2018/02/26/MySQL%E7%B4%A2%E5%BC%95%E5%8E%9F%E7%90%86%E4%BB%A5%E5%8F%8A%E6%85%A2%E6%9F%A5%E8%AF%A2%E4%BC%98%E5%8C%96/
