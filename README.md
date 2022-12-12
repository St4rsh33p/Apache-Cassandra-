# Apache-Cassandra
Project from Data Engineer using Apache Cassandra

# Description of the Project

- This project has two parts. The first part is the pre_processing the files. 

- I'll be explaining step by step how i made it.

## Step By Step

The first part of the project is an explanation about how to create the lifepaths to process de data files. The project itself does it for you. 

I just wrote a bit of code to understand few things like which were the files i was searching for and also created a graphic representation with pandas of de datafile we were using.

![image](https://user-images.githubusercontent.com/107310236/207034846-9d41e8da-7026-43da-af1b-a8dcc176d8b0.png)

![image](https://user-images.githubusercontent.com/107310236/207034941-61399f53-02be-4cad-8d74-0e13a5c63fff.png)

This way i have a graphic representation of the file and it was easier for me to work it out.

## Second Part Of The Project

In the second part of the project, you have to complete the apache cassandra coding portion. 

- The First Step is to create a Key Space

```Python

try:
    session.execute("""CREATE KEYSPACE IF NOT EXISTS project 
                    WITH REPLICATION =
                    {'class' : 'SimpleStrategy', 'replication_factor' : 1}""")

except Exception as e:
    print(e)
```

Than set the Key Space
```Python
try:
    session.set_keyspace('project')
except Exception as e:
    print(e)
```

- The next step is create the table, insert the data and select it to prove you made it as supossed.

### Create Tables

```Python
query = "CREATE TABLE IF NOT EXISTS event_data"
query = query + "(artist text, firstName text, gender text , itemInSession int , lastName text, length float, level text, \
                    location text, sessionId int, song text, userId text, PRIMARY KEY(artist, song))"
try:
    session.execute(query)
except Exception as e:
    print(e)
```
### Insert Data

```Python
query = "INSERT INTO event_data(artist, firstName, gender, itemInSession, lastName, length,level, location, sessionId, song, userId)"
query = query + " VALUES (%s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s)"
## TO-DO: Assign which column element should be assigned for each column in the INSERT statement.
## For e.g., to INSERT artist_name and user first_name, you would change the code below to `line[0], line[1]`
session.execute(query, (line[0], line[1], line[2], int(line[3]), line[4], float(line[5]),line[6], line[7], int(line[8]), line[9],line[10]))
```

### Select the Data

```Python
query = "SELECT artist, song, length FROM event_data WHERE sessionId = 338 AND itemInSession = 4 ALLOW FILTERING;"
try:
    rows = session.execute(query)
except Exception as e:
    print(e)
    
for row in rows:
    print (row.artist, row.song, row.length)
```

Than you capy and repeat the cells you already made for the other queries.


At the end, you drop the tables.

### Drop the tables

```Python
query = "drop table event_data"
try:
    rows = session.execute(query)
except Exception as e:
    print(e)

query = "drop table event_data1"
try:
    rows = session.execute(query)
except Exception as e:
    print(e)
```


