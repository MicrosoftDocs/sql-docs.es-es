---
description: Conectarse a orígenes de datos
title: Conexión a orígenes de datos | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connections [ADO]
ms.assetid: 82770486-37bd-4c90-885f-6817a7c77ad7
author: rothja
ms.author: jroth
ms.openlocfilehash: 4234cf0e54ae3f3427264835e9f88a715052c2fa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037705"
---
# <a name="connecting-to-data-sources"></a>Conectarse a orígenes de datos
Un objeto de **conexión** ADO representa una sesión única con un origen de datos, incluido un DBMS, un almacén de archivos o un archivo de texto delimitado por comas. En el caso de un sistema de base de datos cliente/servidor, la conexión ADO puede ser una conexión de red real con el servidor.  
  
 El objeto de **conexión** admite varias [propiedades y métodos](../../reference/ado-api/connection-object-properties-methods-and-events.md) para especificar configuraciones de conexión, abrir y cerrar conexiones, crear y ejecutar comandos en el origen de datos y proporcionar información sobre el diseño del origen de datos subyacente en forma de conjuntos de filas de esquema, etc. Dependiendo de la funcionalidad admitida por el proveedor, es posible que algunas colecciones, métodos o propiedades de un objeto de **conexión** no estén disponibles.  
  
 Puede conectarse a un origen de datos mediante un objeto de **conexión** o mediante un objeto de **conjunto de registros** .  
  
 Esta sección contiene los temas siguientes.  
  
-   [Uso de un objeto de conexión](./using-a-connection-object.md)  
  
-   [Mediante un objeto de conjunto de registros](./using-a-recordset-object.md)  
  
-   [Creación de una cadena de conexión](./creating-a-connection-string.md)  
  
-   [Especificar propiedades de conexión](./specifying-connection-properties.md)  
  
-   [Control de transacciones](./controlling-transactions-ado.md)