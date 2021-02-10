---
description: Método Write
title: Write (método) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_Write
- _Stream::Write
helpviewer_keywords:
- Write method [ADO]
ms.assetid: 02982e6a-ac5f-4af2-b82e-ce12534b84b2
author: rothja
ms.author: jroth
ms.openlocfilehash: 312eba9678843932103875acd2a170a2e98f148c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056050"
---
# <a name="write-method"></a>Método Write
Escribe datos binarios en un objeto de [secuencia](./stream-object-ado.md) .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Stream.Write Buffer  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Buffer*  
 **Variante** que contiene una matriz de bytes que se va a escribir.  
  
## <a name="remarks"></a>Observaciones  
 Los bytes especificados se escriben en el objeto de **secuencia** sin ningún espacio intermedio entre cada byte.  
  
 La [posición](./position-property-ado.md) actual se establece en el byte que sigue a los datos escritos. El método **Write** no trunca el resto de los datos de una secuencia. Si desea truncar estos bytes, llame a [seteos](./seteos-method.md).  
  
 Si escribe más allá de la posición actual de [EOS](./eos-property.md) , se aumentará el [tamaño](./size-property-ado-stream.md) de la **secuencia** para que contenga cualquier byte nuevo y **EOS** se moverá al nuevo último byte de la **secuencia**.  
  
> [!NOTE]
>  El método **Write** se usa con secuencias binarias ([Type](./type-property-ado-stream.md) es **adTypeBinary**). En el caso de secuencias de texto (el **tipo** es **adTypeText**), use [WRITETEXT](./writetext-method.md).  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de secuencia (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Método WriteText](./writetext-method.md)