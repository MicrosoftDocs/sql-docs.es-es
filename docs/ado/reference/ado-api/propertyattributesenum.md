---
description: PropertyAttributesEnum
title: PropertyAttributesEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- PropertyAttributesEnum
helpviewer_keywords:
- PropertyAttributesEnum enumeration [ADO]
ms.assetid: 96a01955-a6b4-4cbf-9c73-52bcd1e9fb25
author: rothja
ms.author: jroth
ms.openlocfilehash: 694170521f4fc99d5dd9c99d15f48e7461acde09
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040865"
---
# <a name="propertyattributesenum"></a>PropertyAttributesEnum
Especifica los atributos de un objeto de [propiedad](./property-object-ado.md) .  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adPropNotSupported**|0|Indica que el proveedor no admite la propiedad.|  
|**adPropRequired**|1|Indica que el usuario debe especificar un valor para esta propiedad antes de inicializar el origen de datos.|  
|**adPropOptional**|2|Indica que el usuario no necesita especificar un valor para esta propiedad antes de inicializar el origen de datos.|  
|**adPropRead**|512|Indica que el usuario puede leer la propiedad.|  
|**adPropWrite**|1024|Indica que el usuario puede establecer la propiedad.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Paquete: **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums. PropertyAttributes. NOTSUPPORTED|  
|AdoEnums. PropertyAttributes. Required|  
|AdoEnums. PropertyAttributes. opcional|  
|AdoEnums. PropertyAttributes. READ|  
|AdoEnums. PropertyAttributes. WRITE|  
  
## <a name="applies-to"></a>Se aplica a  
 [Propiedad Attributes (ADO)](./attributes-property-ado.md)