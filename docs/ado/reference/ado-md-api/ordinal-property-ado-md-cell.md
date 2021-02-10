---
description: Propiedad ordinal (celda de ADO MD)
title: Propiedad ordinal (celda ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Cell::Ordinal
- Ordinal
helpviewer_keywords:
- Ordinal property [ADO MD]
ms.assetid: a6001168-b954-47f0-ba0d-c05c4cc40c58
author: rothja
ms.author: jroth
ms.openlocfilehash: 4d76ed341fe71f7a5927f6a7e84a63322adcf13f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051046"
---
# <a name="ordinal-property-ado-md-cell"></a>Propiedad ordinal (celda de ADO MD)
Identifica de forma única una [celda](./cell-object-ado-md.md) por su posición dentro de un Cellset.  
  
## <a name="return-values"></a>Valores devueltos  
 Devuelve un entero **largo** y es de solo lectura.  
  
## <a name="remarks"></a>Observaciones  
 El valor ordinal de la celda identifica de forma única la celda dentro de un Cellset. Conceptualmente, las celdas se numeran en un conjunto de celdas como si el conjunto de celdas fuera una matriz de dimensión *p*, donde *p* es el número de [ejes](./axes-collection-ado-md.md). Las celdas se numeran a partir de cero en orden principal de fila. Esta es la fórmula para calcular el número ordinal de una celda:  
  
 El valor ordinal de la celda se puede usar con la propiedad [Item](./item-property-ado-md-cellset.md) del objeto [Cellset](./cellset-object-ado-md.md) para recuperar rápidamente la [celda](./cell-object-ado-md.md).  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Cell (ADO MD)](./cell-object-ado-md.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de eje (VBScript)](./axis-example-vbscript.md)   
 [Objeto Cellset (ADO MD)](./cellset-object-ado-md.md)   
 [Propiedad Item (ADO MD Cellset)](./item-property-ado-md-cellset.md)   
 [Propiedad ordinal (posición de ADO MD)](./ordinal-property-ado-md-position.md)