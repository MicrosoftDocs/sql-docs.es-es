---
description: Propiedad CommandStream (ADO)
title: CommandStream (propiedad, ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Command::CommandStream
helpviewer_keywords:
- CommandStream property [ADO]
ms.assetid: f78f61b6-87e0-48dc-961e-83d0e20da58e
author: rothja
ms.author: jroth
ms.openlocfilehash: 817ee84681b4b13895bc493034ceabade1f4773e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167754"
---
# <a name="commandstream-property-ado"></a>Propiedad CommandStream (ADO)
Indica la secuencia que se usa como entrada para un objeto de [comando](./command-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve la secuencia que se usa como entrada para un objeto de **comando** . El formato de esta secuencia es específico del proveedor; Consulte la documentación del proveedor para obtener más información. Esta propiedad es similar a la propiedad [CommandText](./commandtext-property-ado.md) , que se usa para especificar una cadena para la entrada de un **comando**.  
  
## <a name="remarks"></a>Observaciones  
 **CommandStream** y **CommandText** son mutuamente excluyentes. Cuando el usuario establece la propiedad **CommandStream** , la propiedad **CommandText** se establecerá en la cadena vacía (""). Si el usuario establece la propiedad **CommandText** , la propiedad **CommandStream** se establecerá en **Nothing**.  
  
 El proveedor define los comportamientos de los métodos **Command. Parameters. Refresh** y **Command. Prepare** . No se pueden actualizar los valores de los parámetros de una secuencia.  
  
 El flujo de entrada no está disponible para otros objetos ADO que devuelven el origen de un **comando**. Por ejemplo, si el [origen](./source-property-ado-recordset.md) de un [conjunto de registros](./recordset-object-ado.md) se establece en un objeto de **comando** que tiene una secuencia como su entrada, **Recordset. Source** continúa devolviendo la propiedad **CommandText** , que contiene una cadena vacía (""), en lugar del contenido de la secuencia de la propiedad **CommandStream** .  
  
 Cuando se usa una secuencia de comandos (tal y como se especifica en **CommandStream**), los únicos valores de [CommandTypeEnum](./commandtypeenum.md) válidos para la propiedad [CommandType](./commandtype-property-ado.md) son **adCmdText** y **adCmdUnknown**. Cualquier otro valor generará un error.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Command (ADO)](./command-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [CommandText (propiedad, ADO)](./commandtext-property-ado.md)   
 [Propiedad Dialect](./dialect-property.md)   
 [CommandTypeEnum](./commandtypeenum.md)