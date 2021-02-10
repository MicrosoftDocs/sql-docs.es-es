---
description: Propiedad Type (objeto Stream de ADO)
title: Propiedad Type (secuencia de ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::Type
- _Stream::get_Type
- _Stream::put_Type
helpviewer_keywords:
- Type property [ADO Stream]
ms.assetid: f6a17e8c-7a28-48d0-bded-76b9e0cf7639
author: rothja
ms.author: jroth
ms.openlocfilehash: f68e21972be70369d231ede6788be5d4b200b69d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056450"
---
# <a name="type-property-ado-stream"></a>Propiedad Type (objeto Stream de ADO)
Indica el tipo de datos contenidos en la [secuencia](./stream-object-ado.md) (binario o de texto).  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor [StreamTypeEnum](./streamtypeenum.md) que especifica el tipo de datos contenidos en el objeto de **secuencia** . El valor predeterminado es **adTypeText**. Sin embargo, si los datos binarios se escriben inicialmente en una **secuencia** nueva y vacía, el **tipo** se cambiará a **adTypeBinary**.  
  
## <a name="remarks"></a>Observaciones  
 La propiedad **Type** es de lectura y escritura solo cuando la posición actual está al principio de la **secuencia** ([Position](./position-property-ado.md) es 0) y de solo lectura en cualquier otra posición.  
  
 La propiedad **Type** determina qué métodos deben usarse para leer y escribir la **secuencia**. En el caso de las **secuencias** de texto, use [READTEXT](./readtext-method.md) y [WRITETEXT](./writetext-method.md). En el caso de las **secuencias** binarias, use [lectura](./read-method.md) y [escritura](./write-method.md).  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de secuencia (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [RecordType (propiedad, ADO)](./recordtype-property-ado.md)   
 [Tipo (propiedad, ADO)](./type-property-ado.md)