---
description: SET SHOWPLAN_XML (Transact-SQL)
title: SET SHOWPLAN_XML (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET SHOWPLAN_XML
- SET_SHOWPLAN_XML_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- statements [SQL Server], estimates
- SET SHOWPLAN_XML statement
- execution information and estimates [SQL Server]
- statements [SQL Server], information without processing
- XML [SQL Server], statement execution information
- SHOWPLAN_XML option
- estimated execution information [SQL Server]
ms.assetid: a467a1b3-10a5-43c4-9085-13d8aed549c9
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d0065350fbf3c3c774602f28aeb98d6e7685df4c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206958"
---
# <a name="set-showplan_xml-transact-sql"></a>SET SHOWPLAN_XML (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Hace que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no ejecute instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)]. En su lugar, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve información detallada de cómo se van a ejecutar las instrucciones con el formato de un documento XML bien definido.

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintaxis

```syntaxsql
SET SHOWPLAN_XML { ON | OFF }
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Comentarios

La opción SET SHOWPLAN_XML se establece en tiempo de ejecución, no en tiempo de análisis.

Cuando la opción SET SHOWPLAN_XML está activada, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve información sobre el plan de ejecución de cada instrucción sin ejecutarla y no se ejecutan las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)]. Cuando esta opción está establecida en ON, se devuelve información acerca del plan de ejecución de todas las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] siguientes hasta que se vuelve a establecer en OFF. Por ejemplo, si se ejecuta una instrucción CREATE TABLE cuando SET SHOWPLAN_XML es ON y después se ejecuta una instrucción SELECT en la que se especifica la tabla creada, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve un mensaje de error, ya que la tabla no existe. Por ello, las referencias posteriores que se hagan a la tabla generarán un error. Cuando SET SHOWPLAN_XML está establecida en OFF, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ejecuta las instrucciones sin generar ningún informe.

SET SHOWPLAN_XML está diseñada para devolver datos de tipo **nvarchar(max)** para aplicaciones como la utilidad **sqlcmd**, donde otras herramientas usan después la salida XML para mostrar y procesar la información del plan de consulta.

> [!NOTE]
> La vista de administración dinámica, **sys.dm_exec_query_plan**, devuelve la misma información que SET SHOWPLAN XML con el tipo de datos **xml**. Esta información se devuelve desde la columna **query_plan** de **sys.dm_exec_query_plan**. Para más información, vea [sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md).

SET SHOWPLAN_XML no se puede especificar en un procedimiento almacenado. Debe ser la única instrucción en un lote.

SET SHOWPLAN_XML devuelve información como un conjunto de documentos XML. Cada lote después de la instrucción SET SHOWPLAN_XML ON se refleja en la salida con un único documento. Cada documento contiene el texto de las instrucciones del lote, seguido de los detalles de los pasos de ejecución. El documento muestra los costos estimados, el número de filas, los índices a los que se ha obtenido acceso y los tipos de operadores utilizados, el orden de combinación y más información acerca de los planes de ejecución.

> [!NOTE]
> Si se selecciona **Incluir plan de ejecución real** en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], esta opción SET no general la salida del plan de presentación XML. Desactive la opción **Incluir plan de ejecución real** antes de usar la opción SET.

### <a name="location-of-showplan-output"></a>Ubicación de la salida de SHOWPLAN

El documento que contiene el esquema XML de la salida XML de SET SHOWPLAN_XML se copia durante la instalación en un directorio local del equipo en el que se instala Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Encontrará el documento en la unidad que contiene los archivos de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], en una ruta de acceso parecida a la siguiente:

- `\Microsoft SQL Server\130\Tools\Binn\schemas\sqlserver\2004\07\showplan\showplanxml.xsd`

En la ruta de acceso anterior, SQL Server 2016 usa el nodo `130\`. El número 130 se obtiene del primer nodo del valor devuelto por `SELECT @@VERSION`, que es 13. En SQL Server 2017, se usaría `140\` en la ruta de acceso, ya que el primer nodo de su valor @@VERSION es 14. En SQL Server 2019, el primer valor de @@VERSION es 15.

El esquema del plan de presentación se puede encontrar también en [este sitio web](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409).

## <a name="permissions"></a>Permisos

Para utilizar SET SHOWPLAN_XML, debe disponer de permisos suficientes para ejecutar las instrucciones en las que se ejecuta SET SHOWPLAN_XML, y debe tener el permiso SHOWPLAN para todas las bases de datos que contengan objetos a los que se hace referencia.

En el caso de las instrucciones SELECT, INSERT, UPDATE, DELETE, EXEC *stored_procedure* y EXEC *user_defined_function*, para producir un plan de presentación, el usuario debe:

- Tener los permisos correspondientes para ejecutar las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)].

- Tener el permiso SHOWPLAN en todas las bases de datos que contengan objetos a los que hacen referencia las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)], como tablas, vistas, etc.

Para las demás instrucciones, como DDL, USE *database_name*, SET, DECLARE, SQL dinámico, etc., solo son necesarios los permisos apropiados para ejecutar las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)].

## <a name="examples"></a>Ejemplos

Las dos instrucciones siguientes utilizan la opción SET SHOWPLAN_XML para mostrar la forma en que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] analiza y optimiza el uso de índices en las consultas.

La primera consulta usa el operador de comparación Es igual a (=) en la cláusula WHERE de una columna indizada. La segunda consulta utiliza el operador LIKE en la cláusula WHERE. De este modo, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe utilizar un recorrido de índice clúster para encontrar los datos que cumplen la condición de la cláusula WHERE. Los valores de los atributos **EstimateRows** y **EstimatedTotalSubtreeCost** son inferiores en la primera consulta indexada, lo que indica que se procesa mucho más rápidamente y que usa menos recursos que la no indexada.

```sql
USE AdventureWorks2012;
GO
SET SHOWPLAN_XML ON;
GO
-- First query.
SELECT BusinessEntityID
FROM HumanResources.Employee
WHERE NationalIDNumber = '509647174';
GO
-- Second query.
SELECT BusinessEntityID, JobTitle
FROM HumanResources.Employee
WHERE JobTitle LIKE 'Production%';
GO
SET SHOWPLAN_XML OFF;
```

## <a name="see-also"></a>Vea también

[Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)
