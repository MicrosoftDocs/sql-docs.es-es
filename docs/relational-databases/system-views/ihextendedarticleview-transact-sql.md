---
description: IHextendedArticleView (Transact-SQL)
title: IHextendedArticleView (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- IHextendedArticleView_TSQL
- IHextendedArticleView
dev_langs:
- TSQL
helpviewer_keywords:
- IHextendedArticleView view
ms.assetid: 19ef0a12-3214-4bb0-9c25-a665897e65a2
author: stevestein
ms.author: sstein
ms.openlocfilehash: c91f65a94e0eb2699b4146d1bafdeebc0c4201d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186206"
---
# <a name="ihextendedarticleview-transact-sql"></a>IHextendedArticleView (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vista **IHextendedArticleView** expone información sobre los artículos en una publicación que no es de SQL Server. Esta vista se almacena en la base de datos de **distribución** .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|Identificador único del publicador.|  
|**publication_id**|**int**|Identificador único de la publicación.|  
|**artículo**|**sysname**|Nombre del artículo.|  
|**destination_object**|**sysname**|Nombre del objeto publicado en el suscriptor.|  
|**source_owner**|**sysname**|Propietario del objeto publicado en el publicador.|  
|**source_object**|**sysname**|Nombre del objeto publicado en el publicador.|  
|**description**|**nvarchar(255)**|Descripción del artículo.|  
|**creation_script**|**nvarchar(255)**|Script de creación de esquemas para el artículo.|  
|**del_cmd**|**nvarchar(255)**|Comando ejecutado para DELETE.|  
|**filter**|**int**|Identificador para el procedimiento almacenado utilizado para definir la partición horizontal.|  
|**filter_clause**|**ntext**|Cláusula WHERE utilizada para filtrar horizontalmente el artículo.|  
|**ins_cmd**|**nvarchar(255)**|Comando ejecutado para INSERT.|  
|**pre_creation_cmd**|**tinyint**|Comando de creación previa para DROP TABLE, DELETE TABLE o TRUNCATE:<br /><br /> **0** = ninguno.<br /><br /> **1** = quitar.<br /><br /> **2** = eliminar.<br /><br /> **3** = truncar.|  
|**status**|**tinyint**|Máscara de bits para las opciones y estado del artículo; puede ser el resultado OR lógico bit a bit de uno o más de estos valores:<br /><br /> **1** = el artículo está activo.<br /><br /> **8** = incluir el nombre de la columna en las instrucciones INSERT.<br /><br /> **16** = usar instrucciones con parámetros.<br /><br /> **24** = ambos incluyen el nombre de la columna en las instrucciones INSERT y usan instrucciones con parámetros.<br /><br /> Por ejemplo, un artículo activo que usa instrucciones con parámetros tendría un valor de **17** en esta columna. Un valor de **0** significa que el artículo está inactivo y no se han definido propiedades adicionales.|  
|**type**|**tinyint**|Tipo de artículo.<br /><br /> **1** = artículo basado en registro.<br /><br /> **3** = artículo basado en registro con filtro manual.<br /><br /> **5** = artículo basado en registro con vista manual.<br /><br /> **7** = artículo basado en registro con filtro manual y vista manual.|  
|**upd_cmd**|**nvarchar(255)**|Comando ejecutado para UPDATE.|  
|**schema_option**|**binary**|Indica qué se va a incluir en el script. Vea [sp_addarticle &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) para obtener una lista de las opciones de esquema admitidas.|  
|**dest_owner**|**sysname**|Propietario del objeto publicado en la base de datos de destino.|  
  
## <a name="see-also"></a>Vea también  
 [Replicación de bases de datos heterogéneas](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
