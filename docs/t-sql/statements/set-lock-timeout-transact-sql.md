---
description: SET LOCK_TIMEOUT (Transact-SQL)
title: SET LOCK_TIMEOUT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- LOCK_TIMEOUT_TSQL
- SET_LOCK_TIMEOUT_TSQL
- SET LOCK_TIMEOUT
- LOCK_TIMEOUT
dev_langs:
- TSQL
helpviewer_keywords:
- timeout options [SQL Server], locks
- releasing locks
- LOCK_TIMEOUT option
- SET LOCK_TIMEOUT statement
- locking [SQL Server], time-outs
- wait time for lock releases [SQL Server]
ms.assetid: dd0c389e-956d-435e-bf71-e16624a0a215
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 037fe5a16b873f325c73f6c4b81a2ec0ab029015
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189479"
---
# <a name="set-lock_timeout-transact-sql"></a>SET LOCK_TIMEOUT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Especifica el número de milisegundos que una instrucción espera a que se libere un bloqueo.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
SET LOCK_TIMEOUT timeout_period  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *timeout_period*  
 Es el número de milisegundos que pasarán antes de que [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelva un error de bloqueo. El valor -1 (predeterminado) indica que no hay límite de espera (es decir que se espera indefinidamente).  
  
 Cuando se espera un bloqueo durante más tiempo que el indicado, se devuelve un error. El valor 0 significa no esperar y devolver un mensaje en cuanto se encuentre un bloqueo.  
  
## <a name="remarks"></a>Comentarios  
 Al principio de una conexión, este valor es -1. Después de cambiarlo, el nuevo valor permanece en vigor para el resto de la conexión.  
  
 La opción SET LOCK_TIMEOUT se establece en tiempo de ejecución, no en tiempo de análisis.  
  
 La sugerencia de bloqueo READPAST es una alternativa a esta opción SET.  
  
 Las instrucciones CREATE DATABASE, ALTER DATABASE y DROP DATABASE no respetan el parámetro SET LOCK_TIMEOUT.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-set-the-lock-timeout-to-1800-milliseconds"></a>A. Establecer el tiempo de espera de bloqueo en 1800 milisegundos  
 En el ejemplo siguiente se establece el período de tiempo de espera de bloqueo en `1800` milisegundos.  
  
```sql  
SET LOCK_TIMEOUT 1800;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-set-the-lock-timeout-to-wait-forever-for-a-lock-to-be-released"></a>B. Establecer el tiempo de espera de bloqueo para esperar indefinidamente a se libere un bloqueo  
 En este ejemplo se establece el tiempo de espera de bloqueo para que espere indefinidamente y no expire nunca. Este es el comportamiento predeterminado que ya está establecido al principio de cada conexión.  
  
```sql  
SET LOCK_TIMEOUT -1;  
```  
  
 En el ejemplo siguiente se establece el período de tiempo de espera de bloqueo en `1800` milisegundos. En esta versión, [!INCLUDE[ssDW](../../includes/ssdw-md.md)] analizará la instrucción correctamente, pero omitirá el valor 1800 y seguirá usando el comportamiento predeterminado.  
  
```sql  
SET LOCK_TIMEOUT 1800;  
```  
  
## <a name="see-also"></a>Consulte también  
 [@@LOCK_TIMEOUT &#40;Transact-SQL&#41;](../../t-sql/functions/lock-timeout-transact-sql.md)   
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  

