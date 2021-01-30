---
description: sp_helpdistributor (Transact-SQL)
title: sp_helpdistributor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpdistributor_TSQL
- sp_helpdistributor
helpviewer_keywords:
- sp_helpdistributor
ms.assetid: 37b0983e-3b69-4f0f-977e-20efce0a0b97
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c23613a7b79ea7bfa418425a32b6d2ee9bf62699
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186984"
---
# <a name="sp_helpdistributor-transact-sql"></a>sp_helpdistributor (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Muestra información acerca del distribuidor, la base de datos de distribución, el directorio de trabajo y la [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuenta de usuario del agente. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicaciones o en cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpdistributor [ [ @distributor= ] 'distributor' OUTPUT ]  
    [ , [ @distribdb= ] 'distribdb' OUTPUT ]  
    [ , [ @directory= ] 'directory' OUTPUT ]  
    [ , [ @account= ] 'account' OUTPUT ]  
    [ , [ @min_distretention= ] min_distretention OUTPUT ]  
    [ , [ @max_distretention= ] max_distretention OUTPUT ]  
    [ , [ @history_retention= ] history_retention OUTPUT ]  
    [ , [ @history_cleanupagent= ] 'history_cleanupagent' OUTPUT ]  
    [ , [ @distrib_cleanupagent = ] 'distrib_cleanupagent' OUTPUT ]  
    [ , [ @publisher = ] 'publisher' ]   
    [ , [ @local = ] 'local' ]  
    [ , [ @rpcsrvname= ] 'rpcsrvname' OUTPUT ]  
    [ , [ @publisher_type = ] 'publisher_type' OUTPUT ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @distributor = ] 'distributor' OUTPUT` Es el nombre del distribuidor. Distributor es de **tipo sysname y su** valor predeterminado **%** es, que es el único valor que devuelve un conjunto de resultados.  
  
`[ @distribdb = ] 'distribdb' OUTPUT` Es el nombre de la base de datos de distribución. *distribdb* es de **tipo sysname y su** valor predeterminado es **%** , que es el único valor que devuelve un conjunto de resultados.  
  
`[ @directory = ] 'directory' OUTPUT` Es el directorio de trabajo. el *directorio* es de tipo **nvarchar (255)** y su valor predeterminado es **%** , que es el único valor que devuelve un conjunto de resultados.  
  
`[ @account = ] 'account' OUTPUT` Es la [!INCLUDE[msCoName](../../includes/msconame-md.md)] cuenta de usuario de Windows. *account* es de tipo **nvarchar (255)** y su valor predeterminado **%** es, que es el único valor que devuelve un conjunto de resultados.  
  
`[ @min_distretention = ] _min_distretentionOUTPUT` Es el período mínimo de retención de la distribución, en horas. *min_distretention* es de **tipo int** y su valor predeterminado es **-1**.  
  
`[ @max_distretention = ] _max_distretentionOUTPUT` Es el período máximo de retención de distribución, en horas. *max_distretention* es de **tipo int** y su valor predeterminado es **-1**.  
  
`[ @history_retention = ] _history_retentionOUTPUT` Es el período de retención del historial, en horas. *history_retention* es de **tipo int** y su valor predeterminado es **-1**.  
  
`[ @history_cleanupagent = ] 'history_cleanupagent' OUTPUT` Es el nombre del agente de limpieza del historial. *history_cleanupagent* es de tipo **nvarchar (100)** y su valor predeterminado es **%** , que es el único valor que devuelve un conjunto de resultados.  
  
`[ @distrib_cleanupagent = ] 'distrib_cleanupagent' OUTPUT` Es el nombre del agente de limpieza de distribución. *distrib_cleanupagent* es de tipo **nvarchar (100)** y su valor predeterminado es **%** , que es el único valor que devuelve un conjunto de resultados.  
  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @local = ] 'local'` Indica si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe obtener valores del servidor local. *local* es de tipo **nvarchar (5)** y su valor predeterminado es NULL.  
  
`[ @rpcsrvname = ] 'rpcsrvname' OUTPUT` Es el nombre del servidor que emite llamadas a procedimientos remotos. *rpcsrvname* es de **tipo sysname y su** valor predeterminado es **%** , que es el único valor que devuelve un conjunto de resultados.  
  
`[ @publisher_type = ] 'publisher_type' OUTPUT` Es el tipo de publicador del publicador. *publisher_type* es de **tipo sysname y su** valor predeterminado es **%** , que es el único valor que devuelve un conjunto de resultados.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**distribuidor**|**sysname**|Nombre del distribuidor.|  
|**base de datos de distribución**|**sysname**|Nombre de la base de datos de distribución.|  
|**Active**|**nvarchar(255)**|Nombre del directorio de trabajo.|  
|**account**|**nvarchar(255)**|Nombre de la cuenta de usuario de Windows.|  
|**min distrib retention**|**int**|Período mínimo de retención de la distribución.|  
|**max distrib retention**|**int**|Período máximo de retención de la distribución.|  
|**history retention**|**int**|Período de retención del historial.|  
|**history cleanup agent**|**nvarchar(100**|Nombre del Agente de limpieza del historial.|  
|**distribution cleanup agent**|**nvarchar(100**|Nombre del Agente de limpieza de distribución.|  
|**rpc server name**|**sysname**|Nombre del distribuidor remoto o local.|  
|**rpc login name**|**sysname**|Inicio de sesión utilizado por las llamadas a procedimientos remotos al distribuidor remoto.|  
|**tipo de publicador**|**sysname**|Tipo de publicador; puede ser uno de los siguientes:<br /><br /> **MSSQLSERVER**<br /><br /> **ORACLE**<br /><br /> **ORACLE GATEWAY**|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helpdistributor** se utiliza en todos los tipos de replicación.  
  
 Si se especifican uno o varios parámetros de salida al ejecutar **sp_helpdistributor**, se asignan valores a todos los parámetros de salida establecidos en NULL y no se devuelve ningún conjunto de resultados. Si no se especifica ningún parámetro de salida, se devuelve un conjunto de resultados.  
  
## <a name="permissions"></a>Permisos  
 Las siguientes columnas del conjunto de resultados o parámetros de salida se devuelven a los miembros del rol fijo de servidor **sysadmin** en el publicador y el rol fijo de base de datos **db_owner** en la base de datos de publicación:  
  
|Columna del conjunto de resultados|Parámetro de salida|  
|-----------------------|----------------------|  
|account|**\@Código**|  
|min distrib retention|**\@min_distretention**|  
|max distrib retention |**\@max_distretention**|  
|history retention|**\@history_retention**|  
|history cleanup agent|**\@history_cleanupagent**|  
|distribution cleanup agent|**\@distrib_cleanupagent**|  
|rpc login name|ninguno|  
  
 La siguiente columna de conjuntos de resultados se devuelve a los usuarios de la lista de acceso a la publicación para una publicación en el distribuidor:  
  
-   directory  
  
 Las siguientes columnas de conjuntos de resultados se devuelven a todos los usuarios.  
  
|Columna del conjunto de resultados|Parámetro de salida|  
|-----------------------|----------------------|  
|distributor|**\@distribuidor**|  
|base de datos de distribución|**\@distribdb**|  
|rpc server name|**\@rpcsrvname**|  
|publisher type|**\@publisher_type**|  
  
## <a name="see-also"></a>Consulte también  
 [Ver y modificar las propiedades del distribuidor y del publicador](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [sp_adddistpublisher &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_dropdistpublisher &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)  
  
  
