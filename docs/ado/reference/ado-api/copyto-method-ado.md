---
description: CopyTo (método) (ADO)
title: CopyTo (método) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_CopyTo
- _Stream::CopyTo
helpviewer_keywords:
- CopyTo method [ADO]
ms.assetid: b4aa5714-916b-48b8-8b09-cc2708379602
author: rothja
ms.author: jroth
ms.openlocfilehash: 07c5a4bc4db5fc05cb24ee51ac7a9ec989dd1e29
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171402"
---
# <a name="copyto-method-ado"></a>CopyTo (método) (ADO)
Copia el número especificado de caracteres o bytes (dependiendo del [tipo](./type-property-ado-stream.md)) de la [secuencia](./stream-object-ado.md) en otro objeto de **flujo** .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Stream.CopyTo DestStream, NumChars  
```  
  
#### <a name="parameters"></a>Parámetros  
 *DestStream*  
 Valor de variable de objeto que contiene una referencia a un objeto de **secuencia** abierto. La **secuencia** actual se copia en la **secuencia** de destino especificada por *DestStream*. El **flujo** de destino ya debe estar abierto. Si no es así, se produce un error en tiempo de ejecución.  
  
> [!NOTE]
>  Es posible que el parámetro *DestStream* no sea un proxy de objeto de **flujo** porque requiere acceso a una interfaz privada en el objeto de **secuencia** que no se puede enviar de forma remota al cliente.  
  
 *NumChars*  
 Opcional. Valor **entero** que especifica el número de bytes o caracteres que se van a copiar desde la posición actual en la **secuencia** de origen al **flujo** de destino. El valor predeterminado es-1, que especifica que se copian todos los caracteres o bytes de la posición actual a [EOS](./eos-property.md).  
  
## <a name="remarks"></a>Observaciones  
 Este método copia el número especificado de caracteres o bytes, empezando por la posición actual especificada por la propiedad [Position](./position-property-ado.md) . Si el número especificado es mayor que el número de bytes disponible hasta **EOS**, solo se copian los caracteres o bytes de la posición actual a **EOS** . Si el valor de *NumChars* es-1, o se omite, se copian todos los caracteres o bytes a partir de la posición actual.  
  
 Si hay caracteres o bytes existentes en el flujo de destino, todo el contenido más allá del punto en el que finaliza la copia permanece y no se trunca. La **posición** se convierte en el byte inmediatamente posterior al último byte copiado. Si desea truncar estos bytes, llame a [seteos](./seteos-method.md).  
  
 **CopyTo** se debe usar para copiar datos en un **flujo** de destino del mismo tipo que la **secuencia** de origen (la configuración de la propiedad de **tipo** es **adTypeText** o bien **adTypeBinary**). En el caso de los objetos de **flujo** de texto, puede cambiar la configuración de la propiedad [CharSet](./charset-property-ado.md) del **flujo** de destino para convertir de un juego de caracteres a otro. Además, los objetos de **flujo** de texto se pueden copiar correctamente en objetos de **flujo** binario, pero los objetos de **flujo** binario no se pueden copiar en objetos de **flujo** de texto.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de secuencia (ADO)](./stream-object-ado.md)