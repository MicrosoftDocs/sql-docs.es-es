---
description: Propiedad Size (Stream de ADO)
title: Propiedad Size (secuencia de ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::Size
helpviewer_keywords:
- Size property [ADO Stream]
ms.assetid: a487c241-d953-4c31-ae7e-6358d5cf6733
author: rothja
ms.author: jroth
ms.openlocfilehash: 36d1da9d1b95480eeaf3018b49018df7541c510f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170264"
---
# <a name="size-property-ado-stream"></a>Propiedad Size (Stream de ADO)
Indica el tamaño de la secuencia en número de bytes.  
  
## <a name="return-values"></a>Valores devueltos  
 Devuelve un valor de **tipo Long** que especifica el tamaño de la secuencia en número de bytes. El valor predeterminado es el tamaño de la secuencia o-1 si no se conoce el tamaño de la secuencia.  
  
## <a name="remarks"></a>Observaciones  
 **El tamaño** solo se puede usar con objetos de [secuencia](./stream-object-ado.md) abiertos.  
  
> [!NOTE]
>  Cualquier número de bits se puede almacenar en un objeto de **secuencia** , limitado solo por los recursos del sistema. Si la **secuencia** contiene más bits de los que se pueden representar con un valor **Long** , **el tamaño** se trunca y, por lo tanto, no representa con precisión la longitud de la **secuencia**.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de secuencia (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Propiedad Size (parámetro de ADO)](./size-property-ado-parameter.md)