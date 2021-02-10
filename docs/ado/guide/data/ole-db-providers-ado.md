---
description: Proveedores OLE DB (ADO)
title: Proveedores de OLE DB (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB providers [ADO]
- ADO, OLE DB providers
ms.assetid: 6e0488c3-934d-4976-99dc-65c580dc7a3c
author: rothja
ms.author: jroth
ms.openlocfilehash: 7e70762a2cbaa6e2dd3d8d91b824591799605748
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032667"
---
# <a name="ole-db-providers-ado"></a>Proveedores OLE DB (ADO)
OLE DB define un conjunto de interfaces COM para proporcionar a las aplicaciones acceso uniforme a los datos almacenados en diversos orígenes de información. Este enfoque permite a un origen de datos compartir sus datos a través de las interfaces que admiten la cantidad de funcionalidad de DBMS adecuada para el origen de datos. Por diseño, la arquitectura de alto rendimiento de OLE DB se basa en el uso de un modelo de servicios flexible basado en componentes. En lugar de tener un número previsto de capas intermediarias entre la aplicación y los datos, OLE DB solo requiere tantos componentes como sean necesarios para realizar una tarea determinada.  
  
 Por ejemplo, supongamos que un usuario desea ejecutar una consulta. Considere los casos siguientes:  
  
-   Los datos residen en una base de datos relacional para la que existe un controlador ODBC pero ningún proveedor de OLE DB nativo: la aplicación utiliza ADO para comunicarse con el proveedor de OLE DB para ODBC, que, a continuación, carga el controlador ODBC adecuado. El controlador pasa la instrucción SQL al DBMS, que recupera los datos.  
  
-   Los datos residen en Microsoft SQL Server para los que hay un proveedor de OLE DB nativo: la aplicación utiliza ADO para comunicarse directamente con el proveedor de OLE DB de Microsoft SQL Server. No se requieren intermediarios.  
  
-   Los datos residen en Microsoft Exchange Server, para los que hay un proveedor de OLE DB pero que no expone un motor para procesar consultas SQL: la aplicación utiliza ADO para comunicarse con el proveedor de OLE DB para Microsoft Exchange y llama a un componente de procesador de consultas de OLE DB para controlar las consultas.  
  
-   Los datos residen en el sistema de archivos NTFS de Microsoft en forma de documentos: se tiene acceso a los datos mediante un proveedor de OLE DB nativo en el servicio de indización de Microsoft, que indexa el contenido y las propiedades de los documentos en el sistema de archivos para permitir búsquedas de contenido eficientes.  
  
 En todos los ejemplos anteriores, la aplicación puede consultar los datos. Las necesidades del usuario se cumplen con un número mínimo de componentes. En cada caso, los componentes adicionales se usan solo si es necesario y solo se invocan los componentes necesarios. Esta carga de la demanda de componentes reutilizables y compartibles contribuye en gran medida al alto rendimiento cuando se usa OLE DB.  
  
 Los proveedores se dividen en dos categorías: aquellas que proporcionan datos y que proporcionan servicios. Un proveedor de datos posee sus propios datos y los expone en formato tabular a la aplicación. Un proveedor de servicios encapsula un servicio mediante la generación y el consumo de datos, lo que aumenta las características de las aplicaciones de ADO. También se puede definir un proveedor de servicios como componente de servicio, que debe funcionar junto con otros proveedores de servicios o componentes.  
  
 ADO proporciona una interfaz coherente y de nivel superior para los diversos proveedores de OLE DB.  
  
 Esta sección contiene los temas siguientes.  
  
-   [Proveedores de datos](./data-providers.md)  
  
-   [Proveedores de servicios y componentes](./service-providers-and-components.md)