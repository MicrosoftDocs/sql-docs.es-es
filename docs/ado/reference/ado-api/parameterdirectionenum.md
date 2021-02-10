---
description: ParameterDirectionEnum
title: ParameterDirectionEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ParameterDirectionEnum
helpviewer_keywords:
- ParameterDirectionEnum enumeration [ADO]
ms.assetid: c66aa6e6-d4f0-4f0f-9640-e08ae6cfdef3
author: rothja
ms.author: jroth
ms.openlocfilehash: 7b352c5504571d50e8840e0d05e0d906a07d4b36
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041125"
---
# <a name="parameterdirectionenum"></a>ParameterDirectionEnum
Especifica si el [parámetro](./parameter-object.md) representa un parámetro de entrada, un parámetro de salida, una entrada y un parámetro de salida, o el valor devuelto de un procedimiento almacenado.  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adParamInput**|1|Predeterminada. Indica que el parámetro representa un parámetro de entrada.|  
|**adParamInputOutput**|3|Indica que el parámetro representa un parámetro de entrada y de salida.|  
|**adParamOutput**|2|Indica que el parámetro representa un parámetro de salida.|  
|**adParamReturnValue**|4|Indica que el parámetro representa un valor devuelto.|  
|**adParamUnknown**|0|Indica que la dirección del parámetro es desconocida.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Paquete: **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums. ParameterDirection. INPUT|  
|AdoEnums. ParameterDirection. INPUTOUTPUT|  
|AdoEnums. ParameterDirection. OUTPUT|  
|AdoEnums. ParameterDirection. RETURNVALUE|  
|AdoEnums. ParameterDirection. UNKNOWN|  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Método CreateParameter (ADO)](./createparameter-method-ado.md)  
    :::column-end:::
    :::column:::
        [Propiedad Direction](./direction-property.md)  
    :::column-end:::
:::row-end:::