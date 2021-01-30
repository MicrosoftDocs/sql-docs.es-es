---
description: managed_backup.fn_get_parameter managed_backup (Transact-SQL)
title: managed_backup managed_backup.fn_get_parameter (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/03/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- smart_admin.fn_get_parameter_TSQL
- smart_admin.fn_get_parameter
- fn_get_parameter_TSQL
- fn_get_parameter
dev_langs:
- TSQL
helpviewer_keywords:
- fn_get_parameter
- smart_admin.fn_get_parameter
ms.assetid: ed94e54d-4516-4806-a8ce-f013d3a04122
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: a30e80c084768df7a40977c052a8649adeae33fc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207387"
---
# <a name="managed_backupfn_get_parameter-transact-sql"></a>managed_backup.fn_get_parameter managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Devuelve una tabla de 0, 1 o más filas de pares de parámetro y valor.  
  
 Utilice este procedimiento almacenado para revisar todas las opciones de configuración o una opción de configuración concreta para Administración inteligente.  
  
 Si el parámetro no se ha configurado nunca, la función devuelve 0 filas.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
managed_backup.fn_get_parameter ('parameter_name' | '' | NULL )  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argumentos  
 parameter_name  
 Nombre del parámetro. parameter_name es **nvarchar (128)**. Si se proporciona NULL o una cadena vacía como argumento de la función, se devuelven pares nombre-valor para todos los parámetros de Administración inteligente configurados.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|parameter_name|NVARCHAR(128)|Nombre del parámetro. A continuación se muestra una lista de los parámetros devueltos:<br/><br/>**FileRetentionDebugXevent**<br/><br/>**SSMBackup2WADebugXevent**<br/><br/>**SSMBackup2WANotificationEmailIds**<br/><br/>**SSMBackup2WAEnableUserDefinedPolicy**<br/><br/>**SSMBackup2WAEverConfigured**<br/><br/>**StorageOperationDebugXevent**|  
|parameter_value|NVARCHAR(128)|Valor establecido actual del parámetro.|  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Requiere permisos SELECT en la función.  
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente devuelve todos los parámetros que se han configurado al menos una vez, y sus valores actuales.  
  
```  
USE MSDB  
GO  
SELECT *   
FROM managed_backup.fn_get_parameter (NULL)  
  
```  
  
 El ejemplo siguiente devuelve el identificador de correo electrónico especificado para recibir las notificaciones de error. Si no se devuelve ninguna fila, significa que esta opción de notificación por correo electrónico no se ha habilitado.  
  
```  
USE MSDB  
GO  
SELECT *  
FROM managed_backup.fn_get_parameter ('SSMBackup2WANotficationEmailIds')  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [Copia de seguridad administrada en Microsoft Azure para SQL Server](../../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md)  
  
  
