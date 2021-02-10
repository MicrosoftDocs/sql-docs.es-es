---
description: Servicio de forma de datos de Microsoft para OLE DB (proveedor de servicios ADO)
title: Servicio de forma de datos de Microsoft para OLE DB (proveedor de servicios ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- providers [ADO], data shaping service for OLE DB
- data shaping service for OLE DB [ADO]
ms.assetid: 523009ce-e01b-4e2d-a7df-816d7688aff0
author: rothja
ms.author: jroth
ms.openlocfilehash: a83d37c301d34c273514e4d6eb0551bc86429bd4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029379"
---
# <a name="microsoft-data-shaping-service-for-ole-db-overview"></a>Introducción al servicio de forma de datos de Microsoft para OLE DB
> [!IMPORTANT]
>  Esta característica se quitará en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. En su lugar, las aplicaciones deben usar XML.

 El servicio de forma de datos de Microsoft para el proveedor de servicios de OLE DB admite la construcción de objetos de [conjunto de registros](../../reference/ado-api/recordset-object-ado.md) jerárquicos (con forma) de un proveedor de datos.

## <a name="provider-keyword"></a>Palabra clave Provider
 Para invocar el servicio de forma de datos para OLE DB, especifique la palabra clave y el valor siguientes en la cadena de conexión.

```vb
"Provider=MSDataShape"
```

## <a name="dynamic-properties"></a>Propiedades dinámicas
 Cuando se invoca este proveedor de servicios, se agregan las siguientes propiedades dinámicas a la colección [Properties](../../reference/ado-api/properties-collection-ado.md) del objeto[Connection](../../reference/ado-api/connection-object-ado.md) .

|Nombre de propiedad dinámica|Descripción|
|---------------------------|-----------------|
|**Nombres de reformación únicos**|Indica si se permiten objetos de **conjunto de registros** con valores duplicados para sus propiedades de nombre de **cambio de forma** . Si esta propiedad dinámica es **true** y se crea un nuevo **conjunto de registros** con el mismo nombre de cambio de forma especificado por el usuario que un **conjunto de registros** existente, el nuevo nombre de cambio de forma del objeto de conjunto de **registros** se modifica para que sea único. Si esta propiedad es **false** y se crea un nuevo **conjunto de registros** con el mismo nombre de cambio de forma especificado por el usuario que el **conjunto de registros** existente, ambos objetos de conjunto de **registros** tendrán el mismo nombre de cambio de forma. Por lo tanto, no se puede cambiar la forma de ningún **conjunto de registros** siempre que existan ambos conjuntos de registros.<br /><br /> El valor predeterminado de la propiedad es **false**.|
|**Proveedor de datos**|Indica el nombre del proveedor que proporcionará las filas a las que se va a dar forma. Este valor puede ser NONE si no se va a usar un proveedor para proporcionar filas.|

 También puede establecer propiedades dinámicas que se pueden escribir especificando sus nombres como palabras clave en la cadena de conexión. Por ejemplo, en Microsoft Visual Basic, establezca la propiedad dinámica del **proveedor de datos** en "MSDASQL" especificando:

```vb
Dim cn as New ADODB.Connection
cn.Open "Provider=MSDataShape;Data Provider=MSDASQL"
```

 También puede establecer o recuperar una propiedad dinámica especificando su nombre como índice de la propiedad [Properties](../../reference/ado-api/properties-collection-ado.md) . Por ejemplo, en el ejemplo de código siguiente se obtiene e imprime el valor actual de la propiedad dinámica del **proveedor de datos** y, a continuación, se establece un nuevo valor si es CN. DataProvider se ha establecido en "MSDataShape" (ya sea directa o indirectamente a través de la cadena de conexión) y no se ha abierto la conexión:

```vb
Debug.Print cn.Properties("Data Provider")
cn.Properties("Data Provider") = "MSDASQL"
```

> [!NOTE]
>  La propiedad dinámica, el **proveedor de datos**, solo se puede establecer en un objeto de **conexión sin** abrir. Una vez abierta la conexión, la propiedad del **proveedor de datos** pasa a ser de solo lectura.

 Para obtener más información sobre el modelado de datos, vea forma de [datos](../data/data-shaping-overview.md).

## <a name="see-also"></a>Consulte también
 [Apéndice A: Proveedores](./appendix-a-providers.md)