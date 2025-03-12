
```powershell	
$query="SELECT top 10000 * FROM FASTExportData.dbo.TEST_71_1M_12m"
$target="localhost"

#baseline using mssql bcp
bcp "FASTExportData.dbo.TEST_71_1M_12m" out C:\TEMP\BCP_1M71C.out -S localhost -U "FastUser" -P "FastPassword" -A "|" -w -b 10000


#MSSQL > CSV monothread
.\FastBCP.exe `
--connectiontype "mssql" `
--server "localhost" `
--database "FASTExportData" `
--trusted `
--query "SELECT top 1000 * FROM dbo.TEST_71_1M_12m" `
--directory "C:\temp" `
--fileoutput "TEST1000.csv" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method "None" 


# MSSQL > CSV multithread - Random based on modulo
.\FastBCP.exe `
--connectiontype "mssql" `
--server "localhost" `
--database "FASTExportData" `
--trusted `
--query "SELECT * FROM dbo.TEST_71_1M_12m" `
--directory "C:\temp" `
--fileoutput "TEST_71c_1Mr.csv" `
--decimalseparator "," `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method "Random" `
--distributekeycolumn "ID_ARTICLE" `
--paralleldegree 8 `
--merge "True" `
--runid "mssql_to_csv_parallel8_random"

# MSSQL > CSV multithread - Random based on modulo
.\FastBCP.exe `
--connectiontype "mssql" `
--server "localhost" `
--database "FASTExportData" `
--user "FastUser" `
--password "FastPassword" `
--query "SELECT * FROM dbo.TEST_71_1M_12m" `
--directory "C:\temp" `
--fileoutput "TEST_71c_1Mr.csv" `
--decimalseparator "," `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method "Random" `
--distributekeycolumn "ID_ARTICLE" `
--paralleldegree 8 `
--merge "True" `
--runid "mssql_to_csv_parallel8_random"

.\FastBCP.exe `
--connectiontype "mssql" `
--server "localhost" `
--database "tpch10_collation_bin2" `
--trusted `
--sourceschema "dbo" `
--sourcetable "orders" `
--directory "D:\temp" `
--fileoutput "mssql_orders.csv" `
--decimalseparator "," `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method "RangeId" `
--distributekeycolumn "o_orderkey" `
--paralleldegree -2 `
--merge "True" `
--runid "mssql_to_csv_parallel-2_rangeid"

.\FastBCP.exe `
--connectiontype "mssql" `
--server "localhost" `
--database "tpch10_collation_bin2" `
--trusted `
--query "select * from orders WHERE o_orderdate between '19950101' and '19951231'" `
--directory "D:\temp" `
--fileoutput "mssql_orders.csv" `
--decimalseparator "," `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method "RangeId" `
--distributekeycolumn "o_orderkey" `
--paralleldegree -2 `
--merge "True" `
--runid "mssql_to_csv_parallel-2_rangeid_using_query"


.\FastBCP.exe `
--connectiontype "mssql" `
--server "localhost" `
--database "tpch10_collation_bin2" `
--trusted `
--sourceschema "dbo" `
--sourcetable "orders" `
--directory "D:\temp" `
--fileoutput "mssql_orders.csv" `
--decimalseparator "," `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method "Ntile" `
--distributekeycolumn "o_orderkey" `
--paralleldegree 12 `
--merge "True" `
--runid "mssql_to_csv_parallel12_Ntile"


.\FastBCP.exe `
--connectiontype "mssql" `
--server "localhost" `
--database "tpch10_collation_bin2" `
--trusted `
--query "select * from orders WHERE o_orderdate between '19950101' and '19951231'" `
--directory "D:\temp" `
--fileoutput "mssql_orders.csv" `
--decimalseparator "," `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method "Ntile" `
--distributekeycolumn "o_orderkey" `
--paralleldegree 12 `
--merge "True" `
--runid "mssql_to_csv_parallel12_Ntile_using_query"

# Partial extract mssql to csv using query and Ntile Parallel Method
.\FastBCP.exe `
--connectiontype "mssql" `
--server "localhost" `
--database "tpch10_collation_bin2" `
--trusted `
--query "select * from orders WHERE o_orderdate between '19950101' and '19951231'" `
--directory "D:\temp" `
--fileoutput "mssql_orders.csv" `
--decimalseparator "," `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method "Ntile" `
--distributekeycolumn "o_orderdate" `
--paralleldegree 12 `
--merge "True" `
--runid "mssql_to_csv_parallel12_Ntile_using_query"

.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost" `
--database "tpch" `
--user "FastUser" `
--password "FastPassword" `
--query "select * from tpch_10.orders WHERE o_orderdate between '19950101' and '19951231'" `
--directory "D:\temp" `
--fileoutput "mssql_orders.csv" `
--decimalseparator "," `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method "Ntile" `
--distributekeycolumn "o_orderdate" `
--paralleldegree 12 `
--merge "True" `
--runid "pgsql_to_csv_parallel12_Ntile_using_query"

.\FastBCP.exe `
--connectiontype "pgcopy" `
--server "localhost" `
--database "tpch" `
--user "FastUser" `
--password "FastPassword" `
--query "select * from tpch_10.orders WHERE o_orderdate between '19950101' and '19951231'" `
--directory "D:\temp" `
--fileoutput "mssql_orders.csv" `
--decimalseparator "," `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method "Ntile" `
--distributekeycolumn "o_orderdate" `
--paralleldegree 12 `
--merge "True" `
--runid "pgcopy_to_csv_parallel12_Ntile_using_query"



.\FastBCP.exe `
--connectiontype "mssql" `
--server "localhost" `
--database "tpch10_collation_bin2" `
--directory "D:\temp" `
--fileoutput "mssql_orders.csv" `
--trusted `
--query "SELECT * FROM dbo.orders" `
--method "Random" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "o_orderkey" `
--merge "true" `
--paralleldegree 7

# mssql datadriven
.\FastBCP.exe `
--connectiontype "mssql" `
--server "localhost" `
--database "master" `
--directory "C:\temp" `
--fileoutput "TEST.CSV" `
--paralleldegree 6 `
--trusted `
--query $query `
--method "DataDriven" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "DT_PERIODE" `
--merge "false"

# mssql to csv using partition function in a ^parallel datadriven
.\FastBCP.exe `
--connectiontype "mssql" `
--server "localhost" `
--database "tpch10_collation_bin2" `
--directory "D:\temp" `
--fileoutput "mssql_orders.csv" `
--trusted `
--sourceschema "dbo" `
--sourcetable "orders_part" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method "DataDriven" `
--distributekeycolumn "`$partition.PF_DATE(o_orderdate)" `
--merge "true" `
--paralleldegree 10

# mssql to csv using RangeId parallel method
.\FastBCP.exe `
--connectiontype "mssql" `
--server "localhost" `
--database "tpch10_collation_bin2" `
--directory "D:\temp" `
--fileoutput "mssql_orders.csv" `
--trusted `
--method "RangeId" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "o_orderkey" `
--merge "true" `
--paralleldegree 7 `
--sourceschema "dbo" `
--sourcetable "orders"

# mssql Ntile
.\FastBCP.exe `
--connectiontype "mssql" `
--server "localhost" `
--database "tpch10_collation_bin2" `
--directory "D:\temp" `
--fileoutput "mssql_orders.csv" `
--trusted `
--method "Ntile" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "o_orderkey" `
--merge "true" `
--paralleldegree 8 `
--sourceschema "dbo" `
--sourcetable "orders"


$query="SELECT top 10000 * FROM FASTExportData.dbo.TEST_71_1M_12m"
$target="localhost"
#oledb
.\FastBCP.exe `
--connectiontype "oledb" `
--provider "MSOLEDBSQL" `
--server $target `
--directory "C:\temp" `
--fileoutput "TEST.CSV" `
--paralleldegree 1 `
--trusted `
--query $query `
--method "None" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8"

.\FastBCP.exe `
--connectiontype "oledb" `
--provider "MSOLEDBSQL" `
--server $target `
--directory "C:\temp" `
--fileoutput "TEST.CSV" `
--paralleldegree 6 `
--trusted `
--query $query `
--method "Random" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "ID_ARTICLE" `
--merge "True"

.\FastBCP.exe `
--connectiontype "oledb" `
--provider "MSOLEDBSQL" `
--server $target `
--directory "C:\temp" `
--fileoutput "TEST.CSV" `
--paralleldegree 6 `
--user "FastUser" `
--password "FastPassword" `
--query $query `
--method "Random" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "ID_ARTICLE" `
--merge "True"

.\FastBCP.exe `
--connectiontype "oledb" `
--provider "MSOLEDBSQL" `
--server $target `
--directory "C:\temp" `
--fileoutput "TEST.CSV" `
--paralleldegree 6 `
--trusted `
--query $query `
--method "DataDriven" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "DT_PERIODE" `
--merge "False"

.\FastBCP.exe `
--connectiontype "oledb" `
--provider "MSOLEDBSQL" `
--server $target `
--directory "C:\temp" `
--fileoutput "TEST.CSV" `
--paralleldegree 6 `
--trusted `
--query $query `
--method "DataDriven" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "ID_DECOUPE_GEO" `
--merge "True"

#odbc mssql
.\FastBCP.exe `
--connectiontype "odbc" `
--dsn "DSN4FastBCP" `
--directory "C:\temp" `
--fileoutput "TEST.CSV" `
--paralleldegree 1 `
--trusted `
--query "SELECT * FROM [TEST_71_1M_12m]" `
--method "None" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8"

.\FastBCP.exe `
--connectiontype "odbc" `
--dsn "DSN4FastBCP" `
--directory "C:\temp" `
--fileoutput "TEST.CSV" `
--paralleldegree 6 `
--trusted `
--query "SELECT * FROM [TEST_71_1M_12m]" `
--method "Random" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "ID_ARTICLE" `
--merge "True"

# ODBC to csv using parallel Random method
.\FastBCP.exe `
--connectiontype "odbc" `
--dsn "DSN4FastBCP" `
--directory "C:\temp" `
--fileoutput "TEST.CSV" `
--paralleldegree 6 `
--user "FastUser" `
--password "FastPassword" `
--query "SELECT * FROM [TEST_71_1M_12m]" `
--method "Random" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "ID_ARTICLE" `
--merge "True"

.\FastBCP.exe `
--connectiontype "odbc" `
--dsn "DSN4FastBCP" `
--directory "C:\temp" `
--fileoutput "TEST.CSV" `
--paralleldegree 6 `
--trusted `
--query "SELECT * FROM [TEST_71_1M_12m]" `
--method "DataDriven" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "ID_DECOUPE_GEO" `
--merge "True"

#odbc postgresql
.\FastBCP.exe `
--connectiontype "odbc" `
--dsn "DSN4FastBCPPostgreSQLUnicodeDVF" `
--directory "C:\temp" `
--fileoutput "PGTESTDVF.CSV" `
--paralleldegree 0 `
--query "SELECT coalesce(Code_Departement::integer,0) Code_Dep,* FROM dvf_2021.mutation" `
--method "DataDriven" `
--decimalseparator "." `
--delimiter "|" `

--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "Code_Dep" `
--merge "false"


# pgsql simple
.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost:5432" `
--user "FastUser" `
--password "FastPassword" `
--database "FastBCPData" `
--directory "C:\temp" `
--fileoutput "NPGSQLTEST.CSV" `
--paralleldegree 1 `
--query "SELECT * FROM public.test_71_1m_12m" `
--method "None" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8"

.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost:5432" `
--user "FastUser" `
--password "FastPassword" `
--database "FastBCPData" `
--directory "C:\temp" `
--fileoutput "NPGSQLTEST.CSV" `
--paralleldegree 1 `
--query "SELECT * FROM public.test_71_1m_12m" `
--method "None" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--header `
--quote

.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost:5432" `
--user "FastUser" `
--password "FastPassword" `
--database "tpch" `
--directory "C:\temp" `
--fileoutput "NPGSQLTEST_CUSTOMER.CSV" `
--paralleldegree 1 `
--query "SELECT * FROM tpch_10.customer" `
--method "None" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8"

.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost:5432" `
--user "FastUser" `
--password "FastPassword" `
--database "tpch_1252" `
--directory "C:\temp" `
--fileoutput "NPGSQLTEST_NATION.CSV" `
--paralleldegree 1 `
--query "SELECT * FROM tpch_10.nation" `
--method "None" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "1252"



#pgsql DataDriven
.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost:5432" `
--user "FastUser" `
--password "FastPassword" `
--database "FastBCPData" `
--directory "C:\temp" `
--fileoutput "NPGSQLTEST.CSV" `
--paralleldegree 1 `
--query "SELECT * FROM public.test_71_1m_12m" `
--method "DataDriven" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "DT_PERIODE" `
--merge "False"

.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost:5432" `
--user "FastUser" `
--password "FastPassword" `
--database "FastBCPData" `
--directory "C:\temp" `
--fileoutput "NPGSQLTEST.CSV" `
--paralleldegree 2 `
--query "SELECT * FROM public.test_71_1m_12m" `
--method "DataDriven" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "DT_PERIODE" `
--merge "False"

.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost:5432" `
--user "FastUser" `
--password "FastPassword" `
--database "FastBCPData" `
--directory "C:\temp" `
--fileoutput "NPGSQLTEST.CSV" `
--paralleldegree 4 `
--query "SELECT * FROM public.test_71_1m_12m" `
--method "DataDriven" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "DT_PERIODE" `
--merge "False"

.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost:5432" `
--user "FastUser" `
--password "FastPassword" `
--database "FastBCPData" `
--directory "C:\temp" `
--fileoutput "NPGSQLTEST.CSV" `
--paralleldegree 6 `
--query "SELECT * FROM public.test_71_1m_12m" `
--method "DataDriven" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "DT_PERIODE" `
--merge "False"

.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost:5432" `
--user "FastUser" `
--password "FastPassword" `
--database "FastBCPData" `
--directory "C:\temp" `
--fileoutput "NPGSQLTEST.CSV" `
--paralleldegree 0 `
--query "SELECT * FROM public.test_71_1m_12m" `
--method "DataDriven" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "DT_PERIODE" `
--merge "False"

#pgsql Random
.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost:5432" `
--user "FastUser" `
--password "FastPassword" `
--database "FastBCPData" `
--directory "C:\temp" `
--fileoutput "NPGSQLTEST.CSV" `
--paralleldegree 6 `
--query "SELECT * FROM public.test_71_1m_12m" `
--method "Random" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "ID_ARTICLE" `
--merge "true"

.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost:5432" `
--user "FastUser" `
--password "FastPassword" `
--database "FastBCPData" `
--directory "C:\temp" `
--fileoutput "NPGSQLTEST.CSV" `
--paralleldegree 6 `
--query "SELECT * FROM public.test_71_1m_12m" `
--method "Random" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "ID_ARTICLE" `
--merge "true" `
--batchsize "1/0"

.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost:5432" `
--user "FastUser" `
--password "FastPassword" `
--database "tpch" `
--directory "C:\temp" `
--fileoutput "NPGSQLTEST_CUSTOMER.CSV" `
--paralleldegree 6 `
--query "SELECT * FROM tpch_10.customer" `
--method "Random" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "C_CUSTKEY" `
--merge "true"

.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost" `
--user "FastUser" `
--password "FastPassword" `
--database "tpch" `
--directory "C:\temp" `
--fileoutput "NPGSQLTEST_LINEITEM.CSV" `
--paralleldegree 6 `
--query "SELECT * FROM tpch_10.lineitem" `
--method "Random" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "l_orderkey" `
--merge "true"

./FastBCP `
--connectiontype "pgsql" `
--server "localhost" `
--user "pytabextract_pguser" `
--password "pytabextract_pguser" `
--database "tpch" `
--directory "/tmp/FastBCP" `
--fileoutput "NPGSQLTEST_LINEITEM.CSV" `
--paralleldegree 8 `
--query "SELECT * FROM tpch_10.lineitem" `
--method "Random" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "L_ORDERKEY" `
--merge "true"



#pgcopy simple
.\FastBCP.exe `
--connectiontype "pgcopy" `
--server "localhost:5432" `
--user "FastUser" `
--password "FastPassword" `
--database "FastBCPData" `
--directory "C:\temp" `
--fileoutput "NPGCOPYTEST.CSV" `
--paralleldegree 1 `
--query "SELECT * FROM public.test_71_1m_12m" `
--method "None" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8"

#pgcopy Random
.\FastBCP.exe `
--connectiontype "pgcopy" `
--server "localhost:5432" `
--user "FastUser" `
--password "FastPassword" `
--database "FastBCPData" `
--directory "C:\temp" `
--fileoutput "NPGCOPYTEST.CSV" `
--paralleldegree 6 `
--query "SELECT * FROM public.test_71_1m_12m" `
--method "Random" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "ID_ARTICLE" `
--merge "True"

.\FastBCP.exe `
--connectiontype "pgcopy" `
--server "localhost:5432" `
--user "FastUser" `
--password "FastPassword" `
--database "FastBCPData" `
--directory "C:\temp" `
--fileoutput "NPGCOPYTEST.CSV" `
--paralleldegree 0 `
--query "SELECT * FROM public.test_71_1m_12m" `
--method "Random" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "ID_ARTICLE" `
--merge "True"

.\FastBCP.exe `
--connectiontype "pgcopy" `
--server "localhost:5432" `
--user "FastUser" `
--password "FastPassword" `
--database "FastBCPData" `
--directory "C:\temp" `
--fileoutput "NPGCOPYTEST.CSV" `
--paralleldegree 0 `
--query "SELECT * FROM public.test_71_1m_12m" `
--method "Random" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "ID_ARTICLE" `
--merge "True" `


.\FastBCP.exe `
--connectiontype "pgcopy" `
--server "localhost:5432" `
--user "FastUser" `
--password "FastPassword" `
--database "tpch" `
--directory "C:\temp" `
--fileoutput "NPGSQLTEST_CUSTOMER.CSV" `
--paralleldegree 6 `
--query "SELECT * FROM tpch_10.customer" `
--method "Random" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "C_CUSTKEY" `
--merge "True"

./FastBCP `
--connectiontype "pgcopy" `
--server "localhost" `
--user "pytabextract_pguser" `
--password "pytabextract_pguser" `
--database "tpch" `
--directory "/tmp/FastBCP" `
--fileoutput "NPGSQLTEST_LINEITEM.CSV" `
--paralleldegree 8 `
--query "SELECT * FROM tpch_10.lineitem" `
--method "Random" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "L_ORDERKEY" `
--merge "True"

#Postgresql to csv using pgcopy and parallel DataDriven method
.\FastBCP.exe `
--connectiontype "pgcopy" `
--server $target `
--user "FastUser" `
--password "FastPassword" `
--database "FastBCPData" `
--directory "C:\temp" `
--fileoutput "NPGCOPYTEST.CSV" `
--paralleldegree 6 `
--query "SELECT * FROM public.test_71_1m_12m" `
--method "DataDriven" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "DT_PERIODE" `
--merge "False"

.\FastBCP.exe `
--connectiontype "pgcopy" `
--server "localhost" `
--user "FastUser" `
--password "FastPassword" `
--database "tpch" `
--directory "D:\temp" `
--fileoutput "NPGSQLTEST_LINEITEM.CSV" `
--paralleldegree 8 `
--query "SELECT * FROM tpch_10.lineitem order by l_orderkey" `
--method "DataDriven" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "(L_ORDERKEY/10000000)::int" `
--merge "True"

./FastBCP `
--connectiontype "pgcopy" `
--server "localhost" `
--user "pytabextract_pguser" `
--password "pytabextract_pguser" `
--database "tpch" `
--directory "/tmp/FastBCP" `
--fileoutput "NPGSQLTEST_LINEITEM_SORTED.CSV" `
--query "SELECT * FROM tpch_10.lineitem order by l_orderkey" `
--method "DataDriven" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "(L_ORDERKEY/10000000)::int" `
--merge "True" `
--paralleldegree 6


#pgsql Ctid
.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost" `
--user "FastUser" `
--password "FastPassword" `
--database "FastBCPData" `
--directory "D:\temp" `
--fileoutput "NPGCOPYTEST.CSV" `
--paralleldegree 6 `
--sourceschema "public" `
--sourcetable "test_71_1m_12m" `
--method "Ctid" `
--decimalseparator "." `
--delimiter "|" `

--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--merge "true"

.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost" `
--user "FastUser" `
--password "FastPassword" `
--database "tpch" `
--directory "D:\temp" `
--fileoutput "orders_ctid.CSV" `
--paralleldegree 16 `
--sourceschema "tpch_10" `
--sourcetable "orders" `
--method "Ctid" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--merge "true"

.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost" `
--user "FastUser" `
--password "FastPassword" `
--database "tpch" `
--directory "D:\temp" `
--fileoutput "lineitem_ctid.CSV" `
--paralleldegree 16 `
--sourceschema "tpch_10" `
--sourcetable "lineitem" `
--method "Ctid" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--merge "true"

# postgresql to csv using ctid method
.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost:15432" `
--user "FastUser" `
--password "FastPassword" `
--database "tpch" `
--directory "D:\temp" `
--fileoutput "orders_ctid.csv" `
--paralleldegree 16 `
--sourceschema "tpch_10" `
--sourcetable "orders" `
--method "Ctid" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--merge "false"

# Parallel Export to CSV file
.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost" `
--database "tpch" `
--user "FastUser" `
--password "FastPassword" `
--sourceschema "tpch_10" `
--sourcetable "orders" `
--directory "D:\temp" `
--fileoutput "pgsql_orders.csv" `
--decimalseparator "," `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method "Ntile" `
--distributekeycolumn "o_orderkey" `
--paralleldegree 12 `
--merge "True" `
--runid "pgsql_to_csv_parallel12_Ntile"

$target="NZSQLREC:5480"
#Netezza Random
.\FastBCP.exe `
--connectiontype "nzoledb" `
--provider "NZOLEDBSQL" `
--server $target `
--user "FastUser" `
--password "FastPassword" `
--database "FastBCPData" `
--directory "C:\temp" `
--fileoutput "NZOLEDBTEST.CSV" `
--paralleldegree 6 `
--query "SELECT * FROM test_71_1m_12m" `
--method "Random" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "ID_ARTICLE" `
--merge "true"


#Oracle OleDB
$target=ORADEVE #TNS Entry
$stagelogin="xxxx"
$stagepwd="xxxxxx"
$query="SELECT * FROM SOC1.ADR"

.\FastBCP.exe `
--connectiontype "oledb" `
--provider "OraOLEDB.Oracle" `
--server $target `
--directory "C:\temp" `
--fileoutput "ADR.CSV" `
--paralleldegree 6 `
--user $stagelogin `
--password $stagepwd `
--query $query `
--method "DataDriven" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "ORA_HASH(SIGADR,6-1)" `
--merge "true"


#MySQL
.\FastBCP.exe `
--connectiontype "mysql" `
--server "localhost" `
--user "FastUser" `
--password "FastPassword" `
--database "tpch" `
--directory "D:\temp" `
--fileoutput "MYSQLTEST_ORDERS.CSV" `
--query "SELECT * FROM tpch.orders" `
--decimalseparator "." `
--delimiter "|" `

--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method "None"

.\FastBCP.exe `
--connectiontype "mysql" `
--server "localhost" `
--user "FastUser" `
--password "FastPassword" `
--database "tpch" `
--directory "D:\temp" `
--fileoutput "MYSQLTEST_ORDERS.CSV" `
--query "SELECT * FROM tpch.orders" `
--decimalseparator "." `
--delimiter "|" `

--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "o_orderkey" `
--method "Random" `
--merge "true" `
--paralleldegree 6


# MySQL to CSV using parallel method DataDriven
.\FastBCP.exe `
--connectiontype "mysql" `
--server "localhost" `
--user "FastUser" `
--password "FastPassword" `
--database "tpch" `
--directory "D:\temp" `
--fileoutput "MYSQLTEST_ORDERS.CSV" `
--query "SELECT * FROM tpch.orders_part" `
--decimalseparator "." `
--delimiter "|" `

--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--distributekeycolumn "o_orderdate_year" `
--method "DataDriven" `
--merge "true" `
--paralleldegree 7


# Oracle Natif
.\FastBCP.exe `
--connectiontype "oraodp" `
--server "localhost:1521/FREEPDB1" `
--user "TPCH_IN" `
--password "TPCH_IN" `
--sourceschema "TPCH_IN" `
--directory "D:\temp" `
--fileoutput "ORACLE_ORDERS.CSV" `
--query "SELECT * FROM TPCH_IN.ORDERS FETCH FIRST 10 ROWS ONLY" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method "None"

.\FastBCP.exe `
--connectiontype "oraodp" `
--server "localhost:1521/FREEPDB1" `
--user "TPCH_IN" `
--password "TPCH_IN" `
--sourceschema "TPCH_IN" `
--directory "D:\temp" `
--fileoutput "ORACLE_ORDERS.CSV" `
--query "SELECT * FROM (SELECT * FROM TPCH_IN.ORDERS) a where rownum <=10" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method "None"

.\FastBCP.exe `
--connectiontype "oraodp" `
--server "localhost:1521/FREEPDB1" `
--user "TPCH_IN" `
--password "TPCH_IN" `
--sourceschema "TPCH_IN" `
--directory "D:\temp" `
--fileoutput "ORACLE_ORDERS.CSV" `
--query "SELECT * FROM TPCH_IN.ORDERS" `
--decimalseparator "." `
--delimiter "|" `

--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method "Random" `
--distributekeycolumn "O_ORDERKEY" `
--paralleldegree -4

# Oracle to CSV Parallel using Rowid method and half of the cpu
.\FastBCP.exe `
--connectiontype "oraodp" `
--server "localhost:1521/ORCLPDB" `
--user "TPCH_IN" `
--password "TPCH_IN" `
--sourceschema "TPCH_IN" `
--sourcetable "ORDERS_FLAT" `
--directory "D:\temp" `
--fileoutput "ORACLE_ORDERS.CSV" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method "Rowid" `
--paralleldegree -2


# postgresql to parquet parallel using Ctid 
.\FastBCP.exe `
--connectiontype "pgsql" `
--server "localhost" `
--database "tpch" `
--user "FastUser" `
--password "FastPassword" `
--sourceschema "tpch_10" `
--sourcetable "orders" `
--directory "D:\temp" `
--fileoutput "pgcopy_orders.parquet" `
--method "Ctid" `
--paralleldegree -2 `
--merge "False" `
--runid "pgsql_to_parquet_parallel12_Ctid"

.\FastBCP.exe `
--connectiontype "pgcopy" `
--server "localhost" `
--database "tpch" `
--user "FastUser" `
--password "FastPassword" `
--query "with T1 AS (select *, extract(year from o_orderdate)::int orderyear from tpch_10.orders) SELECT * FROM T1" `
--directory "D:\temp" `
--fileoutput "pgcopy_orders.parquet" `
--method "DataDriven" `
--distributekeycolumn "orderyear" `
--paralleldegree 12 `
--merge "False" `
--runid "pgsql_to_parquet_parallel12_DataDriven"

.\FastBCP.exe `
--connectiontype "mssql" `
--server "localhost" `
--database "faker" `
--trusted `
--sourceschema "dbo" `
--sourcetable "faker_s1000" `
--directory "D:\temp" `
--fileoutput "mssql_faker_s1000.parquet" `
--method "None" `
--runid "mssql_to_parquet_NoParallel"


.\FastBCP.exe `
--connectiontype "mssql" `
--trusted `
--server "localhost" `
--database "tpch10_collation_bin2" `
--directory "D:\temp" `
--fileoutput "mssql_orders.parquet" `
--method "RangeId" `
--distributekeycolumn "o_orderkey" `
--paralleldegree 7 `
--sourceschema "dbo" `
--sourcetable "orders" `
--merge "False"

# source mssql to parquet files in Parallel with Ntile method
.\FastBCP.exe `
--connectiontype "mssql" `
--trusted `
--server "localhost" `
--database "tpch10_collation_bin2" `
--directory "D:\temp" `
--fileoutput "mssql_orders.parquet" `
--method "Ntile" `
--distributekeycolumn "o_orderkey" `
--paralleldegree 7 `
--sourceschema "dbo" `
--sourcetable "orders" `
--merge "False"

# source mssql to parquet files in Parallel with Ntile method and query as source
.\FastBCP.exe `
--connectiontype "mssql" `
--trusted `
--server "localhost" `
--database "tpch10_collation_bin2" `
--directory "D:\temp" `
--fileoutput "mssql_orders_1995.parquet" `
--method "Ntile" `
--distributekeycolumn "o_orderkey" `
--paralleldegree 7 `
--query "select * from dbo.orders where o_orderdate between '1995-01-01' and '1995-12-31'" `
--merge "True"


# source clickhouse to csv files in Parallel with Ntile method
.\FastBCP.exe `
--connectiontype "clickhouse" `
--server "localhost:8123" `
--database "default" `
--user "default" `
--password "jKpHQEfXC91VCWrXoMb3" `
--sourceschema "default" `
--sourcetable "orders" `
--directory "D:\temp" `
--fileoutput "clickhouse_orders.csv" `
--method "Ntile" `
--distributekeycolumn "o_orderkey" `
--paralleldegree 7 `
--merge "True"


# source Hana to csv files in Parallel with Ntile method
.\FastBCP.exe `
--connectiontype "hana" `
--server "hxehost:39015" `
--database "HXE" `
--user "SYSTEM" `
--password "AetP202440" `
--sourceschema "TPCH" `
--sourcetable "ORDERS" `
--directory "D:\temp" `
--fileoutput "hana_orders.csv" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method "Ntile" `
--distributekeycolumn "O_ORDERKEY" `
--paralleldegree 12 `
--merge "True"


# Hanaexport (slower and heavier than hana direct (should be avoid)
.\FastBCP.exe `
--connectiontype "hanaexport" `
--server "hxehost:39015" `
--database "HXE" `
--user "SYSTEM" `
--password "AetP202440" `
--sourceschema "TPCH" `
--sourcetable "ORDERS" `
--directory "D:\temp" `
--fileoutput "hana_orders.csv" `
--decimalseparator "." `
--delimiter "|" `
--dateformat "yyyy-MM-dd HH:mm:ss" `
--encoding "UTF-8" `
--method "None" `

```
