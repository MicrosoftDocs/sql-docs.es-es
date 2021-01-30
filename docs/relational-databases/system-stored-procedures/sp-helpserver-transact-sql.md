---
description: sp_helpserver (Transact-SQL)
title: sp_helpserver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpserver
- sp_helpserver_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpserver
ms.assetid: e8f42de7-c738-41c3-8bf5-dbd559dc7184
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 83d212a6cfb817f3aa28c87ab0999d0488cec5e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211921"
---
# <a name="sp_helpserver-transact-sql"></a>sp_helpserver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Presenta información acerca de un servidor remoto o de replicación concreto, o acerca de todos los servidores de los dos tipos. Proporciona la siguiente información sobre el servidor: nombre, nombre de red, estado de replicación, número de identificación y nombre de intercalación. Asimismo proporciona valores de tiempo de espera para conectarse a servidores vinculados o para realizar consultas en los mismos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpserver [ [ @server = ] 'server' ]   
  [ , [ @optname = ] 'option' ]   
  [ , [ @show_topology = ] 'show_topology' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @server = ] 'server'` Es el servidor sobre el que se envía información. Cuando no se especifica el *servidor* , informa de todos los servidores de **master.sys. Servers**. *Server* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @optname = ] 'option'` Es la opción que describe el servidor. *Option* es de tipo **VARCHAR (** 35 **)**, su valor predeterminado es NULL y debe ser uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**compatible con la intercalación**|Afecta a la ejecución de consultas distribuidas sobre servidores vinculados. Si esta opción se establece en TRUE,|  
|**acceso a datos**|Habilita y deshabilita un servidor vinculado para el acceso a consultas distribuidas.|  
|**dist**|Distribuidor.|  
|**dpub**|Publicador remoto de este distribuidor.|  
|**lazy schema validation**|Omite la comprobación del esquema de las tablas remotas al comienzo de la consulta.|  
|**Pub**|Editor.|  
|**RPC**|Habilita RPC desde el servidor especificado.|  
|**salida de RPC**|Habilita RPC en el servidor especificado.|  
|**sub**|Suscriptor.|  
|**sistema**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**usar intercalación remota**|Usa la intercalación de una columna remota en lugar de la del servidor local.|  
  
`[ @show_topology = ] 'show_topology'` Es la relación del servidor especificado con otros servidores. *show_topology* es de tipo **VARCHAR (** 1 **)** y su valor predeterminado es NULL. Si *show_topology* no es igual a **t** o es null, **sp_helpserver** devuelve las columnas enumeradas en la sección conjuntos de resultados. Si *show_topology* es igual a **t**, además de las columnas enumeradas en los conjuntos de resultados, **sp_helpserver** también devuelve información de **TopX** y **Topy** .  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error).  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre de servidor.|  
|**network_name**|**sysname**|El nombre de red del servidor.|  
|**status**|**VARCHAR (** 70 **)**|Estado del servidor.|  
|**id**|**Char (** 4 **)**|Número de identificación del servidor.|  
|**collation_name**|**sysname**|Intercalación del servidor.|  
|**connect_timeout**|**int**|Valor del tiempo de espera para conectar a un servidor vinculado.|  
|**query_timeout**|**int**|Valor del tiempo de espera para consultas sobre un servidor vinculado.|  
  
## <a name="remarks"></a>Observaciones  
 Un servidor puede tener varios estados.  
  
## <a name="permissions"></a>Permisos  
 No se comprueba ningún permiso.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-displaying-information-about-all-servers"></a>A. Presentar información acerca de todos los servidores  
 En este ejemplo se presenta información acerca de todos los servidores mediante el uso de `sp_helpserver` sin parámetros.    
  
```  
USE master;  
GO  
EXEC sp_helpserver;  
```  
  
### <a name="b-displaying-information-about-a-specific-server"></a>B. Presentar información acerca de un servidor específico  
 En este ejemplo se presenta toda la información acerca del servidor `SEATTLE2`.  
  
```  
USE master;  
GO  
EXEC sp_helpserver 'SEATTLE2';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sp_adddistpublisher &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_addserver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [sp_addsubscriber &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addsubscriber-transact-sql.md)   
 [sp_changesubscriber &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changesubscriber-transact-sql.md)   
 [sp_dropserver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropserver-transact-sql.md)   
 [sp_dropsubscriber &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropsubscriber-transact-sql.md)   
 [sp_helpdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [sp_helpremotelogin &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpremotelogin-transact-sql.md)   
 [sp_helpsubscriberinfo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md)   
 [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
