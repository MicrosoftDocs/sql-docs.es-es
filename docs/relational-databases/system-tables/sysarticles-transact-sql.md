---
description: sysarticles (Transact-SQL)
title: sysarticles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysarticles
- sysarticles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysarticles system table
ms.assetid: 9d9d5d51-6d8f-4e42-84a9-82e58eb0301e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1acd04a1a32e494b10e65a53ac0bdd57dacf27b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178043"
---
# <a name="sysarticles-transact-sql"></a>sysarticles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada artículo definido en la base de datos local. Esta tabla se almacena en la base de datos publicada.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|La columna de identidad que proporciona un número de identificación único para el artículo.|  
|**creation_script**|**nvarchar(255)**|El script de esquema del artículo.|  
|**del_cmd**|**nvarchar(255)**|El tipo de comando de replicación utilizado al replicar eliminaciones con artículos de la tabla. Para más información, vea [Especificar cómo se propagan los cambios para los artículos transaccionales](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**description**|**nvarchar(255)**|La entrada descriptiva del artículo.|  
|**dest_table**|**sysname**|Nombre de la tabla de destino.|  
|**filter**|**int**|El Id. de procedimiento almacenado, utilizado para la partición horizontal.|  
|**filter_clause**|**ntext**|La cláusula WHERE del artículo, utilizada para el filtrado horizontal.|  
|**ins_cmd**|**nvarchar(255)**|El tipo de comando de replicación utilizado al replicar inserciones con artículos de la tabla. Para más información, vea [Especificar cómo se propagan los cambios para los artículos transaccionales](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**name**|**sysname**|El nombre asociado al artículo, único en la publicación.|  
|**objid**|**int**|El Id. de objeto de la tabla publicada.|  
|**pubid**|**int**|Id. de la publicación a la que pertenece el artículo.|  
|**pre_creation_cmd**|**tinyint**|El comando anterior a la creación para DROP TABLE, DELETE TABLE o TRUNCATE:<br /><br /> **0** = ninguno.<br /><br /> **1** = quitar.<br /><br /> **2** = eliminar.<br /><br /> **3** = truncar.|  
|**status**|**tinyint**|Máscara de bits para las opciones y estado del artículo; puede ser el resultado OR lógico bit a bit de uno o más de estos valores:<br /><br /> **1** = el artículo está activo.<br /><br /> **8** = incluir el nombre de la columna en las instrucciones INSERT.<br /><br /> **16** = usar instrucciones con parámetros.<br /><br /> **24** = ambos incluyen el nombre de la columna en las instrucciones INSERT y usan instrucciones con parámetros.<br /><br /> **64** = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> Por ejemplo, un artículo activo que usa instrucciones con parámetros tendría un valor de **17** en esta columna. Un valor de **0** significa que el artículo está inactivo y no se han definido propiedades adicionales.|  
|**sync_objid**|**int**|El Id. de la tabla o vista que representa la definición del artículo.|  
|**type**|**tinyint**|Tipo de artículo:<br /><br /> **1** = artículo basado en registro.<br /><br /> **3** = artículo basado en registro con filtro manual.<br /><br /> **5** = artículo basado en registro con vista manual.<br /><br /> **7** = artículo basado en registro con filtro manual y vista manual.<br /><br /> **8** = ejecución de procedimiento almacenado.<br /><br /> **24** = ejecución de procedimiento almacenado serializable.<br /><br /> **32** = procedimiento almacenado (solo esquema).<br /><br /> **64** = vista (solo esquema).<br /><br /> **128** = función (solo esquema).|  
|**upd_cmd**|**nvarchar(255)**|El tipo de comando de replicación utilizado al replicar actualizaciones con artículos de la tabla. Para más información, vea [Especificar cómo se propagan los cambios para los artículos transaccionales](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**schema_option**|**Binary(8**|Máscara de bits de las opciones de generación de esquema para el artículo, que controla qué partes del esquema del artículo se incluirán en un script para la entrega al suscriptor. Para obtener más información sobre las opciones de esquema, vea [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).|  
|**dest_owner**|**sysname**|Propietario de la tabla de la base de datos de destino.|  
|**ins_scripting_proc**|**int**|El procedimiento almacenado o el script registrados personalizados que se ejecutan cuando se replica una instrucción INSERT.|  
|**del_scripting_proc**|**int**|El procedimiento almacenado o el script registrados personalizados que se ejecutan cuando se replica una instrucción DELETE.|  
|**upd_scripting_proc**|**int**|El procedimiento almacenado o el script registrados personalizados que se ejecutan cuando se replica una instrucción UPDATE.|  
|**custom_script**|**nvarchar(2048)**|El procedimiento almacenado o el script registrados personalizados que se ejecutan al final del desencadenador DDL.|  
|**fire_triggers_on_snapshot**|**bit**|Indica si se ejecutan desencadenadores replicados al aplicar la instantánea; puede ser uno de estos valores:<br /><br /> **0** = no se ejecutan los desencadenadores.<br /><br /> **1** = se ejecutan los desencadenadores.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addarticle &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)  
  
  
