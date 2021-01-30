---
title: sys.sp_rda_reconcile_batch (Transact-SQL) | Microsoft Docs
description: Aprenda a usar sys.sp_rda_reconcile_batch para reconciliar el identificador de lote en la tabla de SQL Server habilitada para Stretch con el identificador de lote almacenado en la tabla remota de Azure.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_reconcile_batch
- sys.sp_rda_reconcile_batch_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_reconcile_batch stored procedure
ms.assetid: 6d21eac3-7b6c-4fe0-8bc4-bf503f3948a6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 858da83b7529198da29cc5665219e9b83dfe3a41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211789"
---
# <a name="syssp_rda_reconcile_batch-transact-sql"></a>sys.sp_rda_reconcile_batch (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Concilia el ID. de lote almacenado en la tabla de SQL Server habilitada para Stretch con el identificador de lote almacenado en la tabla remota de Azure.  
  
 Normalmente solo tiene que ejecutar **sp_rda_reconcile_batch** si ha eliminado manualmente los datos migrados más recientemente de la tabla remota. Al eliminar manualmente los datos remotos que incluyen el lote más reciente, los ID. de lote no están sincronizados y la migración se detiene.  
 
 Para eliminar los datos que ya se han migrado a Azure, consulte la sección Comentarios de esta página.
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
   
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_rda_reconcile_batch @objname = '@objname'  
  
```  
  
## <a name="arguments"></a>Argumentos  
 \@objName = '*\@ objName*'  
 Nombre de la tabla de SQL Server habilitada para Stretch.  
  
## <a name="permissions"></a>Permisos  
 Requiere permisos de db_owner.  
  
## <a name="remarks"></a>Observaciones  
 Si desea eliminar los datos que ya se han migrado a Azure, haga lo siguiente.  
  
1.  Pausar la migración de datos. Para más información, vea [Pausa y reanudación de la migración de datos &#40;Stretch Database&#41;](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md).  
  
2.  Elimine los datos de la tabla de ensayo SQL Server ejecutando un comando DELETE con la sugerencia STAGE_ONLY. Para obtener más información, consulte [hacer actualizaciones y eliminaciones administrativas](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md#adminHints).
  
3.  Elimine los mismos datos de la tabla remota de Azure mediante la ejecución de un comando DELETE con la sugerencia REMOTE_ONLY.  
  
4.  Ejecute **sp_rda_reconcile_batch**.  
  
5.  Reanudar la migración de datos. Para más información, vea [Pausa y reanudación de la migración de datos &#40;Stretch Database&#41;](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md).  
  
## <a name="example"></a>Ejemplo  
 Para conciliar los identificadores de lote, ejecute la siguiente instrucción.  
  
```sql  
EXEC sp_rda_reconcile_batch @objname = N'StretchEnabledTableName';  
```  
  
  
