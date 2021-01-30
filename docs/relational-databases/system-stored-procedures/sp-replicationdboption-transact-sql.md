---
description: sp_replicationdboption (Transact-SQL)
title: sp_replicationdboption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replicationdboption_TSQL
- sp_replicationdboption
helpviewer_keywords:
- sp_replicationdboption
ms.assetid: d021864e-3f21-4d1a-89df-6c1086f753bf
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e56271516d9ef2df60e993cd73963d074b503d42
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211873"
---
# <a name="sp_replicationdboption-transact-sql"></a>sp_replicationdboption (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Establece una opción de base de datos de replicación para la base de datos especificada. Este procedimiento almacenado se ejecuta en el publicador o el suscriptor de cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_replicationdboption [ @dbname= ] 'db_name'   
        , [ @optname= ] 'optname'   
        , [ @value= ] 'value'   
    [ , [ @ignore_distributor= ] ignore_distributor ]  
    [ , [ @from_scripting = ] from_scripting ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @dbname = ] 'dbname'` Es la base de datos para la que se establece la opción de base de datos de replicación. *db_name* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @optname = ] 'optname'` Es la opción de base de datos de replicación para habilitar o deshabilitar. *optname* es de **tipo sysname** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**merge publish**|Se puede utilizar la base de datos para publicaciones de combinación.|  
|**publicar**|Se puede utilizar la base de datos para otros tipos de publicaciones.|  
|**ID**|La base de datos es una base de datos de suscripciones.|  
|**sync with backup**|La base de datos está habilitada para una copia de seguridad coordinada. Para obtener más información, vea [Habilitar copias de seguridad coordinadas para la replicación transaccional &#40;la programación de la replicación con Transact-SQL&#41;](../../relational-databases/replication/administration/enable-coordinated-backups-for-transactional-replication.md).|  
  
`[ @value = ] 'value'` Indica si se debe habilitar o deshabilitar la opción de base de datos de replicación dada. *Value* es de **tipo sysname** y puede ser **true** o **false**. Cuando este valor es **false** y *optname* es **Merge Publish**, también se quitan las suscripciones a la base de datos publicada de mezcla.  
  
`[ @ignore_distributor = ] ignore_distributor` Indica si este procedimiento almacenado se ejecuta sin conectarse al distribuidor. *ignore_distributor* es de **bits**, con un valor predeterminado de **0**, lo que significa que el distribuidor debe estar conectado al nuevo estado de la base de datos de publicación y actualizarse. Solo se debe especificar el valor **1** si no se puede tener acceso al distribuidor y se usa **sp_replicationdboption** para deshabilitar la publicación.  
  
`[ @from_scripting = ] from_scripting` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_replicationdboption** se utiliza en la replicación de instantáneas, la replicación transaccional y la replicación de mezcla.  
  
 Este procedimiento crea o quita tablas específicas del sistema de replicación, cuentas de seguridad, etc., según las opciones proporcionadas. Establece el **is_published** correspondiente (replicación de instantáneas o transacational), **is_merge_published** (replicación de mezcla) o **is_distributor** bits en la tabla del sistema **Master. Databases** y crea las tablas del sistema necesarias.  
  
 Para deshabilitar la publicación, la base de datos de publicaciones debe estar en línea. Si existe una instantánea de base de datos para la base de datos de publicaciones, se debe quitar la instantánea antes de deshabilitar la publicación. Las instantáneas de base de datos son copias de solo lectura y sin conexión de bases de datos, y no están relacionadas con una instantánea de replicación. Para más información, vea [Instantáneas de base de datos &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md).  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_replicationdboption**.  
  
## <a name="see-also"></a>Consulte también  
 [Configurar la publicación y la distribución](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Eliminar una publicación](../../relational-databases/replication/publish/delete-a-publication.md)   
 [Disable Publishing and Distribution](../../relational-databases/replication/disable-publishing-and-distribution.md)  (Deshabilitar la publicación y la distribución)  
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
