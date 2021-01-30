---
description: Método SkipLine
title: Método SkipLine | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_SkipLine
- _Stream::SkipLine
helpviewer_keywords:
- Skipline method [ADO]
ms.assetid: 0abc00fe-ee09-4c8e-b1f2-48ee9c5f3329
author: rothja
ms.author: jroth
ms.openlocfilehash: 826e90b65131453bb50cc70e3efbc3ad8796c89f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166541"
---
# <a name="skipline-method"></a>Método SkipLine
Omite una línea completa cuando se lee una [secuencia](./stream-object-ado.md)de texto.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Stream.SkipLine  
```  
  
## <a name="remarks"></a>Observaciones  
 Se omiten todos los caracteres hasta el separador de línea siguiente, incluido. De forma predeterminada, el valor de [LineSeparator](./lineseparator-property-ado.md) es **adCRLF**. Si intenta omitir más allá de [EOS](./eos-property.md), la posición actual permanecerá en **EOS**.  
  
 El método **SkipLine** se usa con secuencias de texto (el [tipo](./type-property-ado-stream.md) es **adTypeText**).  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de secuencia (ADO)](./stream-object-ado.md)