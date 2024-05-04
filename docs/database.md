# Database

## PostgreSQL

### Specialties

* Multi-version concurrency control
    * i.e. A row of records is versioned into multiple tuples that are representing the same logical row but in different states/versions. The newest version represents the up-to-date record for that row. Different processes can access a record concurrently.
* Can define custom type
    * ProgreSQL allows users to define custome types/objects in a table schema.

### How It Works Internally

_Study notes from [presentation by Hussein Nasser](https://youtu.be/Q56kljmIN14?si=zSISqnNQNV-7KUJe)._

#### Overview

![postgres-architecture](./images/postgres-architecture.png)

#### Post Master Process

A parent process that starts at the early stage of the application. Exposes the application to port 5432, such that it's ready for connection.

#### Backend Processes

Each backend process is responsible for maintaining a connection to its consumer.

The number of backend process is capped by the parameter `max_connections`.

#### Background Workers

A background worker is responsible for executing the query or command that a consumer initiated.

The number of background workers is capped by the parameter `max_worker_processes`.

#### Background Writers

A background writer is responsible for flushing the data that is stored in a Page into the filesystem, which will eventually write the data into the disk. It wakes up occasionally to clean up dirty Pages/Shared Memory.

#### Checkpointer

Checkpointer is responsible for flushing everything - i.e. both WAL records and Pages to the disk, and creating a checkpoint, indicating that everything now is consistent.

#### Logger

Logger is responsible for logging the status of the database.

#### Autovacuum Launcher and Workers

Autovacuum worker is responsible for cleaning up the tuples that have older versions and are not being accessed by any of the processes.
