
#  Implementing TimescaleDB for Improved Time Series Performance

## Overview

In the previous code implementation, the system did not utilize time series for calculating time intervals. Instead, it fetched data from the database in multiple chunks, processed it into multiple time intervals, combined the results, and sent them to the timing-plain-ui frontend. This approach consumed significant resources, occasionally leading to long execution times or even crashing the database due to limited resources when handling large data.

To optimize performance, we have integrated TimescaleDB, a PostgreSQL extension, into the system. This extension handles time series data more efficiently, eliminating the need to manually calculate time intervals. Performance tests demonstrated a significant reduction in execution time.



## Performance Test Results On Low Spec

Test Environment: [http://clean-db-opt-test.rhythm-dev.com:5000/](http://clean-db-opt-test.rhythm-dev.com:5000/) (AWS)
<br/> Resources: 1GB memory, 1 CPU
<br/> Module: Arrivals on Green
<br/>Location: SR347 | Cobblestone North


### Date Range: 02/21/2022 - 02/21/2022
| Interval                 | Initial Execution Time       | After Implementing TimescaleDB |
|--------------------------|------------------------------|-----------------------------------------------|
| 15-min                   | 1.14 seconds                 | 833.15 milliseconds                          |
| 30-min                   | 1.49 seconds                 | 710.81 milliseconds                          |
| 1-hour                   | 1.11 seconds                 | 729.7 milliseconds                          |


### Date Range: 02/20/2022 - 02/28/2022

| Interval                 | Initial Execution Time       | After Implementing TimescaleDB |
|--------------------------|------------------------------|--------------------------------|
| 1-hour                   | 11.08 seconds                | 2.32 seconds                   |
| 6-hour                   | 22.96 seconds                | 4.2 seconds                  |
| 12-hour                  | 35.93 seconds                | 7.2 seconds                   |
| 24-hour                  | 1 minutes 30 seconds         | 13.38 seconds                     |



<br/>
The performance test results show that implementing the TimescaleDB extension resulted in significant improvements in execution time across various time intervals on a low-spec system. It is advisable to use TimescaleDB for future databases that might become more complex

## Installing TimescaleDB on Windows

To install the TimescaleDB extension on an existing Windows setup, follow these steps:

1.  Copy the "cpyFile\postgresql.conf" file to `C:\ProgramData\Rhythm Engineering\codeGREEN\pgdb`.
    
**Note:** _You can also enable the TimescaleDB extension in PostgreSQL by modifying your existing postgresql.conf file, from `;shared_preload_libraries = ''` to `shared_preload_libraries = 'timescaledb'` instead of copying postgresql.conf_


2.  Copy all files in the "cpyFile\lib" folder to `C:\ProgramFiles\Rhythm Engineering\codeGREEN\bin\pgsql\lib`.


3.  Copy all files in the "cpyFile\extension" folder to `C:\Program Files\Rhythm Engineering\codeGREEN\bin\pgsql\share\extension`.

    
4.  Run the following PostgreSQL query to generate TimescaleDB functions in the database:
    `CREATE EXTENSION IF NOT EXISTS timescaledb;` 

## Installing TimescaleDB on Docker

To install TimescaleDB on a new Docker container, Open your terminal or command prompt and run the following command to create a new Docker container with TimescaleDB:

    `docker run -d --name timingplans-db -p 5432:5432 -e TZ=GMT -e POSTGRES_USER=timingplans -e POSTGRES_PASSWORD=timingplans timescale/timescaledb:latest-pg12`

## Installing TimescaleDB on an Existing PostgreSQL Docker Container

If you already have a PostgreSQL Docker container and want to install TimescaleDB on it, follow this documentataion:

https://www.knyl.me/blog/install-timescaledb-on-an-existing-postgresql-container/


## Updating Node Server Code

After successfully installing the TimescaleDB extension, update the Node server code to take advantage of the improved time series handling. This includes modifying database queries to utilize TimescaleDB's features and optimizing the data processing logic as needed.

By implementing TimescaleDB and updating the Node server code, the system should see a substantial improvement in performance and resource usage when dealing with time series data.
