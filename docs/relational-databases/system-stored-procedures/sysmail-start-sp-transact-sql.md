---
description: sysmail_start_sp (Transact-SQL)
title: sysmail_start_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_start_sp
- sysmail_start_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_start_sp
ms.assetid: 25fd7bb6-cfdd-463f-bea8-c6fcb805d3f5
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: c3685409d82c2c1591ae129b93c4c3036f5925c1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181896"
---
# <a name="sysmail_start_sp-transact-sql"></a>sysmail_start_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Inicia el Correo electrónico de base de datos iniciando los objetos de [!INCLUDE[ssSB](../../includes/sssb-md.md)] que usa el programa externo.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sysmail_start_sp  
```  
  
## <a name="arguments"></a>Argumentos  
 Ninguno  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 La característica Correo electrónico de base de datos no se habilita o ni se instala con la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilice el Asistente para configuración de Correo electrónico de base de datos con el fin de habilitar e instalar los objetos de Correo electrónico de base de datos.  
  
 Este procedimiento almacenado está en la base de datos **msdb** . Este procedimiento almacenado inicia la cola de Correo electrónico de base de datos que contiene las solicitudes de mensajes salientes y habilita la activación de [!INCLUDE[ssSB](../../includes/sssb-md.md)] para el programa externo.  
  
 Cuando las colas se inician, el programa externo de Correo electrónico de base de datos puede procesar mensajes. Este procedimiento permite reiniciar las colas una vez detenidas las colas con el **sysmail_stop_sp** procedimiento almacenado.  
  
> [!NOTE]  
>  Este procedimiento almacenado solamente inicia las colas del Correo electrónico de base de datos. No activa la entrega de mensajes de [!INCLUDE[ssSB](../../includes/sssb-md.md)] en la base de datos.  
  
## <a name="permissions"></a>Permisos  
 Los permisos de ejecución para este procedimiento tienen como valor predeterminado los miembros del rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se muestra el inicio de Correo electrónico de base de datos en la base de datos **msdb** . En este ejemplo se da por supuesto que el Correo electrónico de base de datos está habilitado.  
  
```  
USE msdb ;  
GO  
  
EXECUTE dbo.sysmail_start_sp ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail.md)   
 [Correo electrónico de base de datos opción de configuración del servidor XPs](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md)   
 [sysmail_stop_sp &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sysmail-stop-sp-transact-sql.md)   
 [Correo electrónico de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
