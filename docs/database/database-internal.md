# Database Internal

## Components

A database is usually consist of two parts - a frontend that talks to the users, and a storage engine that persists the data.

### Frontend

A database frontend defines a couple things - the APIs that a user can use to query the data or execute the commands, and the data format that presents the data to the users.

### Storage Engine

A storage engine is responsible for persisting the data to the disk and extracting the data from the disk.

A storage engine is also usually tasked to implement indexes, ensure the ACID in transactions, and persist the WAL records.

### SQL vs. NoSQL

To compare SQL and NoSQL from the inside, they are mainly different in how data is stored and presents to the users.

In SQL, data is stored in tables with columns and rows. A table with a large amount of records/rows, has its records written into Pages first before they are flushed into the disk. Similarly, when loading data from the disk, the storage engine loads a subset of data into a Page (i.e. the first Page), filter out the results, and then continue with the next Page. Thus, data will be loaded frequently from the disk to a Page for a large data set.

In NoSQL, data is stored in documents - i.e. not restricted to structured data. One document represents a record of data that can be retrieved as a whole. The data retrieval of a NoSQL database can usually load the data needed as many as possible into a Page, and thus improves the efficiency by reducing the need of going back and forth between the disk and Pages. And thus, NoSQL database usually excels in high thoughput requirements.

