---
description: sp_showpendingchanges (Transact-SQL)
title: sp_showpendingchanges (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_showpendingchanges
- sp_showpendingchanges_TSQL
helpviewer_keywords:
- sp_showpendingchanges
ms.assetid: 8013a792-639d-4550-b262-e65d30f9d291
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ac2c44e836a56c6fa503b6144e99761294ab473a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207308"
---
# <a name="sp_showpendingchanges-transact-sql"></a>sp_showpendingchanges (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve un conjunto de resultados que muestra los cambios que esperan para replicarse. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicaciones y en el suscriptor de la base de datos de suscripciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
> [!NOTE]  
>  Este procedimiento proporciona una aproximación del número de cambios y las filas que están implicadas en ellos. Por ejemplo, el procedimiento recupera información del Editor o del Suscriptor, pero no de ambos al mismo tiempo. La información que está almacenada en el otro nodo podría producir un conjunto menor de cambios para sincronizar de lo que el procedimiento calcula.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_showpendingchanges [ [ @destination_server = ] 'destination_server' ]  
    [ , [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article']  
    [ , [ @show_rows = ] show_rows]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @destination_server = ] 'destination_server'` Es el nombre del servidor donde se aplican los cambios replicados. *destination_server* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname y su** valor predeterminado es NULL. Cuando se especifica la *publicación* , los resultados solo se limitan a la publicación especificada.  
  
`[ @article = ] 'article'` Es el nombre del artículo. *article* es de **tipo sysname y su** valor predeterminado es NULL. Cuando se especifica *article* , los resultados solo se limitan al artículo especificado.  
  
`[ @show_rows = ] 'show_rows'` Especifica si el conjunto de resultados contiene información más específica sobre los cambios pendientes, con un valor predeterminado de **0**. Si se especifica el valor **1** , el conjunto de resultados contiene las columnas is_delete y ROWGUID.  
  
## <a name="result-set"></a>Tipo de cursor  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|destination_server|**sysname**|El nombre del servidor en el que se replican los cambios.|  
|pub_name|**sysname**|Nombre de la publicación.|  
|destination_db_name|**sysname**|El nombre de la base de datos en la que se replican los cambios.|  
|is_dest_subscriber|**bit**|Indica que los cambios se replican en un suscriptor. Un valor de **1** indica que los cambios se replican en un suscriptor. **0** significa que los cambios se replican en un publicador.|  
|article_name|**sysname**|Nombre del artículo de la tabla en la que se originaron los cambios.|  
|pending_deletes|**int**|Número de eliminaciones a la espera de ser replicadas.|  
|pending_ins_and_upd|**int**|Número de inserciones y actualizaciones a la espera de ser replicadas.|  
|is_delete|**bit**|Indica si el cambio pendiente es una eliminación. Un valor de **1** indica que el cambio es una eliminación. Requiere un valor de **1** para @show_rows .|  
|rowguid|**uniqueidentifier**|GUID que identifica la fila que cambió. Requiere un valor de **1** para @show_rows .|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 sp_showpendingchanges se utiliza en la replicación de mezcla.  
  
 sp_showpendingchanges se utiliza durante la solución de problemas de la replicación de mezcla.  
  
 El resultado de sp_showpendingchanges no incluye las filas de la generación 0.  
  
 Cuando un artículo especificado para el *artículo* no pertenece a la publicación especificada para su *publicación,* se devuelve un recuento de 0 para pending_deletes y pending_ins_and_upd.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor sysadmin o del rol fijo de base de datos db_owner pueden ejecutar sp_showpendingchanges.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
