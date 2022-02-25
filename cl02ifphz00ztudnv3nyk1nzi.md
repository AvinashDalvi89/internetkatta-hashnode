## Import large MySQL DB into AWS RDS

Hello Devs,

I am going to share tips to import large MySQL DBs into AWS RDS.

### Why am I writing this ?

I took on one project to help my friend to migrate a site from Google cloud to AWS. As part of this activity moving code and setting up server configuration was easy and smooth. Main challenge was moving DB to AWS RDS. You might be thinking why ? Reason is DB was very heavy like a single table having 1 million records. I tried to import it by using a general method like `mysql - u username - p -h hostname -A database < database.sql ` I tried this almost 10-15 times every time the server connection pipe broke. Tried to increase TTL values also but it doesn't help me to solve this issue.

While searching on google I came across [nice explanation video](https://www.youtube.com/watch?v=ecRT_GPQPGE) and a new method which was new learning for me.  It was a simple command `source path of file`.

Curious to know ? What should I do ?

Let me explain the option to import a heavy database. First understand what the `source` command is.

As the name says `source` it is similar to running mysql commands only. But this can be used to run chunks of mysql commands in batch mode.

Normal MySQL command will run like this


```
mysql > select * from tables;
mysql > insert into column tables values;
```
similar way but what you can do is dump the above command into one file like `dbquery.sql` and keep it one of the destinations.

```
ssh> nano dbquery.sql
ssh> ls
ssh> dbquery.sql
ssh> pwd
ssh> /home/ubuntu
```
Path for file will be `/home/ubuntu/dbquery.sql`

Then login to MySQL into the terminal.

```
ssh> mysql -u username -p password
mysql> use database;
mysql> source /home/ubuntu/dbquery.sql;
Query OK, 11513 rows affected (0.56 sec)
Records: 11513  Duplicates: 0  Warnings: 0

Query OK, 11464 rows affected (0.80 sec)
Records: 11464  Duplicates: 0  Warnings: 0

Query OK, 11531 rows affected (0.56 sec)
Records: 11531  Duplicates: 0  Warnings: 0

Query OK, 11352 rows affected (0.56 sec)
Records: 11352  Duplicates: 0  Warnings: 0

Query OK, 11714 rows affected (0.66 sec)
Records: 11714  Duplicates: 0  Warnings: 0

Query OK, 11581 rows affected (0.77 sec)
Records: 11581  Duplicates: 0  Warnings: 0

Query OK, 11608 rows affected (0.56 sec)
Records: 11608  Duplicates: 0  Warnings: 0

Query OK, 11230 rows affected (0.57 sec)
Records: 11230  Duplicates: 0  Warnings: 0

Query OK, 11701 rows affected (5.36 sec)
Records: 11701  Duplicates: 0  Warnings: 0

Query OK, 11436 rows affected (2.86 sec)
Records: 11436  Duplicates: 0  Warnings: 0
```
This will start dumping into the database show record till now.

Hope it will solve your issue if you come across a similar problem. Feel free to comment if you have any issues.
If you like my blog please don't forget to like the article. It will encourage me to write more helpful articles. You can reach out to me over my twitter handle @aviboy2006
