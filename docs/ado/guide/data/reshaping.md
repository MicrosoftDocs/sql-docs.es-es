---
description: Cambiar la forma
title: Reforma | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- reshaping previously shaped Recordset [ADO]
- data shaping [ADO], reshaping
ms.assetid: b1c965b7-3dad-4de6-9e0e-502ca8785be3
author: rothja
ms.author: jroth
ms.openlocfilehash: c4604bcb4713f02ffb3f804f86b429d1db3a6aab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037085"
---
# <a name="reshaping"></a>Cambiar la forma
A un **conjunto de registros** creado por una cláusula de un comando de forma se le puede asignar un nombre de *alias* (normalmente con la palabra clave as). Se puede hacer referencia al alias de un **conjunto de registros** con forma en un comando completamente diferente. Es decir, puede reutilizar o *cambiar la forma* de un conjunto de **registros** con forma anterior en un nuevo comando Shape. Para admitir esta característica, ADO proporciona una propiedad, un [nombre de reformación](../../../ado/reference/ado-api/reshape-name-property-dynamic-ado.md).  
  
 La remodelación tiene dos funciones principales. La primera es asociar un **conjunto** de registros existente a un nuevo **conjunto de registros** primario.  
  
## <a name="example"></a>Ejemplo  
  
```  
rs1.Open "SHAPE {select * from Customers} " & _  
         "APPEND ({select * from Orders} AS chapOrders " & _  
         "RELATE CustomerID to CustomerID)", cn  
  
rs2.Open "SHAPE {select * from Employees} " & _  
         "APPEND (chapOrders RELATE EmployeeID to EmployeeID)", cn  
```  
  
 La segunda función es habilitar el acceso sin capítulo a los objetos de **conjunto de registros** secundarios existentes, con la sintaxis "Shape \<recordset reshape name> ".  
  
> [!NOTE]
>  No puede anexar columnas a un **conjunto de registros** existente, volver a dar forma a un **conjunto** de registros con parámetros o a los objetos de **conjunto de registros** en cualquier cláusula COMPUTE intermedia, o realizar operaciones de agregado en cualquier descendiente del **conjunto de registros** desde el **conjunto de registros** al que se vuelve a dar forma. El **conjunto de registros** que se está reformando y el comando de forma nueva deben usar la misma [conexión](../../../ado/reference/ado-api/connection-object-ado.md).  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de la forma de datos](../../../ado/guide/data/data-shaping-example.md)
