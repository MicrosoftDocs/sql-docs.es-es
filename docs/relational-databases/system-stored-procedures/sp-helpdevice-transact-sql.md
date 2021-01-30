---
description: sp_helpdevice (Transact-SQL)
title: sp_helpdevice (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpdevice
- sp_helpdevice_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpdevice
ms.assetid: 1a5eafa7-384e-4691-ba05-978eb73bbefb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 95c63d822851d7d1ccf9493e3e0d4cc9beedd624
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204736"
---
# <a name="sp_helpdevice-transact-sql"></a>sp_helpdevice (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Proporciona información acerca de los dispositivos de copia de seguridad de Microsoft® SQL Server™.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, se recomienda usar la vista de catálogo [Sys.backup_devices](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md) .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpdevice [ [ @devname = ] 'name' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @devname = ] 'name'` Es el nombre del dispositivo de copia de seguridad para el que se envía información. El valor de *Name* siempre es **sysname**.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**device_name**|**sysname**|Nombre del dispositivo lógico.|  
|**physical_name**|**nvarchar(260)**|Nombre de archivo físico.|  
|**description**|**nvarchar(255)**|Descripción del dispositivo.|  
|**status**|**int**|Número que corresponde a la descripción del estado en la columna **Descripción** .|  
|**cntrltype**|**smallint**|Tipo de controlador del dispositivo:<br /><br /> 2 = Dispositivo de disco<br /><br /> 5 = Dispositivo de cinta|  
|**size**|**int**|Tamaño del dispositivo en páginas de 2 KB.|  
  
## <a name="remarks"></a>Observaciones  
 Si se especifica *Name* , **sp_helpdevice** muestra información sobre el dispositivo de volcado de memoria especificado. Si no se especifica *Name* , **sp_helpdevice** muestra información acerca de todos los dispositivos de volcado en la vista de catálogo **Sys.backup_devices** .  
  
 Los dispositivos de volcado de memoria se agregan al sistema mediante **sp_addumpdevice**.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se proporciona información acerca de todos los dispositivos de volcado de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
EXEC sp_helpdevice;  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_addumpdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)   
 [sp_dropdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdevice-transact-sql.md)   
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
