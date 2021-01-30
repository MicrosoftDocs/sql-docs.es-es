---
description: Objeto Property (ADO)
title: Property (objeto) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Property
helpviewer_keywords:
- Property object [ADO]
ms.assetid: b2a4767c-03c7-4935-a3bc-df3e1a38a009
author: rothja
ms.author: jroth
ms.openlocfilehash: a24503889e8bd7deab3bca6072da9114f0a0efdd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170524"
---
# <a name="property-object-ado"></a>Objeto Property (ADO)
Representa una característica dinámica de un objeto ADO definido por el proveedor.  
  
## <a name="remarks"></a>Observaciones  
 Los objetos ADO tienen dos tipos de propiedades: integrado y dinámico.  
  
 Las propiedades integradas son aquellas que se implementan en ADO y que están inmediatamente disponibles para cualquier objeto nuevo, utilizando la `MyObject.Property` Sintaxis. No aparecen como objetos de **propiedad** en la colección de [propiedades](./properties-collection-ado.md) de un objeto, por lo que, aunque se pueden cambiar sus valores, no se pueden modificar sus características.  
  
 El proveedor de datos subyacente define las propiedades dinámicas y aparecen en la colección de **propiedades** para el objeto ADO adecuado. Por ejemplo, una propiedad específica del proveedor puede indicar si un objeto de [conjunto de registros](./recordset-object-ado.md) admite transacciones o actualizaciones. Estas propiedades adicionales aparecerán como objetos de **propiedad** en la colección de **propiedades** del objeto de **conjunto de registros** . Solo se puede hacer referencia a las propiedades dinámicas a través de la colección mediante la `MyObject.Properties(0)` `MyObject.Properties("Name")` Sintaxis o.  
  
 No se puede eliminar ningún tipo de propiedad.  
  
 Un objeto de **propiedad** dinámica tiene cuatro propiedades integradas propias:  
  
-   La propiedad [Name](./name-property-ado.md) es una cadena que identifica la propiedad.  
  
-   La propiedad [Type](./type-property-ado.md) es un entero que especifica el tipo de datos de la propiedad.  
  
-   La propiedad [Value](./value-property-ado.md) es una variante que contiene el valor de la propiedad. **Value** es la propiedad predeterminada de un objeto **Property** .  
  
-   La propiedad [attributes](./attributes-property-ado.md) es un valor largo que indica las características de la propiedad específica del proveedor.  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos del objeto Property](./property-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Command (objeto) (ADO)](./command-object-ado.md)   
 [Connection (objeto) (ADO)](./connection-object-ado.md)   
 [Field (objeto)](./field-object.md)   
 [Colección Properties (ADO)](./properties-collection-ado.md)   
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)