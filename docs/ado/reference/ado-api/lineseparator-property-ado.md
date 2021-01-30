---
description: Separador de línea (propiedad, ADO)
title: Propiedad LineSeparator (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::LineSeparator
helpviewer_keywords:
- LineSeparator property [ADO]
ms.assetid: 0b20fbb8-6b83-48ec-b442-f96c8a4bafbb
author: rothja
ms.author: jroth
ms.openlocfilehash: 629d2948bbbf62987f1352712df02521cf002189
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167176"
---
# <a name="lineseparator-property-ado"></a>Separador de línea (propiedad, ADO)
Indica el carácter binario que se va a utilizar como separador de línea en los objetos de [flujo](./stream-object-ado.md) de texto.  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor de [LineSeparatorsEnum](./lineseparatorsenum.md) que indica el carácter separador de línea utilizado en la **secuencia**. El valor predeterminado es **adCRLF**.  
  
## <a name="remarks"></a>Observaciones  
 **LineSeparator** se usa para interpretar las líneas al leer el contenido de una **secuencia** de texto. Las líneas se pueden omitir con el método [SkipLine](./skipline-method.md) .  
  
 **LineSeparator** solo se utiliza con objetos de **flujo** de texto (el [tipo](./type-property-ado-stream.md) es **adTypeText**). Esta propiedad se omite si el **tipo** es **adTypeBinary**.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de secuencia (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Objeto de secuencia (ADO)](./stream-object-ado.md)