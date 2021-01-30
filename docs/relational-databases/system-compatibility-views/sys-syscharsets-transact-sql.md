---
description: sys.syscharsets (Transact-SQL)
title: sys.syscharsets (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.syscharsets
- syscharsets
- sys.syscharsets_TSQL
- syscharsets_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syscharsets system table
- sys.syscharsets compatibility view
ms.assetid: f16d987c-bd19-4668-9ef7-785b8fb9ff5b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 171e0d788f14f7bf9a741c53988136e962a6cb2f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171836"
---
# <a name="syssyscharsets-transact-sql"></a>sys.syscharsets (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una fila para cada juego de caracteres y criterio de ordenación definido para su uso por el [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Uno de los criterios de ordenación está **marcado como el** criterio de ordenación predeterminado. Es el único que se utiliza en realidad.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**type**|**smallint**|Tipo de entidad que representa esta fila:<br /><br /> 1001 = juego de caracteres.<br /><br /> 2001 = Criterio de ordenación.|  
|**id**|**tinyint**|Id. único del orden o juego de caracteres. Observe que los órdenes y los juegos de caracteres no pueden compartir el mismo número de Id. El intervalo de identificadores entre 1 y 240 está reservado para uso del [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|**csid**|**tinyint**|Si la fila representa un juego de caracteres, este campo no se usa. Si la fila representa un orden, este campo es el Id. del juego de caracteres sobre el que se basa el orden. Se supone que existe en la tabla una fila con este Id. de juego de caracteres.|  
|**status**|**smallint**|Bits de información del estado interno del sistema.|  
|**name**|**sysname**|Nombre único para el orden o juego de caracteres. Este campo solo debe contener las letras A-Z o a-z, números 0 - 9 y caracteres de subrayado (_) y debe empezar por una letra.|  
|**description**|**nvarchar(255)**|Descripción opcional de las características del orden o juego de caracteres.|  
|**binarydefinition**|**varbinary (6000)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**definir**|**image**|Definición interna del orden o juego de caracteres. La estructura de los datos de este campo depende del tipo.|  
  
## <a name="see-also"></a>Consulte también  
 [Asignar tablas del sistema a vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
