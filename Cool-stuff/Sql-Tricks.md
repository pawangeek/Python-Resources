# SQL Optimization Tricks

### 1. Try not to use select * to query SQL, but select specific fields.
**Counter-example**
```SQL
select * from employee;
```
**Positive example**
```SQL
select id，name from employee;
```
### 2. If you know that there is only one query result, it is recommended to use limit 1
**Counter-example**
```SQL
select id，name from employee where name='jay'
```
**Positive example**
```SQL
select id，name from employee where name='jay' limit 1;
```
### 3. Try to avoid using or in the where clause to join conditions
**Counter-example**
```SQL
select * from user where userid = 1 or age = 18
```
**Positive example**
```SQL
//Use union all
select * from user where userid=1
union all
select * from user where age = 18

//Or write two separate SQL
select * from user where userid=1
select * from user where age = 18
```
### 4. Optimize limit paging
**Counter-example**
```SQL
select id，name，age from employee limit 10000，10;
```
**Positive example**
```SQL
//Solution 1: Return the largest record (offset) of the last query
select id，name from employee where id>10000 limit 10.

//Solution 2: order by + index
select id，name from employee order by id  limit 10000，10
```
### 5. Optimize your like statement
**Counter-example**
```SQL
select userId，name from user where userId like '%123';
```
**Positive example**
```SQL
select userId，name from user where userId like '123%';
```
### 6. Use where conditions to limit the data to be queried to avoid returning extra rows
**Counter-example**
```SQL
List<Long> userIds = sqlMap.queryList("select userId from user where isVip=1");
boolean isVip = userIds.contains(userId);
```
**Positive example**
```SQL
Long userId = sqlMap.queryObject("select userId from user where userId='userId' and isVip='1' ")
boolean isVip = userId！=null;
```
### 7. You should avoid using the != or <> operator in the where clause as much as possible, otherwise the engine will give up using the index and perform a full table scan
**Counter-example**
```SQL
select age,name  from user where age <>18;
```
**Positive example**
```SQL
//You can consider separate two sql write

select age,name  from user where age <18;
select age,name  from user where age >18;
```
### 8. If you insert too much data, consider bulk insertion
**Counter-example**
```SQL
for(User u :list){
 INSERT into user(name,age) values(#name#,#age#)   
}
```
**Positive example**
```SQL
//One batch of 500 inserts, carried out in batches

insert into user(name,age) values
<foreach collection="list" item="item" index="index" separator=",">
    (#{item.name},#{item.age})
</foreach>
```
### 9. Use the distinct keyword with caution
**Counter-example**
```SQL
SELECT DISTINCT * from  user;
```
**Positive example**
```SQL
select DISTINCT name from user;
```
### 10. Remove redundant and duplicate indexes
**Counter-example**
```SQL
KEY `idx_userId` (`userId`)  
KEY `idx_userId_age` (`userId`,`age`)
```
**Positive example**
```SQL
//Delete the userId index, because the combined index (A, B)
// is equivalent to creating the (A) and (A, B) indexes

KEY `idx_userId_age` (`userId`,`age`)
```
### 11. If the amount of data is large, optimize your modify/delete statement
**Counter-example**
```SQL
//Delete 100,000 or 1 million+ at a time?
delete from user where id <100000;

//Or use single cycle operation, low efficiency and long time
for（User user：list）{
   delete from user； }
```
**Positive example**
```SQL
//Delete in batches, such as 500 each time

delete user where id<500
delete product where id>=500 and id<1000；
```
### 12. Consider using default values ​​instead of null in the where clause
**Counter-example**
```SQL
select * from user where age is not null;
```
**Positive example**
```SQL
select * from user where age>0; //Set 0 as default
```
### 13. Try to replace union with union all
**Counter-example**
```SQL
select * from user where userid=1
union  
select * from user where age = 10
```
**Positive example**
```SQL
select * from user where userid=1
union all  
select * from user where age = 10
```
### 14. Use numeric fields as much as possible. If the fields only contain numeric information, try not to design them as a character type.
**Counter-example**
```SQL
`king_id` varchar（20） NOT NULL
```
**Positive example**
```SQL
`king_id` int(11) NOT NULL;
```
### 15. Use varchar/nvarchar instead of char/nchar whenever possible
**Counter-example**
```SQL
`deptName` char(100) DEFAULT NULL;
```
**Positive example**
```SQL
`deptName` varchar(100) DEFAULT NULL;
```
### 16. Use explain to analyze your SQL plan
```SQL
explain select * from user where userid = 10086 or age =18;
```
