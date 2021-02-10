---
description: Propiedad dinámica Reshape Name (ADO)
title: Nombre de cambio de forma Property-Dynamic (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Reshape Name property [ADO]
ms.assetid: 690229d1-46cc-42e6-a57d-4438251fe248
author: rothja
ms.author: jroth
ms.openlocfilehash: 3af587c222e093a7918f7e390d66e64380801016
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051606"
---
# <a name="reshape-name-property-dynamic-ado"></a>Propiedad dinámica Reshape Name (ADO)
Especifica un nombre para el objeto de [conjunto de registros](./recordset-object-ado.md) .  
  
## <a name="return-values"></a>Valores devueltos  
 Devuelve un valor de **cadena** que es el nombre del **conjunto de registros**.  
  
## <a name="remarks"></a>Observaciones  
 Los nombres se conservan mientras dure la conexión o hasta que se cierra el **conjunto de registros** .  
  
 La propiedad **Reshape Name** está pensada principalmente para su uso con la característica de cambio de forma del servicio de forma de [datos de Microsoft para OLE DB](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) proveedor de servicios. Los nombres deben ser únicos para participar en el cambio de forma.  
  
 Esta propiedad es de solo lectura, pero se puede establecer indirectamente cuando se crea un **conjunto de registros** . Por ejemplo, si una cláusula de un comando Shape crea un **conjunto de registros** y le asigna un nombre de alias mediante la palabra clave **as** , el alias se asigna a la propiedad **Reshape Name** . Si no se declara ningún alias, se asigna a la propiedad **nombre de reforma** un nombre único generado por el servicio de forma de datos. Si el nombre de alias es el mismo que el nombre de un **conjunto de registros** existente, no se puede cambiar la forma de ningún **conjunto de registros** hasta que se libere uno de ellos. El comportamiento predeterminado se puede cambiar estableciendo un nombre único en la propiedad **nombre de la reformación** de la conexión ADO en **true**. Al establecer esta propiedad, se concede al servicio de forma de datos permiso para cambiar el nombre asignado por el usuario, si es necesario, para garantizar la exclusividad. Para obtener más información sobre cómo remodelar, vea [servicio de forma de datos de Microsoft para OLE DB (proveedor de servicios ADO)](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md).  
  
 Utilice la propiedad **Reshape Name** cuando desee hacer referencia a un **conjunto de registros** en un comando Shape o cuando no conozca el nombre porque lo generó el servicio de forma de datos. En ese caso, podría generar un comando de forma concatenando el comando en torno a la cadena devuelta por la propiedad **Reshape Name** .  
  
 **Reshape Name** es una propiedad dinámica anexada a la colección de [propiedades](./properties-collection-ado.md) del objeto de **conjunto de registros** cuando la propiedad [CursorLocation](./cursorlocation-property-ado.md) está establecida en **adUseClient**.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Servicio de forma de datos de Microsoft para OLE DB (proveedor de servicios ADO)](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md)   
 [Comandos Shape en general](../../guide/data/shape-commands-in-general.md)   
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)