## MySQL cheatsheet for search operation

In this blog I am going to explain three different problems while working with MySql data operation. 

These three problems can help you to solve issues if came across same situation. I called this cheatsheet because this is something that has magic which helps do it in an easy way without knowing the bigger picture.

![tenor (1).gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1608996774364/LgBNgZnTn.gif)
Let's get started with questions : 

1. Search and Replace column name in table : 
```
UPDATE
   table 
SET
   column_name = REPLACE(column_name, 'value2', 'value1') 
WHERE
   column_name like '%value1%';
```
2. Finding column names across all tables when you are known to the skeleton and you have to find out which table belongs to this column. This is like a bottom to top approach by using wild card search `*`. what is bottom to top approach will explain the end of this article for new readers. 
```
SELECT
   COLUMN_NAME AS 'ColumnName',
   TABLE_NAME AS 'TableName' 
FROM
   INFORMATION_SCHEMA.COLUMNS 
WHERE
   COLUMN_NAME LIKE '%MyName%' 
ORDER BY
   TableName,
   ColumnName;
```
3. How to find duplicate records based on keys or column groups in the same table ? 

Let's take example tables for reference. 

**Table 1** : 
![Table1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1608994626956/hl2_8wNbA.png)
```
select * from
(select callTime,callType,employee,phoneNumber,count(*) as n from table2 group by callTime,callType,employee,phoneNumber) x
where x.n > 1;
```

These are three questions till now came across. I will come with more hacks whenever I will get my next questions. As I promise above let me explain. 

**What is the bottom to top approach ? **

When you are new to any application or projects, sometimes it is difficult to find exact things. This kind of case generally came in the developer's timespan. How to tackle this and get the right information. Let's first understand what the **Top to bottom** approach is when you know where to start project exploration like documentation, tutorials and application entry points etc. 

Now exactly reverse to this is the **Bottom to top** approach. Start from wild card search by finding known words or known keys inside the project and find occurrences. Once you get this can go from the file which last execution does go from it top level by find function reference or definition like wise.   In the above example I explain the same approach to find exact question answers by using the search operation method in MySQL. There you are!

![tenor.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1608996457967/r6OUyxhLc.gif)

Hope you like my blog. Thanks for reading my blog. If you have any questions you can reach out to me at my twitter handle - @aviboy2006