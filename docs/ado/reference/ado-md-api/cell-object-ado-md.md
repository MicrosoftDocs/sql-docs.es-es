---
description: Objeto Cell (ADO MD)
title: Objeto Cell (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Cell
helpviewer_keywords:
- Cell object [ADO MD]
ms.assetid: dcc2f044-b785-4a29-9bc5-b673f66eedf9
author: rothja
ms.author: jroth
ms.openlocfilehash: 88c9adb5ec2167bba4183d2040b16d19d5b25711
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100057050"
---
# <a name="cell-object-ado-md"></a>Objeto Cell (ADO MD)
Representa los datos en la intersección de las coordenadas del eje contenidas en un Cellset.  
  
## <a name="remarks"></a>Observaciones  
 La propiedad [Item](./item-property-ado-md-cellset.md) de un objeto [Cellset](./cellset-object-ado-md.md) devuelve un objeto **Cell** .  
  
 Con las colecciones y las propiedades de un objeto **Cell** , puede hacer lo siguiente:  
  
-   Devuelve los datos de la **celda** con la propiedad [Value](./value-property-ado-md.md) .  
  
-   Devuelve la cadena que representa la presentación con formato de la propiedad **Value** con la propiedad [FormattedValue](./formattedvalue-property-ado-md.md) .  
  
-   Devuelve el valor ordinal de la **celda** dentro del **Cellset** con la propiedad [ordinal](./ordinal-property-ado-md-cell.md) .  
  
-   Determinar la posición de la **celda** dentro del [CubeDef](./cubedef-object-ado-md.md) con la colección [positions](./positions-collection-ado-md.md) .  
  
-   Recupere otra información sobre la **celda** con la colección de [propiedades](../ado-api/properties-collection-ado.md) de ADO estándar.  
  
 La colección **Properties** contiene propiedades proporcionadas por el proveedor. En la tabla siguiente se enumeran las propiedades que pueden estar disponibles. La lista de propiedades real puede variar en función de la implementación del proveedor. Consulte la documentación del proveedor para obtener una lista más completa de las propiedades disponibles.  
  
|Nombre|Descripción|  
|----------|-----------------|  
|BackColor|Color de fondo que se usa al mostrar la celda.|  
|FontFlags|Máscara de máscara que detalla los efectos de la fuente.|  
|FontName|Fuente utilizada para mostrar el valor de la celda.|  
|FontSize|Tamaño de fuente utilizado para mostrar el valor de la celda.|  
|ForeColor|Color de primer plano que se usa al mostrar la celda.|  
|FormatString|Valor en una cadena con formato.|  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos](./cell-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de eje (VBScript)](./axis-example-vbscript.md)   
 [Objeto Cellset (ADO MD)](./cellset-object-ado-md.md)   
 [Colección positions (ADO MD)](./positions-collection-ado-md.md)   
 [Colección de propiedades (ADO)](../ado-api/properties-collection-ado.md)