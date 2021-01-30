---
description: sp_helpmergepartition (Transact-SQL)
title: sp_helpmergepartition (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergepartition
- sp_helpmergepartition_TSQL
helpviewer_keywords:
- sp_helpmergepartition
ms.assetid: 184188cc-f519-445d-97ce-aae38f1eb550
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 06994bb4f0e606f0d67e797603e6f4a23899076b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185731"
---
# <a name="sp_helpmergepartition-transact-sql"></a>sp_helpmergepartition (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información de partición para la publicación de combinación especificada. Este procedimiento almacenado se ejecuta en el publicador de cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpmergepartition [ @publication= ] 'publication'   
    [ , [ @suser_sname = ] 'suser_sname' ]  
    [ , [ @host_name = ] 'host_name' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @suser_sname = ] 'suser_sname'` Es el valor de SUSER_SNAME que se usa para definir una partición. *SUSER_SNAME* es de **tipo sysname y su** valor predeterminado es NULL. Proporcione este parámetro para limitar el conjunto de resultados tan solo a las particiones en que SUSER_SNAME se resuelve en el valor suministrado.  
  
> [!NOTE]  
>  Cuando se proporciona *SUSER_SNAME* , *host_name* debe ser null.  
  
`[ @host_name = ] 'host_name'` Es el valor de HOST_NAME que se usa para definir una partición. *host_name* es de **tipo sysname y su** valor predeterminado es NULL. Proporcione este parámetro para limitar el conjunto de resultados tan solo a las particiones en que HOST_NAME se resuelve en el valor suministrado.  
  
> [!NOTE]  
>  Cuando se proporciona *SUSER_SNAME* , *host_name* debe ser null.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**partition**|**int**|Identifica la partición del suscriptor.|  
|**host_name**|**sysname**|Valor utilizado al crear la partición para una suscripción filtrada por el valor de la función [host_name](../../t-sql/functions/host-name-transact-sql.md) en el suscriptor.|  
|**suser_sname**|**sysname**|Valor utilizado al crear la partición para una suscripción filtrada por el valor de la función [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) en el suscriptor.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Ubicación de la instantánea de datos filtrados para la partición del suscriptor.|  
|**date_refreshed**|**datetime**|Última fecha en que el trabajo de instantáneas se ha ejecutado para generar la instantánea de datos filtrados para la partición.|  
|**dynamic_snapshot_jobid**|**uniqueidentifier**|Identifica el trabajo que crea la instantánea de datos filtrados para una partición.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helpmergepartition** se utiliza en la replicación de mezcla.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** y del rol fijo de base de datos **db_owner** pueden ejecutar **sp_helpmergepartition**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_addmergepartition &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md)   
 [sp_dropmergepartition &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergepartition-transact-sql.md)  
  
  
