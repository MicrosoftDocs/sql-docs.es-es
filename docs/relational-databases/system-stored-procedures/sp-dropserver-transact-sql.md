---
description: sp_dropserver (Transact-SQL)
title: sp_dropserver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropserver_TSQL
- sp_dropserver
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropserver
ms.assetid: 0fc83e35-0caa-49a3-a4b6-a1890d4f46ef
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 9dd47ba517e0a38005887af42c71346e8af64645
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187014"
---
# <a name="sp_dropserver-transact-sql"></a>sp_dropserver (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Quita un servidor de la lista de servidores remotos y vinculados conocidos de la instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![icono de vínculo](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
sp_dropserver [ @server = ] 'server'   
     [ , [ @droplogins = ] { 'droplogins' | NULL} ]  
```  
  
## <a name="arguments"></a>Argumentos  
 *servidor*  
 Es el servidor que se va a quitar. *server* es de tipo **sysname** y no tiene ningún valor predeterminado. el *servidor* debe existir.  
  
 *droplogins*  
 Indica que también se deben quitar los inicios de sesión de servidores remotos y vinculados relacionados para el *servidor* si se especifica **droplogins** . **`@droplogins`** es de tipo **Char (10)** y su valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 Si ejecuta **sp_dropserver** en un servidor que tiene asociadas entradas de inicio de sesión de servidor vinculado y remoto, o que está configurada como publicador de replicación, se devuelve un mensaje de error. Para quitar todos los inicios de sesión de servidor remoto y vinculado de un servidor al quitar el servidor, use el argumento **droplogins** .  
  
 no se puede ejecutar **sp_dropserver** dentro de una transacción definida por el usuario.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER ANY LINKED SERVER en el servidor.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se quita el servidor remoto `ACCOUNTS` y todos los inicios de sesión remotos asociados de la instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
sp_dropserver 'ACCOUNTS', 'droplogins';  
```  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addserver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [sp_dropremotelogin &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropremotelogin-transact-sql.md)   
 [sp_helpremotelogin &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpremotelogin-transact-sql.md)   
 [sp_helpserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
