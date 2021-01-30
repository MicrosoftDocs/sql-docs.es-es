---
description: managed_backup.fn_get_current_xevent_settings managed_backup (Transact-SQL)
title: managed_backup managed_backup.fn_get_current_xevent_settings (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_get_current_xevent_settings
- smart_admin.fn_get_current_xevent_settings_TSQL
- fn_get_current_xevent_settings_TSQL
- smart_admin.fn_get_current_xevent_settings
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.fn_get_current_xevent_settings
- fn_get_current_xevent_settings
ms.assetid: 95d3adaa-bb9d-4833-b8b4-3d9fd4f9c82a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: e0bd15419c867134a66cf678cbb6355df0c4142f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207403"
---
# <a name="managed_backupfn_get_current_xevent_settings-transact-sql"></a>managed_backup.fn_get_current_xevent_settings managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Devuelve una fila para cada tipo de evento extendido compatible con Administración inteligente.  
  
 Utilice esta función para devolver o revisar la configuración de eventos extendidos para identificar el tipo de evento configurable y las configuraciones actuales.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
smart_admin.fn_get_current_xevent_settings ()   
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argumentos  
 Esta función no tiene argumentos.  
  
## <a name="table-returned"></a>Tabla devuelta  
 Los canales de administración, análisis y operativos de Eventos extendidos son necesarios, están habilitados de manera predeterminada y no son configurables.  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|Event_name|NVARCHAR(128)|Tipo de evento extendido|  
|is_configurable|NVARCHAR(128)|Este valor se establece en **true** si el evento es configurable; de lo contrario, se establece en **false**.|  
|is_enabled|NVARCHAR(128)|Se establece en True si el evento está habilitado y en False si no lo está. Utilice smart_admin.sp_set_parameter para habilitar eventos de depuración.|  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Requiere permisos **Select** en la función.  
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente devuelve todos los eventos extendidos con su estado actual.  
  
```  
SELECT *   
FROM smart_admin.fn_get_current_xevent_settings ()  
  
```  
  
  
