---
description: sysmergeschemaarticles (Transact-SQL)
title: sysmergeschemaarticles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysmergeschemaarticles_TSQL
- sysmergeschemaarticles
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergeschemaarticles system table
ms.assetid: b5085979-2f76-48e1-bf3b-765a84003dd9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 579acde4f82556f176e5923a66db48c1faafe27f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210193"
---
# <a name="sysmergeschemaarticles-transact-sql"></a>sysmergeschemaarticles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Realiza un seguimiento de los artículos solo de esquema de la replicación de mezcla. Esta tabla se almacena en las bases de datos de publicación y de suscripciones.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre del artículo solo de esquema de la publicación de combinación.|  
|**type**|**tinyint**|Indica el tipo de artículo solo de esquema, que puede ser uno de los siguientes:<br /><br /> **0x20** = artículo solo de esquema de procedimiento almacenado.<br /><br /> **0x40** = artículo de solo esquema de vista o vista indizada.|  
|**objid**|**int**|Identificador de objeto del objeto base del artículo. Puede ser el identificador de objeto de un procedimiento, una vista, una vista indizada o una función de usuario.|  
|**artid**|**uniqueidentifier**|IDENTIFICADOR del artículo.|  
|**description**|**nvarchar(255)**|Descripción del artículo.|  
|**pre_creation_command**|**tinyint**|Acción predeterminada que se debe realizar cuando se crea el artículo en la base de datos de suscripciones.<br /><br /> **0 =** Ninguno: Si la tabla ya existe en el suscriptor, no se realiza ninguna acción.<br /><br /> **1** = quita la tabla antes de volver a crearla.<br /><br /> **2** = delete: emite una eliminación basada en la cláusula WHERE del filtro de subconjunto.<br /><br /> **3** = TRUNCATE: igual que **2**, pero elimina páginas en lugar de filas. Sin embargo, no precisa la cláusula WHERE.|  
|**pubid**|**uniqueidentifier**|Identificador único de la publicación.|  
|**status**|**tinyint**|Indica el estado del artículo solo de esquema, que puede ser uno de los siguientes:<br /><br /> **1** = sin sincronizar: el script de procesamiento inicial para publicar la tabla se ejecuta la próxima vez que se ejecuta el agente de instantáneas.<br /><br /> **2** = activo: se ha ejecutado el script de procesamiento inicial para publicar la tabla.<br /><br /> **5** = New_inactive-que se va a agregar.<br /><br /> **6** = New_active-que se va a agregar.|  
|**creation_script**|**nvarchar(255)**|La ruta de acceso y el nombre de un script de creación previa opcional del esquema del artículo que se utiliza para crear la tabla de destino.|  
|**schema_option**|**Binary(8**|El mapa de bits de la opción de generación de esquemas del artículo solo de esquema específico, que puede ser el resultado de OR lógico bit a bit de uno o más de estos valores:<br /><br /> **0x00** = deshabilita el scripting por el agente de instantáneas y usa el CreationScript proporcionado.<br /><br /> **0x01** = genera la creación del objeto (CREATE TABLE, CREATE PROCEDURE, etc.).<br /><br /> **0x10** = generar un índice clúster correspondiente.<br /><br /> **0x20** = convierte los tipos de datos definidos por el usuario en tipos de datos base.<br /><br /> **0x40** = genera índices no clúster o índices correspondientes.<br /><br /> **0x80** = incluye la integridad referencial declarada en las claves principales.<br /><br /> **0x100** = replica los desencadenadores de usuario en un artículo de tabla, si se define.<br /><br /> **0x200** = Replica las restricciones de clave externa. Si la tabla a la que se hace referencia no forma parte de una publicación, no se replica ninguna restricción de clave externa de una tabla publicada.<br /><br /> **0x400** = Replica las restricciones check.<br /><br /> **0x800** = replicar valores predeterminados.<br /><br /> **0x1000** = Replica la intercalación de nivel de columna.<br /><br /> **0x2000** = Replica las propiedades extendidas asociadas con el objeto de origen del artículo publicado.<br /><br /> **0x4000** = Replica claves únicas si se define en un artículo de tabla.<br /><br /> **0x8000** = replica una clave principal y las claves únicas de un artículo de tabla como restricciones mediante instrucciones ALTER TABLE.<br /><br /> Para obtener más información sobre los valores posibles de **schema_option**, vea [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).|  
|**destination_object**|**sysname**|Nombre del objeto de destino en la base de datos de suscripciones. Este valor se aplica únicamente a los artículos solo de esquema, como procedimientos almacenados, vistas y UDF.|  
|**destination_owner**|**sysname**|Propietario del objeto en la base de datos de suscripciones, si no es **DBO**.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
