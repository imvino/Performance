
#  Implementing TimescaleDB for Improved Time Series Performance

## Overview

In the previous code implementation, the system did not utilize time series for calculating time intervals. Instead, it fetched data from the database in multiple chunks, processed it into multiple time intervals, combined the results, and sent them to the timing-plain-ui frontend. This approach consumed significant resources, occasionally leading to long execution times or even crashing the database due to limited resources when handling large data.

To optimize performance, we have integrated TimescaleDB, a PostgreSQL extension, into the system. This extension handles time series data more efficiently, eliminating the need to manually calculate time intervals. Performance tests demonstrated a significant reduction in execution time.


## Performance Test Results

Test Environment: [http://clean-db-opt-test.rhythm-dev.com:5000/](http://clean-db-opt-test.rhythm-dev.com:5000/) 
Resources: 1GB memory, 1 CPU 
Module: Arrivals on Green 
Location: SR347 | Cobblestone North 
Date Range: 02/20/2022 - 02/28/2022

### 1-hour interval

-   Initial execution time: 11.08 seconds
-   Execution time after implementing TimescaleDB: 3.48 seconds

### 6-hour interval

-   Initial execution time: 22.96 seconds
-   Execution time after implementing TimescaleDB: 10.79 seconds

### 12-hour interval

-   Initial execution time: 35.93 seconds
-   Execution time after implementing TimescaleDB: 19.4 seconds

### 24-hour interval

-   Initial execution time: 1minutes 30 seconds
-   Execution time after implementing TimescaleDB: 20 seconds
-   Note: TimescaleDB is more suited for shorter time frames (less than 24 hours). The 24-hour interval test did not yield results.

The performance test results show significant improvements in execution time across various time intervals after implementing the TimescaleDB extension. These enhancements are especially noticeable for shorter time frames, where the extension is most effective.

## Installing TimescaleDB on Windows

To install the TimescaleDB extension on an existing Windows setup, follow these steps:

1.  Copy the "cpyFile\postgresql.conf" file to `C:\ProgramData\Rhythm Engineering\codeGREEN\pgdb`.
    
2.  Copy all files in the "cpyFile\lib" folder to `C:\ProgramFiles\Rhythm Engineering\codeGREEN\bin\pgsql\lib`.

3.  Copy all files in the "cpyFile\extension" folder to `C:\Program Files\Rhythm Engineering\codeGREEN\bin\pgsql\share\extension`.
    
4.  Run the following PostgreSQL query to generate TimescaleDB functions in the database:
    `CREATE EXTENSION IF NOT EXISTS timescaledb;` 

## Updating Node Server Code

After successfully installing the TimescaleDB extension, update the Node server code to take advantage of the improved time series handling. This includes modifying database queries to utilize TimescaleDB's features and optimizing the data processing logic as needed.

By implementing TimescaleDB and updating the Node server code, the system should see a substantial improvement in performance and resource usage when dealing with time series data.
