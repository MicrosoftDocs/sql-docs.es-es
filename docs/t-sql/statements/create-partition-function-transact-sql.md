---
description: CREATE PARTITION FUNCTION (Transact-SQL)
title: CREATE PARTITION FUNCTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/19/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE PARTITION FUNCTION
- PARTITION
- PARTITION FUNCTION
- PARTITION_FUNCTION_TSQL
- PARTITION_TSQL
- CREATE_PARTITION_FUNCTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- RANGE RIGHT partition functions
- RANGE LEFT partition functions
- partitioned indexes [SQL Server], functions
- functions [SQL Server], creating
- partition functions [SQL Server], creating
- partitioned tables [SQL Server], functions
- CREATE PARTITION FUNCTION statement
ms.assetid: 9dfe8b76-721e-42fd-81ae-14e22258c4f2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7e1c2b7c6c3b6e12ae3865a5c35ad8f9904cba4f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188564"
---
# <a name="create-partition-function-transact-sql"></a>CREATE PARTITION FUNCTION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crea una función en la base de datos actual que asigna las filas de una tabla o un índice a particiones según los valores de una columna especificada. El uso de CREATE PARTITION FUNCTION constituye el primer paso para la creación de una tabla o un índice con particiones. En [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], una tabla o un índice puede tener un máximo de 15.000 particiones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE PARTITION FUNCTION partition_function_name ( input_parameter_type )  
AS RANGE [ LEFT | RIGHT ]   
FOR VALUES ( [ boundary_value [ ,...n ] ] )   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *partition_function_name*  
 Es el nombre de la función de partición. Los nombres de las funciones de partición deben ser únicos en la base de datos y ajustarse a las reglas para los [identificadores](../../relational-databases/databases/database-identifiers.md).  
  
 *input_parameter_type*  
 Es el tipo de datos de la columna utilizada para la partición. Todos los tipos de datos son válidos para su uso como columnas de partición, excepto **text**, **ntext**, **image**, **xml**, **timestamp**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)**, tipos de datos de alias o tipos de datos definidos por el usuario CLR.  
  
 La columna en sí, conocida como columna de partición, se especifica en la instrucción CREATE TABLE o CREATE INDEX.  
  
 *boundary_value*  
 Especifica los valores de límite para cada partición de una tabla o un índice con particiones que utiliza *partition_function_name*. Si *boundary_value* está vacío, la función de partición asigna la tabla o el índice completos utilizando *partition_function_name* a una sola partición. Solo es posible utilizar una columna de partición, especificada en una instrucción CREATE TABLE o CREATE INDEX.  
  
 *boundary_value* es una expresión constante que puede hacer referencia a variables. Éstas incluyen variables o funciones de tipo definido por el usuario y funciones definidas por el usuario. No pueden hacer referencia a expresiones de [!INCLUDE[tsql](../../includes/tsql-md.md)]. *boundary_value* debe coincidir con el tipo de datos proporcionado en *input_parameter_type* o ser susceptible de convertirse de forma implícita a él, y no se puede truncar durante la conversión implícita de forma que el tamaño y la escala del valor no coincidan con el de su *input_parameter_type* correspondiente.  

> [!NOTE]  
>  Si *boundary_value* está compuesto por los literales **datetime** o **smalldatetime**, estos se evalúan suponiendo que el idioma de la sesión sea us_english. Este comportamiento se ha desaprobado. Para asegurarse de que la definición de la función de partición se comporta según lo esperado en todos los idiomas de la sesión, se recomienda usar constantes que se interpreten de la misma forma en todas las configuraciones de idioma, como el formato aaaammdd; o bien, convierta literales de forma explícita a un estilo específico. Para determinar el idioma de la sesión del servidor, ejecute `SELECT @@LANGUAGE`.
>
> Para obtener más información, vea [Conversión no determinista de las cadenas de fecha literales en valores DATE](../data-types/nondeterministic-convert-date-literals.md).
  
 *...n*  
 Especifica el número de valores proporcionados por *boundary_value*, que no puede superar 14 999. El número de particiones creadas es igual a *n* + 1. Los valores no se tienen que enumerar en orden. Si los valores no están en orden, [!INCLUDE[ssDE](../../includes/ssde-md.md)] los ordena, crea la función y muestra una advertencia para indicar que los valores proporcionados no estaban en orden. Si *n* incluye valores duplicados, el motor de base de datos devuelve un error.  
  
 **LEFT** | RIGHT  
 Especifica el lado de cada intervalo de valores de límite, derecho o izquierdo, al que pertenece *boundary_value* [ **,** _...n_ ], cuando [!INCLUDE[ssDE](../../includes/ssde-md.md)] ordena los valores del intervalo en orden ascendente de izquierda a derecha. Si no se especifica, el valor predeterminado es LEFT.  
  
## <a name="remarks"></a>Observaciones  
 El ámbito de una función de partición está limitado a la base de datos en la que se crea. Dentro de la base de datos, las funciones de partición residen en un espacio de nombres independiente de las demás funciones.  
  
 Las filas cuya columna de partición tenga valores NULL se colocan en la partición situada más a la izquierda, a menos que se especifique NULL como un valor de límite y se indique RIGHT. En este caso, la partición situada más a la izquierda es una partición vacía y los valores NULL se colocan en la siguiente.  
  
## <a name="permissions"></a>Permisos  
 Para ejecutar CREATE PARTITION FUNCTION, se puede utilizar cualquiera de los siguientes permisos:  
  
-   Permiso ALTER ANY DATASPACE. De forma predeterminada, este permiso corresponde a los miembros del rol fijo de servidor **sysadmin** y a los roles fijos de base de datos **db_owner** y **db_ddladmin** .  
  
-   Permiso CONTROL o ALTER en la base de datos en la que se está creando la función de partición.  
  
-   Permiso CONTROL SERVER o ALTER ANY DATABASE en el servidor de la base de datos en la que se está creando la función de partición.  
  
##  <a name="examples"></a><a name="BKMK_examples"></a> Ejemplos  
  
### <a name="a-creating-a-range-left-partition-function-on-an-int-column"></a>A. Crear una función de partición RANGE LEFT en una columna int  
 La siguiente función de partición realizará cuatro particiones en una tabla o un índice.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF1 (int)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
```  
  
 En la tabla siguiente se muestra cómo se crearían particiones en una tabla que usa esta función de partición en la columna de partición **col1**.  
  
|Partition|1|2|3|4|  
|---------------|-------|-------|-------|-------|  
|**Valores**|**col1** <= `1`|**col1** > `1` AND **col1** <= `100`|**col1** > `100` AND **col1** <=`1000`|**col1** > `1000`|  
  
### <a name="b-creating-a-range-right-partition-function-on-an-int-column"></a>B. Crear una función de partición RANGE RIGHT en una columna int  
 La siguiente función de partición utiliza los mismos valores para *boundary_value* [ **,** _...n_ ] que el ejemplo anterior, con la excepción de que especifica RANGE RIGHT.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF2 (int)  
AS RANGE RIGHT FOR VALUES (1, 100, 1000);  
```  
  
 En la tabla siguiente se muestra cómo se crearían particiones en una tabla que usa esta función de partición en la columna de partición **col1**.  
  
|Partition|1|2|3|4|  
|---------------|-------|-------|-------|-------|  
|**Valores**|**col1** \< `1`|**col1** >= `1` AND **col1** \< `100`|**col1** >= `100` AND **col1** \< `1000`|**col1** >= `1000`| 
  
### <a name="c-creating-a-range-right-partition-function-on-a-datetime-column"></a>C. Crear una función de partición RANGE RIGHT en una columna datetime  
 La siguiente función de partición divide una tabla o un índice en 12 particiones, una para cada mes de un año natural de valores en una columna **datetime**.  
  
```sql  
CREATE PARTITION FUNCTION [myDateRangePF1] (datetime)  
AS RANGE RIGHT FOR VALUES ('20030201', '20030301', '20030401',  
               '20030501', '20030601', '20030701', '20030801',   
               '20030901', '20031001', '20031101', '20031201');  
```  
  
 En la tabla siguiente se muestra cómo se crearían particiones en una tabla o un índice que usa esta función de partición en la columna de partición **datecol**.  
  
|Partition|1|2|...|11|12|  
|---------------|-------|-------|---------|--------|--------|  
|**Valores**|**datecol** \< `February 1, 2003`|**datecol** >= `February 1, 2003` AND **datecol** \< `March 1, 2003`||**datecol** >= `November 1, 2003` AND **col1** \< `December 1, 2003`|**datecol** >= `December 1, 2003`| 
  
### <a name="d-creating-a-partition-function-on-a-char-column"></a>D. Crear una función de partición en una columna char  
 La siguiente función de partición realiza cuatro particiones en una tabla o un índice.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF3 (char(20))  
AS RANGE RIGHT FOR VALUES ('EX', 'RXE', 'XR');  
```  
  
 En la tabla siguiente se muestra cómo se crearían particiones en una tabla que usa esta función de partición en la columna de partición **col1**.  
  
|Partition|1|2|3|4|  
|---------------|-------|-------|-------|-------|  
|**Valores**|**col1** \< `EX`...|**col1** >= `EX` AND **col1** \< `RXE`...|**col1** >= `RXE` AND **col1** \< `XR`...|**col1** >= `XR`| 
  
### <a name="e-creating-15000-partitions"></a>E. Crear 15.000 particiones  
 La siguiente función de partición realiza 15.000 particiones en una tabla o un índice.  
  
```sql  
--Create integer partition function for 15,000 partitions.  
DECLARE @IntegerPartitionFunction nvarchar(max) = 
    N'CREATE PARTITION FUNCTION IntegerPartitionFunction (int) 
    AS RANGE RIGHT FOR VALUES (';  
DECLARE @i int = 1;  
WHILE @i < 14999  
BEGIN  
SET @IntegerPartitionFunction += CAST(@i as nvarchar(10)) + N', ';  
SET @i += 1;  
END  
SET @IntegerPartitionFunction += CAST(@i as nvarchar(10)) + N');';  
EXEC sp_executesql @IntegerPartitionFunction;  
GO  
```  
  
### <a name="f-creating-partitions-for-multiple-years"></a>F. Crear particiones para varios años  
 La siguiente función de partición realizará 50 particiones en una tabla o un índice en una columna **datetime2**. Hay una partición para cada mes entre enero de 2007 y enero de 2011.  
  
```sql  
--Create date partition function with increment by month.  
DECLARE @DatePartitionFunction nvarchar(max) = 
    N'CREATE PARTITION FUNCTION DatePartitionFunction (datetime2) 
    AS RANGE RIGHT FOR VALUES (';  
DECLARE @i datetime2 = '20070101';  
WHILE @i < '20110101'  
BEGIN  
SET @DatePartitionFunction += '''' + CAST(@i as nvarchar(10)) + '''' + N', ';  
SET @i = DATEADD(MM, 1, @i);  
END  
SET @DatePartitionFunction += '''' + CAST(@i as nvarchar(10))+ '''' + N');';  
EXEC sp_executesql @DatePartitionFunction;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Tablas e índices con particiones](../../relational-databases/partitions/partitioned-tables-and-indexes.md)   
 [$PARTITION &#40;Transact-SQL&#41;](../../t-sql/functions/partition-transact-sql.md)   
 [ALTER PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-function-transact-sql.md)   
 [DROP PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-function-transact-sql.md)   
 [CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.partition_functions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-functions-transact-sql.md)   
 [sys.partition_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-parameters-transact-sql.md)   
 [sys.partition_range_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-range-values-transact-sql.md)   
 [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  

