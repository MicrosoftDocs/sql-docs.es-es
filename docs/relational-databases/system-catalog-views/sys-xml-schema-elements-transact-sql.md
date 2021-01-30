---
description: sys.xml_schema_elements (Transact-SQL)
title: sys.xml_schema_elements (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.xml_schema_elements
- sys.xml_schema_elements_TSQL
- xml_schema_elements
- xml_schema_elements_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_elements catalog view
ms.assetid: 190ed0cd-0c5e-4607-9db4-9e77cacf17d7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e8b8e424b022ea8d05532eb6046c5780c26ef1de
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211983"
---
# <a name="sysxml_schema_elements-transact-sql"></a>sys.xml_schema_elements (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila por componente de esquema XML que es un tipo **symbol_space** de **E**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|**--**|Hereda columnas de [sys.xml_schema_components](../../relational-databases/system-catalog-views/sys-xml-schema-components-transact-sql.md).|  
|**is_default_fixed**|**bit**|1 = El valor predeterminado es un valor fijo. Este valor no se puede reemplazar en la instancia de XML.<br /><br /> 0 = El valor predeterminado no es un valor fijo para el elemento. (valor predeterminado).|  
|**is_abstract**|**bit**|1 = El elemento es abstracto y no se puede usar en un documento de instancia. En el documento de instancia debe aparecer un miembro del grupo de sustitución del elemento.<br /><br /> 0 = El elemento no es abstracto. (valor predeterminado).|  
|**is_nillable**|**bit**|1 = El elemento tiene el atributo nillable.<br /><br /> 0 = El elemento no tiene el atributo nillable. (predeterminado).|  
|**must_be_qualified**|**bit**|1 = El elemento debe ser explícitamente un espacio de nombres calificado.<br /><br /> 0 = El elemento puede ser implícitamente un espacio de nombres calificado. (predeterminado).|  
|**is_extension_blocked**|**bit**|1 = El reemplazo con una instancia de un tipo de extensión está bloqueado.<br /><br /> 0 = Se permite el reemplazo con un tipo de extensión. (predeterminado).|  
|**is_restriction_blocked**|**bit**|1 = El reemplazo con una instancia de un tipo de restricción está bloqueado.<br /><br /> 0 = Se permite el reemplazo con un tipo de restricción. (predeterminado).|  
|**is_substitution_blocked**|**bit**|1 = No se puede usar una instancia de un grupo de sustitución.<br /><br /> 0 = Se permite el reemplazo con un grupo de sustitución. (predeterminado).|  
|**is_final_extension**|**bit**|1 = El reemplazo con una instancia de un tipo de extensión no se permite.<br /><br /> 0 = Se permite el reemplazo en una instancia de un tipo de extensión. (predeterminado).|  
|**is_final_restriction**|**bit**|1 = El reemplazo con una instancia de un tipo de restricción no se permite.<br /><br /> 0 = Se permite el reemplazo en una instancia de un tipo de restricción. (predeterminado).|  
|**default_value**|**nvarchar (4000)**|Valor predeterminado del elemento. NULL, si no se proporciona un valor predeterminado.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Esquemas XML &#40;las vistas de catálogo del sistema de tipos XML&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)  
  
  
