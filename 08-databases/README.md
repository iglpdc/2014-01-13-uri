Databases
---------

In this section, we will briefly cover databases, focusing on 1. why they exist,
2. when to use them, and 3. available database types.

Fundamentally, a database is a methodology of storing data. Believe it or not,
you have already worked with a database - your filesystem.

##What is a file system?

I'm assuming you're on a computer. Go to your desktop right now. Are there files?
Are there folders? A file system is that. The
[wiki explanation](http://en.wikipedia.org/wiki/File_system) is a tad more complicated.
Don't worry about reading it right now, but definitely go back and check it out
at some point.

As you use a computer every day, you probably already knew all about file systems.
What you probably haven't thought about is the fact that organizing data into files
and folders was a conscious choice made by operating-system developers many years
ago. A logical extension of this is that organizing data into a file system was
not the only option available to developers. It was chosen over other options
because it handled common-case usage well.

##Why should I care?

In a word: scalability. It turns out that creating millions of files is not considered
"common-case usage". In fact, file systems explicitly sacrificed this ability for
other, more generally useful, features. However, alternative methods of storing
data lived on. In fact, they did much more than live on. They have grown exponentially
over the years, working alongside or independent of file systems in order to handle
cases where a lot of data needs to be stored.

Think of some services you use online. Amazon, Gmail, Facebook, and many others
all have the problem of handling large amounts of data. They cannot store every
product, message, and profile as a file. For the reasons listed above, this would
fail very quickly.

##What's the solution?

[Databases](http://en.wikipedia.org/wiki/Database). In colloquial use, this is a
blanket term given to approaches that aren't using a file system, and is a very
large field in of itself. There are multiple types of databases, each optimized
for a different type of data.

If you deal with large amounts of data on a daily basis, you should take the time
to thoroughly understand the pros and cons of all available database options
before making a final decision on the database type to use. With that said,
here's a rule of thumb:

 * If your data would make sense as a large Excel spreadsheet (ie. it's *homogenous*),
   then you should consider a [relational databases](https://en.wikipedia.org/wiki/Relational_database)
   like mySQL or SQLite.
 * If you can't (ie. it's *heterogeneous*), then a non-relational [document databases](https://en.wikipedia.org/wiki/Document-oriented_database)
   such as mongoDB or rethinkDB may be better.

Note that there are many other considerations to take into account. For example,
if you are working with graphs (such as social network data), then you'll want
to use a [graph database](http://en.wikipedia.org/wiki/Graph_database) such
as neo4j. As previously stated, taking the time to understand and use databases
properly will help you immensely.

If you need more convincing, feel free to google around. Here is a question on
[stack exchange](http://programmers.stackexchange.com/questions/190482/why-use-a-database-instead-of-just-saving-your-data-to-disk),
which was extended into a full [ars technica](http://arstechnica.com/information-technology/2013/05/why-use-a-database-instead-of-just-saving-your-data-to-disk/)
article. There are also a host of videos and articles across the internet touting
the advantages of various database schema.

##SQL and SQLite

SQL, or **S**tructured **Q**uery **L**anguage, is the classic interface to relational
databases. It's well tested and established, and has many features ensuring speed
and stability in common use cases (e.g. ACID compliance).

For the rest of this tutorial, we will use a smaller-scale relational database
called SQLite. It's easy to set up and provides most of the benefits of more
complicated databases, especially in cases where we're analyzing static datasets.

In order to use SQLite in R, we will need to install both SQLite and its R bindings.
With an internet connection, we can do this straight from R.

```
install.packages("RSQLite")
```

Now, whenever we want to use SQLite in R, we simply have to import it with

```
library("RSQLite")
```

To initialize a database, we create a variable in our workspace.

```
db <- dbConnect(SQLite(), dbname="Test.sqlite")
```

Here, we start a database called "Test.sqlite". If this database already exists,
it gets loaded into the workspace. If it doesn't exist, it gets created. To
read and write data from the database, we must use SQL (remember that it's a
language!) to send "queries". Let's create a table (think of it like a spreadsheet
or a data-frame) and add some values [source](http://sandymuspratt.blogspot.com/2012/11/r-and-sqlite-part-1.html).

```
# Create a table
dbSendQuery(conn = db, "CREATE TABLE School (SchID INTEGER, Location TEXT, Authority TEXT, SchSize TEXT)")
# Add some sample data to that table
dbSendQuery(conn = db, "INSERT INTO School VALUES (1, 'urban', 'state', 'medium')")
dbSendQuery(conn = db, "INSERT INTO School VALUES (2, 'urban', 'independent', 'large')")
dbSendQuery(conn = db, "INSERT INTO School VALUES (3, 'rural', 'state', 'small')")
```

We can view the data we added.

```
dbListTables(db)              # The tables in the database
dbListFields(db, "School")    # The columns in a table
dbReadTable(db, "School")     # The data in a table
```

and we can delete it.

```
dbRemoveTable(db, "School")     # Remove the School table
```

More generally, you'll want to load larger amounts of data. This is best accomplished
by importing csv files, which is a basic spreadsheet format with **c**omma-**s**eparated **v**alues.

```
dbWriteTable(conn = db, name = "Student", value = "student.csv", row.names = FALSE, header = TRUE)
dbWriteTable(conn = db, name = "Class", value = "class.csv", row.names = FALSE, header = TRUE)
dbWriteTable(conn = db, name = "School", value = "school.csv", row.names = FALSE, header = TRUE)
```

We also read data with SQL queries, namely with "SELECT".

```
dbSendQuery(conn = db, "SELECT * FROM School")
```

In this example, the primary advantage comes when the size of your dataset is
larger than your computer's available memory. Rather than freezing your computer
(as would happen with that size of data in a spreadsheet), a database will
selectively load and unload data as needed. In more complex examples, there
are even more advantages. Below, we'll briefly discuss the bigger ones.

####Complex queries

Generally, you won't be interested in iterating through all of your data. Rather,
you'll want to filter it by some combination of properties (ex. the GMT time of all
tweets happening in New England over the last six months). With databases, these filter operations
are dead simple.

####Centralized servers

One big aspect of databases is that they don't need to be on every computer to be
used. This is why you don't need to download everybody's information every time
you go onto Facebook, or every product's information when you log on to Amazon.
You can use documents stored on other computers almost as easily as if it was on
your own computer. All you need is the address of the computer hosting the database,
and open ports to communicate properly. Then, rather than iterating through all
of the data on your own computer, your code will use the data on this remote
server. This means that you don't have to copy files between computers to use
scripts. Furthermore, multiple people can work off of the same database, which
is especially useful for collaborative projects.

##In conclusion
Databases are immensely useful. This tutorial is not nearly enough to cover all
of the amazing abilities databases have to offer. Take the time to learn about
databases, and they will help you. Good luck!
