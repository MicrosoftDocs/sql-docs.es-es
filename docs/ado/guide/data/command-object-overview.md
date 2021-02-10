---
description: Información general sobre objetos de comando
title: Información general sobre el objeto Command | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- command object [ADO]
ms.assetid: e84a14b1-3c2a-4f7d-a966-9e08a93948df
author: rothja
ms.author: jroth
ms.openlocfilehash: c23ce0b24fcd5fb82f306c1746f7a277bed293ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033314"
---
# <a name="command-object-overview"></a>Información general sobre objetos de comando
Con un objeto de **comando** , puede hacer lo siguiente:  
  
-   Defina el texto ejecutable del comando (por ejemplo, una instrucción SQL o un procedimiento almacenado) mediante la propiedad **CommandText** .  
  
-   Defina consultas con parámetros o argumentos de procedimientos almacenados mediante el uso de objetos de **parámetro** y la colección de **parámetros** .  
  
-   Ejecutar un comando y devolver un objeto de **conjunto de registros** , si es necesario, mediante el método **Execute** .  
  
-   Especifique el tipo de comando mediante la propiedad **CommandType** antes de la ejecución para optimizar el rendimiento.  
  
-   Especifique información específica sobre el texto del comando mediante la propiedad **Dialect** del objeto **Command** .  
  
-   Controlar si el proveedor guarda una versión preparada (o compilada) del comando antes de la ejecución mediante la propiedad **Prepared** .  
  
-   Establezca el número de segundos que un proveedor esperará a que se ejecute un comando mediante la propiedad **CommandTimeout** .  
  
-   Asocie una conexión abierta a un objeto de **comando** estableciendo su propiedad **ActiveConnection** .  
  
-   Establezca la propiedad **nombre** para identificar el objeto de **comando** como un método en el objeto de **conexión** asociado.  
  
-   Pase un objeto de **comando** a la propiedad de **origen** de un **conjunto de registros** para obtener los datos.  
  
-   Pase un objeto de **flujo** que contenga un comando (por ejemplo, un comando XML) a un proveedor que lo admita.
