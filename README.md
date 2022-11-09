# SQL-Injection-Attack-Seed-Lab
Task 1: Get Familiar with SQL Statements
The objective of this task is to get familiar with SQL commands by playing with the provided database. The
data used by our web application is stored in a MySQL database, which is hosted on our MySQL container.
We have created a database called sqllab users, which contains a table called credential. The table
stores the personal information (e.g. eid, password, salary, ssn, etc.) of every employee. In this task, you
need to play with the database to get familiar with SQL queries.
Please get a shell on the MySQL container (see the container manual for instruction; the manual is linked
to the lab’s website). Then use the mysql client program to interact with the database. The user name is
root and password is dees. 


Task 2: SQL Injection Attack on SELECT Statement
SQL injection is basically a technique through which attackers can execute their own malicious SQL statements generally referred as malicious payload. Through the malicious SQL statements, attackers can steal
information from the victim database; even worse, they may be able to make changes to the database. Our
employee management web application has SQL injection vulnerabilities, which mimic the mistakes frequently made by developers.
We will use the login page from www.seed-server.com for this task. The login page is shown in
Figure 1. It asks users to provide a user name and a password. The web application authenticate users based
on these two pieces of data, so only employees who know their passwords are allowed to log in. Your job,
as an attacker, is to log into the web application without knowing any employee’s credential

Task 2.1: SQL Injection Attack from webpage. Your task is to log into the web application as the
administrator from the login page, so you can see the information of all the employees. We assume that
you do know the administrator’s account name which is admin, but you do not the password. You need to
decide what to type in the Username and Password fields to succeed in the attack.
Task 2.2: SQL Injection Attack from command line. Your task is to repeat Task 2.1, but you need to
do it without using the webpage. You can use command line tools, such as curl, which can send HTTP
requests. One thing that is worth mentioning is that if you want to include multiple parameters in HTTP
requests, you need to put the URL and the parameters between a pair of single quotes; otherwise, the special
characters used to separate parameters (such as &) will be interpreted by the shell program, changing the
meaning of the command. The following example shows how to send an HTTP GET request to our web
application, with two parameters (username and Password) attached:
$ curl ’www.seed-server.com/unsafe_home.php?username=alice&Password=11’
If you need to include special characters in the username or Password fields, you need to encode
them properly, or they can change the meaning of your requests. If you want to include single quote in those
fields, you should use %27 instead; if you want to include white space, you should use %20. In this task,
you do need to handle HTTP encoding while sending requests using curl.
Task 2.3: Append a new SQL statement. In the above two attacks, we can only steal information from
the database; it will be better if we can modify the database using the same vulnerability in the login page.
An idea is to use the SQL injection attack to turn one SQL statement into two, with the second one being
the update or delete statement. In SQL, semicolon (;) is used to separate two SQL statements. Please try to
run two SQL statements via the login page. 

Task 3: SQL Injection Attack on UPDATE Statement
If a SQL injection vulnerability happens to an UPDATE statement, the damage will be more severe, because attackers can use the vulnerability to modify databases. In our Employee Management application,
there is an Edit Profile page (Figure 2) that allows employees to update their profile information, including
nickname, email, address, phone number, and password. To go to this page, employees need to log in first. 

Task 3.2: Modify other people’ salary. After increasing your own salary, you decide to punish your boss
Boby. You want to reduce his salary to 1 dollar. Please demonstrate how you can achieve that.
Task 3.3: Modify other people’ password. After changing Boby’s salary, you are still disgruntled, so
you want to change Boby’s password to something that you know, and then you can log into his account and
do further damage. Please demonstrate how you can achieve that. You need to demonstrate that you can
successfully log into Boby’s account using the new password. One thing worth mentioning here is that the
database stores the hash value of passwords instead of the plaintext password string. You can again look at
the unsafe edit backend.php code to see how password is being stored. It uses SHA1 hash function
to generate the hash value of password.
3.4 Task 4: Countermeasure — Prepared Statement
The fundamental problem of the SQL injection vulnerability is the failure to separate code from data. When
constructing a SQL statement, the program (e.g. PHP program) knows which part is data and which part
is code. Unfortunately, when the SQL statement is sent to the database, the boundary has disappeared; the
boundaries that the SQL interpreter sees may be different from the original boundaries that was set by the
developers. To solve this problem, it is important to ensure that the view of the boundaries are consistent in
the server-side code and in the database. The most secure way is to use prepared statement. Task. In this task, we will use the prepared statement mechanism to fix the SQL injection vulnerabilities.
For the sake of simplicity, we created a simplified program inside the defense folder. We will make
changes to the files in this folder. If you point your browser to the following URL, you will see a page
similar to the login page of the web application. This page allows you to query an employee’s information,
but you need to provide the correct user name and password.
