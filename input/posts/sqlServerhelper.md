Title: Sql Server Important Query
Published: 07/20/2019
Tags:
    - SQL Server
    - sqlcmd
    - Introduction
    - C#
    - POCO
    - Sql
    - Introduction
---

Get All Table Row Count
=========================

**method1:**

```sql
CREATE TABLE #counts
(
    table_name varchar(255),
    row_count int
)

EXEC sp_MSForEachTable @command1='INSERT #counts (table_name, row_count) SELECT ''?'', COUNT(*) FROM ?'
SELECT table_name, row_count FROM #counts ORDER BY table_name, row_count DESC
DROP TABLE #counts
```

**method2:**

```sql
SELECT o.NAME,
  i.rowcnt 
FROM sysindexes AS i
  INNER JOIN sysobjects AS o ON i.id = o.id 
WHERE i.indid < 2  AND OBJECTPROPERTY(o.id, 'IsMSShipped') = 0
ORDER BY o.NAME
```

**method3:**

```sql
-- Shows all user tables and row counts for the current database 
-- Remove is_ms_shipped = 0 check to include system objects 
-- i.index_id < 2 indicates clustered index (1) or hash table (0) 
SELECT o.name,
  ddps.row_count 
FROM sys.indexes AS i
  INNER JOIN sys.objects AS o ON i.OBJECT_ID = o.OBJECT_ID
  INNER JOIN sys.dm_db_partition_stats AS ddps ON i.OBJECT_ID = ddps.OBJECT_ID
  AND i.index_id = ddps.index_id 
WHERE i.index_id < 2  AND o.is_ms_shipped = 0 ORDER BY o.NAME 
```

Describe Table
*****************

```sql
exec sp_help 'table_name';
```

```sql
select *
from INFORMATION_SCHEMA.COLUMNS
where TABLE_NAME='Mct_GL'
```

Force Database Delete(Database Is in Use)
*******************************************

```sql
Use master;
ALTER database DatabaseName set offline with ROLLBACK IMMEDIATE;
DROP database DatabaseName;
```

Get table constraints
**********************

```sql
Select C.*, (Select definition From sys.default_constraints Where object_id = C.object_id) As dk_definition,
(Select definition From sys.check_constraints Where object_id = C.object_id) As ck_definition,
(Select name From sys.objects Where object_id = D.referenced_object_id) As fk_table,
(Select name From sys.columns Where column_id = D.parent_column_id And object_id = D.parent_object_id) As fk_col
From sys.objects As C
Left Join (Select * From sys.foreign_key_columns) As D On D.constraint_object_id = C.object_id 
Where C.parent_object_id = (Select object_id From sys.objects Where type = 'U'
And name = 'Table Name Here');
```


Database Size estimation
==============================

```
  SELECT 
    t.NAME AS TableName,
    s.Name AS SchemaName,
    p.rows,
    SUM(a.total_pages) * 8 AS TotalSpaceKB, 
    CAST(ROUND(((SUM(a.total_pages) * 8) / 1024.00), 2) AS NUMERIC(36, 2)) AS TotalSpaceMB,
    SUM(a.used_pages) * 8 AS UsedSpaceKB, 
    CAST(ROUND(((SUM(a.used_pages) * 8) / 1024.00), 2) AS NUMERIC(36, 2)) AS UsedSpaceMB, 
    (SUM(a.total_pages) - SUM(a.used_pages)) * 8 AS UnusedSpaceKB,
    CAST(ROUND(((SUM(a.total_pages) - SUM(a.used_pages)) * 8) / 1024.00, 2) AS NUMERIC(36, 2)) AS UnusedSpaceMB
FROM 
    sys.tables t
INNER JOIN      
    sys.indexes i ON t.OBJECT_ID = i.object_id
INNER JOIN 
    sys.partitions p ON i.object_id = p.OBJECT_ID AND i.index_id = p.index_id
INNER JOIN 
    sys.allocation_units a ON p.partition_id = a.container_id
LEFT OUTER JOIN 
    sys.schemas s ON t.schema_id = s.schema_id
WHERE 
    t.NAME NOT LIKE 'dt%' 
    AND t.is_ms_shipped = 0
    AND i.OBJECT_ID > 255 
GROUP BY 
    t.Name, s.Name, p.Rows
ORDER BY 
    TotalSpaceMB DESC, t.Name
```



sqlcmd Utility
=====================

Download and Install appropriate version of sqlcmd.Check the following atricle about sqlcmd utility and latest vesion of download link

[https://docs.microsoft.com/en-us/sql/tools/sqlcmd-utility?view=sql-server-ver15](https://docs.microsoft.com/en-us/sql/tools/sqlcmd-utility?view=sql-server-ver15)

Connecting and Running Script using sqlcmd
*************************************************

Example:

```sql
sqlcmd -S.\SQLEXPRESS14 -i e:\ASH\test\posinventoryfullscript.sql -o e:\ASH\test\posoutput.txt
```


SQL Server localdb Cheet sheet
====================================

```
    SqlLocalDb info
    SQLLocalDB info v11.0
    SQLLocalDB share v11.0 Mare
    SQLLocalDB info v11.0
    SqlLocalDB create NewInstance
    SqlLocalDB info NewInstance
    SqlLocalDB create Test 11.0
    SqlLocalDB info Test
    SQLLocalDB create “Test instance” 
    SqlLocalDB start Test
```

SQL ServerManagement Studio Connect:
	ServerName:(LocalDB)\ followed by the name of the LocalDB instance (v11.0)
	Example:(LocalDB)\Test
	Example:(LocalDB)\v11.0
And Windows Authentication

```
    SqlLocalDB stop MSSQLLocaDB
    cd C:\Program Files\Microsoft SQL Server\110\Tools\Binn\
    sqlcmd -S (localDB)\v11.0
```

```
    CREATE DATABASE TestSQlCMD;
    GO
    USE TestSQlCMD;
    GO
    CREATE TABLE dbo.Pesron(
    ID INT,
    Name VARCHAR(50)
    );
    GO
```





Generate class from database table
===========================================

```sql
declare @TableName sysname = 'Division'
declare @Result varchar(max) = 'public class ' + @TableName + '
{'

select @Result = @Result + '
    public ' + ColumnType + NullableSign + ' ' + ColumnName + ' { get; set; }
'
from
(
    select 
        replace(col.name, ' ', '_') ColumnName,
        column_id ColumnId,
        case typ.name 
            when 'bigint' then 'long'
            when 'binary' then 'byte[]'
            when 'bit' then 'bool'
            when 'char' then 'string'
            when 'date' then 'DateTime'
            when 'datetime' then 'DateTime'
            when 'datetime2' then 'DateTime'
            when 'datetimeoffset' then 'DateTimeOffset'
            when 'decimal' then 'decimal'
            when 'float' then 'double'
            when 'image' then 'byte[]'
            when 'int' then 'int'
            when 'money' then 'decimal'
            when 'nchar' then 'string'
            when 'ntext' then 'string'
            when 'numeric' then 'decimal'
            when 'nvarchar' then 'string'
            when 'real' then 'float'
            when 'smalldatetime' then 'DateTime'
            when 'smallint' then 'short'
            when 'smallmoney' then 'decimal'
            when 'text' then 'string'
            when 'time' then 'TimeSpan'
            when 'timestamp' then 'long'
            when 'tinyint' then 'byte'
            when 'uniqueidentifier' then 'Guid'
            when 'varbinary' then 'byte[]'
            when 'varchar' then 'string'
            else 'UNKNOWN_' + typ.name
        end ColumnType,
        case 
            when col.is_nullable = 1 and typ.name in ('bigint', 'bit', 'date', 'datetime', 'datetime2', 'datetimeoffset', 'decimal', 'float', 'int', 'money', 'numeric', 'real', 'smalldatetime', 'smallint', 'smallmoney', 'time', 'tinyint', 'uniqueidentifier') 
            then '?' 
            else '' 
        end NullableSign
    from sys.columns col
        join sys.types typ on
            col.system_type_id = typ.system_type_id AND col.user_type_id = typ.user_type_id
    where object_id = object_id(@TableName)
) t
order by ColumnId

set @Result = @Result  + '
}'

print @Result
```

Links
=============
(https://www.sqlshack.com/how-to-connect-and-use-microsoft-sql-server-express-localdb/)[https://www.sqlshack.com/how-to-connect-and-use-microsoft-sql-server-express-localdb/]