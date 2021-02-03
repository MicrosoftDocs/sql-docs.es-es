---
description: TRUNCATE TABLE (Transact-SQL)
title: TRUNCATE TABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TRUNCATE
- TRUNCATE TABLE
- TRUNCATE_TSQL
- TRUNCATE_TABLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- row removal [SQL Server], TRUNCATE TABLE statement
- table truncating [SQL Server]
- removing rows
- TRUNCATE TABLE statement
- deleting rows
- dropping rows
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0da5fd480fcb5be450b1f45a9da63d0b0b7177f0
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99233226"
---
# <a name="truncate-table-transact-sql"></a>TRUNCATE TABLE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Quita todas las filas de una tabla o las particiones especificadas de una tabla, sin registrar las eliminaciones individuales de filas. TRUNCATE TABLE es similar a la instrucción DELETE sin una cláusula WHERE; no obstante, TRUNCATE TABLE es más rápida y utiliza menos recursos de registros de transacciones y de sistema.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
TRUNCATE TABLE   
    { database_name.schema_name.table_name | schema_name.table_name | table_name }  
    [ WITH ( PARTITIONS ( { <partition_number_expression> | <range> }   
    [ , ...n ] ) ) ]  
[ ; ]  
  
<range> ::=  
<partition_number_expression> TO <partition_number_expression>  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
TRUNCATE TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *database_name*  
 Es el nombre de la base de datos.  
  
 *schema_name*  
 Es el nombre del esquema al que pertenece la tabla.  
  
 *table_name*  
 Es el nombre de la tabla que se va a truncar o de la que se van a quitar todas las filas. *table_name* debe ser un valor literal. *table_name* no puede ser la variable o la función de **OBJECT_ID()** .  
  
 WITH ( PARTITIONS ( { \<*partition_number_expression*> | \<*range*> } [ , ...n ] ) )    
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level))
  
 Especifica las particiones para truncar o desde las que se quitan todas las filas. Si la tabla no se particiona, el argumento `WITH PARTITIONS` generará un error. Si no se especifica la cláusula `WITH PARTITIONS`, se truncará toda la tabla.  
  
 *\<partition_number_expression>* se puede especificar de estas maneras: 
  
-   Proporcionando el número de una partición, como por ejemplo: `WITH (PARTITIONS (2))`  
  
-   Proporcionando los números de partición para varias particiones individuales separadas por comas, como por ejemplo: `WITH (PARTITIONS (1, 5))`  
  
-   Proporcionando ambos intervalos y particiones individuales, como por ejemplo: `WITH (PARTITIONS (2, 4, 6 TO 8))`  
  
-   *\<range>* se puede especificar como números de partición separados por la palabra **TO**, como por ejemplo: `WITH (PARTITIONS (6 TO 8))`  
  
 Para truncar una tabla con particiones, la tabla y los índices deben estar alineados (con particiones en la misma función de partición).  
  
## <a name="remarks"></a>Observaciones  
 En comparación con la instrucción DELETE, `TRUNCATE TABLE` ofrece las siguientes ventajas:  
  
-   Se utiliza menos espacio del registro de transacciones.  
  
     La instrucción DELETE quita una a una las filas y graba una entrada en el registro de transacciones por cada fila eliminada. `TRUNCATE TABLE` quita los datos al cancelar la asignación de las páginas de datos utilizadas para almacenar los datos de la tabla y solo graba en el registro de transacciones las cancelaciones de asignación de páginas.  
  
-   Por regla general, se utilizan menos bloqueos.  
  
     Si se ejecuta la instrucción DELETE con un bloqueo de fila, se bloquea cada fila de la tabla para su eliminación. `TRUNCATE TABLE` siempre bloquea la tabla (incluido un bloqueo de esquema (SCH-M)) y la página, pero no cada fila.  
  
-   Las páginas cero se conservan en la tabla sin excepciones.  
  
     Después de ejecutar una instrucción DELETE, la tabla puede seguir conteniendo páginas vacías. Por ejemplo, no se puede cancelar la asignación de las páginas vacías de un montón sin un bloqueo de tabla exclusivo (LCK_M_X) como mínimo. Si en la operación de eliminación no se utiliza un bloqueo de tabla, la tabla (montón) contiene muchas páginas vacías. En el caso de los índices, la operación de eliminación puede dejar páginas vacías, aunque la asignación de estas páginas se puede cancelar rápidamente mediante un proceso de limpieza en segundo plano.  
  
 `TRUNCATE TABLE` quita todas las filas de una tabla, pero permanecen la estructura y sus columnas, las restricciones, los índices, etc. Para quitar la definición de tabla además de los datos, utilice la instrucción `DROP TABLE`.  
  
 Si la tabla contiene una columna de identidad, el contador para dicha columna se restablece al valor de inicialización definido para ella. Si no se define ningún valor de inicialización, se utiliza el valor predeterminado 1. Para conservar el contador de identidad, utilice DELETE.  
 
 > [!NOTE]
 > Se puede revertir una operación `TRUNCATE TABLE`.
  
## <a name="restrictions"></a>Restricciones  
 No se puede usar `TRUNCATE TABLE` en los siguientes casos:  
  
-   Tablas a las que se hace referencia mediante una restricción FOREIGN KEY. (Puede truncar una tabla que tenga una clave externa que haga referencia a sí misma). 
  
-   Tablas que participan en una vista indizada.  
  
-   Tablas que se publican mediante replicación transaccional o replicación de mezcla.  

-   Tablas temporales con versiones del sistema.

-   Tablas a las que se hace referencia con una restricción EDGE.  
  
 En el caso de las tablas con una o más de estas características, utilice la instrucción DELETE.  
  
 TRUNCATE TABLE no puede activar un desencadenador porque la operación no registra eliminaciones de filas individuales. Para obtener más información, vea [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md). 
 
 En [!INCLUDE[sssdwfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[sspdw](../../includes/sspdw-md.md)]:

- `TRUNCATE TABLE` no se permite dentro de la instrucción EXPLAIN.

- `TRUNCATE TABLE` no se puede ejecutar dentro de una transacción.
  
## <a name="truncating-large-tables"></a>Truncar tablas de gran tamaño  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ofrece la posibilidad de quitar o truncar las tablas con más de 128 extensiones sin mantener bloqueos simultáneos en todas las extensiones necesarias para la eliminación.  
  
## <a name="permissions"></a>Permisos  
 El permiso mínimo necesario es `ALTER` en *table_name*. Los permisos `TRUNCATE TABLE` se adjudican de manera predeterminada al propietario de la tabla, a los miembros del rol fijo de servidor `sysadmin` y a los roles fijos de base de datos `db_owner` y `db_ddladmin`, y no se pueden transferir. No obstante, puede incorporar la instrucción `TRUNCATE TABLE` en un módulo, por ejemplo un procedimiento almacenado, y conceder los permisos correspondientes al módulo mediante la cláusula `EXECUTE AS`.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-truncate-a-table"></a>A. Truncar una tabla  
 En el siguiente ejemplo se quitan todos los datos de la tabla `JobCandidate`. Se incluyen instrucciones `SELECT` antes y después de la instrucción `TRUNCATE TABLE` para comparar los resultados.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT COUNT(*) AS BeforeTruncateCount   
FROM HumanResources.JobCandidate;  
GO  
TRUNCATE TABLE HumanResources.JobCandidate;  
GO  
SELECT COUNT(*) AS AfterTruncateCount   
FROM HumanResources.JobCandidate;  
GO  
```  
  
### <a name="b-truncate-table-partitions"></a>B. Truncar la tabla de particiones  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level))
  
 En el ejemplo siguiente se trunca las particiones especificadas de una tabla con particiones. La sintaxis de `WITH (PARTITIONS (2, 4, 6 TO 8))` provoca que los números de partición 2, 4, 6, 7 y 8 se trunquen.  
  
```sql  
TRUNCATE TABLE PartitionTable1   
WITH (PARTITIONS (2, 4, 6 TO 8));  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)   
 [IDENTITY &#40;propiedad de Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)  
