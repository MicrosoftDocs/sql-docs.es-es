---
description: Propiedad CommandType (ADO)
title: CommandType (propiedad) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::CommandType
helpviewer_keywords:
- CommandType property [ADO]
ms.assetid: ca44809c-8647-48b6-a7fb-0be70a02f53e
author: rothja
ms.author: jroth
ms.openlocfilehash: debd8c2630ba9fa7369b356950f979a64eee59d0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99155721"
---
# <a name="commandtype-property-ado"></a>Propiedad CommandType (ADO)
Indica el tipo de un objeto de [comando](./command-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve uno o más valores de [CommandTypeEnum](./commandtypeenum.md) .  
  
> [!NOTE]
>  No use los valores **CommandTypeEnum** de **adCmdFile** o **adCmdTableDirect** con **CommandType**. Estos valores solo se pueden usar como opciones con los métodos [Open](./open-method-ado-recordset.md) y [Requery](./requery-method.md) de un [conjunto de registros](./recordset-object-ado.md).  
  
## <a name="remarks"></a>Observaciones  
 Utilice la propiedad **CommandType** para optimizar la evaluación de la propiedad [CommandText](./commandtext-property-ado.md) .  
  
 Si el valor de la propiedad **CommandType** se establece en el valor predeterminado, **adCmdUnknown**, puede experimentar un rendimiento reducido porque ADO debe realizar llamadas al proveedor para determinar si la propiedad **CommandText** es una instrucción SQL, un procedimiento almacenado o un nombre de tabla. Si sabe qué tipo de comando está usando, el establecimiento de la propiedad **CommandType** indica a ADO que vaya directamente al código relevante. Si la propiedad **CommandType** no coincide con el tipo de comando de la propiedad **CommandText** , se produce un error al llamar al método [Execute](./execute-method-ado-command.md) .  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Command (ADO)](./command-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades ActiveConnection, CommandText, CommandTimeout, CommandType, size y Direction (VB)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vb.md)   
 [Ejemplo de las propiedades ActiveConnection, CommandText, CommandTimeout, CommandType, size y Direction (VC + +)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vc.md)   
 [Ejemplo de las propiedades ActiveConnection, CommandText, CommandTimeout, CommandType, size y Direction (JScript)](./activeconnection-commandtext-timeout-type-size-example-jscript.md)