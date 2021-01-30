---
description: sp_replcmds (Transact-SQL)
title: sp_replcmds (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replcmds_TSQL
- sp_replcmds
helpviewer_keywords:
- sp_replcmds
ms.assetid: 7e932f80-cc6e-4109-8db4-2b7c8828df73
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d8d61544e171e102c04f829ab7c302048c930d21
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193517"
---
# <a name="sp_replcmds-transact-sql"></a>sp_replcmds (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve los comandos de las transacciones marcadas para replicación. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
> [!IMPORTANT]  
>  El procedimiento **sp_replcmds** debe ejecutarse únicamente para solucionar problemas de replicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_replcmds [ @maxtrans = ] maxtrans  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @maxtrans = ] maxtrans` Es el número de transacciones de las que se va a devolver información. *maxtrans* es de **tipo int** y su valor predeterminado es **1**, que especifica la siguiente transacción que espera para la distribución.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**identificador de artículo**|**int**|Id. del artículo.|  
|**partial_command**|**bit**|Indica si se trata de un comando parcial o no.|  
|**command**|**varbinary (1024)**|El valor del comando.|  
|**xactid**|**binary(10)**|Id. de la transacción.|  
|**xact_seqno**|**varbinary(16)**|El número de secuencia de la transacción.|  
|**publication_id**|**int**|Id. de la publicación.|  
|**command_id**|**int**|IDENTIFICADOR del comando en [MSrepl_commands](../../relational-databases/system-tables/msrepl-commands-transact-sql.md).|  
|**command_type**|**int**|Tipo de comando.|  
|**originator_srvname**|**sysname**|Servidor en el que se originó la transacción.|  
|**originator_db**|**sysname**|Base de datos en la que se originó la transacción.|  
|**pkHash**|**int**|Solo para uso interno.|  
|**originator_publication_id**|**int**|Id. de la publicación en la que se originó la transacción.|  
|**originator_db_version**|**int**|Versión de la base de datos en la que se originó la transacción.|  
|**originator_lsn**|**varbinary(16)**|Identifica el número de flujo de registro (LSN) para el comando de la publicación en la que se origina.|  
  
## <a name="remarks"></a>Observaciones  
 el proceso de registro del log utiliza **sp_replcmds** en la replicación transaccional.  
  
 La replicación trata el primer cliente que ejecuta **sp_replcmds** dentro de una base de datos determinada como lector del registro.  
  
 Este procedimiento puede generar comandos para tablas calificadas por el propietario o no calificar el nombre de la tabla (valor predeterminado). Agregar nombres de tabla calificados permite replicar datos de tablas que pertenecen a un usuario específico de una base de datos a tablas que pertenecen al mismo usuario en otra base de datos.  
  
> [!NOTE]  
>  Debido a que el nombre de tabla en la base de datos de origen está calificado mediante el nombre del propietario, el propietario de la tabla en la base de datos de destino deberá tener ese mismo nombre.  
  
 Los clientes que intentan ejecutar **sp_replcmds** dentro de la misma base de datos reciben el error 18752 hasta que el primer cliente se desconecte. Una vez que el primer cliente se desconecta, otro cliente puede ejecutar **sp_replcmds** y se convierte en el nuevo lector del registro.  
  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)] Si **sp_replcmds** no puede replicar un comando de texto porque no se recuperó el puntero de texto en la misma transacción, se agrega un mensaje de advertencia número 18759 al registro de errores de y al registro de aplicación de Windows.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_replcmds**.  
  
## <a name="see-also"></a>Consulte también  
 [Mensajes de error](../../relational-databases/native-client-odbc-error-messages/error-messages.md)   
 [sp_repldone &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-repldone-transact-sql.md)   
 [sp_replflush &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md)   
 [sp_repltrans &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-repltrans-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
