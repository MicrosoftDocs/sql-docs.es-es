---
description: SET ANSI_NULLS (Transact-SQL)
title: SET ANSI_NULLS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/24/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET_ANSI_NULLS_TSQL
- ANSI_NULLS
- SET ANSI_NULLS
- ANSI_NULLS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET ANSI_NULLS statement
- not equal to operator (<>)
- ANSI_NULLS option
- equals operator (=)
- null values [SQL Server], comparison operators
- comparison operators [SQL Server], null values
ms.assetid: aae263ef-a3c7-4dae-80c2-cc901e48c755
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current || azuresqldb-current'
ms.openlocfilehash: 5aded31238cbcfa5abc33b981531796d1b910e13
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342116"
---
# <a name="set-ansi_nulls-transact-sql"></a>SET ANSI_NULLS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Especifica el comportamiento conforme a ISO de los operadores de comparación Es igual a (=) y No es igual a (<>) cuando se usan con valores NULL en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Sintaxis

### <a name="syntax-for-ssnoversion-mdmd-and-sssodfull-mdmd"></a>Sintaxis para [!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] y [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)]
```syntaxsql
SET ANSI_NULLS { ON | OFF }
```

### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>Sintaxis para [!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] y [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)]
```syntaxsql
SET ANSI_NULLS ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Observaciones
Cuando ANSI_NULLS se establece en ON, una instrucción SELECT que usa WHERE *nombre_columna* = **NULL** devuelve cero filas aunque haya valores NULL en *nombre_columna*. Una instrucción SELECT que usa WHERE *column_name* <> **NULL** devuelve cero filas aunque haya valores que no sean NULL en *column_name*.  
  
Cuando ANSI_NULLS se establece en OFF, los operadores de comparación Es igual a (=) y No es igual a (<>) no siguen el estándar ISO. Una instrucción SELECT que usa WHERE *column_name* = **NULL** devuelve las filas que tienen valores NULL en *column_name*. Una instrucción SELECT que usa WHERE *column_name* <> **NULL** devuelve las filas que tienen valores no NULL en la columna. Además, una instrucción SELECT que usa WHERE *column_name* <> *XYZ_value* devuelve todas las filas que no son *XYZ_value* y que no son NULL.  
  
Cuando ANSI_NULLS es ON, todas las comparaciones con un valor NULL se evalúan como UNKNOWN. Cuando SET ANSI_NULLS es OFF, la comparación de cualquier dato con un valor NULL se evalúa como TRUE si el valor del dato es NULL. Si no se especifica SET ANSI_NULLS, se aplica el valor de la opción de base de datos ANSI_NULLS. Para más información sobre la opción de base de datos ANSI_NULLS, vea [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  

En la tabla siguiente se muestra cómo el valor de ANSI_NULLS afecta a los resultados de una serie de expresiones booleanas con valores NULL y no NULL.  
  
|Expresión booleana|SET ANSI_NULLS ON|SET ANSI_NULLS OFF|  
|---------------|---------------|------------|  
|NULL = NULL|DESCONOCIDO|TRUE|  
|1 = NULL|DESCONOCIDO|FALSE|  
|NULL <> NULL|DESCONOCIDO|FALSE|  
|1 <> NULL|DESCONOCIDO|TRUE|  
|NULL > NULL|DESCONOCIDO|DESCONOCIDO|  
|1 > NULL|DESCONOCIDO|DESCONOCIDO|  
|NULL IS NULL|TRUE|TRUE|  
|1 IS NULL|FALSE|FALSE|  
|NULL IS NOT NULL|FALSE|FALSE|  
|1 IS NOT NULL|TRUE|TRUE|  

SET ANSI_NULLS ON solo afecta a una comparación si uno de los operandos es una variable que es NULL o un NULL literal. Si ambos lados de la comparación son columnas o expresiones compuestas, la configuración no afecta a la comparación.  
  
Para que un script funcione como se pretende, independientemente de la opción de base de datos ANSI_NULLS o de la opción SET ANSI_NULLS, use IS NULL e IS NOT NULL en las comparaciones que puedan contener valores NULL.  
  
ANSI_NULLS se debe establecer en ON para ejecutar consultas distribuidas.  
  
Igualmente se debe establecer en ON al crear o cambiar índices en columnas calculadas o vistas indexadas. Si SET ANSI_NULLS es OFF, las instrucciones CREATE, UPDATE, INSERT y DELETE producirán errores en tablas con índices en columnas calculadas y vistas indizadas. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve un error que muestra todas las opciones SET que infringen los valores necesarios. Además, al ejecutar una instrucción SELECT, si SET ANSI_NULLS es OFF, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] omite los valores de índice en vistas o columnas calculadas y resuelve la operación de selección como si no hubiera tales índices en las tablas o vistas.  
  
> [!NOTE]  
> ANSI_NULLS es una de las siete opciones SET a las que se deben asignar unos valores requeridos para tratar índices en columnas calculadas o vistas indizadas. Las opciones `ANSI_PADDING`, `ANSI_WARNINGS`, `ARITHABORT`, `QUOTED_IDENTIFIER`, y `CONCAT_NULL_YIELDS_NULL` también se deben establecer en ON, y `NUMERIC_ROUNDABORT` en OFF.  
  
 El controlador ODBC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client y el proveedor OLE DB de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] establecen automáticamente ANSI_NULLS en ON al conectarse. Esta opción se puede configurar en los orígenes de datos ODBC, en los atributos de conexión ODBC o en las propiedades de conexión OLE DB establecidas en la aplicación antes de conectarse a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El valor predeterminado de SET ANSI_NULLS es OFF.  
  
Cuando ANSI_DEFAULTS es ON, se habilita ANSI_WARNINGS.  
  
El valor de ANSI_NULLS se define en tiempo de ejecución, no en tiempo de análisis.  
  
Para ver la configuración actual de este valor, ejecute la siguiente consulta:
  
```sql  
DECLARE @ANSI_NULLS VARCHAR(3) = 'OFF';  
IF ( (32 & @@OPTIONS) = 32 ) SET @ANSI_NULLS = 'ON';  
SELECT @ANSI_NULLS AS ANSI_NULLS;   
```  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se usan los operadores de comparación Es igual a (`=`) y No es igual a (`<>`) para realizar comparaciones con valores `NULL` y distintos de NULL en una tabla. En este ejemplo también se muestra que `IS NULL` no se ve afectado por el valor `SET ANSI_NULLS`.  
  
```sql  
-- Create table t1 and insert values.  
CREATE TABLE dbo.t1 (a INT NULL);  
INSERT INTO dbo.t1 values (NULL),(0),(1);  
GO  
  
-- Print message and perform SELECT statements.  
PRINT 'Testing default setting';  
DECLARE @varname int;   
SET @varname = NULL;  
  
SELECT a  
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO 
```

Ahora, establezca ANSI_NULLS en ON y pruebe.

```sql
PRINT 'Testing ANSI_NULLS ON';  
SET ANSI_NULLS ON;  
GO  
DECLARE @varname int;  
SET @varname = NULL  
  
SELECT a   
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO  
```

Ahora, establezca ANSI_NULLS en OFF y pruebe.  

```sql
PRINT 'Testing ANSI_NULLS OFF';  
SET ANSI_NULLS OFF;  
GO  
DECLARE @varname int;  
SET @varname = NULL;  
SELECT a   
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO  
  
-- Drop table t1.  
DROP TABLE dbo.t1;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SESSIONPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/sessionproperty-transact-sql.md)   
 [= &#40;Igual a&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/equals-transact-sql.md)   
 [IF...ELSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/if-else-transact-sql.md)   
 [&#60;&#62; &#40;No es igual a&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/not-equal-to-transact-sql-traditional.md)   
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_DEFAULTS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-defaults-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [WHILE &#40;Transact-SQL&#41;](../../t-sql/language-elements/while-transact-sql.md)  
  
