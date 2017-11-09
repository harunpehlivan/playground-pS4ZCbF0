# SQL 2017 New features
![SQL2017](https://www.codeproject.com/KB/database/1210268/SQL2017_1.png)
## Background
SQL Server 2017 released on (general availability release) on October 2017 (Really? we are just trying to understand features of SQL 2016, ok, jokes apart :)) SQL 2017 released in parts, its first part, that is SQL 2017 CTP 1.0 (SQL version 14.0.1.246) was released on Nov-2016 (How can they release 2017 version in 2016?). Until now, SQL 2017 has come up with its 10 releases, the current release is SQL 2017 Release GA (SQL version 14.0.1000.169) [check here](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-release-notes#GA) for available releases in October 2017.

*(**[See here for What's new in SQL 2016](https://www.codeproject.com/Articles/1111938/SQL-New-Features)**)*

## Introduction
This version of SQL mainly connects to Linux and brings the power of SQL to Linux. In short, now you can install SQL 2017 on Linux (it's a great move), additionally SQL can be used in linux based docker container also. In this build, SQL gives you development language choice, you can develop it on-premise or cloud-based.

In this build, SQL also improves performance, scalability and features, in each of its parts, like Database Engine, Integration Services, Master Data Services, Analysis Services, etc. In this article, we will take a look at them one by one.

Following are some Database Engine new features.

### identity_cache
![identity_cache](https://www.codeproject.com/KB/database/1210268/IDENTITY_CACHE.png)

This option helps you to avoid discrepancy in values of identity columns, in the case of server shut down unexpectedly or any failover occur, or even if you switch to some secondary server. This option is used with '**ALTER DATABASE SCOPED CONFIGURATION**' statement. This statement is used to enable database configuration settings. The syntax is as follows:

```javascript
ALTER DATABASE SCOPED CONFIGURATION
{      
     {  [ FOR SECONDARY] SET <set_options>  }  
}
| CLEAR PROCEDURE_CACHE
| SET < set_options >
[;]  

< set_options > ::=  
{
    MAXDOP = { <value> | PRIMARY}  
    | LEGACY_CARDINALITY_ESTIMATION = { ON | OFF | PRIMARY}  
    | PARAMETER_SNIFFING = { ON | OFF | PRIMARY}  
    | QUERY_OPTIMIZER_HOTFIXES = { ON | OFF | PRIMARY}
    | IDENTITY_CACHE = { ON | OFF }
}
```

### Adaptive Query Processing Improvements
![Adaptive](https://www.codeproject.com/KB/database/1210268/Q_Process.png)

Do you want to improve query execution performance? Then this feature will help you. This feature is supported in SQL server and Azure SQL databases.

Do you know normal optimization flow of SQL Query execution? If no, then check the below steps:

1. Initially Query optimizer calculates all feasible query execution plan for a given query.
2. Then it gets the lowest cost /fastest plan.
3. Finally, the lowest estimated plan will be chosen by the optimizer to execute query and then execution started.

Now in this process, there are some drawbacks:

1. If lowest estimated execution plan may go wrong, then it affects performance.
2. If insufficient memory gets allocated for processing, then it leads to spills to disk.
3. If assigned memory is incorrectly sized, then it leads to memory waste.

To overcome all these issues, you can use this SQL 2017 feature.

*Batch Mode Memory Grant Feedback*

This feedback (technique) re-calculate (actually) required memory for the execution plan and grant it from cache

*Batch Mode Adaptive Joins*

To execute plan faster, it has Hash join and nested loop join, after scanning the first input of execution plan, it decides which join to use to produce output in optimal speed.

*Interleaved Execution*
In interleaved execution, it takes a 'pause' in optimization execution plan, if it encountered multi-statement table valued functions. Then, it just calculates perfect cardinality and then resumes optimization.

### Automatic Tuning
![Auto_Tunbe](https://www.codeproject.com/KB/database/1210268/Auto_Tune.png)

As the words suggest, this feature checks out the problems in query performance, identifies them and fixes them with the recommended solution. There are a couple of automatic tuning techniques available in this feature:

1. Automatic Correction (Plan)
2. Automatic Management (Index)

*Automatic Correction (Plan)*

This plan is available in SQL 2017 DB, it basically scans problematic plan (performance wise) and then fixes it with the recommended solution

*Automatic Management (Index)*

This feature is available in SQL 2017 Azure DB, here it basically identifies and corrects the order of indices. The indices which are removed and indices which are added.

### Graph DB
![Graph DB](https://www.codeproject.com/KB/database/1210268/Graph_DB.png)
The graph database feature is newly introduced in SQL 2017.

*What is Graph DB ?*

Basically, it is the collection of edge and nodes, where edge is the relationship between nodes and the node is just an entity, so single edge can connect with multiple nodes. A Graph db acts just like a relational database, you can use Graph db in the following cases:

1. If you have a data in hierarchical format and you want to save multiple parent for a node
2. If you need to check and analyze interlinked relationship and data
3. If you have many to many relationships

Here, keyword **MATCH** is used to query the graph table and sort the data, with the help of only a single query, user can query across the graph and relational data, for more details, checkout [here](https://docs.microsoft.com/en-us/sql/relational-databases/graphs/sql-graph-overview)

## Always Available (Cross Database Access)
With the help this feature, cross database transactions are now possible against different SQL instances (same SQL instance can be connected to different instances). It also supports database distributed transactions. [SQL 2016 also supports cross database access but only for only in same SQL server instance.]

## DTA Improvements
![DTA](https://www.codeproject.com/KB/database/1210268/DTA.png)

In SQL 2017, there is performance improvement in Database tuning advisor (DTA) with additional options for tuning advisor.

*for those who don't know what is Database tuning advisor:*

It is a database engine that inspects the query (which is processed) and then put forward in a way by which you can improve its performance, may be by changing database structure (e.g. index, keys). There are couple of ways in which you can use it:

Please refer to the [documentation](https://tech.io/doc) to learn more about adding programming exercises within your contribution.

- using GUI (interface)
- using command utility

If you want to know more about it, then you can [check here](https://docs.microsoft.com/en-us/sql/tools/dta/lesson-1-1-launching-database-engine-tuning-advisor).

## New String Members (Functions)
SQL 2017 introduces some new *string* functions like TRANSLATE, CONCAT_WS, STRING_AGG, TRIM. Let's take a a look at them one by one.

### TRANSLATE
It basically takes a string as input and then translates the characters with some new characters, see below syntax:

```javascript
TRANSLATE ( inputString, characters, translations)
```

in the above syntax length of 'characters' should be same as 'translations', otherwise the above function will return an error.
e.g.

```javascript
TRANSLATE ('6*{10+10}/[6-4]','[]{}','()()')
```

output of the above example will be **6*(10+10)/(6-4)**.

so, we can see **curly** and **square** braces translated to round braces.

[This function acts the same as **REPLACE** but it is simpler to use than **REPLACE**, if we want the same output using **REPLACE**, then we need to write it as below, which is difficult to understand:]

```javascript
SELECT REPLACE(REPLACE(REPLACE(REPLACE('6*{10+10}/[6-4]','{','('), '}', ')'), '[', '('), ']', ')');
```

### CONCATE_WS
It simply concats all input arguments with the specified input delimiter. See below syntax:

```javascript
CONCAT_WS ( separator, argument1, argument1 [, argumentN]… )
```

It produces the single string by concatenating all arguments with the help of the separators, it needs minimum 2 arguments to produce output, otherwise error will be thrown:

For example:

```javascript
SELECT CONCAT_WS(',','Count numers', 'one', 'two', 'three', 'four' ) AS counter;
```

The output of above syntax will be:

**one, two, three, four**

You can also use database column names instead of hard coded strings.

### TRIM
(finally :)) SQL 2017 introduced this new function. It simply works as the C# trim function, remove all spaces from start and end of the string. The syntax is as below:

```javascript
SELECT TRIM('     trim me    ') AS result;
```

The output of the above syntax will be:

**trim me**

This function will not remove spaces that lay in between the string.

### STRING_AGG
It concatenate values of string with the help of separators and does not add separator at the end of the string. The input could be VARCHAR, NVARCHAR, optionally you can specify the order of the result with the help of WITHIN GROUP clause.

See the syntax below:

```javascript
STRING_AGG ( expression, separator ) [ <order_clause> ]

<order_clause> ::=  
    WITHIN GROUP ( ORDER BY <order_by_expression_list> [ ASC | DESC ] )
```

Check out the below example:

```javascript
SELECT city,
    STRING_AGG (name, ';') WITHIN GROUP (ORDER BY name ASC) AS names
FROM Students GROUP BY city;
```

In the above example, we have concatenated all names separated by semicolon ( ; ) WITHIN GROUP helps us to make it order by. The output will be displayed as follows:

| City | Names |
| ------ | ----------- |
| City 1   | name1;name2;name3;name4 |
| City 1   | name1;name2;name3;name4 |

## What's New in SSRS (Reporting Services) in SQL 2017
![SSRS](https://www.codeproject.com/KB/database/1210268/SSRS.png)

- From now onwards, there is no SSRS setup available with SQL server setup, you need to download it from download center [here].
- Now, Query designer has the support of DAX, now native DAX queries can be created against SSAS (Analysis services), this comes under the latest release of SQL tools and report builder
- SSRS now supports RESTful API that support OpenAPI compliant, API can be found [here](https://app.swaggerhub.com/apis/microsoft-rs/SSRS/2.0)
- You can now add attachments to your comments.
- You can add comments to reports.
- Improvement in reporting services web portal (this feature is already available in SQL 2016)

## What's New in SSIS (Integration Services) in SQL 2017
- You can now execute SSIS package on Linux, additionally loading, extracting and transforming of data is now possible on Linux
- Scale out feature allows complex integration to multiple machine with high performance package execution, even multiple package execution request is possible. Scale out management is carried out all operation with the help of Scale Out Master and Scale Out Workers, for more details, [check here](https://docs.microsoft.com/en-us/sql/integration-services/scale-out/integration-services-ssis-scale-out)
- Microsoft DAX and CRM online now gets support of ODataConnection manager and oDataSource

## What's New in SSAS (Analysis Services) in SQL 2017
- New Get Data interface is introduced in SQL 2017 which is similar to MS Excel, power BI, it also provides transformation of data and data mashup, you can do it by using query builder and M expressions
- Tabular mode for SSAS is introduced in SQL 2012, now it become more powerful in SQL 2017
- SQL 2017 come up with new 'Encoding hints' feature, which is used to optimize large in-memory tabular data
- Performance improvement for power PIVOT

For more information on SSAS in SQL 2017, switch [here](https://docs.microsoft.com/en-us/sql/analysis-services/what-s-new-in-sql-server-analysis-services-2017)

## Machine Learning
![SSRS](https://www.codeproject.com/KB/database/1210268/MachineLearning.png)

We know SQL 2016 is now supporting R services, it has now renamed to SQL Server Machine learning services. With the help of this, you can easily use R or Python scripts using SQL server.

In this new feature, Python can be run in stored procedure, even you can remotely execute it using SQL server, this will really helpful to Python developers.

Currently, this feature is not supported on Linux, we need to wait for the next announcement.

To use machine learning in an intelligent and powerful way, SQL uses the following solutions.

- revoscalepy
- microsoftml

**revoscalepy** is the new library to give base to high performance algorithms, computing and remote contexts, it is basically based on RevoScaleR (It is a package of R services).

**microsoftml** is the Microsoft R server package that supports machine language algorithms, Microsoft developed this library for internal machine learning, but over the years, it has improved and supports fast data streaming, large text transformations, etc.

## Linux Support
![Linux Support](https://www.codeproject.com/KB/database/1210268/Linux.png)

Basically, the main goal of this build is to release product on Linux, this build of SQL release with the name "SQL 2017 on Linux and Windows", here are few key features of "SQL on Linux":

Core database engine capabilities
- Support to IPV6
- NFS support
- AD authentication on linux
- Support for encryption
- Installation of SSIS package on Linux is possible
- configuration tool (command line) available MSSQL-conf
- Seamless and unattended installation
- SQL for Visual studio code (VS Code is already available on Linux)
- Cross platform script generator

To know more about SQL on Linux, switch [here](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-overview).

**References**

[MSDN and Microsoft sites](https://www.microsoft.com/en-us/sql-server/sql-server-2017)

**Finally**
There is lot to talk and learn about SQL Server, we will continue this journey in the next version of this article, till then enjoy it. Suggestion and queries are always welcome.

Happy querying!

-koolprasad