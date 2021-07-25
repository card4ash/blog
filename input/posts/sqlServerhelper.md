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


Crud Stored Procedure
=======================

```sql
CREATE TABLE ProcedureTemplate
  (
	TemplateName VARCHAR (200) NULL,
	Template VARCHAR (MAX) NULL
  )
  GO
  INSERT INTO ProcedureTemplate(TemplateName, Template)
  VALUES('Insert','
GO
/****** Object:  StoredProcedure [dbo].[usp_~N_Ins]    Script Date: 3/6/2015 3:32:54 PM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
IF EXISTS(SELECT NAME FROM SYSOBJECTS WHERE NAME = ''usp_~N_Ins'')
  BEGIN
	DROP PROCEDURE usp_~N_Ins
  END
  GO
/* ============================================================================
 Author:      ~U
 Create date: ~D
 Description: Add description

				History
 Date			Name				Comments
 ==============================================================================

 ==============================================================================*/
CREATE PROCEDURE [dbo].[usp_~N_Ins]
(
	~I
)
AS
BEGIN

	SET FMTONLY OFF;
    SET NOCOUNT ON;
    DECLARE @ErrorMessage		VARCHAR(MAX)
			,@ErrorProcedure	VARCHAR(255)
			,@ErrorSeverity		INT
			,@ErrorState		INT
			,@ErrorLine			INT

    BEGIN TRY
		INSERT INTO ~N
		(
			~C
		)
		VALUES
		(
			~V
		)

    END TRY
    BEGIN CATCH

         SELECT  @ErrorMessage		= ERROR_MESSAGE()
				,@ErrorSeverity		= ERROR_SEVERITY()
				,@ErrorProcedure	= ERROR_PROCEDURE()
				,@ErrorState		= ERROR_STATE()
				,@ErrorLine			= ERROR_LINE()

		SET @ErrorMessage = ''Procedure: '' + @ErrorProcedure + space(1) + 
		@ErrorMessage + '' Line: '' + CONVERT(VARCHAR(20),@ErrorLine)


        RAISERROR ( @ErrorMessage, @ErrorSeverity, @ErrorState)

    END CATCH
END
GO
')
GO
 INSERT INTO ProcedureTemplate(TemplateName, Template)
 VALUES('Update','
GO
/****** Object:  StoredProcedure [dbo].[usp_~N_Upd]    Script Date: 3/6/2015 3:32:54 PM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
IF EXISTS(SELECT NAME FROM SYSOBJECTS WHERE NAME = ''usp_~N_Upd'')
  BEGIN
	DROP PROCEDURE usp_~N_Upd
  END
  GO
/* ============================================================================
 Author:      ~U
 Create date: ~D
 Description: Add description

				History
 Date			Name				Comments
 ==============================================================================

 ==============================================================================*/
CREATE PROCEDURE [dbo].[usp_~N_Upd]
(
	@RecID INT,
	~I
)
AS
BEGIN

	SET FMTONLY OFF;
    SET NOCOUNT ON;
         DECLARE @ErrorMessage		VARCHAR(MAX)
				,@ErrorProcedure	VARCHAR(255)
				,@ErrorSeverity		INT
				,@ErrorState		INT
				,@ErrorLine			INT

    BEGIN TRY
		UPDATE ~N
		SET ~C
		WHERE RecID = @RecID

    END TRY
    BEGIN CATCH

         SELECT  @ErrorMessage		= ERROR_MESSAGE()
				,@ErrorSeverity		= ERROR_SEVERITY()
				,@ErrorProcedure	= ERROR_PROCEDURE()
				,@ErrorState		= ERROR_STATE()
				,@ErrorLine			= ERROR_LINE()

		SET @ErrorMessage = ''Procedure: '' + @ErrorProcedure + '' '' + 
		@ErrorMessage + '' Line: '' + CONVERT(VARCHAR(20),@ErrorLine)


        RAISERROR ( @ErrorMessage, @ErrorSeverity, @ErrorState)

    END CATCH
END
GO')
go
 INSERT INTO ProcedureTemplate(TemplateName, Template)
 VALUES('Delete','
GO
/****** Object:  StoredProcedure [dbo].[usp_~N_Del]    Script Date: 3/6/2015 3:32:54 PM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
IF EXISTS(SELECT NAME FROM SYSOBJECTS WHERE NAME = ''usp_~N_Del'')
  BEGIN
	DROP PROCEDURE usp_~N_Del
  END
  GO
/* ============================================================================
 Author:      ~U
 Create date: ~D
 Description: Add description

				History
 Date			Name				Comments
 ==============================================================================

 ==============================================================================*/
CREATE PROCEDURE [dbo].[usp_~N_Del]
(
	@RecID INT

)
AS
BEGIN

	SET FMTONLY OFF;
    SET NOCOUNT ON;
         DECLARE @ErrorMessage		VARCHAR(MAX)
				,@ErrorProcedure	VARCHAR(255)
				,@ErrorSeverity		INT
				,@ErrorState		INT
				,@ErrorLine			INT

    BEGIN TRY
		DELETE ~N
		WHERE RecID = @RecID

    END TRY
    BEGIN CATCH

         SELECT  @ErrorMessage		= ERROR_MESSAGE()
				,@ErrorSeverity		= ERROR_SEVERITY()
				,@ErrorProcedure	= ERROR_PROCEDURE()
				,@ErrorState		= ERROR_STATE()
				,@ErrorLine			= ERROR_LINE()

		SET @ErrorMessage = ''Procedure: '' + @ErrorProcedure + '' '' + 
		@ErrorMessage + '' Line: '' + CONVERT(VARCHAR(20),@ErrorLine)


        RAISERROR ( @ErrorMessage, @ErrorSeverity, @ErrorState)

    END CATCH
END
GO')
GO
 INSERT INTO ProcedureTemplate(TemplateName, Template)
 VALUES('Set','
GO
/****** Object:  StoredProcedure [dbo].[usp_~N_Set]    Script Date: 3/6/2015 3:32:54 PM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
IF EXISTS(SELECT NAME FROM SYSOBJECTS WHERE NAME = ''usp_~N_Set'')
  BEGIN
	DROP PROCEDURE usp_~N_Set
  END
  GO
/* ============================================================================
 Author:      ~U
 Create date: ~D
 Description: Add description

				History
 Date			Name				Comments
 ==============================================================================

 ==============================================================================*/
CREATE PROCEDURE [dbo].[usp_~N_Set]
(
	@RecID INT,
	~I
)
AS
BEGIN

	SET FMTONLY OFF;
    SET NOCOUNT ON;
         DECLARE @ErrorMessage		VARCHAR(MAX)
				,@ErrorProcedure	VARCHAR(255)
				,@ErrorSeverity		INT
				,@ErrorState		INT
				,@ErrorLine			INT

    BEGIN TRY
		SELECT ~C
		FROM ~N
		WHERE ~V

    END TRY
    BEGIN CATCH

         SELECT  @ErrorMessage		= ERROR_MESSAGE()
				,@ErrorSeverity		= ERROR_SEVERITY()
				,@ErrorProcedure	= ERROR_PROCEDURE()
				,@ErrorState		= ERROR_STATE()
				,@ErrorLine			= ERROR_LINE()

		SET @ErrorMessage = ''Procedure: '' + @ErrorProcedure + '' '' + 
		@ErrorMessage + '' Line: '' + CONVERT(VARCHAR(20),@ErrorLine)


        RAISERROR ( @ErrorMessage, @ErrorSeverity, @ErrorState)

    END CATCH
END
')
GO
-------------------------END --------------------------------------------------------------------------------------------------------------------

-------------------------------------Stored procedure here --------------------------------------------------------------------------------------
GO
/****** Object:  StoredProcedure [dbo].[usp_CreateCRUDbyTableName]    Script Date: 3/6/2015 3:32:54 PM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
IF EXISTS(SELECT NAME FROM SYSOBJECTS WHERE NAME = 'usp_CreateCRUDbyTableName')
  BEGIN
	DROP PROCEDURE usp_CreateCRUDbyTableName
  END
GO
/* ============================================================================
 Author:      Mitchell Guzman
 Create date: 2015-04-23
 Description: Creates the CRUD by Table Name

 Example:
 EXEC usp_CreateCRUDbyTableName @TableName = 'MarginRounder'
				History
 Date			Name				Comments
 ==============================================================================

 ==============================================================================*/
CREATE PROCEDURE [dbo].[usp_CreateCRUDbyTableName]
(
  @TableName VARCHAR(255)
)
AS
BEGIN
	--Get Insert Template
	DECLARE	@InputParams		VARCHAR(2000) = ''
			,@OutputParams		VARCHAR(2000) = ''
			,@InsertTemplate	VARCHAR(MAX)
			,@UpdateTemplate	VARCHAR(MAX)
			,@DeleteTemplate	VARCHAR(MAX)
			,@SetTemplate		VARCHAR(MAX)
			,@Columns			VARCHAR(MAX) = ''
			,@Columns1			VARCHAR(MAX) = ''
			,@User				VARCHAR(50) = ''
			,@CurDate			VARCHAR(20)
			,@ErrorMessage		VARCHAR(MAX)
			,@ErrorProcedure	VARCHAR(255)
			,@ErrorSeverity		INT
			,@ErrorState		INT
			,@ErrorLine			INT

	BEGIN TRY
		--Input params
		SELECT @User = REPLACE(SYSTEM_USER,'MIDDLE_EARTH\',''), 
                       @CurDate = CONVERT(VARCHAR(20),GETDATE(),100)

		SELECT  @InputParams = @InputParams + CHAR(9) + '@' + sc.name  + ' ' +  
		UPPER(st.name)  + ' ' + case when sc.user_type_id in (167,175,231,239) 
		then '(' + convert(varchar(20),CASE WHEN CONVERT(VARCHAR(6),sc.max_length) = '-1' 
		THEN 'MAX' ELSE CONVERT(VARCHAR(6),sc.max_length) END ) + ')' ELSE '' END + '' + 
		CASE WHEN sc.user_type_id in(59,60,106,108,122) THEN '(' + convert(varchar(5),sc.precision) + 
		',' + convert(varchar(5),sc.scale) + ')' ELSE '' END +  ',' + CHAR(10)  
		FROM sys.columns SC (NOLOCK) JOIN SYSOBJECTS SO(NOLOCK) ON SO.ID = SC.object_ID 
		JOIN sys.types ST (NOLOCK) ON ST.user_TYPE_ID = SC.user_TYPE_ID  WHERE so.type = 'U'  
		and so.name = @TableName and sc.is_identity = 0 order by Column_ID
		SELECT @InputParams = LEFT(@InputParams,LEN(@InputParams) - 2)

		SELECT @OutputParams = @OutputParams + CHAR(9)  + sc.name  + ' ' +  UPPER(st.name)  + 
		' ' + case when sc.user_type_id in (167,175,231,239) 
        then '(' + convert(varchar(20),sc.max_length) + ')' 
		ELSE '' END + '' + CASE WHEN sc.user_type_id in(59,60,106,108,122) THEN '(' + 
		convert(varchar(5),sc.precision) + ',' + convert(varchar(5),sc.scale) + ')' ELSE '' 
		END +  ',' + CHAR(10)  FROM sys.columns SC (NOLOCK) JOIN SYSOBJECTS SO(NOLOCK) 
		ON SO.ID = SC.object_ID JOIN sys.types ST (NOLOCK) ON ST.user_TYPE_ID = SC.user_TYPE_ID  
		WHERE so.type = 'U'  and so.name = @TableName and sc.is_identity = 0 order by Column_ID
		SELECT @OutputParams = LEFT(@OutputParams,LEN(@OutputParams) - 2)

		SELECT @Columns = @Columns + CHAR(9) +  SC.NAME + ',' + CHAR(10) + CHAR(9) + CHAR(9)
		FROM SYS.OBJECTS SO (NOLOCK)
		JOIN SYS.COLUMNS SC (NOLOCK) ON SC.object_id = SO.object_id
		WHERE SO.NAME = @TableName
		 AND sc.is_identity = 0
		 ORDER BY SC.NAME

		SET @Columns = LEFT(@Columns,LEN(@Columns) - 4)

		SELECT @Columns1 = @Columns1 + CHAR(9) + '@' +  SC.NAME + ',' + CHAR(10)+ CHAR(9) + CHAR(9)
		FROM SYS.OBJECTS SO (NOLOCK)
		JOIN SYS.COLUMNS SC (NOLOCK) ON SC.object_id = SO.object_id
		WHERE SO.NAME = @TableName
		 AND sc.is_identity = 0
		ORDER BY SC.NAME

		SET @Columns1 = LEFT(@Columns1,LEN(@Columns1) - 4)
		---------------------------------------
		---Insert
		---------------------------------------
		SELECT @InsertTemplate = Template
		FROM ProcedureTemplate t (NOLOCK)
		WHERE TemplateName = 'Insert'

		SET @InsertTemplate = REPLACE(@InsertTemplate,'~N',@TableName)
		SET @InsertTemplate = REPLACE(@InsertTemplate,'~U',@User)
		SET @InsertTemplate = REPLACE(@InsertTemplate,'~I',@InputParams)
		SET @InsertTemplate = REPLACE(@InsertTemplate,'~C',@Columns)
		SET @InsertTemplate = REPLACE(@InsertTemplate,'~V',@Columns1)
		SET @InsertTemplate = REPLACE(@InsertTemplate,'~D',@CurDate)

		SELECT @InsertTemplate

		------------------------------------
		---Update
		------------------------------------
		SELECT @UpdateTemplate = Template
		FROM ProcedureTemplate t (NOLOCK)
		WHERE TemplateName = 'Update'

		SET @Columns1 = ''

		SELECT @Columns1 = @Columns1 + CHAR(9) + SC.NAME + ' = ' + '@' +  
		SC.NAME + ',' + CHAR(10)+ CHAR(9) + CHAR(9)+ CHAR(9)
		FROM SYS.OBJECTS SO (NOLOCK)
		JOIN SYS.COLUMNS SC (NOLOCK) ON SC.object_id = SO.object_id
		WHERE SO.NAME = @TableName
		 AND sc.is_identity = 0
		ORDER BY SC.NAME

		SET @Columns1 = LEFT(@Columns1,LEN(@Columns1) - 5)

		SET @UpdateTemplate = REPLACE(@UpdateTemplate,'~N',@TableName)
		SET @UpdateTemplate = REPLACE(@UpdateTemplate,'~U',@User)
		SET @UpdateTemplate = REPLACE(@UpdateTemplate,'~I',@InputParams)
		SET @UpdateTemplate = REPLACE(@UpdateTemplate,'~C',@Columns1)
		SET @UpdateTemplate = REPLACE(@UpdateTemplate,'~D',@CurDate)

		SELECT @UpdateTemplate

		------------------------------------
		---DELETE
		------------------------------------
		SELECT @DeleteTemplate = Template
		FROM ProcedureTemplate t (NOLOCK)
		WHERE TemplateName = 'Delete'

		SET @DeleteTemplate = REPLACE(@DeleteTemplate,'~N',@TableName)
		SET @DeleteTemplate = REPLACE(@DeleteTemplate,'~U',@User)
		SET @DeleteTemplate = REPLACE(@DeleteTemplate,'~D',@CurDate)

		SELECT @DeleteTemplate

		------------------------------------
		---SET
		------------------------------------
		SELECT @SetTemplate = Template
		FROM ProcedureTemplate t (NOLOCK)
		WHERE TemplateName = 'Set'

		SET @InputParams = ''
		SELECT  @InputParams = @InputParams + CHAR(9) + '@' + sc.name  + ' ' +  
		UPPER(st.name)  + ' ' + case when sc.user_type_id in (167,175,231,239) 
		then '(' + convert(varchar(20),CASE WHEN CONVERT(VARCHAR(20),sc.max_length) = '-1' 
		THEN 'MAX' ELSE CONVERT(VARCHAR(20),sc.max_length) END) + ')' ELSE '' END + '' + CASE 
		WHEN sc.user_type_id in(59,60,106,108,122) THEN '(' + convert(varchar(5),sc.precision) + ',' + 
		convert(varchar(5),sc.scale) + ')' ELSE '' END +  CHAR(9) + ' = NULL,' + CHAR(10)    
		FROM sys.columns SC (NOLOCK) JOIN SYSOBJECTS SO(NOLOCK) ON SO.ID = 
		SC.object_ID JOIN sys.types ST (NOLOCK) ON ST.user_TYPE_ID = SC.user_TYPE_ID  
		WHERE so.type = 'U'  and so.name = @TableName and sc.is_identity = 0 order by Column_ID
		SELECT @InputParams = LEFT(@InputParams,LEN(@InputParams) - 2)


		SET @Columns = ''
		SELECT @Columns = @Columns + CHAR(9) +  SC.NAME + ',' + CHAR(10) + CHAR(9) + CHAR(9) + CHAR(9)
		FROM SYS.OBJECTS SO (NOLOCK)
		JOIN SYS.COLUMNS SC (NOLOCK) ON SC.object_id = SO.object_id
		WHERE SO.NAME = @TableName
		ORDER BY SC.name
		 --AND sc.is_identity = 0

		SET @Columns1 = ''
		SELECT @Columns1 = @Columns1 + CHAR(9) +  '(@' + SC.NAME + ' IS NULL OR ' + 
		SC.NAME + ' =  @' + SC.NAME + ')' + CHAR(10)  + CHAR(9) + CHAR(9)  + ' AND'
		FROM SYS.OBJECTS SO (NOLOCK)
		JOIN SYS.COLUMNS SC (NOLOCK) ON SC.object_id = SO.object_id
		WHERE SO.NAME = @TableName
		ORDER BY SC.name

		IF RIGHT(@Columns1,3) = 'AND'
		  BEGIN
			SET @Columns1 = LEFT(@Columns1,LEN(@Columns1) - 3)
		  END
		SET @Columns = LEFT(@Columns,LEN(@Columns) - 5)

		SET @SetTemplate = REPLACE(@SetTemplate,'~N',@TableName)
		SET @SetTemplate = REPLACE(@SetTemplate,'~C',@Columns)
		SET @SetTemplate = REPLACE(@SetTemplate,'~U',@User)
		SET @SetTemplate = REPLACE(@SetTemplate,'~I',@InputParams)
		SET @SetTemplate = REPLACE(@SetTemplate,'~V',@Columns1)
		SET @SetTemplate = REPLACE(@SetTemplate,'~D',@CurDate)

		SELECT @SetTemplate
	 END TRY
	 BEGIN CATCH

         SELECT  @ErrorMessage		= ERROR_MESSAGE()
				,@ErrorSeverity		= ERROR_SEVERITY()
				,@ErrorProcedure	= ERROR_PROCEDURE()
				,@ErrorState		= ERROR_STATE()
				,@ErrorLine			= ERROR_LINE()

		SET @ErrorMessage = 'Procedure: ' + @ErrorProcedure + ' ' + 
		@ErrorMessage + ' Line: ' + CONVERT(VARCHAR(6),@ErrorLine)

        RAISERROR ( @ErrorMessage, @ErrorSeverity, @ErrorState)

    END CATCH
	END

```

then

```EXEC usp_CreateCRUDbyTableName @TableName = '' ```

Links
=============
(https://www.sqlshack.com/how-to-connect-and-use-microsoft-sql-server-express-localdb/)[https://www.sqlshack.com/how-to-connect-and-use-microsoft-sql-server-express-localdb/]