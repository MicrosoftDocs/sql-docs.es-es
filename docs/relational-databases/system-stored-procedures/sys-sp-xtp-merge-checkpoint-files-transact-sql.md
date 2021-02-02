---
description: sys.sp_xtp_merge_checkpoint_files (Transact-SQL)
title: sys.sp_xtp_merge_checkpoint_files (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/28/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_xtp_merge_checkpoint_files_TSQL
- sys.sp_xtp_merge_checkpoint_files
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_xtp_merge_checkpoint_files
ms.assetid: da04df2a-f7a1-41e7-a1ef-2d5d68919892
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8e6e990cd4db15656ef8102713f0e51b685f5f30
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99234883"
---
# <a name="syssp_xtp_merge_checkpoint_files-transact-sql"></a>sys.sp_xtp_merge_checkpoint_files (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  **Sys.sp_xtp_merge_checkpoint_files** combina todos los archivos Delta y de datos en el intervalo de transacciones especificado.  
  
 Para obtener más información, vea [crear y administrar el almacenamiento de objetos de Memory-Optimized](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
||  
|-|  
|**Nota**: este procedimiento almacenado está en desuso en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] . Ya no es necesario y no se puede utilizar, a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] .|  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.sp_xtp_merge_checkpoint_files database_name, @transaction_lower_bound, @transaction_upper_bound  
[;]  
```  
  
## <a name="arguments"></a>Argumentos  
 *database_name*  
 Nombre de la base de datos en la que se va a invocar la combinación. Si la base de datos no tiene tablas en memoria, este procedimiento devuelve un error de usuario. Si la base de datos está sin conexión, devuelve un error.  
  
 *lower_bound_Tid*  
 El límite inferior (BIGINT) de las transacciones de un archivo de datos, tal como se muestra en [sys.dm_db_xtp_checkpoint_files &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-files-transact-sql.md) correspondiente al archivo de punto de comprobación de inicio de la combinación. Se genera un error si se especifica un valor no válido para transactonId.  
  
 *upper_bound_Tid*  
 El límite superior (BIGINT) de las transacciones para un archivo de datos, tal como se muestra en [sys.dm_db_xtp_checkpoint_files &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-files-transact-sql.md). Se genera un error si se especifica un valor no válido para transactonId.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="cursors-returned"></a>Cursores devueltos  
 None  
  
## <a name="permissions"></a>Permisos  
 Requiere el rol fijo de servidor sysadmin y el rol fijo de base de datos db_owner.  
  
## <a name="remarks"></a>Notas  
 Combina todos los archivos delta y de datos del intervalo válido para generar un único archivo delta y de datos. Este procedimiento no respeta la directiva de combinación.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [OLTP en memoria &#40;optimización en memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
