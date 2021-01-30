---
description: sp_prepare (Transact SQL)
title: sp_prepare (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/28/2018
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursor_prepare_TSQL
- sp_cursor_prepare
dev_langs:
- TSQL
helpviewer_keywords:
- sp_prepare
ms.assetid: f328c9eb-8211-4863-bafa-347e1bf7bb3f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fe4da6ff8d3cdcbeb7ba809bf198c44373b99c9e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196025"
---
# <a name="sp_prepare-transact-sql"></a>sp_prepare (Transact SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

Prepara una [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucción con parámetros y devuelve un *identificador* de instrucción para la ejecución.  `sp_prepare` se invoca especificando el identificador 11 en un paquete de flujo TDS.  
  
 ![Icono de vínculo de artículo](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
sp_prepare handle OUTPUT, params, stmt, options  
```  
  
## <a name="arguments"></a>Argumentos  
 *asa*  
 Es un identificador de *controlador preparado* generado por SQL Server. *Handle* es un parámetro necesario con un valor devuelto **int** .  
  
 *params*  
 Identifica instrucciones con parámetros. La definición de *params* de variables se sustituye por los marcadores de parámetros de la instrucción. *params* es un parámetro necesario que requiere un valor de entrada **ntext**, **nchar** o **nvarchar** . Escriba un valor NULL si la instrucción no tiene parámetros.  
  
 *instrucción*  
 Define el conjunto de resultados del cursor. El parámetro *stmt* es obligatorio y llama a para un valor de entrada **ntext**, **nchar** o **nvarchar** .  
  
 *options*  
 Parámetro opcional que devuelve una descripción de las columnas del conjunto de resultados del cursor. *Options* requiere el siguiente valor de entrada int:  
  
|Value|Descripción|  
|-----------|-----------------|  
|0x0001|RETURN_METADATA|  
  
## <a name="examples"></a>Ejemplos  
A. En el ejemplo siguiente se prepara y se ejecuta una instrucción sencilla.  
  
```sql  
DECLARE @P1 INT;  
EXEC sp_prepare @P1 OUTPUT,   
    N'@P1 NVARCHAR(128), @P2 NVARCHAR(100)',  
    N'SELECT database_id, name FROM sys.databases WHERE name=@P1 AND state_desc = @P2';  
EXEC sp_execute @P1, N'tempdb', N'ONLINE';  
EXEC sp_unprepare @P1;  
```

B. En el ejemplo siguiente se prepara una instrucción en la base de datos AdventureWorks2016 y, después, se ejecuta con el identificador.

```sql
-- Prepare query
DECLARE @P1 INT;  
EXEC sp_prepare @P1 OUTPUT,   
    N'@Param INT',  
    N'SELECT *
FROM Sales.SalesOrderDetail AS sod
INNER JOIN Production.Product AS p ON sod.ProductID = p.ProductID
WHERE SalesOrderID = @Param
ORDER BY Style DESC;';  

-- Return handle for calling application
SELECT @P1;
GO
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
-----------
1

(1 row affected)
```

A continuación, la aplicación ejecuta la consulta dos veces con el valor de identificador 1, antes de descartar el plan preparado.

```sql
EXEC sp_execute 1, 49879;  
GO

EXEC sp_execute 1, 48766;
GO

EXEC sp_unprepare 1; 
GO
```
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  

