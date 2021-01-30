---
description: 'Vistas de catálogo de esquemas: sys. Schemas'
title: Sys. Schemas (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- schemas_TSQL
- sys.schemas_TSQL
- schemas
- sys.schemas
dev_langs:
- TSQL
helpviewer_keywords:
- sys.schemas catalog view
ms.assetid: 29af5ce5-2af7-4103-8f08-3ec92603ba05
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b5f245888ac8dc6d9b5c98f9f3558be4b0b8ef1e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200938"
---
# <a name="schemas-catalog-views---sysschemas"></a>Vistas de catálogo de esquemas: sys. Schemas
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Contiene una fila por cada esquema de la base de datos.  
  
> [!NOTE]  
>  Los esquemas de la base de datos son diferentes de los esquemas XML, que se utilizan para definir el modelo de contenido de los documentos XML.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre del esquema. Es único en la base de datos.|  
|**schema_id**|**int**|Id. del esquema. Es único en la base de datos.|  
|**principal_id**|**int**|Id. de la entidad de seguridad propietaria del esquema.|  
  
## <a name="remarks"></a>Observaciones  
Los esquemas de base de datos actúan como espacios de nombres o contenedores para objetos, como tablas, vistas, procedimientos y funciones, que se pueden encontrar en la vista de catálogo **Sys. Objects** .  

Cada esquema tiene un propietario. El propietario es una [entidad](../../relational-databases/security/authentication-access/principals-database-engine.md)de seguridad.
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** . Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
[Entidades de seguridad](../../relational-databases/security/authentication-access/principals-database-engine.md)

[Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   

[Vistas de catálogo de esquemas &#40;Transact-SQL&#41;](./catalog-views-transact-sql.md)   

[sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)  
  
