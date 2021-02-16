---
description: SET ARITHABORT (Transact-SQL)
title: SET ARITHABORT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ARITHABORT_TSQL
- ARITHABORT
- SET ARITHABORT
- SET_ARITHABORT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- terminating queries
- queries [SQL Server], terminating
- overflow errors [SQL Server]
- ARITHABORT option
- divide-by-zero errors
- SET ARITHABORT statement
- ending queries [SQL Server]
- stopping queries
ms.assetid: f938a666-fdd1-4233-b97f-719f27b1a0e6
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 00a2c6564c9e296a23876b97e27bce3ec9dd906d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349996"
---
# <a name="set-arithabort-transact-sql"></a>SET ARITHABORT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Termina una consulta cuando se produce un error de desbordamiento o división por cero durante su ejecución.  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  

### <a name="syntax-for-ssnoversion-mdmd-and-sssodfull-mdmd"></a>Sintaxis para [!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] y [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)]
```syntaxsql
SET ARITHABORT { ON | OFF }
```

### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>Sintaxis para [!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] y [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)]
```syntaxsql
SET ARITHABORT ON
```
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Comentarios
Establezca siempre ARITHABORT en ON en las sesiones de inicio de sesión. Establecer ARITHABORT en OFF puede afectar negativamente a la optimización de consultas, lo que produce problemas de rendimiento.  
  
> [!WARNING]  
>  La configuración predeterminada de ARITHABORT para [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] es ON. Las aplicaciones cliente que establecen ARITHABORT en OFF podrían recibir distintos planes de consulta, lo que dificulta la solución de problemas de consultas con un rendimiento bajo. Es decir, la misma consulta podría ejecutarse más deprisa en Management Studio y más despacio en la aplicación. Al solucionar problemas de consultas con [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], use siempre la configuración de ARITHABORT del cliente.  
  
Si SET ARITHABORT y SET ANSI WARNINGS son ON, estas condiciones de error pueden terminar una consulta.  
  
Si SET ARITHABORT es ON y SET ANSI WARNINGS es OFF, estas condiciones de error pueden cancelar un lote. Si los errores se producen en una transacción, ésta se revierte. Si SET ARITHABORT es OFF y se produce uno de estos errores, aparece un mensaje de advertencia y se asigna el valor NULL al resultado de la operación aritmética.  
  
Si SET ARITHABORT y SET ANSI WARNINGS son OFF y se produce uno de estos errores, aparece un mensaje de advertencia y se asigna el valor NULL al resultado de la operación aritmética.  
  
> [!NOTE]  
>  Si ni SET ARITHABORT ni SET ARITHIGNORE son ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve NULL y muestra un mensaje de advertencia después de ejecutar la consulta.  
  
Cuando ANSI_WARNINGS tiene el valor ON y el nivel de compatibilidad de la base de datos se establece en 90 o superior, ARITHABORT se establece en ON implícitamente, independientemente de su configuración de valor. Si el nivel de compatibilidad de la base de datos está establecido en 80 o en un nivel inferior, debe configurarse explícitamente la opción ARITHABORT en ON.  
  
Si al evaluar una expresión con SET ARITHABORT en OFF, una instrucción INSERT, DELETE o UPDATE encuentra un error aritmético, desbordamiento, división por cero o error de dominio, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inserta o actualiza un valor NULL. Si la columna de destino no acepta valores NULL, no se puede efectuar la acción de inserción o actualización y el usuario recibe un error.  
  
Si SET ARITHABORT o SET ARITHIGNORE es OFF y SET ANSI_WARNINGS es ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devolverá un mensaje de error cuando haya errores de división por cero o desbordamiento.  
  
Si SET ARITHABORT está establecido en OFF y se produce un error de anulación durante la evaluación de una condición booleana de una instrucción IF, se ejecutará la rama FALSE.
  
SET ARITHABORT también debe ser ON al crear o cambiar índices en columnas calculadas o vistas indizadas. Si SET ARITHABORT es OFF, las instrucciones CREATE, UPDATE, INSERT y DELETE provocarán errores en tablas con índices en columnas calculadas y vistas indizadas.
  
La opción SET ARITHABORT se establece en tiempo de ejecución, no en tiempo de análisis.  
  
Para ver la configuración actual de SET ARITHABORT, ejecute la siguiente consulta:
  
```sql  
DECLARE @ARITHABORT VARCHAR(3) = 'OFF';  
IF ( (64 & @@OPTIONS) = 64 ) SET @ARITHABORT = 'ON';  
SELECT @ARITHABORT AS ARITHABORT;  
  
```  
  
## <a name="permissions"></a>Permisos  
Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
En este ejemplo se muestran errores de división por cero y desbordamiento con las opciones de `SET ARITHABORT`.  
  
```sql  
-- SET ARITHABORT  
-------------------------------------------------------------------------------  
-- Create tables t1 and t2 and insert data values.  
CREATE TABLE t1 (  
   a TINYINT,   
   b TINYINT  
);  
CREATE TABLE t2 (  
   a TINYINT  
);  
GO  
INSERT INTO t1   
VALUES (1, 0);  
INSERT INTO t1   
VALUES (255, 1);  
GO  
  
PRINT '*** SET ARITHABORT ON';  
GO  
-- SET ARITHABORT ON and testing.  
SET ARITHABORT ON;  
GO  
  
PRINT '*** Testing divide by zero during SELECT';  
GO  
SELECT a / b AS ab   
FROM t1;  
GO  
  
PRINT '*** Testing divide by zero during INSERT';  
GO  
INSERT INTO t2  
SELECT a / b AS ab    
FROM t1;  
GO  
  
PRINT '*** Testing tinyint overflow';  
GO  
INSERT INTO t2  
SELECT a + b AS ab   
FROM t1;  
GO  
  
PRINT '*** Resulting data - should be no data';  
GO  
SELECT *   
FROM t2;  
GO  
  
-- Truncate table t2.  
TRUNCATE TABLE t2;  
GO  
  
-- SET ARITHABORT OFF and testing.  
PRINT '*** SET ARITHABORT OFF';  
GO  
SET ARITHABORT OFF;  
GO  
  
-- This works properly.  
PRINT '*** Testing divide by zero during SELECT';  
GO  
SELECT a / b AS ab    
FROM t1;  
GO  
  
-- This works as if SET ARITHABORT was ON.  
PRINT '*** Testing divide by zero during INSERT';  
GO  
INSERT INTO t2  
SELECT a / b AS ab    
FROM t1;  
GO  
PRINT '*** Testing tinyint overflow';  
GO  
INSERT INTO t2  
SELECT a + b AS ab   
FROM t1;  
GO  
  
PRINT '*** Resulting data - should be 0 rows';  
GO  
SELECT *   
FROM t2;  
GO  
  
-- Drop tables t1 and t2.  
DROP TABLE t1;  
DROP TABLE t2;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ARITHIGNORE &#40;Transact-SQL&#41;](../../t-sql/statements/set-arithignore-transact-sql.md)   
 [SESSIONPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/sessionproperty-transact-sql.md)  
  
  
