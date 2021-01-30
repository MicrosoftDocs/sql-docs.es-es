---
description: sys.endpoint_webmethods (Transact-SQL)
title: sys.endpoint_webmethods (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.endpoint_webmethods_TSQL
- sys.endpoint_webmethods
- endpoint_webmethods_TSQL
- sys.http_soap_methods_TSQL
- endpoint_webmethods
- sys.http_soap_methods
dev_langs:
- TSQL
helpviewer_keywords:
- sys.endpoint_webmethods catalog view
ms.assetid: 7dad0cf6-eafa-47cf-98cc-75ba8d3c7959
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7d528284dd7a8cb3efcbbd84d66a38362c0141f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203913"
---
# <a name="sysendpoint_webmethods-transact-sql"></a>sys.endpoint_webmethods (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Contiene una fila para cada método SOAP definido en un extremo HTTP habilitado por SOAP. La combinación de las columnas endpoint_id y namespace es única.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|endpoint_id|**int**|Identificador del extremo en el que está definido WEBMETHOD.|  
|espacio de nombres|**nvarchar (384)**|Espacio de nombres para WEBMETHOD.|  
|method_alias|**nvarchar (64)**|Alias del método.<br /><br /> Nota: [!INCLUDE[tsql](../../includes/tsql-md.md)] los identificadores permiten caracteres que no son válidos en nombres de método WSDL.<br /><br /> El alias se utiliza para asignar el nombre expuesto en la descripción WSDL del extremo al objeto ejecutable de [!INCLUDE[tsql](../../includes/tsql-md.md)] subyacente real que se llama al invocar WEBMETHOD.|  
|object_name|**nvarchar(776**|Nombre de objeto al que se redirige WEBMETHOD, como se especifica en la opción NAME =. Las partes del nombre se separan con un punto (.) y se delimitan mediante corchetes, `[``]` .<br /><br /> El nombre de objeto debe constar de tres partes, como se especifica en la opción WSDL.|  
|result_schema|**tinyint**|Opción que determina si, en su caso, XSD se devuelve con una respuesta.<br /><br /> 0 = Ninguna<br /><br /> 1 = Estándar<br /><br /> 2 = Predeterminado|  
|result_schema_desc|**nvarchar(60)**|Descripción de opción que determina si, en su caso, XSD se devuelve con una respuesta.<br /><br /> Ninguno<br /><br /> STANDARD<br /><br /> DEFAULT|  
|result_format|**tinyint**|Opción que determina el formato que reciben los resultados en la respuesta.<br /><br /> 1 = ALL_RESULTS<br /><br /> 2 = ROWSETS_ONLY<br /><br /> 3 = NONE|  
|result_format_desc|**nvarchar(60)**|Descripción de la opción que determina el formato que reciben los resultados en la respuesta.<br /><br /> ALL_RESULTS<br /><br /> ROWSETS_ONLY<br /><br /> Ninguno|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Endpoints Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)  [Vistas de catálogo de extremos &#40;Transact-SQL&#41;]  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
