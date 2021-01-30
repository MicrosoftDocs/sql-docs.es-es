---
description: Propiedad Item (conjunto de celdas de ADO MD)
title: Propiedad Item (ADO MD Cellset) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Item
- Cellset::Item
helpviewer_keywords:
- Item property [ADO MD]
ms.assetid: 0e93d79b-b12e-4e98-889e-c2dfcca20fd0
author: rothja
ms.author: jroth
ms.openlocfilehash: 6d2fe7bb3d13b988b7bc25b6d3f17a5b2ccf945a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164534"
---
# <a name="item-property-ado-md-cellset"></a>Propiedad Item (conjunto de celdas de ADO MD)
Recupera una celda de un [Cellset](./cellset-object-ado-md.md) usando sus coordenadas.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Set  
Cell = Cellset.Item ( Positions)  
```  
  
## <a name="parameters"></a>Parámetros  
 *Positions*  
 **VariantArray** de valores que especifican de forma única una celda. Las *posiciones* pueden ser una de las siguientes:  
  
-   Una matriz de números de posición  
  
-   Una matriz de nombres de miembro  
  
-   La posición ordinal  
  
## <a name="remarks"></a>Observaciones  
 Utilice la propiedad **Item** para devolver un objeto [Cell](./cell-object-ado-md.md) dentro de un objeto [Cellset](./cellset-object-ado-md.md) . Si la propiedad **Item** no encuentra la celda correspondiente al argumento *positions* , se produce un error.  
  
 La propiedad **Item** es la propiedad predeterminada para el objeto **Cellset** . Los siguientes formatos de sintaxis son intercambiables:  
  
```  
  
Cellset.Item ( Positions )Cellset ( Positions )  
```  
  
 El argumento *positions* especifica la celda que se va a devolver. Puede especificar la celda por posición ordinal o por la posición a lo largo de cada eje. Al especificar la celda por posición a lo largo de cada eje, puede especificar el valor numérico de la posición o los nombres de los miembros de cada posición.  
  
 La posición ordinal es un número que identifica de forma única una celda dentro del **Cellset**. Conceptualmente, las celdas se numeran en **un conjunto de celdas como** si el conjunto de **celdas** fuera una matriz de dimensión *p*, donde *p* es el número de ejes. Las celdas se ordenan por importancia de fila. A continuación se muestra la fórmula para calcular el número ordinal de una celda:  
  
 Si los nombres de miembro se pasan como cadenas al **elemento**, los miembros deben aparecer en orden ascendente de los identificadores de eje numéricos. Dentro de un eje, los miembros deben aparecer en orden ascendente de anidamiento de dimensiones; es decir, el miembro de la dimensión más externa viene primero, seguido de los miembros de las dimensiones internas. Cada dimensión debe estar representada por una cadena independiente y la lista de cadenas de miembro debe estar separada por comas.  
  
> [!NOTE]
>  Es posible que el proveedor de datos no admita la recuperación de celdas por nombre de miembro. Consulte la documentación del proveedor para obtener más información.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de celdas (ADO MD)](./cellset-object-ado-md.md)  
  
## <a name="see-also"></a>Consulte también  
 [Objeto Cell (ADO MD)](./cell-object-ado-md.md)   
 [Objeto de conjunto de celdas (ADO MD)](./cellset-object-ado-md.md)