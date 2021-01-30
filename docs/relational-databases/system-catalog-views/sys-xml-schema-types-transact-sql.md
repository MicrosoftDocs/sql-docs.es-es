---
description: sys.xml_schema_types (Transact-SQL)
title: sys.xml_schema_types (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.xml_schema_types_TSQL
- xml_schema_types_TSQL
- sys.xml_schema_types
- xml_schema_types
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_types catalog view
ms.assetid: 441ba49d-f778-4fa1-98c4-ced375a01a34
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c55064092cfee2940e358e7532cc8dcc62caf24e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199817"
---
# <a name="sysxml_schema_types-transact-sql"></a>sys.xml_schema_types (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila por componente de esquema XML que es un tipo **symbol_space** de **T**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**||Hereda columnas de [sys.xml_schema_components](../../relational-databases/system-catalog-views/sys-xml-schema-components-transact-sql.md).|  
|**is_abstract**|**bit**|1 = El tipo es un tipo abstracto. Todas las instancias de un elemento de este tipo deben usar **xsi: Type** para indicar un tipo derivado que no sea abstracto.<br /><br /> 0 = El tipo no es abstracto. (predeterminado).|  
|**allows_mixed_content**|**bit**|1 = Se permite contenido mixto<br /><br /> 0 = No se permite contenido mixto. (predeterminado).|  
|**is_extension_blocked**|**bit**|1 = el reemplazo con una extensión del tipo está bloqueado en instancias cuando el atributo block de la definición de **complexType** o el atributo **BlockDefault** del elemento de información del \<schema> elemento antecesor se establece en "Extension" o "#all".<br /><br /> 0 = El reemplazo con una extensión no está bloqueado.|  
|**is_restriction_blocked**|**bit**|1 = el reemplazo con una restricción del tipo está bloqueado en instancias cuando el atributo block de la definición de **complexType** o el atributo **BlockDefault** del elemento de información del \<schema> elemento antecesor se establece en "Restriction" o "#all".<br /><br /> 0 = El reemplazo con una restricción no está bloqueado. (predeterminado).|  
|**is_final_extension**|**bit**|1 = la derivación por extensión del tipo se bloquea cuando el atributo final de la definición de **complexType** o el atributo **finalDefault** del \<schema> elemento de información del elemento antecesor se establece en "Extension" o "#all".<br /><br /> 0 = Se permite la extensión. (predeterminado).|  
|**is_final_restriction**|**bit**|1 = la derivación por restricción del tipo se bloquea cuando el atributo final de la definición simple o **complexType** o el atributo **finalDefault** del elemento de información del \<schema> elemento antecesor se establece en "Restriction" o "#all".<br /><br /> 0 = Se permite la restricción. (predeterminado).|  
|**is_final_list_member**|**bit**|1 = Este tipo simple no se puede utilizar como tipo de elemento en una lista.<br /><br /> 0 = Este tipo es un tipo complejo o se puede utilizar como un tipo de elemento de lista. (predeterminado).|  
|**is_final_union_member**|**bit**|1 = Este tipo simple no se puede utilizar como tipo de miembro de un tipo de unión.<br /><br /> 0 = Este tipo es un tipo complejo. o se puede utilizar como tipo de miembro de unión. (predeterminado).|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Esquemas XML &#40;las vistas de catálogo del sistema de tipos XML&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)  
  
  
