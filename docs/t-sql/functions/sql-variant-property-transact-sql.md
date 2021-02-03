---
description: SQL_VARIANT_PROPERTY (Transact-SQL)
title: SQL_VARIANT_PROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/25/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SQL_VARIANT_PROPERTY_TSQL
- SQL_VARIANT_PROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- SQL_VARIANT_PROPERTY function
- sql_variant data type
ms.assetid: 50e5c1d9-4e95-4ed0-9c92-435c872a399e
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 00510ffb5bda7fd19b0806dd7a7b2dc1373c8354
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198295"
---
# <a name="sql_variant_property-transact-sql"></a>SQL_VARIANT_PROPERTY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve el tipo de datos base y otra información sobre un valor **sql_variant**.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
SQL_VARIANT_PROPERTY ( expression , property )  
```  
  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *expression*  
 Es una expresión de tipo **sql_variant**.  
  
 *property*  
 Contiene el nombre de la propiedad **sql_variant** para la que se proporciona la información. *property* es **varchar(** 128 **)** y puede ser cualquiera de los siguientes valores:  
  
|Value|Descripción|Tipo base de sql_variant devuelto|  
|-----------|-----------------|----------------------------------------|  
|**BaseType**|Tipo de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], como:<br /><br /> **bigint**<br /><br /> **binary**<br /><br /> **bit**<br /><br /> **char**<br /><br /> **date**<br /><br /> **datetime**<br /><br /> **datetime2**<br /><br /> **datetimeoffset**<br /><br /> **decimal**<br /><br /> **float**<br /><br /> **int**<br /><br /> **money**<br /><br /> **nchar**<br /><br /> **numeric**<br /><br /> **nvarchar**<br /><br /> **real**<br /><br /> **smalldatetime**<br /><br /> **smallint**<br /><br /> **smallmoney**<br /><br /> **time**<br /><br /> **tinyint**<br /><br /> **uniqueidentifier**<br /><br /> **varbinary**<br /><br /> **varchar**|**sysname**<br /><br /> NULL = La entrada no es válida.|  
|**Precisión**|Número de dígitos del tipo de datos base numérico:<br /><br /> **date** = 10<br /><br /> **datetime** = 23<br /><br /> **datetime2** = 27<br /><br /> **datetime2** (s) = 19 cuando s = 0, de lo contrario s + 20<br /><br /> **datetimeoffset** = 34<br /><br /> **datetimeoffset** (s) = 26 cuando s = 0, de lo contrario s + 27<br /><br /> **smalldatetime** = 16<br /><br /> **time** = 16<br /><br /> **time** (s) = 8 cuando s = 0, de lo contrario s + 9<br /><br /> **float** = 53<br /><br /> **real** = 24<br /><br /> **decimal** y **numeric** = 18<br /><br /> **decimal** (p,s) y **numeric** (p,s) = p<br /><br /> **money** = 19<br /><br /> **smallmoney** = 10<br /><br /> **bigint** = 19<br /><br /> **int** = 10<br /><br /> **smallint** = 5<br /><br /> **tinyint** = 3<br /><br /> **bit** = 1<br /><br /> Los demás tipos = 0|**int**<br /><br /> NULL = La entrada no es válida.|  
|**Escala**|Número de dígitos a la derecha del separador decimal del tipo de datos base numérico:<br /><br /> **decimal** y **numeric** = 0<br /><br /> **decimal** (p,s) y **numeric** (p,s) = s<br /><br /> **money** y **smallmoney** = 4<br /><br /> **datetime** = 3<br /><br /> **datetime2** = 7<br /><br /> **datetime2** (s) = s (0 - 7)<br /><br /> **datetimeoffset** = 7<br /><br /> **datetimeoffset** (s) = s (0 - 7)<br /><br /> **time** = 7<br /><br /> **time** (s) = s (0 - 7)<br /><br /> Los demás tipos = 0|**int**<br /><br /> NULL = La entrada no es válida.|  
|**TotalBytes**|Número de bytes necesario para contener los metadatos y los datos del valor. Esta información puede resultar útil al comprobar el tamaño máximo de los datos en una columna **sql_variant**. Si el valor es superior a 900, se produce un error en la creación del índice.|**int**<br /><br /> NULL = La entrada no es válida.|  
|**Intercalación**|Representa la intercalación del valor concreto de **sql_variant**.|**sysname**<br /><br /> NULL = La entrada no es válida.|  
|**MaxLength**|Longitud máxima del tipo de datos, en bytes. Por ejemplo, **MaxLength** de **nvarchar(** 50 **)** es 100 y **MaxLength** de **int**, 4.|**int**<br /><br /> NULL = La entrada no es válida.|  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **sql_variant**  
  
## <a name="examples"></a>Ejemplos  
### <a name="a-using-a-sql_variant-in-a-table"></a>A. Usar un tipo de datos sql_variant en una tabla  
 En el siguiente ejemplo se recupera la información de `SQL_VARIANT_PROPERTY` relativa al valor de `colA``46279.1`, donde `colB` =`1689`, siempre que `tableA` tenga `colA` de tipo `sql_variant` y `colB`.  
  
```sql    
CREATE   TABLE tableA(colA sql_variant, colB int)  
INSERT INTO tableA values ( cast (46279.1 as decimal(8,2)), 1689)  
SELECT   SQL_VARIANT_PROPERTY(colA,'BaseType') AS 'Base Type',  
         SQL_VARIANT_PROPERTY(colA,'Precision') AS 'Precision',  
         SQL_VARIANT_PROPERTY(colA,'Scale') AS 'Scale'  
FROM      tableA  
WHERE      colB = 1689  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] Tenga en cuenta que cada uno de estos tres valores es **sql_variant**.  
  
```  
Base Type    Precision    Scale  
---------    ---------    -----  
decimal      8           2  
  
(1 row(s) affected)  
```  
  
### <a name="b-using-a-sql_variant-as-a-variable"></a>B. Usar un tipo de datos sql_variant como una variable   
 En el siguiente ejemplo se recupera información de `SQL_VARIANT_PROPERTY` sobre una variable denominada @v1.  
  
```sql    
DECLARE @v1 sql_variant;  
SET @v1 = 'ABC';  
SELECT @v1;  
SELECT SQL_VARIANT_PROPERTY(@v1, 'BaseType');  
SELECT SQL_VARIANT_PROPERTY(@v1, 'MaxLength');  
```  
  
## <a name="see-also"></a>Consulte también  
 [sql_variant &#40;Transact-SQL&#41;](../../t-sql/data-types/sql-variant-transact-sql.md)  
  
  

