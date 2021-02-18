---
title: Server (DTA, elemento de Configuration)
description: En la utilidad DTA, el elemento Server de Configuration contiene la información de identificación del servidor en el que desea evaluar una configuración.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Server element
ms.assetid: da9ff870-9cfd-42fe-994b-7b9292681f7d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: b0aa1686a93cfac030785e7d7572eb2157d4d38f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354462"
---
# <a name="server-element-for-configuration-dta"></a>Server (DTA, elemento de Configuration)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Contiene la información de identificación del servidor en el que quiere que el Asistente para la optimización de motor de base de datos evalúe la configuración hipotética (especificada por el elemento **Configuration** ).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
<Configuration>  
    <Server>  
...code removed here...  
    </Server>  
```  
  
## <a name="element-characteristics"></a>Características de los elementos  
  
|Característica|Descripción|  
|--------------------|-----------------|  
|**Tipo y longitud de los datos**|Ninguno.|  
|**Valor predeterminado**|Ninguno.|  
|**Repetición**|Se requiere una vez por elemento **Configuration** .|  
  
## <a name="element-relationships"></a>Relaciones del elemento  
  
|Relación|Elementos|  
|------------------|--------------|  
|**Elemento primario**|[Elemento Configuration &#40;DTA&#41;](../../tools/dta/configuration-element-dta.md)|  
|**Elementos secundarios**|[Name &#40;DTA, elemento de Server&#41;](../../tools/dta/name-element-for-server-dta.md)<br /><br /> [Database &#40;DTA, elemento de Configuration&#41;](../../tools/dta/database-element-for-configuration-dta.md)|  
  
## <a name="remarks"></a>Observaciones  
 Solo es posible especificar un elemento **Server** para el elemento **Configuration** . Este elemento tiene el nombre **ServerTypecomplexType** en el [esquema XML del Asistente para la optimización de motor de base de datos](https://go.microsoft.com/fwlink/?linkid=43100). No confunda este elemento **Server** con el elemento secundario de **DTAInput** . Para obtener más información, vea [Server &#40;DTA, elemento&#41;](../../tools/dta/server-element-dta.md).  
  
## <a name="example"></a>Ejemplo  
 Para obtener un ejemplo de uso, vea [Ejemplo de archivo de entrada XML con configuración especificada por el usuario &#40;DTA&#41;](../../tools/dta/xml-input-file-sample-with-user-specified-configuration-dta.md).  
  
## <a name="see-also"></a>Consulte también  
 [Referencia del archivo de entrada XML &#40;Asistente para la optimización de motor de base de datos&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
