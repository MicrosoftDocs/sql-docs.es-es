---
description: '&#x40;&#x40;IO_BUSY (Transact-SQL)'
title: '@@IO_BUSY (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@IO_BUSY'
- '@@IO_BUSY_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- ticks [SQL Server]
- I/O [SQL Server], time spent performing operations
- '@@IO_BUSY function'
- output operations [SQL Server]
- input operations [SQL Server]
- time [SQL Server], I/O operations
ms.assetid: 3c26770c-41ae-4e34-8c82-7bef920ffbca
author: cawrites
ms.author: chadam
ms.openlocfilehash: 82501e158fcb976bde028e5849fba67cdf48a9ff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158707"
---
# <a name="x40x40io_busy-transact-sql"></a>&#x40;&#x40;IO_BUSY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve el tiempo que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha invertido en realizar operaciones de entrada y salida desde la última vez que se inició [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El resultado se expresa en incrementos de tiempo de CPU ("tics") y se acumula para todas las CPU, por lo que puede superar el tiempo que ha transcurrido realmente. Multiplique por @@TIMETICKS para convertir a microsegundos.  
  
> [!NOTE]  
>  Si el tiempo devuelto en @@CPU_BUSY o @@IO_BUSY supera aproximadamente 49 días de tiempo de CPU acumulado, recibirá una advertencia de desbordamiento aritmético. En este caso, el valor de las variables @@CPU_BUSY, @@IO_BUSY y @@IDLE no es exacto.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
@@IO_BUSY  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 **integer**  
  
## <a name="remarks"></a>Comentarios  
 Para mostrar un informe que contenga varias estadísticas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ejecute sp_monitor.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se muestra cómo devolver el número de milisegundos que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha dedicado a realizar operaciones de entrada o salida desde que se inició hasta la hora actual. Para evitar el desbordamiento aritmético al convertir el valor a microsegundos, en el ejemplo se convierte uno de los valores al tipo de datos **float**.  
  
```sql  
SELECT @@IO_BUSY*@@TIMETICKS AS 'IO microseconds',   
   GETDATE() AS 'as of';  
```  
  
 Éste es un conjunto de resultados típico:  
  
```  
  
IO microseconds as of                   
--------------- ----------------------  
4552312500      12/5/2006 10:23:00 AM   
```  
  
## <a name="see-also"></a>Vea también  
 [sys.dm_os_sys_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)   
 [@@CPU_BUSY &#40;Transact-SQL&#41;](../../t-sql/functions/cpu-busy-transact-sql.md)   
 [sp_monitor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)   
 [Funciones estadísticas del sistema &#40;Transact-SQL&#41;](../../t-sql/functions/system-statistical-functions-transact-sql.md)  
  
  
