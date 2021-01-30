---
description: sys.server_assembly_modules (Transact-SQL)
title: sys.server_assembly_modules (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- server_assembly_modules_TSQL
- sys.server_assembly_modules
- server_assembly_modules
- sys.server_assembly_modules_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_assembly_modules catalog view
ms.assetid: af799e38-2d16-49b2-bcf5-6f9199af899e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: dead9bfa36010e7ffc2835595bcfa4710d582206
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182443"
---
# <a name="sysserver_assembly_modules-transact-sql"></a>sys.server_assembly_modules (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada módulo de ensamblado de los desencadenadores de nivel de servidor de tipo TA. Esta vista asigna los desencadenadores de ensamblado a la implementación CLR subyacente. Puede combinar esta relación con **Sys.server_triggers**. El ensamblado debe cargarse en la base de datos **maestra** . La tupla (object_id) es la clave de la relación.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Se trata de una referencia FOREIGN KEY al objeto en el que se define este módulo de ensamblado.|  
|**assembly_id**|**int**|Id. del ensamblado desde el que se creó este módulo. El ensamblado debe estar cargado en la base de datos maestra.|  
|**assembly_class**|**sysname**|Nombre de la clase del ensamblado que define este módulo.|  
|**assembly_method**|**sysname**|Nombre del método de la clase que define este módulo. Es NULL para las funciones de agregado (AF).|  
|**execute_as_principal_id**|**int**|Id. de la entidad de seguridad de servidor EXECUTE AS.<br /><br /> NULL de manera predeterminada o si EXECUTE AS CALLER.<br /><br /> IDENTIFICADOR de la entidad de seguridad especificada si EXECUTe AS SELF EXECUTe AS \<principal> .<br /><br /> -2 = EXECUTE AS OWNER.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md) (Vistas de catálogo de objetos [Transact-SQL])  
  
  
