---
description: sys.xml_schema_facets (Transact-SQL)
title: sys.xml_schema_facets (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.xml_schema_facets
- xml_schema_facets_TSQL
- sys.xml_schema_facets_TSQL
- xml_schema_facets
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_facets catalog view
ms.assetid: 4402dde9-1877-4872-8550-140dc2a177d2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: daa48ff28467b17f95d7caed304cf84244f50f7a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159571"
---
# <a name="sysxml_schema_facets-transact-sql"></a>sys.xml_schema_facets (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila por faceta (Restriction) de una definición de tipo XML (corresponde a **sys.xml_types**).  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**xml_component_id**|**int**|Id. del componente (tipo) XML al que pertenece este aspecto.|  
|**facet_id**|**int**|Id. (ordinal en base 1) del aspecto, único en el identificador del componente.|  
|**kind**|**char(2)**|Tipo de aspecto:<br /><br /> LG = Longitud<br /><br /> LN = Longitud mínima<br /><br /> LX = Longitud máxima<br /><br /> PT = Patrón (expresión normal)<br /><br /> EU = Enumeración<br /><br /> IN = Valor inclusivo mínimo<br /><br /> IX = Valor inclusivo máximo<br /><br /> EN = Valor exclusivo mínimo<br /><br /> EX = Valor exclusivo máximo<br /><br /> DT = Dígitos totales<br /><br /> DF = Dígitos de fracción<br /><br /> WS = Normalización de espacio en blanco|  
|**kind_desc**|**nvarchar (60)**|Descripción del tipo de aspecto:<br /><br /> LENGTH<br /><br /> MINIMUM_LENGTH<br /><br /> MAXIMUM_LENGTH<br /><br /> PATTERN<br /><br /> ENUMERATION<br /><br /> MINIMUM_INCLUSIVE_VALUE<br /><br /> MAXIMUM_INCLUSIVE_VALUE<br /><br /> MINIMUM_EXCLUSIVE_VALUE<br /><br /> MAXIMUM_EXCLUSIVE_VALUE<br /><br /> TOTAL_DIGITS<br /><br /> FRACTION_DIGITS<br /><br /> WHITESPACE_NORMALIZATION|  
|**is_fixed**|**bit**|1 = El aspecto tiene un valor fijo, especificado previamente.<br /><br /> 0 = No tiene un valor fijo. (predeterminado).|  
|**value**|**nvarchar (4000)**|Valor fijo, especificado previamente del aspecto.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Esquemas XML &#40;las vistas de catálogo del sistema de tipos XML&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)  
  
  
