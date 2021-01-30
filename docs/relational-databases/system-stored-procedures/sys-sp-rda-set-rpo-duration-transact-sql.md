---
title: sys.sp_rda_set_rpo_duration (Transact-SQL) | Microsoft Docs
description: Más información sobre sys.sp_rda_set_rpo_duration. Utilice este procedimiento almacenado para establecer el número de horas de datos migrados que SQL Server conserva en una tabla de ensayo.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_set_rpo_duration
- sys.sp_rda_set_rpo_duration_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_set_rpo_duration stored procedure
ms.assetid: 95c80c5b-9252-4612-9ea7-544c48834fd2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: cc7bbf5ddcde440bb359616201602a9c463d87e6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186619"
---
# <a name="syssp_rda_set_rpo_duration-transact-sql"></a>sys.sp_rda_set_rpo_duration (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Establece el número de horas de datos migrados que SQL Server conserva en una tabla de ensayo para ayudar a garantizar una restauración completa de la base de datos remota de Azure, en caso de que sea necesaria una restauración a un momento dado.    
    
 Para obtener más información, consulte [Stretch Database reduce el riesgo de pérdida de datos en los datos de Azure al conservar temporalmente las filas migradas](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md#stretchRPO).  
   
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)    
     
## <a name="syntax"></a>Sintaxis    
    
```    
    
sp_rda_set_rpo_duration [ @duration_hrs = ] duration_hrs    
    
```    
    
## <a name="arguments"></a>Argumentos    
 [ @duration_hrs =] *duration_hrs*    
 Es el número de horas (valor entero no NULL) de los datos migrados que desea que SQL Server conservar en la base de datos habilitada para Stretch actual. El valor predeterminado y el valor mínimo son 8 horas.    
 
 > [!NOTE]
 > Los valores más altos requieren más espacio de almacenamiento en SQL Server.
    
## <a name="permissions"></a>Permisos    
 Requiere permisos de db_owner.    
    
## <a name="remarks"></a>Observaciones    
 Para obtener el valor actual, ejecute [sys.sp_rda_get_rpo_duration &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-rda-get-rpo-duration-transact-sql.md).    
    
## <a name="see-also"></a>Consulte también    
 [sys.sp_rda_get_rpo_duration &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-rda-get-rpo-duration-transact-sql.md)     
 [Restaurar bases de datos habilitadas para Stretch (Stretch Database)](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)     
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)    
    
  
