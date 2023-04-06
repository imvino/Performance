
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
| 15-min                   | 1.14 seconds                 | 484.16 milliseconds                          |
| 30-min                   | 1.49 seconds                 | 637.74 milliseconds                          |
| 1-hour                   | 1.11 seconds                 | 514.92 milliseconds                          |


### Date Range: 02/20/2022 - 02/28/2022

| Interval                 | Initial Execution Time       | After Implementing TimescaleDB |
|--------------------------|------------------------------|--------------------------------|
| 1-hour                   | 11.08 seconds                | 3.48 seconds                   |
| 6-hour                   | 22.96 seconds                | 10.79 seconds                  |
| 12-hour                  | 35.93 seconds                | 19.4 seconds                   |
| 24-hour                  | 1 minutes 30 seconds         | 20 seconds                     |


-   Note: TimescaleDB is more suited for shorter time frames (less than 24 hours). The 24-hour interval test did not yield results.

## Performance Test Results On Mid Spec

Test Environment: http://localhost:5000/ (Docker)
<br/> Resources: 3GB memory, 4 CPU
<br/> Module: Arrivals on Green
<br/> Location: SR347 | Cobblestone North

### Date Range: 02/21/2022 - 02/21/2022
| Interval                 | Initial Execution Time | After Implementing TimescaleDB |
|--------------------------|-----------------------|-----------------------------------------------|
| 15-min                   | 610.58 milliseconds        | 400 milliseconds                              |
| 30-min                   | 659.48 milliseconds        | 416 milliseconds                              |
| 1-hour                   | 755.86 milliseconds              | 642 milliseconds                              |


### Date Range: 02/20/2022 - 02/28/2022

| Interval                 | Initial Execution Time | After Implementing TimescaleDB |
|--------------------------|-----------------------|-----------------------------------------------|
| 1-hour                   | 3.28 seconds              | 1.02 seconds                                  |
| 6-hour                   | 4.17 seconds         | 2.95 seconds                                 |
| 12-hour                  | 6.45 seconds         | 5.62 seconds                                  |
| 24-hour                  | 12 seconds  | 9.81 seconds                                    |

<br/>
The performance test results show that implementing the TimescaleDB extension resulted in significant improvements in execution time across various time intervals on a low-spec system. Although the difference is minimal on a mid-spec system, it is still advisable to use TimescaleDB for future databases that might become more complex

## Installing TimescaleDB on Windows

To install the TimescaleDB extension on an existing Windows setup, follow these steps:

1.  Copy the "cpyFile\postgresql.conf" file to `C:\ProgramData\Rhythm Engineering\codeGREEN\pgdb`.
    
**Note:** _You can also enable the TimescaleDB extension in PostgreSQL by modifying your existing postgresql.conf file, from `;shared_preload_libraries = ''` to `shared_preload_libraries = 'timescaledb'` instead of copying postgresql.conf_


2.  Copy all files in the "cpyFile\lib" folder to `C:\ProgramFiles\Rhythm Engineering\codeGREEN\bin\pgsql\lib`.


3.  Copy all files in the "cpyFile\extension" folder to `C:\Program Files\Rhythm Engineering\codeGREEN\bin\pgsql\share\extension`.

    
4.  Run the following PostgreSQL query to generate TimescaleDB functions in the database:
    `CREATE EXTENSION IF NOT EXISTS timescaledb;` 

## Updating Node Server Code

After successfully installing the TimescaleDB extension, update the Node server code to take advantage of the improved time series handling. This includes modifying database queries to utilize TimescaleDB's features and optimizing the data processing logic as needed.

By implementing TimescaleDB and updating the Node server code, the system should see a substantial improvement in performance and resource usage when dealing with time series data.
