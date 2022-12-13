# Apache-Cassandra
Project from Data Engineer using Apache Cassandra

# Description of the Project

- This project has two parts. The first part is the pre_processing the files. 

- I'll be explaining step by step how i made it.

## Step By Step

The first part of the project is an explanation about how to create the lifepaths to process de data files. The project itself does it for you. 

I just created a graphic representation with pandas of de datafile we were using.

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

I created 3 tables, one for each query. 

```Python
query = "CREATE TABLE IF NOT EXISTS music_history"
query = query + "(artist text, itemInSession int , length float, \
                  sessionId int, song text, PRIMARY KEY(artist, song))"
try:
    session.execute(query)
except Exception as e:
    print(e)
```
### Insert Data

```Python
file = 'event_datafile_new.csv'

with open(file, encoding = 'utf8') as f:
    csvreader = csv.reader(f)
    next(csvreader) 
    for line in csvreader:
        Insert_query = "INSERT INTO music_history(artist, itemInSession, length, sessionId, song)"
        Insert_query = Insert_query + " VALUES (%s, %s, %s, %s, %s)"
        session.execute(Insert_query, (line[0], int(line[3]), float(line[5]), int(line[8]), line[9]))
```

### Select the Data

```Python
query = "SELECT artist, song, length FROM music_history WHERE sessionId = 338 AND itemInSession = 4 ALLOW FILTERING;"
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
query = "drop table music_history"
try:
    rows = session.execute(query)
except Exception as e:
    print(e)

query = "drop table artist_library"
try:
    rows = session.execute(query)
except Exception as e:
    print(e)
    
query = "drop table listeners_registry"
try:
    rows = session.execute(query)
except Exception as e:
    print(e)
```


