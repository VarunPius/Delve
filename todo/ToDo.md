https://dev.to/jon_snow789/20-github-repositories-every-developer-should-bookmarkhigh-value-resources-4jm6?utm_source=pocket_saves
    - Dev Roadmap
    - https://github.com/practical-tutorials/project-based-learning
    - https://github.com/codecrafters-io/build-your-own-x


# PYTHON:
https://mathspp.com/blog/pydonts/dunder-methods

# Create blog
https://github.com/gregrickaby/nextjs-github-pages
https://dev.to/github/how-to-host-a-static-nextjs-site-on-github-pages-4pe0

# SQL
LISTAGG / STRING_AGG
INTERSECT
EXCEPT
cast ::
https://stackoverflow.com/questions/34627026/in-vs-any-operator-in-postgresql
EXISTS NULL case:https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-exists/


MySQL:
DATE_ADD(date, INTERVAL value addunit): addunit same as above
DATEDIFF( )
DAYNAME( ), DAYOFMONTH( ), DAYOFWEEK( ), DAYOFYEAR( )
EXTRACT(_ FROM hiredate) | second, minute, hour, day, week, month, quarter, year, etc
     https://medium.com/analytics-vidhya/mysql-functions-cheatsheet-with-examples-3a08bb36d074
DATE_FORMAT(date, format)


Postgres:
date addn:
     SELECT date '2027-05-20' + integer '5';      -- 5 days
     SELECT date '2027-05-20' + interval '5 minute';
     SELECT date '2027-05-20' + time '05:45';
     SELECT date '2027-05-20' + interval '5 week';
     SELECT date '2027-05-20' + interval '5 month';
date difference:
     SELECT ('2015-01-12'::date - '2015-01-01'::date) AS days;
     DATE_PART('day', MAX(joindate) - MIN(joindate)) as date_diff
     select extract(day from 'DATE_A'::timestamp - 'DATE_B'::timestamp);
EXTRACT( __ FROM published_date) : 
TO_CHAR(NOW():: DATE, 'mm dd, yyyy');




## Window function:
https://www.postgresqltutorial.com/postgresql-window-function/
CUME_DIST	Return the relative rank of the current row.
DENSE_RANK	Rank the current row within its partition without gaps.
FIRST_VALUE	Return a value evaluated against the first row within its partition.
LAG	Return a value evaluated at the row that is at a specified physical offset row before the current row within the partition.
LAST_VALUE	Return a value evaluated against the last row within its partition.
LEAD	Return a value evaluated at the row that is offset rows after the current row within the partition.
NTILE	Divide rows in a partition as equally as possible and assign each row an integer starting from 1 to the argument value.
NTH_VALUE	Return a value evaluated against the nth row in an ordered partition.
PERCENT_RANK	Return the relative rank of the current row (rank-1) / (total rows – 1)
RANK	Rank the current row within its partition with gaps.
ROW_NUMBER	Number the current row within its partition starting from 1.

# Airflow
Operator

# Terms
high cardinality
scd

# Optimization
explain plan
encoding and compression
performance stats

-- teradtaa:
SELECT q.QueryText,AMPCPUTime as CPU_consumption,
MaxAMPCPUTime,
Impact_CPU,
CAST(EXTRACT(HOUR FROM ((q.FirstRespTime - q.StartTime) HOUR(3) TO SECOND(6) ) ) * 3600
     + EXTRACT(MINUTE FROM ((q.FirstRespTime - q.StartTime) HOUR(3) TO SECOND(6) ) ) * 60
     + EXTRACT(SECOND FROM ((q.FirstRespTime - q.StartTime) HOUR(3) TO SECOND(6) ) ) AS DECIMAL(10,2)) AS response_secs
     ,response_secs/60.0  AS response_mins
	 from 
	 DBC.QryLogV q
-- Specify your user name and Query band
WHERE USERNAME = <USER_NAME>
AND QUERYBAND ='UUID=19c1ccc5-ff2a-4d2d-8114-1fb625ad44df;';


# Projects:
 1. Master Python: https://lnkd.in/e5rCbvP8
 2. Learn SQL: https://lnkd.in/efMKFkfX
 3. Learn MySQL: https://lnkd.in/efk-Mi3c
 4. Learn MongoDB: https://lnkd.in/eMKPWtqX
 5. Dominate PySpark: https://lnkd.in/exwA2hKz
 6. Learn Bash, Airflow & Kafka: https://lnkd.in/eyN6u2yd
 7. Learn Git & GitHub: https://lnkd.in/eX_Q8s99
 8. Learn CICD basics: https://lnkd.in/epKGivFY
 9. Decode Data Warehousing: https://lnkd.in/eKnVbFAB
10. Learn DBT: : https://lnkd.in/eG9eaEuE
11. Learn Data Lakes: https://lnkd.in/eQ9xxAJT
12. Learn DataBricks: https://lnkd.in/ePZpCv86
13. Learn Azure Databricks: https://lnkd.in/eBij4akJ
14. Learn Snowflake: https://lnkd.in/erETmtFU
15. Learn Apache NiFi: http://bit.ly/43btwYy
16. Learn Debezium: http://bit.ly/3K6W5gL

Boost Your expertise & Portfolio with 5 Must-Try Projects:
1. Reddit ETL Pipeline - https://lnkd.in/ekmgzGc8
2. Surfline Dashboard - https://lnkd.in/e6AdaDzz
3. Finnhub Streaming Data Pipeline - https://lnkd.in/eCF5kZvE
4. Audiophile End-To-End ELT Pipeline - https://lnkd.in/ercYzXtX
5. Streamify - https://lnkd.in/ePiEwH5k

