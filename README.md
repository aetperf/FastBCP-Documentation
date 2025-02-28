# FastBCP

FastBCP is a command line that export data from a database to CSV or Parquet file(s) (or even binary file(s))

![FastBCP](images/FastBCP.jpg)


## Supported Sources & Platforms

Source | Windows AMD64 | Linux AMD64 | Linux ARM64
:--- | :---: | :---: | :---:
ClickHouse | ✅ | ✅ | ✅
DuckDB | ✅ | ✅ | ✅
MySQL | ✅ | ✅ | ✅
Netezza | ✅ | ✅ | ❌
ODBC | ✅ | ✅  | ✅
OLEDB | ✅ |❌  | ❌
Oracle | ✅ | ✅ | ✅
PostgreSQL | ✅ | ✅ | ✅
SQL Server | ✅ | ✅ | ✅
SAP Hana | ✅ | ✅ | ❌
Teradata | ✅ | ✅ | ❌



## Quick Start
If you want to use it quickly, download the lastest Release and unzip it to a director. 

For the following databases drivers are embedded : Clickhouse, DuckDB, MS SQLServer, Netezza, Oracle, MySQL, PostgreSQL, SAP HANA and Teradata .

You will need to install your ODBC or OLEDB drivers of your favorite database in order to use it with ODBC or OLEDB connectiontype.

You can use the FastBCP Wizard to generate a FastBCP command line : [FastBCP Wizard](./FastBCP_Wizard.html)


 

# Logging 
## Parameter Logs targets using FastBCP_settings.json
FastBCP can log informations in 3 modes :
- Console
- File
- Database (MSSQL and PostgreSQL Only)

You can use several targets at the same time.

You can adapt what and where you want to log using the `FastBCP_settings.json` file.
If this file is not present in the same directory that FastBCP, FastBCP will use the console log only.
You can also use a custom file name for the settings file using the `-f` or `--settingsfile` parameter.


### Log Levels
to configure the log level you can change the MinimumLevel in the ***FastBCP_settings.json*** file.
The log levels are :
- Debug
- Information
- Warning
- Error
- Critical

### Log Database
By default FastBCP will use the `FastBCP_settings.json` file to find where the log database is.
It looks for a mssql instance on localhost, and try to connect using SSPI trusted connection.

You can change the connection string in the ***FastBCP_settings.json*** file.
- The default MS_FastTransferLogs : "MS_FastTransferLogs": "Server=localhost;Database=FastTransferLogs;Integrated Security=SSPI;Encrypt=True;TrustServerCertificate=True"
- Exemple MS_FastTransferLogs : "MS_FastTransferLogs": "Server=mssqlloginstance,2433,;Database=FastTransferLogs;Integrated Security=SSPI;Encrypt=True;TrustServerCertificate=True"

Use a correct connection string for your log database.

In case the database is not reachable, FastBCP will failed and will refuse to continue. 
If you want to continue without logging in the database, you can rename the ***FastBCP_settings.json*** and FastBCP will use the console log only.

### Log Targets
You can configure the log targets in the ***FastBCP_settings.json*** file. in the **"WriteTo"** section.
You can remove or adapt the targets as you want but be carrefull any error in the FastBCP_settings.json file will prevent FastBCP to start.
You can try with and without the FastBCP_settings.json file to see the difference.

# Performance

## Parallelism
FastBCP use 6 differents methods that will allow parallel extract. If a parallel method is choose, FastBCP will run several queries in parallel against the source database to exctract the data :
- **Random** : you will need a **numeric** column (or computation that return a numeric. eg : "YEAR(DT_PERIOD)" ). FastBCP use a trick using a modulo (%) computation on the given column divided by the degree of parallism. You should choose a column that will spread the parallel thread evenly. For exemple a PRODUCT_ID or a CUSTOMER_ID in an ORDER Table should be a good choice. On the contrary a numeric column with always the same value won't scale.
- **DataDriven** : you will need to provide a column (or a computation) and FastBCP will use every distinct values of the provided column to parallel query the source data with a where clause like WHERE MyColumn=$ValueX. The Degree parameter will allow to throttle the workload. A good choice if your want to sort the data in the target file by the driven column.
- **Ctid** : Postgresql Only. This mode will use the ctid column to split the data. It's a good choice if you don't want to sort the Data. A postgreSQL snapshot is used to preserve the extraction from inconsistencies.
- **Rowid** : Oracle Only. This mode will use the ROWID column to split the data. It's a good choice if you don't want to sort the data in the target file(s).  
- **RangeId** : This mode will use the distributed column to split the data using it's max value. It's a good choice if you want to sort data and if the source table is clustered by the distributed column  
- **Ntile** : This mode will use the distributed column to split the data using it's ntiles. It's a good choice if you want to sort data and if the source table is clustered by the distributed column but you have gap in the id data values. The column datatype can be numerical, date or string, nulls data are allowed and will be put in the first ntile.

## Merge or not Merge temporary files
By using `--merge=false` you will be able to generate one file per chunk. It is faster to generate and often faster to import :-)
When using the Datadriven Mode without merge, you will have several directories,one for each value of the driven column on the Hive format ("column=value").


## Some performance results

![FastBCP Performance Dashboard](images/FastBCPv0.2.0_Performance_Dashboard.jpg)

You can have a lookp on the live [FastBCP Performance dashboard](https://public.tableau.com/views/FastBCPPerformanceDashboard/FastBCPPerformance?:language=en-US&publish=yes&:display_count=n&:origin=viz_share_link) here 

# Usage :

For Linux

``` bash
FastBCP [parameters]
````
For Windows

``` powershell
.\FastBCP.exe [parameters]
```

# CLI Parameters (grammar accessible [here](fastbcp_grammar/index.md))

The parameters can be in short pattern using signe dash (-) or long pattern using double dash (--)

**You should prefer long style parameters for readability** :

- `-?` or `--help` will display the help message

- `-C` or `--connectiontype` : The connection type :
    * `clickhouse` : Clickhouse database
    * `hana` : SAP HANA Database
    * `msoledbsql` : Microsoft OLEDB SQL Server
    * `mssql` : Microsoft SQL Server
    * `mysql` : MySQL
    * `nzcopy` : Netezza Copy (WIP)
    * `nzoledb` : Netezza OLEDB
    * `nzsql` : Netezza SQL
    * `odbc` : ODBC (need a DSN or a connection string)
    * `oledb` : OLEDB (only on windows)
    * `oraodp` : Oracle Database ODP.Net
    * `pgcopy` : PostgreSQL Copy
    * `pgsql` : PostgreSQL
    * `teradata` : TeraData

- `-A` or `--trusted` : Trusted connection (if allowed by the rdbms)

- `-U` or `--user` : User name

- `-X` or `--password` : Password

- `-q` or `--query` : The query to extract the data

- `-F` or `--fileinput` : The input file that contains the query

- `-s` or `--sourceschema` : The source schema

- `-T` or `--sourcetable` : The source table

- `-N` or `--dsn` : The DSN for ODBC connection type

- `-P` or `--provider` : The provider for OLEDB connection type

- `-S` or `--server` : The server

- `-I` or `--database` : The database

- `-D` or `--directory` : The directory where the output file will be generated

- `-o` or `--fileoutput` : The output file name

- `-x` or `--timestamped` : Add a timestamp to the output file name 

- `-m` or `--method` : The method to use for parallel extraction can be 
    * `None` : No parallelism
    * `Random` : random distribution using modulo on a numerical column. `--distributekeycolumn` is mandatory
    * `DataDriven` : distribution using values of a column, pseudo-column or an expression. `--distributekeycolumn` is mandatory
    * `Rowid` : distribution using the ROWID field, Oracle like databases only.
    * `Ctid` : distribution using the CTID field, PostgreSQL like databases only.
    * `RangeId` : distribution using the distributed column field and it's interval between min and max value. The column datatype must be numerical and not null. `--distributekeycolumn` is mandatory
    * `Ntile` : distribution using the distributed column field and it's ntile values. The column datatype can be numerical, date or string, nulls data are allowed and will be put in the first ntile. `--distributekeycolumn` is mandatory

    Default is `None`

- `-c` or `--distributekeycolumn` : The column, pseudo-column or expression that will be used in the parallel method (exept for Ctid and Rowid).

- `-p` or `--paralleldegree` : The degree of parallelism

- `-M` or `--merge` : Merge the temporary files or not. Default is `true`. If `false` the temporary files will be kept and can be use as a split method.

- `-d` or `--delimiter` : The delimiter. default is `|`

- `-t` or `--quotes` : Use quotes for string fields. default is `false`. If `true` the quotes inside data will be escaped by a `\`. Nota : line field are detected and replace by '\n'

- `-f` or `--dateformat` : The date format like `yyyy-MM-dd HH:mm:ss`, default `yyyy-MM-dd`

- `-n` or `--decimalseparator` : The decimal separator. default is `.`

- `-e` or `--encoding` : The encoding. default is `UTF-8`

- `-h` or `--noheader` : No header. default is `false`

- `-b` or `--boolformat` : The boolean format. can be
    * automatic
    * true/false
    * 1/0
    * t/f

    default is `automatic`

- `-R` or `--runid` : The run identifier. Default is a GUID



# Parameters details

## Authentification

## Authentification Methods
There are several authentication methods allowed :
- Trusted Connection (`-A` switch or `--trusted`) if allowed by the rdbms
- SQL Login/Password (`-U` "UserName" `-X` "Password" or `--user` "UserName" `--password` "PassWord")

For Login/Password you can also use the environment variables like FASTBCP_USER and FASTBCP_PASSWORD for exemple or store the password in an encrypted file. 
Up to you to decrypt the file and it's content before using it in a shell variable that will be used with FastBCP



## Data retrieval

### Query (`-q` or `--query`)
The query (-q) allow you to specify your extraction as a select statement

### Input File (`-F` or `--fileinput`)
the input file that must contains only one query.
The query must **not** have a trailing semicolon (;)

### Source Schema (`-s` or `--sourceschema`)
If no query nor input file is used the source schema and source table can also be used to extract the sourceschema.sourcetable.
This field is mandatory if some parallel method is used like Rowid, Ctid, RangeId and Ntile

### Source Table (`-T` or `--sourcetable`)
If no query nor input file is used the source schema and source table can also be used to extract the sourceschema.sourcetable.
This field is mandatory if some parallel method is used like Rowid, Ctid, RangeId and Ntile


## Source of the data

### DSN (`-N` or `--dsn`)
if you have choosen an odbc connection type you must define the DSN with the -N or --dsn parameter or define a connection string (-G or --connectionstring) 

### Provider (`-P` or `--provider`)
if you have choosen an oledb connection type you must define the OLEDB provider with the -P or --provider parameter (-P MSOLEDBSQL or -P NZOLEDB for example)

### Server (`-S` or `--server`)
except if you have define a connection string (-G or --connectionstring), you must define a server (or could also be a TNS entry for oracle).
server could be :
- an IP address or a DNS name.
- an IP address or a DNS name and a port number : "myserver.mydomain.com:2433"
- an IP address or a DNS name and a named instance  : "myserver.mydomain.com\myinstance"
- for duckdb you can use a file path : "C:\temp\myduckdb.db" or ":memory:" for an in memory database

### Database (`-I` or `--database`)
Optionnaly define a source database.


## Output

### Directory (`-D` or `--directory`)
Specify where the output file will be generated

### Output file (`-o` or `--fileoutput`)
The name of the final file generated by FastBCP. For parallel extraction without merge, the file name (without file extension) will be used as a prefix for the distributed files.

### Timestamped output (`-x` or `--timestamped` switch)
Will add a timestamp to the output file name if the switch is used


## Performance Parameters 

### Distribution Method (`-m` or `--method`)
You can improve performance by using a method for parallelize the export (`-m` or `--method`) and a degree of parallelism (-p or --paralleldegree).
The method (`-m` or `--method`) can take several values :
- `None` (No parallelism)
- `Random` : this method is linked to a distribution column that must be an integer/bigint and that should have many values  (at least as many as the dop)
- `DataDriven` : this method will use all the values of a column to split the export to different files. If the number of values is greater than the dop, the dop will be used like a throttling.
- `Rowid` : this method use internal hidden field to retrieve chunk of data. Each parallel thread will export a portion of the data based on the ROWID range. Oracle like databases only.
- `Ctid` : this method use internal hidden field to retrieve chunk of data. Each parallel thread will export a portion of the data based on the CTID range. PostgreSQL like databases only.
- `RangeId` : this method use the distributed column field and it's min and max value to retrieve chunk of data. Each parallel thread will export a portion of the data based on a range build using de distributed column values. Best if table is clustered by this field. The unicity of the distributed column is not mandatory.
- `Ntile` : this method use the distributed column field and ntile values to retrieve chunk of data. Each parallel thread will export a portion of the data based on a range build using de distributed column values. Best if table is clustered by this field. The unicity of the distributed column is not mandatory.
 
Table of Distribution Method :

| method     | need a distributed column | database source type |
|:-------    | :------------------------:| :-------------------:|
| None       | No						 | Any					|
| Random     | Yes						 | Any					|
| DataDriven | Yes						 | Any					|
| Ctid		 | No						 | PostgreSQL (pgsql/pgcopy) |
| Rowid		 | No						 | Oracle (oraodp)      |
| RangeId	 | Yes						 | Any					|
| Ntile		 | Yes						 | Any					|


### Distribute Key Column (`-c` or `--distributekeycolumn`)
Define the column (or computation) on the data source that will be used to split the data into several parts. FastBCP will use SQL queries that will run in parallel against the source. Each query will have a where clause that will retrieve a part of the total data.

### Degree of Parallelism (`-p` or `--paralleldegree`)
the degree of parallelism could be 0. In this case the dop we be allign with the number of cores(threads if HT is on) on the machine where you run FastBCP.
If the dop is greater than the number of cores (or threads if HT is on) then it will be downscale to the number of cores/threads of the machine.
If the dop is less than 0 it will be computed as the number of cores/(abs(dop)). For exemple if you have 16 cores and you set dop to -2 then the dop will computed and set to 8.

### Merge (`-M` or `--merge`)
You can specify if the "temporary" files should be merge to the final ouput file and deleted or if you prefer to keep the distributed files without merging them.

## Data Formatting

### Delimiter (`-d` or `--delimiter`)
The character(s) that will be used as field delimiter

### Use Quotes Identifier (`-t` or `--quotes`)
Add quotes "" for string fields (will escape " by \ if found in the data field)
value can be `true` or `false`

### Date Format (`-f` or `--dateforma`)
Date format like "yyyy-MM-dd HH:mm:ss"

### Decimal Separator (`-n` or `--decimalseparator`)
Use for specify the decimal separator (can be `,` or `.`)

### Encoding (`-e` or `--encoding`)
The characterset encoding of the output file. Default is `UTF-8`

### No Header (`-h` or `--noheader` switch)
If you don't want to have a header in the output file, use the -h or --noheader switch

### Boolean Format (`-b` or `--boolformat`)
How boolean are formatted (`true/false`, `t/f` or `1/0`)

## Log Parameters 

### Run Identifier (`-R` or `--runid`)
An identifier for the run (unique or not). 
It will be used in the log to identify the run.

# Examples

### Parallel Extraction using several methods (on windows using powershell and long names parameters) :

#### Random :
``` powershell
.\FastBCP.exe `
--connectiontype mssql `
--server localhost `
--database "tpch10_collation_bin2" `
--trusted `
--query "SELECT * FROM dbo.orders" `
--directory "D:\temp" `
--fileoutput "mssql_orders.csv" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method Random `
--distributekeycolumn "o_orderkey" `
--paralleldegree 7 `
--merge true `
--runid "mssql_to_csv_parallel_random"
```

#### DataDriven

``` powershell
.\FastBCP.exe `
--connectiontype mssql `
--server localhost `
--database "tpch10_collation_bin2" `
--trusted `
--query "SELECT * FROM dbo.orders" `
--directory "D:\temp" `
--fileoutput "mssql_orders.csv" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method DataDriven `
--distributekeycolumn "YEAR(o_orderdate)" `
--paralleldegree 7 `
--merge false `
--runid "mssql_to_csv_parallel_datadriven"
```


#### RangeId

``` powershell
.\FastBCP.exe `
--connectiontype "pgcopy" `
--server "localhost:15432" `
--database "tpch" `
--user "FastUser" `
--password "FastPassword" `
--query "select * from tpch_10.orders WHERE o_orderdate between '19950101' and '19951231'" `
--directory "D:\temp" `
--fileoutput "orders_1995.parquet" `
--encoding "UTF-8" `
--method RangeId `
--distributekeycolumn "o_orderkey" `
--paralleldegree 7 `
--merge true `
--runid "pgcopy_to_parquet_parallel_rangeid"
```



You can find more exemples in the [samples.ps1](samples.ps1) file

