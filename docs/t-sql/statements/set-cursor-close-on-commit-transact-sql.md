---
description: SET CURSOR_CLOSE_ON_COMMIT (Transact-SQL)
title: SET CURSOR_CLOSE_ON_COMMIT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CURSOR_CLOSE_ON_COMMIT
- SET CURSOR_CLOSE_ON_COMMIT
- CURSOR_CLOSE_ON_COMMIT_TSQL
- SET_CURSOR_CLOSE_ON_COMMIT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CURSOR_CLOSE_ON_COMMIT option
- transactions [SQL Server], cursors
- closing cursors
- cursors [SQL Server], closing
- SET CURSOR_CLOSE_ON_COMMIT statement
ms.assetid: 7b976154-98ce-4a06-bbae-7e59c34211f7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 79898f0fca4780c63782f17608f45067002d088f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190382"
---
# <a name="set-cursor_close_on_commit-transact-sql"></a>SET CURSOR_CLOSE_ON_COMMIT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Controla el comportamiento de la instrucción COMMIT TRANSACTION de [!INCLUDE[tsql](../../includes/tsql-md.md)]. El valor predeterminado de esta configuración es OFF. Indica que el servidor no cerrará los cursores cuando se confirme una transacción.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
SET CURSOR_CLOSE_ON_COMMIT { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Comentarios
 Si SET CURSOR_CLOSE_ON_COMMIT es ON, este valor cierra cualquier cursor abierto al confirmar o revertir de conformidad con ISO. Si SET CURSOR_CLOSE_ON_COMMIT es OFF, no se cierra el cursor cuando se confirma una transacción.  
  
> [!NOTE]  
>  Si SET CURSOR_CLOSE_ON_COMMIT es ON, no se cierran los cursores abiertos al revertir si la reversión se aplica a savepoint_name desde una instrucción SAVE TRANSACTION.  
  
 Si SET CURSOR_CLOSE_ON_COMMIT es OFF, la instrucción ROLLBACK cierra solo los cursores asincrónicos que no se han terminado de llenar. Los cursores STATIC o INSENSITIVE abiertos después de realizar modificaciones ya no reflejarán el estado de los datos si las modificaciones se revierten.  
  
 SET CURSOR_CLOSE_ON_COMMIT controla el mismo comportamiento que la opción de base de datos CURSOR_CLOSE_ON_COMMIT. Si CURSOR_CLOSE_ON_COMMIT se establece en ON u OFF, esa configuración se utiliza en la conexión. Si no se ha especificado SET CURSOR_CLOSE_ON_COMMIT, se aplica el valor de la columna **is_cursor_close_on_commit_on** de la vista de catálogo **sys.databases**.  
  
 Al conectarse, el proveedor OLE DB de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el controlador ODBC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client establecen CURSOR_CLOSE_ON_COMMIT en OFF. DB-Library no establece automáticamente el valor de CURSOR_CLOSE_ON_COMMIT.  
  
 Si SET ANSI_DEFAULTS es ON, se habilita SET CURSOR_CLOSE_ON_COMMIT.  
  
 La opción SET CURSOR_CLOSE_ON_COMMIT se establece en tiempo de ejecución, no en tiempo de análisis.  
  
 Para ver la configuración actual de este valor, ejecute la consulta siguiente.  
  
```sql
DECLARE @CURSOR_CLOSE VARCHAR(3) = 'OFF';  
IF ( (4 & @@OPTIONS) = 4 ) SET @CURSOR_CLOSE = 'ON';  
SELECT @CURSOR_CLOSE AS CURSOR_CLOSE_ON_COMMIT;  
```  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se define un cursor en una transacción y se intenta utilizar después de confirmar la transacción.  
  
```sql
-- SET CURSOR_CLOSE_ON_COMMIT  
-------------------------------------------------------------------------------  
SET NOCOUNT ON;  
  
CREATE TABLE t1 (a INT);  
GO   
  
INSERT INTO t1   
VALUES (1), (2);  
GO  
  
PRINT '-- SET CURSOR_CLOSE_ON_COMMIT ON';  
GO  
SET CURSOR_CLOSE_ON_COMMIT ON;  
GO  
PRINT '-- BEGIN TRAN';  
BEGIN TRAN;  
PRINT '-- Declare and open cursor';  
DECLARE testcursor CURSOR FOR  
    SELECT a FROM t1;  
OPEN testcursor;  
PRINT '-- Commit tran';  
COMMIT TRAN;  
PRINT '-- Try to use cursor';  
FETCH NEXT FROM testcursor;  
CLOSE testcursor;  
DEALLOCATE testcursor;  
GO  
PRINT '-- SET CURSOR_CLOSE_ON_COMMIT OFF';  
GO  
SET CURSOR_CLOSE_ON_COMMIT OFF;  
GO  
PRINT '-- BEGIN TRAN';  
BEGIN TRAN;  
PRINT '-- Declare and open cursor';  
DECLARE testcursor CURSOR FOR  
    SELECT a FROM t1;  
OPEN testcursor;  
PRINT '-- Commit tran';  
COMMIT TRAN;  
PRINT '-- Try to use cursor';  
FETCH NEXT FROM testcursor;  
CLOSE testcursor;  
DEALLOCATE testcursor;  
GO  
DROP TABLE t1;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [CLOSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/close-transact-sql.md)   
 [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [ROLLBACK TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_DEFAULTS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-defaults-transact-sql.md)  
  
  
