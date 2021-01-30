---
description: Close (método) (ADO MD)
title: Método Close (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Close
- Cellset::Close
helpviewer_keywords:
- Close method [ADO MD]
ms.assetid: a3aa594d-f9d4-4654-8625-ec20153ff5d9
author: rothja
ms.author: jroth
ms.openlocfilehash: b90dab9bc42bb9aef0de5300c38f238b8ff67b34
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174333"
---
# <a name="close-method-ado-md"></a>Close (método) (ADO MD)
Cierra un Cellset abierto.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Cellset.Close  
```  
  
## <a name="remarks"></a>Observaciones  
 Al utilizar el método **Close** para cerrar un objeto [Cellset](./cellset-object-ado-md.md) , se liberarán los datos asociados, incluidos los datos de los objetos de [celda](./cell-object-ado-md.md), [eje](./axis-object-ado-md.md), [posición](./position-object-ado-md.md)o [miembro](./member-object-ado-md.md) relacionados. Al cerrar un **Cellset** no se quita de la memoria; puede cambiar la configuración de sus propiedades y volver a abrirla más tarde. Para eliminar completamente un objeto de la memoria, establezca la variable de objeto en **Nothing**.  
  
 Después, puede llamar al método [Open](./open-method-ado-md.md) para volver a abrir el **Cellset** usando la misma cadena de origen u otra. Mientras se cierra el objeto **Cellset** , se produce un error al recuperar cualquier propiedad o llamar a cualquier método que haga referencia a los datos o metadatos subyacentes.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de celdas (ADO MD)](./cellset-object-ado-md.md)  
  
## <a name="see-also"></a>Consulte también  
 [Objeto AXIS (ADO MD)](./axis-object-ado-md.md)   
 [Objeto Cell (ADO MD)](./cell-object-ado-md.md)   
 [Objeto miembro (ADO MD)](./member-object-ado-md.md)   
 [Método Open (ADO MD)](./open-method-ado-md.md)   
 [Objeto Position (ADO MD)](./position-object-ado-md.md)   
 [State (propiedad) (ADO MD)](./state-property-ado-md.md)