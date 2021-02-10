---
description: El proveedor OLE DB para la publicación en Internet
title: Proveedor de OLE DB para la publicación en Internet | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB provider for Internet publishing [ADO]
- ADO, Internet publishing
- publishing to Internet [ADO]
- Internet publishing [ADO]
- providers [ADO], OLE DB provider for Internet publishing
ms.assetid: 4869aafa-7401-4ce1-93ce-45406a60274f
author: rothja
ms.author: jroth
ms.openlocfilehash: 9282bdc81d71ecd938f3d115678769915f8e3367
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032407"
---
# <a name="the-ole-db-provider-for-internet-publishing"></a>El proveedor OLE DB para la publicación en Internet
Los objetos de [secuencia](../../../ado/reference/ado-api/stream-object-ado.md) y [registro](../../../ado/reference/ado-api/record-object-ado.md) de ADO se pueden usar con el proveedor de Microsoft OLE DB para la publicación en Internet (proveedor de publicación en Internet) para obtener acceso y manipular recursos, como carpetas web o archivos servidos por Microsoft FrontPage. Con ADO, puede especificar que el origen de un **registro**, **secuencia** o [conjunto de registros](../../../ado/reference/ado-api/recordset-object-ado.md) sea una dirección URL. Después, puede cargar, descargar, trasladar, copiar y eliminar recursos, o manipular directamente las propiedades de los recursos.  
  
 Para obtener un ejemplo de código que usa **registros** y **secuencias** con el proveedor de publicación en Internet, vea el [escenario de publicación en Internet](../../../ado/guide/data/internet-publishing-scenario.md).  
  
 El proveedor de publicación en Internet se instala con Microsoft Windows 2000. Las versiones anteriores del proveedor de publicación en Internet también están disponibles con Microsoft Office 2000 y Microsoft Internet Explorer 5,0.  
  
 Hay tres formas de conectar ADO al proveedor de publicación en Internet:  
  
-   Especifique "URL =" en la cadena de conexión. Por ejemplo:  
  
    ```  
    objConn.Open "URL=https://servername"  
    ```  
  
-   Especifique Msdaipp. DSO para la palabra clave *Provider* de la cadena de conexión. Por ejemplo:  
  
    ```  
    objConn.Open "provider=MSDAIPP.DSO;data source=https://servername"  
    ```  
  
-   Especifique Msdaipp. DSO para la propiedad [Provider](../../../ado/reference/ado-api/provider-property-ado.md) del objeto [Connection](../../../ado/reference/ado-api/connection-object-ado.md) . Por ejemplo:  
  
    ```  
    objConn.Provider = "MSDAIPP.DSO"  
    objConn.Open "https://servername"  
    ```  
  
> [!NOTE]
>  Si Msdaipp. DSO se especifica explícitamente como el valor del proveedor, ya sea con la palabra clave de la cadena de conexión del *proveedor* o la propiedad del **proveedor** , no se puede usar "URL =" en la cadena de conexión. Si lo hace, se producirá un error. En su lugar, solo tiene que especificar la dirección URL como se mostró anteriormente.  
  
 Para obtener información más específica sobre el proveedor de publicación en Internet, vea [proveedor de Microsoft OLE DB para la publicación en Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md), o la documentación del proveedor que se proporciona con la aplicación de origen con la que se instaló el proveedor de OLE DB para la publicación en Internet: Windows 2000, Office 2000 o Internet Explorer 5,0.
