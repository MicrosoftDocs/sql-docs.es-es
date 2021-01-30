---
description: sysschemaarticles (Transact-SQL)
title: sysschemaarticles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysschemaarticles_TSQL
- sysschemaarticles
dev_langs:
- TSQL
helpviewer_keywords:
- sysschemaarticles system table
ms.assetid: 67a1c039-c283-4a9c-bacc-b9b3973590c3
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7a4dc32b20c4b17c683df1a1e1aea88ee8559be4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180365"
---
# <a name="sysschemaarticles-transact-sql"></a>sysschemaarticles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Realiza un seguimiento de los artículos de solo esquema para publicaciones transaccionales y de instantáneas. Esta tabla se almacena en la base de datos de publicación.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|IDENTIFICADOR del artículo.|  
|**creation_script**|**nvarchar(255)**|Ruta de acceso y nombre de un script del esquema del artículo que se utiliza para crear la tabla de destino.|  
|**description**|**nvarchar(255)**|La entrada descriptiva del artículo.|  
|**dest_object**|**sysname**|Nombre del objeto en la base de datos de suscripciones si el artículo es de solo esquema, como un procedimiento almacenado, una vista o una UDF.|  
|**name**|**sysname**|Nombre del artículo de solo esquema de una publicación.|  
|**objid**|**int**|Identificador de objeto del objeto base del artículo. Puede ser el identificador de objeto de un procedimiento, una vista, una vista indizada o una UDF.|  
|**pubid**|**int**|IDENTIFICADOR de la publicación.|  
|**pre_creation_cmd**|**tinyint**|Especifica lo que debería hacer el sistema si detecta un objeto existente con el mismo nombre en el suscriptor cuando se aplica la instantánea para este artículo:<br /><br /> **0** = nada.<br /><br /> **1** = eliminar tabla de destino.<br /><br /> **2** = quitar tabla de destino.<br /><br /> **3** = truncar la tabla de destino.|  
|**status**|**int**|Mapa de bits utilizado para indicar el estado del artículo.|  
|**type**|**tinyint**|Valor que indica el tipo de artículo de solo esquema:<br /><br /> **32** = procedimiento almacenado.<br /><br /> **64** = vista o vista indizada. <br /><br /> **96** = función de agregado.<br /><br /> **128** = función.|  
|**schema_option**|**Binary(8**|Máscara de bits de la opción de generación del esquema para el artículo dado. Especifica la creación automática del procedimiento almacenado en la base de datos de destino para todas las sintaxis CALL/MCALL/XCALL, y puede ser el resultado OR bit a bit lógico de uno o más de estos valores:<br /><br /> **0x00** = deshabilita el scripting por el agente de instantáneas y usa *creation_script*.<br /><br /> **0x01** = genera la creación de objetos (CREATE TABLE, CREATE PROCEDURE, etc.). Este valor es el predeterminado en los artículos de procedimientos almacenados.<br /><br /> **0x02** = genera procedimientos almacenados personalizados para el artículo, si se han definido.<br /><br /> **0x10** = genera el índice clúster correspondiente.<br /><br /> **0x20** = convierte los tipos de datos definidos por el usuario en tipos de datos base.<br /><br /> **0x40**= genera los índices no clúster correspondientes.<br /><br /> **0x80**= incluye la integridad referencial declarada en las claves principales.<br /><br /> **0x73** = genera la instrucción CREATE TABLE, crea índices agrupados y no agrupados, convierte los tipos de datos definidos por el usuario en tipos de datos base y genera scripts de procedimientos almacenados personalizados que se van a aplicar en el suscriptor. Este valor es el predeterminado en todos los artículos excepto en los de procedimientos almacenados.<br /><br /> **0x100**= replica los desencadenadores de usuario en un artículo de tabla, si se define.<br /><br /> **0x200**= Replica las restricciones Foreign Key. Si la tabla a la que se hace referencia no forma parte de una publicación, no se replicará ninguna restricción de clave externa de una tabla publicada.<br /><br /> **0x400**= Replica las restricciones check.<br /><br /> **0x800**= replica los valores predeterminados.<br /><br /> **0x1000**= Replica la intercalación de nivel de columna.<br /><br /> **0x2000**= Replica las propiedades extendidas asociadas con el objeto de origen del artículo publicado.<br /><br /> **0x4000**= Replica claves únicas si están definidas en un artículo de tabla.<br /><br /> **0x8000**= Replica la clave principal y las claves únicas de un artículo de tabla como restricciones mediante instrucciones ALTER TABLE.|  
|**dest_owner**|**sysname**|Propietario de la tabla de la base de datos de destino.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
