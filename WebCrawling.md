# **Web Crawling**

> Muhammad Miftakhus Sobari
>
> 160411100045
>
> Penambangan dan Pencarian Web
>
> Teknik Informatika - Universitas Trunojoyo Madura
>
> Pengampu : Mulaab, S.Si., M.Kom.



*This program will run in Python 3.7 version*

First, we need to import some libraries called *beautifulsoup4*, *requests*, and *sqlite3*. So you need to install these libraries if you haven't install it to your computer.

1. Install the libraries that will be used in this program

Before you type the code below, make sure your computer is **connected to the Internet**.

Open your command window and type :

```
pip install beautifulsoup4
```

This code will download the *beautifulsoap4* library and install it automatically in your computer if its meet the requirement.

**If** the code above **failed**, try this magical code :

```
pip install bs4
```

Don't close your command window yet, type this one :

```
pip install requests
```

Same like before, this code will download *requests* library and install it automatically in your computer if its meet the requirement.

What about the *sqlite3* library ?

You don't have to install it manually, because this library will installed once you install Python.

2. Program List

After libraries installation, make sure your computer is still **connected to the Internet** or this program may not run.

Run your Python 3.7 and follow this

First of all we need to import the libraries we already installed

```
import requests
from bs4 import BeautifulSoup
import sqlite3
```

* *requests* library will be used to make requests to the Internet to download any page you want to crawl
* *BeautifulSoup* library will be used to convert the page you have downloaded to BeautifulSoup object

- *sqlite3* library will be used for database connection and operation

After this we need to make database file and creating tables to save our crawled data from the page you want

```
conn = sqlite3.connect('dbs.db')
conn.execute('drop table if exists Kata')
conn.execute('''CREATE TABLE Kata
         (Katas TEXT NOT NULL,
         Penutur TEXT NOT NULL);''')
```

We will make a new variable *conn* for connecting to the database by creating database file called *dbs*. Then this program will **delete** table named *Kata* if its exist. After that we **create** a table called *Kata* with 2 fields : *Katas* and *Penutur* both using text type data.

Then we need to download a code website html and **convert** it to *BeautifulSoup* object

```
src = "https://jagokata.com/kata-bijak/kata-pepatah.html"
page = requests.get(src)
soup = BeautifulSoup(page.content, 'html.parser')
```

In this program, I use the above link for example. You can change to any website you want

Then we have to search the class of the data we want to crawl

![1550759597544](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1550759597544.png)

The quotes is located in *fbquotes* class of an paragraph, so we have to catch items in that class

```
kata = soup.findAll(class_='fbquote')
```

After this we have to **extract** every data that we catch and save it to the database we already created above :

```
for i in range(len(kata)):
    a = kata[i].getText()
    b = penutur[i].getText()
    conn.execute('INSERT INTO Kata(Katas, Penutur) \
                    VALUES ("%s", "%s")' %(a, b));
```

Using *for* iteration, we extract all data from *kata* variable and save it to the new *a* variable and we insert it to the table *Kata* we created.

Where is the data located ? How can I see it ?

```
cursor = conn.execute("SELECT * from Kata")
for row in cursor:
    print(row)
```

Finally, we have to select all data from *Kata* table above and print it to the screen using *for* iteration.

Here are the complete code :

```
import requests
from bs4 import BeautifulSoup
import sqlite3

conn = sqlite3.connect('dbs.db')
conn.execute('drop table if exists Kata')
conn.execute('''CREATE TABLE Kata
         (Katas TEXT NOT NULL,
         Penutur TEXT NOT NULL);''')

src = "https://jagokata.com/kata-bijak/kata-pepatah.html"
page = requests.get(src)
soup = BeautifulSoup(page.content, 'html.parser')

kata = soup.findAll(class_='fbquote')
penutur = soup.findAll(class_='auteurfbnaam')

for i in range(len(kata)):
    a = kata[i].getText()
    b = penutur[i].getText()
    conn.execute('INSERT INTO Kata(Katas, Penutur) \
                    VALUES ("%s", "%s")' %(a, b));
    
cursor = conn.execute("SELECT * from Kata")
for row in cursor:
    print(row)
```

3. Weaknesses

- In this tutorial, we create the database and table with 2 fields **manually**. So if you want to create another databases with more table fields, just modify the code below to anything you want :

```
conn.execute('''CREATE TABLE Kata
         (Katas TEXT NOT NULL,
         Penutur TEXT NOT NULL);''')
```

- This tutorial will help you to **crawl only 1 page**. You have to crawl it manually if you want to crawl another pages.