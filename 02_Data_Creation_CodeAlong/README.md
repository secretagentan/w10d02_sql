## Intro to SQL Schema's and Inserting Data

---

### GOALS
- Create a database table
- Insert data
- Retrieve data
- Update data
- Delete data


## Connect, Create a Database

Let's make a database!  First, make sure you have PostgreSQL running.  Once you do, open your terminal and type:

```bash
postgres -D /usr/local/var/postgres
```

``` bash
$ psql
```

You should see something like:

``` bash
your_user_name=#
```

Great!  You've entered the PostgreSQL equivalent of IRB: now, you can execute PSQL commands, or PostgreSQL's version of SQL.

Let's use these commands, but before we can, we must create a database.  Let's call it wdi:

``` psql
your_user_name=# CREATE DATABASE wdi;
CREATE DATABASE
```

The semicolon is important! Be sure to always end your SQL queries and commands with semicolons.

Now let's _use_ that database we just created:

``` psql
your_user_name=# \c wdi
You are now connected to database "wdi" as user "your_user_name".
wdi=#
```

## Insert and Query data

Now that we have a database, let's create a table (think of this like, "hey now that we have a workbook/worksheet, let's block off these cells with a border and labels to show it's a unique set of values"):



``` psql
wdi=# CREATE TABLE instructors (
wdi(#  ID          INT   PRIMARY KEY   NOT NULL,
wdi(#  NAME        TEXT                NOT NULL,
wdi(#  WEBSITE     CHAR(50);
CREATE TABLE
```

Notice the different parts of these commands:

``` psql
wdi=# CREATE TABLE instructors (
```

This starts our table creation, it tells PostgreSQL to create a table named "instructors"...

``` psql
wdi(#  ID        INT   PRIMARY KEY   NOT NULL,
wdi(#  NAME      TEXT                NOT NULL,
```

...then, each line after denotes a new column we're going to create for this table, what the column will be called, the data type, whether it's a primary key, and whether the database - when data is added - can allow data without missing values.  In this case, we're not allowing NAME, AGE, or ID to be blank; but we're ok with WEBSITE being blank.

## Create a student table and insert data - Codealong

Now that we've done it to keep track of our instructors, let's create a table for students that collects information about:

- an id that cannot be left blank
- the students name that cannot be left blank
- their age
- and their address that cannot be left blank.

Here's what that query should have looked like:

``` psql
wdi=# CREATE TABLE students (
wdi(#  ID          INT   PRIMARY KEY   NOT NULL,
wdi(#  NAME        TEXT                NOT NULL,
wdi(#  AGE         INT                 NOT NULL,
wdi(#  ADDRESS     CHAR(50)
wdi(#  );
CREATE TABLE
```

Great job! Now let's finally _insert_ some data into that table - remember what cannot be left blank!

We'll insert five students, Jack, Jill, John, Jackie, and Slagathorn. The syntax is as follows:

``` psql
INSERT INTO TABLE_NAME VALUES (value1,value2,value3,...);
```

Let's do it for Jack, together:

``` psql
wdi=# INSERT INTO students VALUES (1, 'Jack Sparrow', 28, '50 Main St, New York, NY');
INSERT 0 1
```

## Insert Data - Independent Practice (10 mins)

Now, you try it for the other students, and pay attention to the order of Jack's parameters and the single quotes - they both matter.

- Jill's full name is Jilly Cakes; she's 30 years old, and lives at 123 Webdev Dr. Boston, MA
- Johns's full name is Johnny Bananas; hes 25 years old, and lives at 555 Five St, Fivetowns, NY
- Jackie's full name is Jackie Lackie; she's 101 years old, and lives at 2 OldForThis Ct, Fivetowns, NY
- Slagathorn's full name is Slaggy McRaggy; he's 28 and prefers not to list his address

You should come up with:

``` psql
wdi=# INSERT INTO students VALUES (2, 'Jilly Cakes', 30, '123 Webdev Dr. Boston, MA');
INSERT 0 1
wdi=# INSERT INTO students VALUES (3, 'Johnny Bananas', 25, '555 Five St, Fivetowns, NY');
INSERT 0 1
wdi=# INSERT INTO students VALUES (4, 'Jackie Lackie', 101, '2 OldForThis Ct, Fivetowns, NY');
INSERT 0 1
wdi=# INSERT INTO students VALUES (5, 'Slaggy McRaggy', 28);
INSERT 0 1
```

## What's in our database? Code Along

So now that we have this data saved, we're going to need to access it at some point, right?  We're going to want to _select_ particular datapoints in our dataset provided certain conditions.  The PostgreSQL SELECT statement is used to fetch the data from a database table which returns data in the form of result table. These result tables are called result-sets. The syntax is just what you would have guessed:

``` psql
SELECT column1, column2, columnN FROM table_name;
```

We can pass in what columns we want to look - like above - at or even get all our table records:

``` psql
SELECT * FROM table_name;
```

For us, we can get all the records back:

``` psql
wdi=# SELECT * FROM students;
 id |      name      | age |                      address
----+----------------+-----+----------------------------------------------------
  1 | Jack Sparrow   |  28 | 50 Main St, New York, NY
  2 | Jilly Cakes    |  30 | 123 Webdev Dr. Boston, MA
  3 | Johnny Bananas |  25 | 555 Five St, Fivetowns, NY
  4 | Jackie Lackie  | 101 | 2 OldForThis Ct, Fivetowns, NY
  5 | Slaggy McRaggy |  28 |
(5 rows)
```

We can get just the name and ages of our students:

``` psql
wdi=# SELECT name, age FROM students;
      name      | age
----------------+-----
 Jack Sparrow   |  28
 Jilly Cakes    |  30
 Johnny Bananas |  25
 Jackie Lackie  | 101
 Slaggy McRaggy |  28
(5 rows)
```

#### Getting more specific

Just like Ruby or JavaScript, all of our comparison and boolean operators can do work for us to select more specific data.

- I want the names of all the students less than a certain age:

``` psql
wdi=# SELECT name FROM students WHERE age < 100;
      name
----------------
 Jack Sparrow
 Jilly Cakes
 Johnny Bananas
 Slaggy McRaggy
(4 rows)
```

- How about the names of students orderedby age? Done:

``` psql
wdi=# SELECT name, age FROM students ORDER BY age;
      name      | age
----------------+-----
 Johnny Bananas |  25
 Jack Sparrow   |  28
 Slaggy McRaggy |  28
 Jilly Cakes    |  30
 Jackie Lackie  | 101
(5 rows)
```

- How about reversed? Ok:

``` psql
wdi=# SELECT name, age FROM students ORDER BY age DESC;
      name      | age
----------------+-----
 Jackie Lackie  | 101
 Jilly Cakes    |  30
 Jack Sparrow   |  28
 Slaggy McRaggy |  28
 Johnny Bananas |  25
(5 rows)
```

- How about those who live in Fivetowns? We can find strings within strings too!

``` psql
wdi=# SELECT * FROM students WHERE address LIKE '%Fivetowns%';
 id |      name      | age |                      address
----+----------------+-----+----------------------------------------------------
  3 | Johnny Bananas |  25 | 555 Five St, Fivetowns, NY
  4 | Jackie Lackie  | 101 | 2 OldForThis Ct, Fivetowns, NY
(2 rows)
```


## Updates to our database - Codealong

Ok, there are some mistakes we've made to our database, but that's cool, cause we can totally update it or delete information we don't like. Let's start by adding one more student:

``` psql
wdi=# INSERT INTO students VALUES (6, 'Miss Take', 500, 'asdfasdfasdf');
INSERT 0 1
```

But oh no, we messed them up - Miss Take doesn't live at asdfasdfasdf, she lives at 100 Main St., New York, NY.  Let's fix it:

``` psql
wdi=# UPDATE students SET address = '100 Main St., New York, NY' WHERE address = 'asdfasdfasdf';
UPDATE 1

wdi=# SELECT * FROM students;
 id |      name      | age |                      address
----+----------------+-----+----------------------------------------------------
  1 | Jack Sparrow   |  28 | 50 Main St, New York, NY
  2 | Jilly Cakes    |  30 | 123 Webdev Dr. Boston, MA
  3 | Johnny Bananas |  25 | 555 Five St, Fivetowns, NY
  4 | Jackie Lackie  | 101 | 2 OldForThis Ct, Fivetowns, NY
  5 | Slaggy McRaggy |  28 |
  6 | Miss Take      | 500 | 100 Main St., New York, NY
(6 rows)
```

But wait, actually, she just cancelled - no big!

``` psql
wdi=# DELETE FROM students where name = 'Miss Take';
DELETE 1

wdi=# SELECT * FROM students;
 id |      name      | age |                      address
----+----------------+-----+----------------------------------------------------
  1 | Jack Sparrow   |  28 | 50 Main St, New York, NY
  2 | Jilly Cakes    |  30 | 123 Webdev Dr. Boston, MA
  3 | Johnny Bananas |  25 | 555 Five St, Fivetowns, NY
  4 | Jackie Lackie  | 101 | 2 OldForThis Ct, Fivetowns, NY
  5 | Slaggy McRaggy |  28 |
(5 rows)

```

## Independent Practice - 20 mins

There's _no way_ you're going to remember the exact syntax of everything we just did, but let's practice a habit you should have been doing since week 1: finding and reading documentation. Checkout [this PostgreSQL tutorial](http://www.tutorialspoint.com/postgresql/) and using the same database and datatable of users, get through a many of these SQL challenges as possible in the next 10 minutes:

- Insert five more students:
  - Nancy Gong is 40 and lives at 200 Horton Ave., Lynbrook, NY
  - Laura Rossi is 70 and listed her address as "Unlisted"
  - David Daniele is 28 and lives at 300 Dannington Ln., Washington, DC.
  - Greg Fitzgerald is 25 and did not list an address
  - Randi Fitz is 28 and lives in Oceanside, NY
- Randi wants her address to be corrected to 25 Broadway, New York, NY
- Get a list of student names and addresses who are older than 30 and order them by their address alphabetically
- Get a list of students whose first name begins with the letter "J"
- Get a list of student names who live in NY or MA

