## Intro to Relational Data Stores

---

### Objectives

- Know what SQL is and what it does
- Explain different kinds of data allowed to be stored in our database
- Be able to understand a Schema and how it represents data in our application
- Understand Primary and Foreign Keys and table relationships

---

## We know about Databases, but what is SQL?

- Let's review: at it's simplest, a relational database is a mechanism to store and retrieve data in a tabular form.
- Spreadsheets are a good analogy

![excel](https://localizelabs.surge.sh/img/excel.jpg?raw=true)

---

- But how do we interact with our database: inserting data, updating data, retrieving data, and deleting data?
- That's where SQL comes in!

---

#### What is SQL?

- SQL stands for Structured Query Language, and it is a language universally used and adapted to interact with relational databases.
- SQL allows us to store and control data in our application
- With SQL, we can do the following with our data
  * Inserting data
  * Querying or retrieving data
  * Updating or deleting data
  * Creating new tables and entire databases
  * Control permissions of who can have access to our data

---

#### Why is SQL important?

- A database is just a repository to store the data and you need to use systems that dictate how the data will be stored and as a client to interact with the data.
- We call these systems "Database Management Systems", they come in _many_ forms:
  - MySQL
  - SQLite
  - PostgreSQL (what we'll be using!)

---

## Tables of Data

- The foundational idea underneath relational databases is a simple but powerful structure.
- Each table is a set of sets, and within a single table all of these sets have the same data structure, containing a list of named fields and their values.
- For convenience, each set within a table is called a row, and each field within that row is part of a larger named column
- It looks a lot like a spreadsheet with named columns and unnamed rows.

---

![sample schema](http://cdn.oreilly.com/excerpts/9780596518776/figs/web/rail_ab01.png)

---

- Databases offer much less of that kind of flexibility, and in return can offer tremendous power because of their obsession with neatly ordered data.
- Every row within a table has to have the same structure for its data, and calculations generally take place outside of the tables, not within them.
- Tables just contain data.

---

#### A simplified example schema
- A schema is its structure described in a formal language supported by the database syntax (it varies between SQL and PostgreSQL)
- It represents the name of the Table (our object) and the attributes corresponding data type
- The term "schema" refers to the organization of data as a blueprint of how the database is constructed

| Field Name    | Data Type |
| ------------- |:---------:|
| id            | :integer  |
| given_name    | :string   |
| middle_name   | :string   |
| family_name   | :string   |
| date_of_birth | :date     |
| grade_point_average  | :float |
| drivers_license | :boolean  |

---

## Database Data Types

#### SQL only stores certain types of data inside our database
- Depending on the database, we can only declare data to be stored in certain select formats
- Most databases respond to similar types of data, but they may have different names

#### When modeling data in a database, each field name will need a matching data type
- When declaring a field name on our Table, we need to give it a certain data type
- Data types can be grouped together in the following way:
 - boolean
 - character values (strings)
 - integer values
 - floating point values
 - dates/time, datetime

#### Here are the data types we will be using, and some examples to use when thinking of data modeling

#### `string`

- strings are most commonly used. They have a max length of 225 characters and are best used for names, addresses, emails, etc
- when using SQL, use single quote strings `' '` some databases do not recognize double quotes `" "`
- but if you're string contains a string i.e. `'it's a nice day'`
- write it as: `"it's a nice day"` or `'it''s a nice day'`

- In SQL, strings are referred to in the following names:
 - **char**: holds a single character. example: 'a'
 - **char (#)**: holds # number of characters. Spaces will be inserted to fill any extra room.
 - **varchar (#)**: holds a maximum of # number of character. Can contain less characters. We will be using this the most often
  - `color varchar(24)`

#### `text`

- examples: large text files, messaging apps, email
- the text data type is best used for large amounts of text
- text also does not have a max length, is is virtually unlimited
- text should be in single quotes `' '` as well
- `description text`

### Remembering how to escape strings (double vs single quotes)

- when using single vs double quotes remember this
`[S]ingle quote for [S]trings, [D]ouble quote for things in the [D]atabase`

- not all databases support the use of double strings `" "`
- to escape single strings, remember they cancel each other out
` 'it''s been suzie''s turn how''d you forget?' `

---

#### `integer`

- examples: `3` `56` `789` `19876`
- integers are used for whole numbers
- the main thing we will use integers for are `id` values on objects
- when making objects, it is convenient to keep track of them with an ordered __unique__ identifier
 - we can accomplish that by adding the `serial` keyword to our `sql` command which auto populates the number



#### `float`
- examples: `3.45` `0.79` `1234.6543918`
- use if exact precision isnt needed i.e. GPA's, "random" numbers
- ruby's `rand` method and javascript's `Math.random()` function both return floating point integer values


#### `decimal`, `bigint`, `double precision`
- `3.4520928172` `0.628172531`
- similar to float, but they are geared towards exact precision.
- use these if precision is necessary i.e. dealing with currency


#### `datetime`, `timestamp`
- examples: `'2004-10-19 10:23:54'`
- calling an object with a datetime value: `user.created_at # => 2016-09-16 10:04:32`

- the format of `datetime` is YYYY-MM-DD HH:MM:SS
- a `timestamp` is a "method" that uses a return value of datetime
- it is commonly used in capturing precise times of when users and data has taken action

---

#### `null`
- `null` is essentially nothing. It is a false value, like ruby's `nil`
- in SQL, to validate data you can append `NOT NULL` to ensure a value is not saved into the database without it
- this lets us control what get's put into our database so things don't break

---

### Bringing that together
- we will get into creating tables and schemas next, but based on the data types above, if we wanted to make a `user` object for our application, we could model it by setting up a basic table like this

```sql
CREATE TABLE user (
  id serial PRIMARY KEY,
  name varchar (50) NOT NULL,
  nickname varchar (25) NOT NULL,
  description text,
  created_at timestamp
);
```

---

## Primary and Foreign Keys

- when modeling data we use integer values called `id` to keep track of the object number we created.
- in the above example where we create our user table we write it like this: `id serial PRIMARY KEY`
- 3 things are done here
 - we give a `field name` called `id` onto our user table
 - we give a "method" called `serial` which allows it to auto-increment meaning we don't have to manually pass it a value
 - and we define it as a `primary key`

- if we did not want to add the `serial` option, but still wanted to ensure an `id` was made and is unique, we could write it like this
```sql
CREATE TABLE user (
  ID INT NOT NULL,
  NAME VARCHAR (35) NOT NULL,
  PRIMARY KEY (ID)
);
```
---

### Primary keys

 - If you were looking at a giant spreadsheet of data, and someone told you to look for data in a certain row/column, what would be the first thing you would ask for?

- You can think of the primary key as an address.  If the rows in a table were mailboxes, then the primary key would be the listing of street addresses.


#### What Primary Keys Do
- In order for a table to qualify as a relational table it must have a primary key
- The primary key consists of one or more columns whose data contained within is used to uniquely identify each row in the table.

#### Primary Key Qualifications
- In order to be a primary key, several conditions must hold true.
 - First, as mentioned, the columns must be unique.
 - Second, no value in the columns can be blank or NULL.
- When defining a table you specify the primary key. A table has just one primary key, and its definition is mandatory.

#### Where is the Primary Key Stored?
- The primary key for each table is stored in an index.  The index is used to enforce the uniqueness requirement.  It also makes it easy for foreign key values to refer back to corresponding primary key values

#### What is an `index`?
- A database index allows a query to efficiently retrieve data from a database.
- Indexes are related to specific tables and consist of one or more keys.  A table can have more than one index built from it.  The keys are a fancy term for the values we want to look up in the index.

- For this example consider the index in the back of a book:
 - The keys for this index are the subject words we reference.  The index entries consist of the key and page number.
 - The keys are in alphabetical order, which makes really easy for us to scan the index, find an entry, note the pages, and then flip the book to the correct pages.

---

### Foreign Keys
- A foreign key is a set of one or more columns in a table that refers to the primary key in another table.
- There isn’t any special code, configurations, or table definitions you need to place to officially “designate” a foreign key.

### What do Foreign Keys do?
- Foreign keys allow us to find relationships between groups of objects
- A FOREIGN KEY in one table points to a PRIMARY KEY in another table.

### An Example

![foreign key 1](https://localizelabs.surge.sh/img/fkey1.png)

![foreign key 2](https://localizelabs.surge.sh/img/fkey2.png)

- Note that the "P_Id" column in the "Orders" table points to the "P_Id" column in the "Persons" table.
- The "P_Id" column in the "Persons" table is the PRIMARY KEY in the "Persons" table.
- The "P_Id" column in the "Orders" table is a FOREIGN KEY in the "Orders" table.
- The FOREIGN KEY constraint is used to prevent actions that would destroy links between tables.
- The FOREIGN KEY constraint also prevents invalid data from being inserted into the foreign key column, because it has to be one of the values contained in the table it points to.

#### More Examples

![foreign keys](http://www.essentialsql.com/wp-content/uploads/2015/05/Example1.png)

![example schema](http://cdn.oreilly.com/excerpts/9780596518776/figs/web/rail_ab04.png)

#### What about an `index`?
- foreign keys are stored as indexes, as they are commonly used with advanced operations such as grouping and joining objects
- the same rule and concepts for `primary key` indexes related to foreign keys

