---
title: database-01-storage
date: 2020-09-14 18:39
tags:
	- Database
	- 数据库原理
	- CMU 15-445
---

# Database Storage

DBMS stores a database as one or more files on disk. A file is organized logically as a sequence of tuples (records). Each DB has its specific format for these persistent files. The DBMS usually uses the file system OS provides instead of engineering one from scratch. Each file is also logically partitioned into fixed-length storage units called pages (blocks), which are the units of both storage allocation and data transfer. A page may contain several tuples.

disk-oriented database >> files >> pages(blocks) >> tuples(records)

## Database Pages

A page is a fixed-size block of data. It can contain tuples, meta-data, indexes, log records... A page is to be self-contained, it stores meta-data to interpret its content like the schema of the table it represents. Most databases use page sizes of 4 to 8 kilobytes by default.

#### Heap file organization

unordered collection of pages, tuples are stored in random order

implementations:

- linked list: 
  

two pointers: HEAD of the free page list; HEAD of the data page list

- page directory:
  
  a page in the header of the file, mapping the page IDs to its location

#### Page layout

1. slotted pages

Most common layout scheme is called slotted pages, containing variable-length records, employing a mapping layer, a slot array to contain the offset of a record location in the page. The slot array maps "slots" to the tuples' starting position offsets. The header of the page keeps track of the number of used slots and the offset of the starting location of the last slot used.

2. log-structured

Instead of storing tuples in pages, the DBMS only records the changes happened. The system appends log records to the file of how the database was modified: inserts store the entire tuple; deletes mark the tuple as deleted; updates contain the delta of the attributes that were modified. It's faster to do sequential write than random access read even in modern disk storage. Downside of this organization is when we read a tuple from the database. The DBMS scans the log backwards and "recreates" the tuple to find what it needs. Building indexes allows it to jump to locations in the log.

#### Record layout

A tuple is a sequence of bytes. It's the DBMS to interpret these bytes into attribute types and values. 

header info

#### Type system

**Integers**

​	examples: INTEGER, BIGINT, SMALLINT, TINYINT

**Variable precision numbers**

​	examples: FLOAT, REAL

**Fixed-point precision numbers**

​	examples: NUMBERIC, DECIMAL

**variable-length data** (large value > page size)

​	store the data on a special "overflow" page and have the tuple contain a reference to that page. Or use an external file to store large values and the tuple contains a pointer to that file

##  Storage Model







## Reference

[CMU 15-445](https://15445.courses.cs.cmu.edu/fall2019/)

[Database System Concepts (7th), Chapter-13](https://www.db-book.com/db7/)