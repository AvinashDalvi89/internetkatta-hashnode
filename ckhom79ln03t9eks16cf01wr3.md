## Create SQL from CSV files for larger updation or insertion

Few days back I came across one problem where I have to create a bulk update script based on a given CSV or Excel file. First I thought of finding something in excel level by writing some macros or functions. 

Then my friend helped me to use the easiest method using a sublime text editor. Would like to know about this approach ? 

As I said above it's simpler to create and update scripts. Let's take an example I have below CSV file content. 

```
mumbai,avinashdalvi@test.com
thane,avinash@malinator.com
bangalore,test@mailinator.com

``` 
Follow below steps : 

1.  Open CSV file in sublime text editor. 
2. Open Replace toolbar from Menu -> Find -> Replace ( You can directly open by short command **Option+Command+F** for Mac user, **Control+Shift+F** for Window or ubuntu user.
3.  Enable regex option in toolbar ( It will look like **.***). You can see 👇🏻 below image.
4.  Considering two fields in CSV, write `(.*),(.*)` in the Find field on the toolbar.  
![Find Toolbar Image](https://cdn.hashnode.com/res/hashnode/image/upload/v1605776179521/MvLW1Rzv6.png)
5. `update table tablename set fieldname1 = '$1' where fieldname = '$2'` in Replace field. `$1` stand for first field and `$2` stand for second first. Vice versa if you have multiple fields you can mention `$3` and so on.
6. Try to click on the find button and check whether it is highlighting properly or not.  
7. Then you click on "Replace All"  and see this output. 


**Output** : 
```
Update table tablename set fieldname1 = 'mumbai' where fieldname = 'avinashdalvi@test.com';
Update table tablename set fieldname1 = 'thane' where fieldname = 'avinash@malinator.com';
Update table tablename set fieldname1 = 'bangalore' where fieldname = 'test@mailinator.com';
```


**Note** : You can write any other SQL like insert or delete to create a SQL script. This is a little hack which can help to save time while creating a script when you are dealing with more numbers of records.

Hope this helps you. Thanks for reading this blog. If any feedback feel free to reach out to me.