---
description: sp_helpmergealternatepublisher (Transact-SQL)
title: sp_helpmergealternatepublisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergealternatepublisher_TSQL
- sp_helpmergealternatepublisher
helpviewer_keywords:
- sp_helpmergealternatepublisher
ms.assetid: a96e365f-5967-4580-9d79-5bacf2d12211
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e7448059958f07ba1aab7e63a1a857e113e21432
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183108"
---
# <a name="sp_helpmergealternatepublisher-transact-sql"></a>sp_helpmergealternatepublisher (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una lista de todos los servidores habilitados como publicadores alternativos para publicaciones de combinación. Este procedimiento almacenado se ejecuta en el suscriptor de la base de datos de suscripciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpmergealternatepublisher [ @publisher = ] 'publisher', [ @publisher_db = ] 'publisher_db', [ @publication = ] 'publication'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre del publicador alternativo. *Publisher* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos de publicación. *publisher_db* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**alternate_publisher**|**sysname**|Nombre del publicador alternativo.|  
|**alternate_publisher_db**|**sysname**|Nombre de la base de datos de publicaciones.|  
|**alternate_publication**|**sysname**|Nombre de la publicación.|  
|**alternate_distributor**|**sysname**|Nombre del distribuidor.|  
|**friendly_name**|**nvarchar(255)**|Descripción del publicador alternativo.|  
|**enabled**|**bit**|Especifica si el servidor es un publicador alternativo. **1** especifica que el publicador está habilitado como publicador alternativo. **0** especifica que no está habilitado.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helpmergealternatepublisher** se utiliza en la replicación de mezcla.  
  
 Durante cada sesión de mezcla, el sistema consulta tanto al publicador como al suscriptor para obtener la lista de publicadores alternativos de cada uno. El proceso de mezcla agrega entradas a la lista de publicadores alternativos o las quita. El resultado es que las listas de publicadores alternativos del suscriptor y el publicador coinciden.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros de la lista de acceso a la publicación pueden ejecutar **sp_helpmergealternatepublisher**.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
