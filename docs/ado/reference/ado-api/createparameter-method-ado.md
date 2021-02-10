---
description: Método CreateParameter (ADO)
title: CreateParameter (método) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::raw_CreateParameter
- Command15::CreateParameter
helpviewer_keywords:
- CreateParameter method [RDS]
ms.assetid: 9666fdcc-0544-4ed7-a97b-c415f2a56d7e
author: rothja
ms.author: jroth
ms.openlocfilehash: c5156f48f17d2d389646f4f752033cf88faf83f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025969"
---
# <a name="createparameter-method-ado"></a>Método CreateParameter (ADO)
Crea un nuevo objeto de [parámetro](./parameter-object.md) con las propiedades especificadas.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Set parameter = command.CreateParameter (Name, Type, Direction, Size, Value)  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve un objeto de **parámetro** .  
  
#### <a name="parameters"></a>Parámetros  
 *Nombre*  
 Opcional. Valor de **cadena** que contiene el nombre del objeto de **parámetro** .  
  
 *Tipo*  
 Opcional. Valor de [DataTypeEnum](./datatypeenum.md) que especifica el tipo de datos del objeto de **parámetro** .  
  
 *Dirección*  
 Opcional. Valor [ParameterDirectionEnum](./parameterdirectionenum.md) que especifica el tipo de objeto de **parámetro** .  
  
 *Tamaño*  
 Opcional. Valor de **tipo Long** que especifica la longitud máxima del valor del parámetro en caracteres o bytes.  
  
 *Valor*  
 Opcional. **Variante** que especifica el valor del objeto de **parámetro** .  
  
## <a name="remarks"></a>Observaciones  
 Use el método **CreateParameter** para crear un nuevo objeto de **parámetro** con el nombre, el tipo, la dirección, el tamaño y el valor especificados. Los valores que se pasan en los argumentos se escriben en las propiedades de **parámetro** correspondientes.  
  
 Este método no anexa automáticamente el objeto de **parámetro** a la colección **Parameters** de un objeto [Command](./command-object-ado.md) . Esto le permite establecer propiedades adicionales cuyos valores se validarán ADO al anexar el objeto de **parámetro** a la colección.  
  
 Si especifica un tipo de datos de longitud variable en el argumento de *tipo* , debe pasar un argumento de *tamaño* o establecer la propiedad [size](./size-property-ado-parameter.md) del objeto de **parámetro** antes de anexarlo a la colección **Parameters** ; de lo contrario, se produce un error.  
  
 Si especifica un tipo de datos numérico (**adNumeric** o **adDecimal**) en el argumento de *tipo* , también debe establecer las propiedades [NumericScale](./numericscale-property-ado.md) y [Precision](./precision-property-ado.md) .  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Command (ADO)](./command-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de los métodos Append y CreateParameter (VB)](./append-and-createparameter-methods-example-vb.md)   
 [Ejemplo de los métodos Append y CreateParameter (VC + +)](./append-and-createparameter-methods-example-vc.md)   
 [Append (método) (ADO)](./append-method-ado.md)   
 [Parameter (objeto)](./parameter-object.md)   
 [Colección de parámetros (ADO)](./parameters-collection-ado.md)