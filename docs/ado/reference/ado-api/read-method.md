---
description: Método Read
title: Read (método) | Microsoft Docs
ms.prod: sql
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Stream::raw_Read
- _Stream::Read
helpviewer_keywords:
- Read method [ADO]
ms.assetid: 838502de-80f1-4eeb-8838-dd3d9403e567
author: rothja
ms.author: jroth
ms.openlocfilehash: 0adf12bc63745f739aaf8b71a92c660c1c1ad2fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170491"
---
# <a name="read-method"></a>Método Read
Lee un número especificado de bytes de un objeto de [flujo](./stream-object-ado.md) binario.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Variant = Stream.Read ( NumBytes)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *NumBytes*  
 Opcional. Valor de **tipo Long** que especifica el número de bytes que se van a leer del archivo o el valor de [StreamReadEnum](./streamreadenum.md) **adReadAll**, que es el valor predeterminado.  
  
## <a name="return-value"></a>Valor devuelto  
 El método **Read** Lee un número especificado de bytes o el flujo completo de un objeto de **secuencia** y devuelve los datos resultantes como una **variante**.  
  
## <a name="remarks"></a>Observaciones  
 Si *numbytes* es mayor que el número de bytes que quedan en la **secuencia**, solo se devuelven los bytes restantes. Los datos leídos no se rellenan para coincidir con la longitud especificada por *numbytes*. Si no quedan bytes para leer, se devuelve una variante con un valor null. **Read** no se puede usar para leer hacia atrás.  
  
> [!NOTE]
>  *Numbytes* siempre mide bytes. En el caso de los objetos de **flujo** de texto (el [tipo](./type-property-ado-stream.md) es **AdTypeText**), use [READTEXT](./readtext-method.md).  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de secuencia (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Método ReadText](./readtext-method.md)