---
title: Sintaxis de Transact-SQL compatible con IntelliSense
description: Obtenga información sobre qué instrucciones y elementos de sintaxis de Transact-SQL son compatibles con IntelliSense de SQL Server Management Studio en SQL Server 2019 (15.x).
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- Transact-SQL IntelliSense
- IntelliSense [SQL Server], Transact-SQL syntax
ms.assetid: 194e8f4f-fd7e-4f32-a169-f23531128004
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/16/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 38c0ce9079dddb67e274fccb071aabbb778fb050
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352909"
---
# <a name="transact-sql-syntax-supported-by-intellisense"></a>Sintaxis de Transact-SQL compatible con IntelliSense
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  En este tema se describen las instrucciones y los elementos de sintaxis de [!INCLUDE[tsql](../../includes/tsql-md.md)] que IntelliSense admite en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
## <a name="statements-supported-by-intellisense"></a>Instrucciones admitidas por IntelliSense  
 En [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], IntelliSense solamente admite las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] de uso más frecuente. Algunas condiciones generales del Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] pueden impedir el funcionamiento de IntelliSense. Para obtener más información, vea [Solución de problemas IntelliSense &#40;SQL Server Management Studio&#41;](./troubleshooting-intellisense.md).  
  
> [!NOTE]  
>  IntelliSense no está disponible para objetos de base de datos cifrados, como funciones definidas por el usuario o procedimientos almacenados cifrados. La ayuda sobre parámetros y la información rápida no están disponibles para los parámetros de los procedimientos almacenados extendidos y los tipos definidos por el usuario de la integración con CLR.  
  
### <a name="select-statement"></a>Instrucción SELECT  
 El Editor de consultas del [!INCLUDE[ssDE](../../includes/ssde-md.md)] proporciona compatibilidad con IntelliSense para los elementos de sintaxis siguientes de la instrucción SELECT:  
  
:::row:::
    :::column:::
        SELECT
    :::column-end:::
    :::column:::
        WHERE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        FROM
    :::column-end:::
    :::column:::
        ORDER BY
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        HAVING
    :::column-end:::
    :::column:::
        UNION
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        FOR
    :::column-end:::
    :::column:::
        GROUP BY
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        TOP
    :::column-end:::
    :::column:::
        OPTION (sugerencia)
    :::column-end:::
:::row-end:::

### <a name="additional-transact-sql-statements-that-are-supported"></a>Instrucciones Transact-SQL adicionales compatibles  
 El Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] también proporciona compatibilidad con IntelliSense para las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] que se muestran en la tabla siguiente.  
  
|Instrucción Transact-SQL|Sintaxis compatible|Excepciones|  
|-----------------------------|----------------------|----------------|  
|[INSERT](../../t-sql/statements/insert-transact-sql.md)|Toda la sintaxis, excepto la cláusula *execute_statement* .|None|  
|[UPDATE](../../t-sql/queries/update-transact-sql.md)|Toda la sintaxis.|None|  
|[DELETE](../../t-sql/statements/delete-transact-sql.md)|Toda la sintaxis.|None|  
|[DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)|Toda la sintaxis.|None|  
|[SET @local_variable](../../t-sql/language-elements/set-local-variable-transact-sql.md)|Toda la sintaxis.|None|  
|[EXECUTE](../../t-sql/language-elements/execute-transact-sql.md)|Ejecución de procedimientos almacenados definidos por el usuario, procedimientos almacenados del sistema, funciones definidas por el usuario y funciones del sistema.|None|  
|[CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)|Toda la sintaxis.|None|  
|[CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md)|Toda la sintaxis.|None|  
|[CREATE PROCEDURE](../../t-sql/statements/create-procedure-transact-sql.md)|Toda la sintaxis.|No hay compatibilidad con IntelliSense para la cláusula EXTERNAL NAME.<br /><br /> En la cláusula AS, IntelliSense solamente es compatible con las instrucciones y la sintaxis que se mencionan en este tema.|  
|[ALTER PROCEDURE](../../t-sql/statements/alter-procedure-transact-sql.md)|Toda la sintaxis.|No hay compatibilidad con IntelliSense para la cláusula EXTERNAL NAME.<br /><br /> En la cláusula AS, IntelliSense solamente es compatible con las instrucciones y la sintaxis que se mencionan en este tema.|  
|[USE](../../t-sql/language-elements/use-transact-sql.md)|Toda la sintaxis.|None|  
  
## <a name="intellisense-in-supported-statements"></a>IntelliSense en las instrucciones compatibles  
 La característica IntelliSense del Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] es compatible con los elementos de sintaxis siguientes cuando se utilizan en una de las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] compatibles:  
  
-   Todos los tipos de combinación, incluso APPLY  
  
-   PIVOT y UNPIVOT  
  
-   Referencias a los objetos de base de datos siguientes:  
  
    -   Bases de datos y esquemas  
  
    -   Tablas, vistas, funciones con valores de tabla y expresiones de tabla  
  
    -   Columnas  
  
    -   Procedimientos y parámetros de procedimientos  
  
    -   Funciones escalares y expresiones escalares  
  
    -   Variables locales  
  
    -   Expresiones de tabla común (CTE)  
  
-   Objetos de base de datos a los que solo se hace referencia en instrucciones CREATE o ALTER del script o del proceso por lotes, pero que no existen en la base de datos porque el script o el proceso por lotes no se han ejecutado todavía. Estos objetos son los siguientes:  
  
    -   Tablas y procedimientos especificados en una instrucción CREATE TABLE o CREATE PROCEDURE del script o del proceso por lotes.  
  
    -   Cambios en las tablas y los procedimientos especificados en una instrucción ALTER TABLE o ALTER PROCEDURE del script o del proceso por lotes.  
  
    > [!NOTE]  
    >  La característica IntelliSense no estará disponible para las columnas de una instrucción CREATE VIEW hasta que se haya ejecutado la instrucción CREATE VIEW.  
  
 La característica IntelliSense no se proporciona para los elementos enumerados anteriormente cuando se utilizan en otras instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] . Por ejemplo, los nombres de columna que se utilizan en una instrucción SELECT son compatibles con IntelliSense, pero no los de columnas que se utilizan en la instrucción CREATE FUNCTION.  
  
## <a name="examples"></a>Ejemplos  
 Dentro de un script o un proceso por lotes de [!INCLUDE[tsql](../../includes/tsql-md.md)] , la característica IntelliSense del Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] solamente es compatible con las instrucciones y la sintaxis mencionadas en este tema. En los ejemplos de código de [!INCLUDE[tsql](../../includes/tsql-md.md)] siguientes se muestran las instrucciones y los elementos de sintaxis admitidos por IntelliSense. Por ejemplo, en el proceso por lotes siguiente, la característica IntelliSense está disponible para la instrucción `SELECT` cuando se codifica por sí misma, pero no cuando la instrucción `SELECT` está contenida en una instrucción `CREATE FUNCTION` .  
  
```  
USE AdventureWorks2012;  
GO  
SELECT Name  
FROM Production.Product  
WHERE Name LIKE N'Road-250%' and Color = N'Red';  
GO  
CREATE FUNCTION Production.ufn_Red250 ()  
RETURNS TABLE  
AS  
RETURN   
(  
    SELECT Name  
    FROM AdventureWorks2012.Production.Product  
    WHERE Name LIKE N'Road-250%'  
      AND Color = N'Red'  
);GO  
```  
  
 Esta funcionalidad también se aplica a los conjuntos de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] de la cláusula AS de una instrucción CREATE PROCEDURE o ALTER PROCEDURE.  
  
 Dentro de un script o un proceso por lotes de [!INCLUDE[tsql](../../includes/tsql-md.md)] , la característica IntelliSense es compatible con los objetos que se han especificado en una instrucción CREATE o ALTER; sin embargo, estos objetos no existen en la base de datos porque no se han ejecutado las instrucciones. Por ejemplo, puede escribir el código siguiente en el Editor de consultas:  
  
```  
USE MyTestDB;  
GO  
CREATE TABLE MyTable  
    (PrimaryKeyCol   INT PRIMARY KEY,  
    FirstNameCol      NVARCHAR(50),  
   LastNameCol       NVARCHAR(50));  
GO  
SELECT   
```  
  
 Después de escribir `SELECT`, IntelliSense enumera **PrimaryKeyCol**, **FirstNameCol** y **LastNameCol** como posibles elementos en la lista de selección, aunque no se haya ejecutado el script y `MyTable` no exista todavía en `MyTestDB`.  
  
