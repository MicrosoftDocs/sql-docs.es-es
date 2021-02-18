---
title: Database (DTA, elemento de Configuration)
description: En la utilidad DTA, el elemento Database de Configuration especifica la base de datos en la que se quiere evaluar una configuración.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Database element
ms.assetid: e91ba243-6cc9-457a-8f5a-134f3c71ae69
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 898623788bbd02e4d1cb7271f65abd178b714121
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342035"
---
# <a name="database-element-for-configuration-dta"></a>Database (DTA, elemento de Configuration)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Especifica la base de datos en la que quiere que el Asistente para la optimización de motor de base de datos evalúe la configuración hipotética (especificada por el elemento **Configuration** ).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
<Server>  
...code removed here...  
    <Database>...</Database>  
```  
  
## <a name="element-characteristics"></a>Características de los elementos  
  
|Característica|Descripción|  
|--------------------|-----------------|  
|**Tipo y longitud de los datos**|Ninguno.|  
|**Valor predeterminado**|Ninguno.|  
|**Repetición**|Obligatoria una o más veces por elemento **Server** .|  
  
## <a name="element-relationships"></a>Relaciones del elemento  
  
|Relación|Elementos|  
|------------------|--------------|  
|**Elemento primario**|[Server &#40;DTA, elemento de Configuration&#41;](../../tools/dta/server-element-for-configuration-dta.md)|  
|**Elementos secundarios**|[Name &#40;DTA, elemento de Database&#41;](../../tools/dta/name-element-for-database-dta.md)<br /><br /> [Schema &#40;DTA, elemento de Database&#41;](../../tools/dta/schema-element-for-database-dta.md)<br /><br /> [Elemento Recommendation &#40;DTA&#41;](../../tools/dta/recommendation-element-dta.md)|  
  
## <a name="remarks"></a>Observaciones  
 Este elemento tiene el nombre **DatabaseTypecomplexType** en el esquema XML del Asistente para la optimización de motor de base de datos. No confunda este elemento **Database** con el que tiene al elemento **Server** como raíz primaria, que se encuentra en la parte superior del archivo de entrada XML. Para obtener más información, vea [Elemento Database para servidor &#40;DTA&#41;](../../tools/dta/database-element-for-server-dta.md).  
  
## <a name="example"></a>Ejemplo  
 Para obtener un ejemplo de uso de este elemento **Database**, vea [Ejemplo de archivo de entrada XML con configuración especificada por el usuario &#40;DTA&#41;](../../tools/dta/xml-input-file-sample-with-user-specified-configuration-dta.md).  
  
## <a name="see-also"></a>Consulte también  
 [Referencia del archivo de entrada XML &#40;Asistente para la optimización de motor de base de datos&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
