---
description: SET SHOWPLAN_ALL (Transact-SQL)
title: SET SHOWPLAN_ALL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET SHOWPLAN_ALL
- SET_SHOWPLAN_ALL_TSQL
- SHOWPLAN_ALL
- SHOWPLAN_ALL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- statements [SQL Server], estimates
- execution information and estimates [SQL Server]
- statements [SQL Server], information without processing
- SET SHOWPLAN_ALL statement
- SHOWPLAN_ALL option
- canceling statement execution
- stopping statement execution
- estimated execution information [SQL Server]
ms.assetid: a500b682-bae4-470f-9e00-47de905b851b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 214827fe76301021faa31ec6a922059b60d088e4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206944"
---
# <a name="set-showplan_all-transact-sql"></a>SET SHOWPLAN_ALL (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Hace que Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no ejecute instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)]. En su lugar, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve información detallada sobre la forma en que se ejecutan las instrucciones y proporciona estimaciones de los recursos que requieren.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
SET SHOWPLAN_ALL { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Observaciones
 La opción SET SHOWPLAN_ALL se establece en tiempo de ejecución, no en tiempo de análisis.  
  
 Cuando la opción `SET SHOWPLAN_ALL` está activada, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve información sobre la ejecución de cada instrucción sin ejecutarla y no se ejecutan las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)]. Cuando esta opción está establecida en ON, se devuelve información sobre todas las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] siguientes hasta que se vuelve a establecer en OFF. Por ejemplo, si se ejecuta una instrucción CREATE TABLE cuando la opción `SET SHOWPLAN_ALL` está activada y, después, se ejecuta una instrucción SELECT en la que se especifica la tabla creada, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve un mensaje de error en el que se indica que la tabla no existe. Por ello, las referencias posteriores que se hagan a la tabla generarán un error. Cuando SET SHOWPLAN_ALL está establecida en OFF, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ejecuta las instrucciones sin generar ningún informe.  
  
 Solo deben utilizar `SET SHOWPLAN_ALL` las aplicaciones escritas para controlar su salida. Puede usar SET SHOWPLAN_TEXT para obtener una salida legible para las aplicaciones Microsoft Win32 del símbolo del sistema, como la utilidad **osql**.  
  
 No es posible especificar SET SHOWPLAN_TEXT y SET SHOWPLAN_ALL en un procedimiento almacenado; deben ser las únicas instrucciones en un lote.  
  
 SET SHOWPLAN_ALL devuelve la información como un conjunto de filas en forma de árbol jerárquico que representa los pasos que sigue el procesador de consultas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al ejecutar cada instrucción. Cada instrucción reflejada en la salida contiene una fila con el texto de la instrucción, seguida de varias filas con los detalles de los pasos de su ejecución. La tabla muestra las columnas que contiene la salida.  
  
|Nombre de la columna|Descripción|  
|-----------------|-----------------|  
|**StmtText**|En las filas que no sean de tipo PLAN_ROW, esta columna contiene el texto de la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)]. En las filas de tipo PLAN_ROW, esta columna contiene una descripción de la operación. Esta columna contiene el operador físico y, opcionalmente, puede contener también el operador lógico. También puede ir seguida de una descripción determinada por el operador físico. Para más información, vea [Referencia de operadores lógicos y físicos del plan de presentación](../../relational-databases/showplan-logical-and-physical-operators-reference.md).|  
|**StmtId**|Número de la instrucción en el lote actual.|  
|**NodeId**|Id. del nodo en la consulta actual.|  
|**Parent**|Id. del nodo del paso primario.|  
|**PhysicalOp**|Algoritmo de implementación física del nodo. Solo para filas de tipo PLAN_ROWS.|  
|**LogicalOp**|Operador algebraico relacional que representa este nodo. Solo para filas de tipo PLAN_ROWS.|  
|**Argument**|Proporciona información adicional acerca de la operación que se realiza. El contenido de esta columna depende del operador físico.|  
|**DefinedValues**|Contiene una lista separada por comas con los valores introducidos por este operador. Estos valores pueden ser expresiones calculadas presentes en la consulta actual (por ejemplo, en la lista SELECT o en la cláusula WHERE) o valores internos introducidos por el procesador de consultas para procesar esta consulta. Posteriormente se podrá hacer referencia a estos valores en cualquier punto de la consulta. Solo para filas de tipo PLAN_ROWS.|  
|**EstimateRows**|Número estimado de filas de salida que produce este operador. Solo para filas de tipo PLAN_ROWS.|  
|**EstimateIO**|Coste* de E/S estimado para este operador. Solo para filas de tipo PLAN_ROWS.|  
|**EstimateCPU**|Coste* de CPU estimado para este operador. Solo para filas de tipo PLAN_ROWS.|  
|**AvgRowSize**|Tamaño medio estimado (en bytes) de la fila que pasa a través de este operador.|  
|**TotalSubtreeCost**|Coste* estimado (acumulado) de esta operación y todas sus operaciones secundarias.|  
|**OutputList**|Contiene una lista separada por comas de las columnas proyectadas por la operación actual.|  
|**:::no-loc text="Warnings":::**|Contiene una lista separada por comas con los mensajes de advertencia relacionados con la operación actual. Los mensajes de advertencia pueden incluir la cadena "NO STATS:()" con una lista de columnas. Este mensaje de advertencia significa que el optimizador de consultas intentó tomar una decisión basada en las estadísticas de la columna, pero no había estadísticas disponibles. En consecuencia, el optimizador de consultas ha tenido que elegir al azar, lo que puede haber provocado la selección de un plan de consulta poco eficiente. Para más información sobre la creación o la actualización de estadísticas de columna (que ayudan al optimizador de consultas a elegir un plan de consulta más eficiente), vea [UPDATE STATISTICS](../../t-sql/statements/update-statistics-transact-sql.md). Opcionalmente, esta columna puede incluir la cadena "MISSING JOIN PREDICATE", que significa que tiene lugar una combinación (de tablas) sin que haya un predicado de combinación. La pérdida accidental de un predicado de combinación puede provocar que la consulta tarde mucho más de lo esperado y que devuelva un conjunto de resultados de gran tamaño. Si aparece esta advertencia, compruebe que la ausencia de predicado de combinación es intencionada.|  
|**:::no-loc text="Type":::**|Tipo de nodo. En el nodo primario de cada consulta, éste es el tipo de instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] (por ejemplo, SELECT, INSERT, EXECUTE, etcétera). En los subnodos que representan planes de ejecución, el tipo es PLAN_ROW.|  
|**Parallel**|**0** = El operador no se ejecuta en paralelo.<br /><br /> **1** = El operador se ejecuta en paralelo.|  
|**EstimateExecutions**|Número estimado de veces que se va a ejecutar este operador durante la ejecución de la consulta actual.|  
|||

 *Las unidades de coste están basadas en una medición interna de tiempo, no en tiempo de reloj. Se usan para determinar el coste relativo de un plan en comparación con otros planes.  
  
## <a name="permissions"></a>Permisos  
 Para utilizar SET SHOWPLAN_ALL, debe disponer de permisos suficientes para ejecutar las instrucciones en las que se ejecuta SET SHOWPLAN_ALL, y debe tener el permiso SHOWPLAN para todas las bases de datos que contengan objetos a los que se hace referencia.  
  
 En el caso de las instrucciones SELECT, INSERT, UPDATE, DELETE, EXEC *stored_procedure* y EXEC *user_defined_function*, para producir un plan de presentación, el usuario debe:  
  
-   Tener los permisos correspondientes para ejecutar las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   Tener el permiso SHOWPLAN en todas las bases de datos que contengan objetos a los que hacen referencia las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)], como tablas, vistas, etc.  
  
 Para las demás instrucciones, como DDL, USE *database_name*, SET, DECLARE, SQL dinámico, etc., solo son necesarios los permisos apropiados para ejecutar las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="examples"></a>Ejemplos  
 Las dos instrucciones siguientes utilizan la opción SET SHOWPLAN_ALL para mostrar la forma en que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] analiza y optimiza el uso de índices en las consultas.  
  
 La primera consulta usa el operador de comparación Es igual a (=) en la cláusula WHERE de una columna indizada. Esto da lugar al valor Clustered Index Seek en la columna **LogicalOp** y al nombre del índice en la columna **Argument**.  
  
 La segunda consulta utiliza el operador LIKE en la cláusula WHERE. De este modo, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe utilizar un recorrido de índice no clúster para encontrar los datos que satisfacen la condición de la cláusula WHERE. Esto da lugar al valor Clustered Index Scan en la columna **LogicalOp** con el nombre del índice en la columna **Argument** y el valor Filter en la columna **LogicalOp** con la condición de la cláusula WHERE en la columna **Argument**.  
  
 Los valores de las columnas **EstimateRows** y **TotalSubtreeCost** son inferiores en la primera consulta indizada, lo que indica que se procesa mucho más rápidamente y que usa menos recursos que la no indizada.  
  
```sql
USE AdventureWorks2012;  
GO  
SET SHOWPLAN_ALL ON;  
GO  
-- First query.  
SELECT BusinessEntityID   
FROM HumanResources.Employee  
WHERE NationalIDNumber = '509647174';  
GO  
-- Second query.  
SELECT BusinessEntityID, EmergencyContactID   
FROM HumanResources.Employee  
WHERE EmergencyContactID LIKE '1%';  
GO  
SET SHOWPLAN_ALL OFF;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET SHOWPLAN_TEXT &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-text-transact-sql.md)   
 [SET SHOWPLAN_XML &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-xml-transact-sql.md)  
  
  
