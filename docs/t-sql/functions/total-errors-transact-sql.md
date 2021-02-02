---
description: '&#x40;&#x40;TOTAL_ERRORS (Transact-SQL)'
title: '@@TOTAL_ERRORS (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@TOTAL_ERRORS'
- '@@TOTAL_ERRORS_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@TOTAL_ERRORS function'
- total errors [SQL Server]
- errors [SQL Server], read/write
- number of disk read/write errors
- disks [SQL Server], errors
- write errors [SQL Server]
- read/write errors
ms.assetid: 09e62428-ee0e-4ef5-b969-da9d255f1199
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 55c3752ecc2f515e8309b753b578d989081233e4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158674"
---
# <a name="x40x40total_errors-transact-sql"></a>&#x40;&#x40;TOTAL_ERRORS (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve el número de errores de escritura en disco que ha encontrado [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desde la última vez que se inició [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
@@TOTAL_ERRORS  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 **integer**  
  
## <a name="remarks"></a>Observaciones  
 Esta función no se ocupa de todos los errores de escritura encontrados por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los errores de escritura ocasionales que no son graves se administran en el propio servidor y no se consideran errores. Para mostrar un informe con varias estadísticas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], incluido el número total de errores, ejecute **sp_monitor**.  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se muestra el número de errores que ha encontrado [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hasta la fecha y hora actuales.  
  
```sql
SELECT @@TOTAL_ERRORS AS 'Errors', GETDATE() AS 'As of';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Errors      As of                   
----------- ----------------------  
0           3/28/2003 12:32:11 PM   
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_monitor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)   
 [Funciones estadísticas del sistema &#40;Transact-SQL&#41;](../../t-sql/functions/system-statistical-functions-transact-sql.md)  
  
  
