## Restore InnoDB MySql from one AWS EC2 to another EC2

I am sharing this experience and recovery option because i faced this issue while one IT person destroy EC2 instance but luckily he forgot to delete volume used for EC2. I am able to restore required MySql Databases. 

Here are the steps for restoring an InnoDB MySQL Database on Ubuntu 12.04:

1. Shut down the mysql service (sudo service mysql stop)
2. Copy the database folder that contains the `.frm` files to `/var/lib/mysql/[DBNAME]`.  As far as I know, this folder must be the same name as the old database.
3. Copy the `ibdata1` file from your old server to `/var/lib/mysql/ibdata1`
4. ***THIS IS IMPORTANT.    Make sure the permissions of the files are correct.I simply did `sudo chmod -R 777 /var/lib/mysql`.  
Note : This is not recommended if your server is a production server.
5. Start the mysql service (`sudo service mysql start`)