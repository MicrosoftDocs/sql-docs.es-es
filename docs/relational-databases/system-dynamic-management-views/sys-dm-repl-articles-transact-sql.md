---
description: sys.dm_repl_articles (Transact-SQL)
title: sys.dm_repl_articles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_repl_articles_TSQL
- dm_repl_articles
- dm_repl_articles_TSQL
- sys.dm_repl_articles
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_repl_articles dynamic management function
ms.assetid: 794d514e-bacd-432e-a8ec-3a063a97a37b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 807f8e1a8cf8b7400d471af1d1151e9652797105
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99138105"
---
# <a name="sysdm_repl_articles-transact-sql"></a>sys.dm_repl_articles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información acerca de los objetos de base de datos publicados como artículos en la tecnología de replicación.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**artcache_db_address**|**varbinary(8**|Dirección de memoria de la estructura de la base de datos en caché de la base de datos de publicaciones.|  
|**artcache_table_address**|**varbinary(8**|Dirección de memoria de la estructura de la tabla en caché de un artículo de la tabla publicada.|  
|**artcache_schema_address**|**varbinary(8**|Dirección de memoria de la estructura del esquema del artículo en caché de un artículo de la tabla publicada.|  
|**artcache_article_address**|**varbinary(8**|Dirección de memoria de la estructura del artículo en caché de un artículo de la tabla publicada.|  
|**artid**|**bigint**|Identifica de forma única cada entrada en esta tabla.|  
|**artfilter**|**bigint**|Id. del procedimiento almacenado usado para filtrar horizontalmente el artículo.|  
|**artobjid**|**bigint**|Id. del objeto publicado.|  
|**artpubid**|**bigint**|IDENTIFICADOR de la publicación a la que pertenece el artículo.|  
|**artstatus**|**tinyint**|Máscara de bits del estado y las opciones del artículo, que puede ser un resultado OR bit a bit lógico de uno o varios de estos valores:<br /><br /> **1** = el artículo está activo.<br /><br /> **8** = incluir el nombre de la columna en las instrucciones INSERT.<br /><br /> **16** = usar instrucciones con parámetros.<br /><br /> **24** = ambos incluyen el nombre de la columna en las instrucciones INSERT y usan instrucciones con parámetros.<br /><br /> Por ejemplo, un artículo activo que utilice instrucciones con parámetros tendrá un valor de 17 en esta columna. Un valor de 0 significa que el artículo no está activo y no tiene otras propiedades definidas.|  
|**arttype**|**tinyint**|Tipo de artículo.<br /><br /> **1** = artículo basado en registro.<br /><br /> **3** = artículo basado en registro con filtro manual.<br /><br /> **5** = artículo basado en registro con vista manual.<br /><br /> **7** = artículo basado en registro con filtro manual y vista manual.<br /><br /> **8** = ejecución de procedimiento almacenado.<br /><br /> **24** = ejecución de procedimiento almacenado serializable.<br /><br /> **32** = procedimiento almacenado (solo esquema).<br /><br /> **64** = vista (solo esquema).<br /><br /> **128** = función (solo esquema).|  
|**wszArtdesttable**|**nvarchar (514)**|Nombre del objeto publicado en el destino.|  
|**wszArtdesttableowner**|**nvarchar (514)**|Propietario del objeto publicado en el destino.|  
|**wszArtinscmd**|**nvarchar (510)**|Comando o procedimiento almacenado usado para inserciones.|  
|**cmdTypeIns**|**int**|Sintaxis de la llamada para el procedimiento almacenado para inserciones. Puede ser uno de estos valores.<br /><br /> **1** = llamada a<br /><br /> **2** = SQL<br /><br /> **3** = ninguno<br /><br /> **7** = desconocido|  
|**wszArtdelcmd**|**nvarchar (510)**|Comando o procedimiento almacenado usado para eliminaciones.|  
|**cmdTypeDel**|**int**|Sintaxis de la llamada para el procedimiento almacenado para eliminaciones. Puede ser uno de estos valores.<br /><br /> **0** = XCALL<br /><br /> **1** = llamada a<br /><br /> **2** = SQL<br /><br /> **3** = ninguno<br /><br /> **7** = desconocido|  
|**wszArtupdcmd**|**nvarchar (510)**|Comando o procedimiento almacenado usado para actualizaciones.|  
|**cmdTypeUpd**|**int**|Sintaxis de la llamada para el procedimiento almacenado para actualizaciones. Puede ser uno de estos valores.<br /><br /> **0** = XCALL<br /><br /> **1** = llamada a<br /><br /> **2** = SQL<br /><br /> **3** = ninguno<br /><br /> **4** = MCALL<br /><br /> **5** = VCALL<br /><br /> **6** = escalal<br /><br /> **7** = desconocido|  
|**wszArtpartialupdcmd**|**nvarchar (510)**|Comando o procedimiento almacenado usado para actualizaciones parciales.|  
|**cmdTypePartialUpd**|**int**|Sintaxis de la llamada para el procedimiento almacenado para actualizaciones parciales. Puede ser uno de estos valores.<br /><br /> **2** = SQL|  
|**numcol**|**int**|Número de columnas en la partición para un artículo filtrado verticalmente.|  
|**artcmdtype**|**tinyint**|Tipo de comando que se está replicando actualmente. Puede ser uno de los siguientes valores.<br /><br /> **1** = insertar<br /><br /> **2** = eliminar<br /><br /> **3** = actualización<br /><br /> **4** = UPDATETEXT<br /><br /> **5** = ninguno<br /><br /> **6** = solo uso interno<br /><br /> **7** = solo uso interno<br /><br /> **8** = actualización parcial|  
|**artgeninscmd**|**nvarchar (510)**|Plantilla del comando INSERT basada en las columnas incluidas en el artículo.|  
|**artgendelcmd**|**nvarchar (510)**|Plantilla del comando DELETE, que puede incluir la clave principal o las columnas incluidas en el artículo, dependiendo de la sintaxis de llamada usada.|  
|**artgenupdcmd**|**nvarchar (510)**|Plantilla del comando UPDATE, que puede incluir la clave principal, las columnas actualizadas o una lista completa de columnas, dependiendo de la sintaxis de llamada usada.|  
|**artpartialupdcmd**|**nvarchar (510)**|Plantilla del comando UPDATE parcial, que incluye la clave principal y las columnas actualizadas.|  
|**artupdtxtcmd**|**nvarchar (510)**|Plantilla del comando UPDATETEXT, que incluye la clave principal y las columnas actualizadas.|  
|**artgenins2cmd**|**nvarchar (510)**|Plantilla del comando INSERT usada al reconciliar un artículo durante el procesamiento simultáneo de instantáneas.|  
|**artgendel2cmd**|**nvarchar (510)**|Plantilla del comando DELETE usada al reconciliar un artículo durante el procesamiento simultáneo de instantáneas.|  
|**fInReconcile**|**tinyint**|Indica si un artículo se ha reconciliado durante el procesamiento simultáneo de instantáneas.|  
|**fPubAllowUpdate**|**tinyint**|Indica si la publicación admite suscripciones de actualización.|  
|**intPublicationOptions**|**bigint**|Mapa de bits que especifica las opciones de publicación adicionales, donde los valores de la opción bit a bit son:<br /><br /> **0x1** : habilitada para la replicación punto a punto.<br /><br /> **0X2** -publicar solo cambios locales.<br /><br /> **0x4** : habilitado para suscriptores que no son de SQL Server.|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW DATABASE STATE en la base de datos de publicaciones para llamar a **dm_repl_articles**.  
  
## <a name="remarks"></a>Observaciones  
 La instancia solo se devuelve para objetos de la base de datos replicada que está cargada actualmente en la caché del artículo de replicación.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vistas de administración dinámica relacionadas con la replicación &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/replication-related-dynamic-management-views-transact-sql.md)  
  
  

