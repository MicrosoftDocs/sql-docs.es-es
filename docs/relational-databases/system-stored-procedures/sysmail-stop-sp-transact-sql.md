---
description: sysmail_stop_sp (Transact-SQL)
title: sysmail_stop_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_stop_sp_TSQL
- sysmail_stop_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_stop_sp
ms.assetid: 045ee36f-5bf0-4626-b5ee-e84db06ce16f
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: ba4f429ae813bcc58dd9fbbd24df2ab1a9300c78
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181907"
---
# <a name="sysmail_stop_sp-transact-sql"></a>sysmail_stop_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Detiene el Correo electrónico de base de datos deteniendo los objetos de [!INCLUDE[ssSB](../../includes/sssb-md.md)] que usa el programa externo.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sysmail_stop_sp  
```  
  
## <a name="arguments"></a>Argumentos  
 Ninguno  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 Este procedimiento almacenado está en la base de datos **msdb** .  
  
 Este procedimiento almacenado detiene la cola de Correo electrónico de base de datos que contiene las solicitudes de mensajes salientes y desactiva la activación de [!INCLUDE[ssSB](../../includes/sssb-md.md)] para el programa externo.  
  
 Cuando las colas se detienen, el programa externo de Correo electrónico de base de datos no procesa mensajes. Este procedimiento almacenado permite detener el Correo electrónico de base de datos para solucionar problemas o realizar tareas de mantenimiento.  
  
 Para iniciar Correo electrónico de base de datos, utilice **sysmail_start_sp**. Tenga en cuenta que **sp_send_dbmail** sigue aceptando correo cuando [!INCLUDE[ssSB](../../includes/sssb-md.md)] se detienen los objetos.  
  
> [!NOTE]  
>  Este procedimiento almacenado solamente detiene las colas del Correo electrónico de base de datos. No desactiva la entrega de mensajes de [!INCLUDE[ssSB](../../includes/sssb-md.md)] en la base de datos. Este procedimiento almacenado no deshabilita los procedimientos almacenados extendidos del Correo electrónico de base de datos para reducir el área expuesta. Para deshabilitar los procedimientos almacenados extendidos, consulte la [opción correo electrónico de base de datos XPS](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) del procedimiento almacenado del sistema **sp_configure** .  
  
## <a name="permissions"></a>Permisos  
 Los permisos de ejecución para este procedimiento tienen como valor predeterminado los miembros del rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se muestra cómo detener Correo electrónico de base de datos en la base de datos **msdb** . En este ejemplo se da por supuesto que el Correo electrónico de base de datos está habilitado.  
  
```  
USE msdb ;  
GO  
  
EXECUTE dbo.sysmail_stop_sp ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail.md)   
 [sysmail_start_sp &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sysmail-start-sp-transact-sql.md)   
 [Correo electrónico de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
