# SQL Optimization Tricks Part 2

### 1. Get advice from PROCEDURE ANALYSE()

```SQL
SELECT … FROM … WHERE … PROCEDURE ANALYSE([max_elements,[max_memory]])

// Example
SELECT col1, col2 FROM table1 PROCEDURE ANALYSE(10, 2000);
```

### 2. Always set an ID for each table

**Counter-example**
```SQL
CREATE TABLE subs (
  email varchar(20) NOT NULL,
  name varchar(20)
);
```
**Positive example**
```SQL
CREATE TABLE subs (
  id int(5) NOT NULL AUTO_INCREMENT,
  email varchar(20) NOT NULL,
  name varchar(20)
);
```
### 3. Use ENUM instead of VARCHAR

**Counter-example**
```SQL
CREATE TABLE Persons (
 PersonID int,
 Status varchar(25)
);
```

**Positive example**
```SQL
CREATE TABLE Persons (
 PersonID int,
 Status enum('Married', 'Single') NOT NULL
);
```

### 4. Optimize your query by caching

```
sudo nano /etc/mysql/my.cnf
```

```
/etc/mysql/my.cnf
...
[mysqld]
query_cache_type=1
query_cache_size = 10M
query_cache_limit=256K
```

### 6. Make use of Prepared Statements

```
* PREPARE – prepare a statement for execution.
* EXECUTE – execute a prepared statement prepared by the PREPARE statement.
* DEALLOCATE PREPARE – release a prepared statement.
```

```SQL
1. Prepare
PREPARE item1 FROM 
  'SELECT itemcode, itemname 
  FROM items 
  WHERE itemcode = ?';
  
// ic stands for itemcode
SET @ic = 'i012';

2. Execute
EXECUTE item1 USING @pc;

3. Deallocate
DEALLOCATE PREPARE item1;
```

### 7. Use the alias of the table and prefix the alias on each column, so that the semantics are more clear

**Counter-example**
```SQL
select * from A 
  inner join B 
  on A.deptId = B.deptId;
```

**Positive example**
```SQL
select memeber.name, deptment.deptName from A member 
 inner join B deptment 
 on member.deptId = deptment.deptId;
```

### 8. If the field type is a string, it must be enclosed in quotation marks

**Counter-example**
```SQL
select * from user where userid =123;
```

**Positive example**
```SQL
select * from user where userid = ‘123’ ;
```

### 9. When using a joint index, pay attention to the order of the index columns, generally following the left-most matching principle

**Counter-example**
```SQL
select * from user where age = 10;
```

**Positive example**
```SQL
//Complies with the left-most matching principle
select * from user where userid=10 and age =10;

//Complies with the left-most matching principle
select * from user where userid =10;
```


### 10. Inner join is preferred still if the left join is used, the result of the left table is as small as possible

**Counter-example**
```SQL
select * from 
  table1 t1 left join table2 t2 
  on t1.size = t2.size 
  where t1.id>2;
```

**Positive example**
```SQL
select * from 
  (select * from table1 where id >2) 
  t1 left join table2 t2 
  on t1.size = t2.size;
```

### 12. Save the IP address as UNSIGNED INT

**Counter-example**
```SQL
CREATE TABLE classes ( 
 id INT AUTO_INCREMENT, 
 ipadd VARCHAR(15) NOT NULL
);
```

**Positive example**
```SQL
CREATE TABLE classes ( 
 id INT AUTO_INCREMENT, 
 ipadd INT(4) UNSIGNED NOT NULL
);
```
