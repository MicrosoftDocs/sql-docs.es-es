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
ms.openlocfilehash: 3c1b23ef249e2dd543772e1e414d347cc2377860
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051436"
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