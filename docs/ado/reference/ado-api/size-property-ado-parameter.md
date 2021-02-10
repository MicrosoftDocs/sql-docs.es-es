---
description: Propiedad Size (parámetro de ADO)
title: Propiedad Size (parámetro de ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Parameter::Size
helpviewer_keywords:
- Size property [ADO Parameter]
ms.assetid: e6bad449-ebdb-4dd3-886a-9e6f1e7ee5d2
author: rothja
ms.author: jroth
ms.openlocfilehash: 57682be31c8640dc68b149495df2b0b36639d419
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051446"
---
# <a name="size-property-ado-parameter"></a>Propiedad Size (parámetro de ADO)
Indica el tamaño máximo, en bytes o caracteres, de un objeto de [parámetro](./parameter-object.md) .  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor **Long** que indica el tamaño máximo en bytes o caracteres de un valor en un objeto de **parámetro** .  
  
## <a name="remarks"></a>Observaciones  
 Utilice la propiedad **size** para determinar el tamaño máximo de los valores escritos o leídos de la propiedad [Value](./value-property-ado.md) de un objeto **Parameter** .  
  
 Si especifica un tipo de datos de longitud variable para un objeto de **parámetro** (por ejemplo, cualquier tipo de **cadena** , como **advarchar**), debe establecer la propiedad **tamaño** del objeto antes de anexarlo a la colección [Parameters](./parameters-collection-ado.md) ; de lo contrario, se produce un error.  
  
 Si ya ha anexado el objeto de **parámetro** a la colección de **parámetros** de un objeto de [comando](./command-object-ado.md) y cambia su tipo a un tipo de datos de longitud variable, debe establecer la propiedad **tamaño** del objeto de **parámetro** antes de ejecutar el objeto de **comando** . de lo contrario, se produce un error.  
  
 Si usa el método [Refresh](./refresh-method-ado.md) para obtener información de parámetros del proveedor y devuelve uno o más objetos de **parámetro** de tipo de datos de longitud variable, ADO puede asignar memoria para los parámetros en función de su tamaño máximo potencial, lo que puede producir un error durante la ejecución. Para evitar un error, debe establecer explícitamente la propiedad **size** para estos parámetros antes de ejecutar el comando.  
  
 La propiedad **size** es de lectura y escritura.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Parameter](./parameter-object.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades ActiveConnection, CommandText, CommandTimeout, CommandType, size y Direction (VB)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vb.md)   
 [Ejemplo de las propiedades ActiveConnection, CommandText, CommandTimeout, CommandType, size y Direction (VC + +)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vc.md)   
 [Ejemplo de las propiedades ActiveConnection, CommandText, CommandTimeout, CommandType, size y Direction (JScript)](./activeconnection-commandtext-timeout-type-size-example-jscript.md)   
 [Propiedad Size (Stream de ADO)](./size-property-ado-stream.md)