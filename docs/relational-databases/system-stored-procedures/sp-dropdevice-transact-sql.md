---
description: sp_dropdevice (Transact-SQL)
title: sp_dropdevice (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropdevice_TSQL
- sp_dropdevice
dev_langs:
- TSQL
helpviewer_keywords:
- backup devices [SQL Server], deleting
- sp_dropdevice
ms.assetid: c8b07189-7c35-414b-acc1-45bd6e7e17c3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5df2e24fa3ca61cb235153e0157dbb953a828629
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200672"
---
# <a name="sp_dropdevice-transact-sql"></a>sp_dropdevice (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita un dispositivo de base de datos o un dispositivo de copia de seguridad de una instancia de [!INCLUDE[ssDEversion2005](../../includes/ssdeversion2005-md.md)] , eliminando la entrada de **master.dbo.sysdispositivos**.  
   
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_dropdevice [ @logicalname = ] 'device'   
    [ , [ @delfile = ] 'delfile' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @logicalname = ] 'device'` Es el nombre lógico del dispositivo de base de datos o del dispositivo de copia de seguridad como se indica en **master.dbo.sysDevices.Name**. *Device* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @delfile = ] 'delfile'` Especifica si se debe eliminar el archivo físico del dispositivo de copia de seguridad. *delfile* es **VARCHAR (7)**. Si se especifica como **DELFILE**, se elimina el archivo de disco físico del dispositivo de copia de seguridad.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 no se puede usar **sp_dropdevice** dentro de una transacción.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol fijo de servidor **diskadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se muestra cómo quitar el dispositivo de volcado de cinta `tapedump1` del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
```  
EXEC sp_dropdevice 'tapedump1';  
```  
  
## <a name="see-also"></a>Vea también  
 [Dispositivos de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [Eliminar un dispositivo de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/delete-a-backup-device-sql-server.md)   
 [sp_addumpdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)   
 [sp_helpdb &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpdb-transact-sql.md)   
 [sp_helpdevice &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpdevice-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
