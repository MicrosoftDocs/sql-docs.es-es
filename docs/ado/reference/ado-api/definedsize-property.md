---
description: Propiedad DefinedSize
title: Propiedad DefinedSize | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field20::DefinedSize
helpviewer_keywords:
- DefinedSize property [ADO]
ms.assetid: 3ee27314-a305-4fbc-8433-9ee9a909afd6
author: rothja
ms.author: jroth
ms.openlocfilehash: 08019584f43891bb8e918b5e5f491c038a4b9afc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025605"
---
# <a name="definedsize-property"></a>Propiedad DefinedSize
Indica la capacidad de los datos de un objeto de [campo](../../../ado/reference/ado-api/field-object.md) .  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve un valor **Long** que refleja el tamaño definido de un campo, que depende del tipo de datos del objeto de campo. consulte [tipo](../../../ado/reference/ado-api/type-property-ado.md) para obtener más información. Para un campo que utiliza un tipo de datos de longitud fija, el valor devuelto es el tamaño del tipo de datos en bytes. Para un campo que utiliza un tipo de datos de longitud variable, es uno de los siguientes:  
  
1.  La longitud máxima del campo en caracteres (para **advarchar** y **adVarWChar**) o en bytes (para **adVarBinary** y **adVarNumeric**) si el campo tiene una longitud definida. Por ejemplo, el campo **advarchar (5)** tiene una longitud máxima de 5.  
  
2.  La longitud máxima del tipo de datos en caracteres (para **adChar** y **adWChar**) o en bytes (para **adBinary** y **adNumeric**) si el campo no tiene una longitud definida.  
  
3.  ~ 0 (bit a bit, el valor no es 0; todos los bits se establecen en 1) si ni el campo ni el tipo de datos tienen una longitud máxima definida.  
  
4.  En los tipos de datos que no tienen una longitud, se establece en ~ 0 (bit a bit, el valor no es 0; todos los bits se establecen en 1).  
  
## <a name="remarks"></a>Observaciones  
 Use la propiedad **DefinedSize** para determinar la capacidad de los datos de un objeto de **campo** .  
  
 Las propiedades **DefinedSize** y [ActualSize](../../../ado/reference/ado-api/actualsize-property-ado.md) son diferentes. Por ejemplo, considere un objeto de **campo** con un tipo declarado de **advarchar** y un valor de propiedad **DefinedSize** de 50, que contiene un solo carácter. El valor de la propiedad **ActualSize** que devuelve es la longitud en bytes del carácter único.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Field](../../../ado/reference/ado-api/field-object.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de propiedades ActualSize y DefinedSize (VB)](../../../ado/reference/ado-api/actualsize-and-definedsize-properties-example-vb.md)   
 [Ejemplo de propiedades ActualSize y DefinedSize (VC + +)](../../../ado/reference/ado-api/actualsize-and-definedsize-properties-example-vc.md)   
 [Propiedad ActualSize (ADO)](../../../ado/reference/ado-api/actualsize-property-ado.md)
