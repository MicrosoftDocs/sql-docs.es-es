---
description: ¿Qué es un bloqueo?
title: ¿Qué es un bloqueo? | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursors [ADO], locking
- locks [ADO], about locking
ms.assetid: f8989555-28c6-4c17-9bf8-7f44a8a5c407
author: rothja
ms.author: jroth
ms.openlocfilehash: 245a0cd91b9d6c7675df146d9d5808b42bb8c618
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036735"
---
# <a name="what-is-a-lock"></a>¿Qué es un bloqueo?
El bloqueo es el proceso por el que un DBMS restringe el acceso a una fila en un entorno de varios usuarios. Cuando una fila o columna está bloqueada de forma exclusiva, no se permite a otros usuarios obtener acceso a los datos bloqueados hasta que se libera el bloqueo. Esto garantiza que dos usuarios no puedan actualizar simultáneamente la misma columna de una fila.  
  
 Los bloqueos pueden ser muy caros desde la perspectiva de los recursos y solo se deben usar cuando sea necesario para conservar la integridad de los datos. En una base de datos en la que cientos o miles de usuarios podrían estar intentando obtener acceso a un registro cada segundo, como una base de datos conectada a Internet, el bloqueo innecesario podría dar lugar a un rendimiento más lento en la aplicación.  
  
 Puede controlar el modo en que el origen de datos y la biblioteca de cursores de ADO administran la simultaneidad seleccionando la opción de bloqueo adecuada.  
  
 Establezca la propiedad **LockType** antes de abrir un **conjunto de registros** para especificar el tipo de bloqueo que debe utilizar el proveedor al abrirlo. Lea la propiedad para devolver el tipo de bloqueo en uso en un objeto de **conjunto de registros** abierto.  
  
 Es posible que los proveedores no admitan todos los tipos de bloqueo. Si un proveedor no admite la configuración de **LockType** solicitada, reemplazará a otro tipo de bloqueo. Para determinar la funcionalidad de bloqueo real disponible en un objeto de **conjunto de registros** , use el método [Supports](../../../ado/reference/ado-api/supports-method.md) con **adUpdate** y **adUpdateBatch**.  
  
 El valor **adLockPessimistic** no se admite si la propiedad [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) está establecida en **adUseClient.** Si se establece un valor no compatible, no se producirá ningún error. en su lugar, se utilizará el **LockType** compatible más cercano.  
  
 La propiedad **LockType** es de lectura y escritura cuando se cierra el **conjunto de registros** y de solo lectura cuando está abierto.  
  
 Esta sección contiene los temas siguientes.  
  
-   [Tipos de bloqueos](../../../ado/guide/data/types-of-locks.md)
