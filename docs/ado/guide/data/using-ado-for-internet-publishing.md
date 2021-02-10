---
description: Utilizar ADO para publicación en Internet
title: Usar ADO para la publicación en Internet | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO, Internet publishing
- publishing to Internet [ADO]
- Internet publishing [ADO]
- urls [ADO]
ms.assetid: d399fce4-b70b-418f-8110-3deb3448863c
author: rothja
ms.author: jroth
ms.openlocfilehash: f7affce9c8596b9155f691ccf2ef61f9f0eca35d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032277"
---
# <a name="using-ado-for-internet-publishing"></a>Utilizar ADO para publicación en Internet
[El proveedor de OLE DB para la publicación en Internet](../../../ado/guide/data/the-ole-db-provider-for-internet-publishing.md) muestra un ejemplo específico del acceso a datos heterogéneos con ADO. Aunque los ejemplos de esta sección son específicos del uso del proveedor de publicación en Internet, los principios que se demuestran deben ser similares al usar ADO con otros proveedores para datos heterogéneos, como un proveedor para un almacén de correo electrónico.  
  
## <a name="urls"></a>URLs  
 Los localizadores de recursos uniformes (URL) se pueden usar como alternativa a las cadenas de conexión y el texto del comando para especificar los orígenes de datos y la ubicación de los archivos y directorios. Puede usar direcciones URL con los objetos [Connection](../../../ado/reference/ado-api/connection-object-ado.md) y **Recordset** existentes y con los objetos **Record** y **Stream** .  
  
 Para obtener más información sobre cómo usar las direcciones URL, consulte [direcciones URL absolutas y relativas](../../../ado/guide/data/absolute-and-relative-urls.md).  
  
## <a name="record-fields"></a>Campos de registro  
 La diferencia entre los datos heterogéneos y los datos homogéneos es que para el primero, cada fila de datos o **registro** puede tener un conjunto diferente de columnas o **campos**. En el caso de los datos homogéneos, cada fila tiene el mismo conjunto de columnas. Para obtener más información sobre los campos específicos del proveedor de publicación en Internet, vea [registros y Provider-Supplied campos adicionales](../../../ado/guide/data/records-and-provider-supplied-fields.md).  
  
### <a name="appending-new-fields"></a>Anexar nuevos campos  
 Se han mejorado varios objetos ADO para que funcionen con objetos de **registro** y de **secuencia** .  
  
-   El método [Append](../../../ado/reference/ado-api/append-method-ado.md) de la colección [Fields](../../../ado/reference/ado-api/fields-collection-ado.md) , que crea y agrega un objeto de [campo](../../../ado/reference/ado-api/field-object.md) a la colección, también puede especificar el valor del **campo**.  
  
-   El método [Update](../../../ado/reference/ado-api/update-method.md) finaliza la adición o eliminación de campos a la colección.  
  
-   Como método abreviado y alternativo al método **Append** , puede crear campos asignando un valor a un campo sin definir o eliminado previamente.  
  
 Esta sección contiene los temas siguientes.  
  
-   [El proveedor OLE DB para la publicación en Internet](../../../ado/guide/data/the-ole-db-provider-for-internet-publishing.md)  
  
-   [Escenario de publicación en Internet](../../../ado/guide/data/internet-publishing-scenario.md)  
  
-   [Direcciones URL absolutas y relativas](../../../ado/guide/data/absolute-and-relative-urls.md)  
  
-   [Registros y campos proporcionados por el proveedor](../../../ado/guide/data/records-and-provider-supplied-fields.md)  
  
## <a name="see-also"></a>Consulte también  
 [Objeto Record (ADO)](../../../ado/reference/ado-api/record-object-ado.md)   
 [Stream (objeto) (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)   
 [Características de ADO para cada versión](../../../ado/guide/ado-history.md)
