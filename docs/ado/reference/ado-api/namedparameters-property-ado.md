---
description: Propiedad NamedParameters (ADO)
title: Propiedad NamedParameters (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Command::NamedParameters
helpviewer_keywords:
- NamedParameters property [ADO]
ms.assetid: 42409387-026c-435f-a9b1-bf4167095875
author: rothja
ms.author: jroth
ms.openlocfilehash: 9ce2c73ec00431f649d24a9effda2a7e9ecb6cfd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170784"
---
# <a name="namedparameters-property-ado"></a>Propiedad NamedParameters (ADO)
Indica si los nombres de parámetro se deben pasar al proveedor.  
  
## <a name="remarks"></a>Observaciones  
 Cuando esta propiedad es true, ADO pasa el valor de la propiedad **Name** de cada parámetro de la colección de **parámetros** para el [objeto de comando](./command-object-ado.md). El proveedor usa un nombre de parámetro para hacer coincidir los parámetros de las propiedades **CommandText** o **CommandStream** . Si esta propiedad es false (el valor predeterminado), los nombres de parámetro se omiten y el proveedor usa el orden de los parámetros para hacer coincidir los valores con los parámetros de las propiedades **CommandText** o **CommandStream** .  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Command (ADO)](./command-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [CommandText (propiedad, ADO)](./commandtext-property-ado.md)   
 [CommandStream (propiedad, ADO)](./commandstream-property-ado.md)   
 [Colección de parámetros (ADO)](./parameters-collection-ado.md)