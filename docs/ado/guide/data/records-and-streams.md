---
description: Registros y secuencias
title: Registros y secuencias | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- streams [ADO]
- streams [ADO], about streams
- records [ADO]
ms.assetid: 4d68868e-2611-4b5c-9a89-7caa5f753151
author: rothja
ms.author: jroth
ms.openlocfilehash: 2de6347377cbdeb464941d766d732c23443efa2a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032567"
---
# <a name="records-and-streams"></a>Registros y secuencias
ADO actualmente proporciona el objeto de [conjunto de registros](../../../ado/reference/ado-api/recordset-object-ado.md) como medio principal para tener acceso a la información de los orígenes de datos, como las bases de datos relacionales. Sin embargo, algunos proveedores admiten los objetos [Record](../../../ado/reference/ado-api/record-object-ado.md) y [Stream](../../../ado/reference/ado-api/stream-object-ado.md) como objetos complementarios o alternativos con los que se pueden manipular los datos de los proveedores. Para obtener información específica sobre el comportamiento del **registro** , consulte la documentación del proveedor.  
  
## <a name="records"></a>Registros  
 Los objetos de **registro** básicamente funcionan como **conjuntos de registros** de una fila. Sin embargo, **los registros** tienen una funcionalidad limitada en comparación con los **conjuntos de registros** y tienen propiedades y métodos diferentes. El origen de los datos de un objeto de **registro** puede ser un comando que devuelve una fila de datos del proveedor. El uso de objetos de **registro** en lugar de objetos de **conjunto de registros** para recibir los resultados de una consulta que devuelve una fila de datos elimina la sobrecarga de crear instancias del objeto de **conjunto de registros** más complejo.  
  
 Los objetos de **registro** pueden servir a otro propósito, especialmente con los proveedores de orígenes de datos que no sean bases de datos relacionales tradicionales, como el [proveedor de Microsoft OLE DB para la publicación en Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Gran parte de la información que se debe procesar existe, no como tablas en las bases de datos, sino como mensajes en los sistemas de correo electrónico y los archivos de los sistemas de archivos modernos. Los objetos **Record** y **Stream** facilitan el acceso a la información almacenada en orígenes distintos de las bases de datos relacionales.  
  
 El objeto de **registro** puede representar y administrar datos como directorios y archivos en un sistema de archivos o carpetas y mensajes en un sistema de correo electrónico. Para estos propósitos, el origen del **registro** puede ser la fila actual de un conjunto de **registros** abierto, una dirección URL absoluta o una dirección URL relativa junto con un objeto de [conexión](../../../ado/reference/ado-api/connection-object-ado.md) abierto.  
  
 Normalmente, un **conjunto de registros** se puede usar para representar un contenedor o un elemento primario en una jerarquía, como una carpeta o un directorio. Un **registro** se puede utilizar para devolver información específica sobre un nodo del contenedor primario, como un archivo o un documento. Los **registros** de motivo principal que se usan para representar este tipo de información son que estos orígenes de datos son heterogéneos. Esto significa que cada **registro** puede tener un conjunto y un número de campos distintos. Los **conjuntos de registros** tradicionales que contienen filas de una base de datos son homogéneos, lo que significa que cada fila tiene el mismo número y tipo de campos.  
  
 Para obtener más información sobre el uso del objeto **Record** para procesar estos datos heterogéneos de proveedores como el proveedor de publicación en Internet, vea [usar ado para la publicación en Internet](../../../ado/guide/data/using-ado-for-internet-publishing.md).  
  
## <a name="streams"></a>Secuencias  
 El objeto **Stream** proporciona los medios para leer, escribir y administrar un flujo de bytes. Esta secuencia de bytes puede ser texto o binaria y solo tiene un tamaño limitado por los recursos del sistema. Normalmente, los objetos de **secuencia** de ADO se utilizan para los siguientes fines:  
  
-   Para contener los datos de un **conjunto de registros** guardado en formato XML. Estas secuencias XML de los **conjuntos de registros** guardados se pueden usar como origen al abrir un nuevo **conjunto de registros**. Para obtener más información, vea [flujos y persistencia](../../../ado/guide/data/streams-and-persistence.md).  
  
-   Para contener [CommandStreams](../../../ado/reference/ado-api/commandstream-property-ado.md) que se van a ejecutar en el proveedor como alternativa a [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md). Por ejemplo, se puede usar XML diagramas como origen de un comando en el proveedor de OLE DB de Microsoft para SQL Server.  
  
-   Para recibir los resultados del proveedor en un formato que no sea un **conjunto de registros**, como los resultados XML del proveedor de OLE DB de Microsoft para SQL Server. Para obtener más información, vea [recuperar conjuntos de datos en secuencias](../../../ado/guide/data/retrieving-resultsets-into-streams.md).  
  
-   Para contener el texto o bytes que componen un archivo o un mensaje, normalmente se usa con proveedores como el proveedor de OLE DB de Microsoft para la publicación en Internet. Para obtener más información sobre este uso de los objetos de **secuencia** , vea [usar ado para la publicación en Internet](../../../ado/guide/data/using-ado-for-internet-publishing.md).  
  
 Un objeto de **secuencia** se puede abrir en:  
  
-   Un archivo simple especificado con una dirección URL.  
  
-   Campo de un **registro** o **conjunto de registros** que contiene un objeto de **secuencia** .  
  
-   Secuencia predeterminada de un **registro** o un objeto de **conjunto de registros** que representa un directorio o archivo compuesto.  
  
-   Campo de recursos que contiene la dirección URL de un archivo simple.  
  
-   Ningún origen determinado. En este caso, se abre un objeto de **flujo** en la memoria. Los datos se pueden escribir en él y, a continuación, guardar en otro **flujo** o archivo.  
  
-   Campo de BLOB en un **conjunto de registros**.  
  
 Esta sección contiene los temas siguientes.  
  
-   [Secuencias y persistencia](../../../ado/guide/data/streams-and-persistence.md)  
  
-   [Secuencias de comandos](../../../ado/guide/data/command-streams.md)  
  
-   [Recuperar conjuntos de resultados en secuencias](../../../ado/guide/data/retrieving-resultsets-into-streams.md)  
  
-   [Utilizar ADO para publicación en Internet](../../../ado/guide/data/using-ado-for-internet-publishing.md)
