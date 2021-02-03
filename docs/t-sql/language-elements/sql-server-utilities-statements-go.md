---
description: 'Instrucciones de utilidades de SQL Server: GO'
title: 'Instrucciones de utilidades de SQL Server: GO | Microsoft Docs'
ms.custom: ''
ms.date: 07/27/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GO
- GO_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- batches [SQL Server], ending
- ending batches [SQL Server]
- GO command
ms.assetid: b2ca6791-3a07-4209-ba8e-2248a92dd738
author: cawrites
ms.author: chadam
ms.openlocfilehash: 527a67471c502eaaabe57751f61bdd81db8ea752
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200480"
---
# <a name="sql-server-utilities-statements---go"></a>Instrucciones de utilidades de SQL Server: GO
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona comandos que no son instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)], pero que las utilidades **sqlcmd** y **osql**, y el Editor de código de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] sí reconocen. Estos comandos se pueden usar para facilitar la legibilidad y la ejecución de lotes y scripts.  
  
  GO indica a las utilidades de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] el final de un lote de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
GO [count]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *count*  
 Es un entero positivo. El lote que precede a GO se ejecutará el número especificado de veces.  
  
## <a name="remarks"></a>Observaciones  
 GO no es una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)], sino un comando reconocido por las utilidades **sqlcmd** y **osql**, así como por el Editor de código de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 Las utilidades de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interpretan GO como una señal de que deben enviar el lote actual de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El lote actual de instrucciones está formado por todas las instrucciones especificadas desde el último comando GO o desde el comienzo de la sesión o script ad hoc si se trata del primer comando GO.  
  
 Una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] no puede ocupar la misma línea que un comando GO. Sin embargo, la línea sí puede contener comentarios.  
  
 Los usuarios deben seguir las reglas de los lotes. Por ejemplo, la ejecución de un procedimiento almacenado después de la primera instrucción de un lote debe incluir la palabra clave EXECUTE. El ámbito de las variables locales (definidas por el usuario) está limitado a un lote y no es posible referirse a ellas después del comando GO.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @MyMsg VARCHAR(50)  
SELECT @MyMsg = 'Hello, World.'  
GO -- @MyMsg is not valid after this GO ends the batch.  
  
-- Yields an error because @MyMsg not declared in this batch.  
PRINT @MyMsg  
GO  
  
SELECT @@VERSION;  
-- Yields an error: Must be EXEC sp_who if not first statement in   
-- batch.  
sp_who  
GO  
```  
  
 Las aplicaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pueden enviar varias instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para su ejecución como un lote. En ese momento, las instrucciones del lote se compilan en un único plan de ejecución. Los programadores que ejecutan instrucciones ad hoc en las utilidades de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o que generan Scripts de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] para ejecutarlas a través de las utilidades de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizan el comando GO para indicar el final de un lote.  
  
 Las aplicaciones basadas en las API de ODBC u OLE DB reciben un error de sintaxis al intentar ejecutar un comando GO. Las utilidades de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nunca envían un comando GO al servidor.  
  
 No use un punto y coma como terminador de instrucción después de GO.
 
```sql
-- Yields an error because ; is not permitted after GO  
SELECT @@VERSION;  
GO;  
```
  
## <a name="permissions"></a>Permisos  
 GO es un comando de utilidad que no requiere permisos. Cualquier usuario puede ejecutarlo.    
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se crean dos lotes. El primer lote solo contiene una instrucción `USE AdventureWorks2012` para establecer el contexto de base de datos. Las instrucciones restantes utilizan una variable local. Por lo tanto, es necesario agrupar todas las declaraciones de variable locales en un único lote. Esto se logra al no usar el comando `GO` hasta después de la última instrucción que hace referencia a la variable.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @NmbrPeople INT  
SELECT @NmbrPeople = COUNT(*)  
FROM Person.Person;  
PRINT 'The number of people as of ' +  
      CAST(GETDATE() AS CHAR(20)) + ' is ' +  
      CAST(@NmbrPeople AS CHAR(10));  
GO  
```  
  
 En el siguiente ejemplo ejecutan las instrucciones del lote dos veces.  
  
```sql  
SELECT DB_NAME();  
SELECT USER_NAME();  
GO 2  
```  
  
  
