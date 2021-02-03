---
description: SET SHOWPLAN_TEXT (Transact-SQL)
title: SET SHOWPLAN_TEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SHOWPLAN_TEXT
- SET_SHOWPLAN_TEXT_TSQL
- SET SHOWPLAN_TEXT
- SHOWPLAN_TEXT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- statements [SQL Server], estimates
- execution information and estimates [SQL Server]
- statements [SQL Server], information without processing
- SET SHOWPLAN_TEXT statement
- canceling statement execution
- SHOWPLAN_TEXT option
- stopping statement execution
- estimated execution information [SQL Server]
ms.assetid: 2c4f3fc8-ff2c-4790-8b74-e7e8ef58f9a6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 418326bb3fcfe0edd07e88405bfb9d216fb8538e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189440"
---
# <a name="set-showplan_text-transact-sql"></a>SET SHOWPLAN_TEXT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Hace que Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no ejecute instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)]. En su lugar, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve información detallada sobre el modo en que se ejecutan las instrucciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
SET SHOWPLAN_TEXT { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Comentarios
 La opción SET SHOWPLAN_TEXT se establece en tiempo de ejecución, no en tiempo de análisis.  
  
 Cuando SET SHOWPLAN_TEXT es ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve información acerca de la ejecución de cada instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)], sin ejecutarla. Cuando esta opción está establecida en ON, se devuelve información acerca del plan de ejecución de todas las instrucciones [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] siguientes hasta que se vuelve a establecer en OFF. Por ejemplo, si se ejecuta una instrucción CREATE TABLE cuando SET SHOWPLAN_TEXT es ON y después se ejecuta una instrucción SELECT en la que se especifica la tabla creada, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve un mensaje de error en el que se indica que la tabla especificada no existe. Por ello, las referencias posteriores que se hagan a la tabla generarán un error. Cuando SET SHOWPLAN_TEXT está establecida en OFF, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ejecuta las instrucciones sin generar ningún informe con información del plan de ejecución.  
  
 SET SHOWPLAN_TEXT permite devolver una salida legible para las aplicaciones de símbolo del sistema de Microsoft Win32, como la utilidad **sqlcmd**. SET SHOWPLAN_ALL devuelve información más detallada, destinada a los programas diseñados para tratarla.  
  
 SET SHOWPLAN_TEXT y SET SHOWPLAN_ALL no se pueden especificar en un procedimiento almacenado. Deben ser las únicas instrucciones de un lote.  
  
 SET SHOWPLAN_TEXT devuelve la información como un conjunto de filas en forma de árbol jerárquico que representa los pasos que sigue el procesador de consultas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al ejecutar cada instrucción. Cada instrucción reflejada en la salida contiene una fila con el texto de la instrucción, seguida de varias filas con los detalles de los pasos de su ejecución. La tabla muestra la columna que contiene la salida.  
  
|Nombre de la columna|Descripción|  
|-----------------|-----------------|  
|**StmtText**|En las filas que no sean de tipo PLAN_ROW, esta columna contiene el texto de la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)]. En las filas de tipo PLAN_ROW, esta columna contiene una descripción de la operación. Esta columna contiene el operador físico y, opcionalmente, puede contener también el operador lógico. También puede ir seguida de una descripción determinada por el operador físico. Para obtener más información acerca de los operadores físicos, vea la columna **Argument** de [SET SHOWPLAN_ALL &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-all-transact-sql.md).|  
|||

 Para obtener más información acerca de los operadores físicos y lógicos que se pueden mostrar en la salida del plan de presentación, vea [Referencia de operadores lógicos y físicos del plan de presentación](../../relational-databases/showplan-logical-and-physical-operators-reference.md).  
  
## <a name="permissions"></a>Permisos  
 Para utilizar SET SHOWPLAN_TEXT, debe disponer de permisos suficientes para ejecutar las instrucciones en las que se ejecuta SET SHOWPLAN_TEXT, y debe tener el permiso SHOWPLAN para todas las bases de datos que contengan objetos a los que se hace referencia.  
  
 En el caso de las instrucciones SELECT, INSERT, UPDATE, DELETE, EXEC *stored_procedure* y EXEC *user_defined_function*, para producir un plan de presentación, el usuario debe:  
  
-   Tener los permisos correspondientes para ejecutar las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   Tener el permiso SHOWPLAN en todas las bases de datos que contengan objetos a los que hacen referencia las instrucciones Transact-SQL, como tablas, vistas, etc.  
  
 Para las demás instrucciones, como DDL, USE *database_name*, SET, DECLARE, SQL dinámico, etc., solo son necesarios los permisos apropiados para ejecutar las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se muestra la forma en que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza los índices al procesar las instrucciones.  
  
 Ésta es la consulta que utiliza un índice:  
  
```sql
USE AdventureWorks2012;  
GO  
SET SHOWPLAN_TEXT ON;  
GO  
SELECT *  
FROM Production.Product   
WHERE ProductID = 905;  
GO  
SET SHOWPLAN_TEXT OFF;  
GO  
```  
  
 El conjunto de resultados es:  
  
```  
StmtText                                             
---------------------------------------------------  
SELECT *  
FROM Production.Product   
WHERE ProductID = 905;   
  
StmtText                                                                                                                                                                                        
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------  
|--Clustered Index Seek(OBJECT:([AdventureWorks2012].[Production].[Product].[PK_Product_ProductID]), SEEK:([AdventureWorks2012].[Production].[Product].[ProductID]=CONVERT_IMPLICIT(int,[@1],0)) ORDERED FORWARD)   
```  
  
 Ésta es la consulta que no utiliza un índice:  
  
```sql
USE AdventureWorks2012;  
GO  
SET SHOWPLAN_TEXT ON;  
GO  
SELECT *  
FROM Production.ProductCostHistory  
WHERE StandardCost < 500.00;  
GO  
SET SHOWPLAN_TEXT OFF;  
GO  
```  
  
 El conjunto de resultados es:  
  
```  
StmtText                                                                  
------------------------------------------------------------------------  
SELECT *  
FROM Production.ProductCostHistory  
WHERE StandardCost < 500.00;   
  
StmtText                                                                                                                                                                                                  
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------  
|--Clustered Index Scan(OBJECT:([AdventureWorks2012].[Production].[ProductCostHistory].[PK_ProductCostHistory_ProductCostID]), WHERE:([AdventureWorks2012].[Production].[ProductCostHistory].[StandardCost]<[@1]))  
```  
  
## <a name="see-also"></a>Consulte también  
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET SHOWPLAN_ALL &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-all-transact-sql.md)  
  
  
