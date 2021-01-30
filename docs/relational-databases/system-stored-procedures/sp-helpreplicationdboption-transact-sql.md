---
description: sp_helpreplicationdboption (Transact-SQL)
title: sp_helpreplicationdboption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpreplicationdboption_TSQL
- sp_helpreplicationdboption
helpviewer_keywords:
- sp_helpreplicationdboption
ms.assetid: 143ce689-108b-49d7-9892-fd3a86897f38
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 17c47d247e154ed80f0daeb8bacdb4984864fa03
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210812"
---
# <a name="sp_helpreplicationdboption-transact-sql"></a>sp_helpreplicationdboption (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Indica si las bases de datos del publicador están habilitadas para la replicación. Este procedimiento almacenado se ejecuta en el publicador de cualquier base de datos. *No es compatible con publicadores de Oracle.*  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpreplicationdboption [ [ @dbname =] 'dbname' ]  
    [ , [ @type = ] 'type' ]  
    [ , [ @reserved = ] reserved ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @dbname = ] 'dbname'` Es el nombre de la base de datos. *dbname* es de **tipo sysname y su** valor predeterminado es **%** . Si **%** es, el conjunto de resultados contiene todas las bases de datos del publicador; de lo contrario, solo se devuelve información sobre la base de datos especificada. No se devuelve ninguna información para las bases de datos en que el usuario no tiene los permisos correspondientes según se describe a continuación.  
  
`[ @type = ] 'type'` Restringe el conjunto de resultados para que contenga solo las bases de datos en las que se ha habilitado el valor de *tipo* de opción de replicación especificado. *Type* es de tipo **sysname** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**publicar**|La replicación transaccional está permitida.|  
|**merge publish**|La replicación de mezcla está permitida.|  
|**replicación permitida** (valor predeterminado)|La replicación transaccional o de mezcla están permitidas.|  
  
`[ @reserved = ] reserved` Especifica si se devuelve información sobre las publicaciones y suscripciones existentes. *Reserved* es de **bit** y su valor predeterminado es 0. Si es **1**, el conjunto de resultados incluye información sobre si la base de datos especificada tiene publicaciones o suscripciones existentes.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre de la base de datos.|  
|**id**|**int**|Identificador de base de datos.|  
|**transpublish**|**bit**|Si la base de datos se ha habilitado para la publicación transaccional o de instantáneas; donde un valor de **1** significa que la publicación de instantáneas o transaccional está habilitada.|  
|**mergepublish**|**bit**|Si la base de datos se ha habilitado para la publicación de mezcla; donde un valor de **1** significa que la publicación de combinación está habilitada.|  
|**Dbowner**|**bit**|Si el usuario es miembro del rol fijo de base de datos **db_owner** ; donde un valor de **1** indica que el usuario es miembro de este rol.|  
|**dbreadonly**|**bit**|Indica si la base de datos está marcada como de solo lectura; donde un valor de **1** significa que la base de datos es de solo lectura.|  
|**haspublications**|**bit**|Es si la base de datos tiene publicaciones existentes; donde un valor de **1** significa que hay publicaciones existentes.|  
|**haspullsubscriptions**|**bit**|Es si la base de datos tiene suscripciones de extracción existentes; donde un valor de **1** significa que hay suscripciones de extracción existentes.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helpreplicationdboption** se utiliza en la replicación de instantáneas, transaccional y de mezcla.  
  
## <a name="permissions"></a>Permisos  
 Los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_helpreplicationdboption** para cualquier base de datos. Los miembros del rol fijo de base de datos **db_owner** pueden ejecutar **sp_helpreplicationdboption** para esa base de datos.  
  
## <a name="see-also"></a>Consulte también  
 [sp_replicationdboption &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
