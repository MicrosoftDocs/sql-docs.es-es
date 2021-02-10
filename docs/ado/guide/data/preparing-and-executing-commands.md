---
description: Preparar y ejecutar comandos
title: Preparar y ejecutar comandos | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Command object [ADO], preparing and executing commands
ms.assetid: 7448d9ee-7f4b-47e3-be54-2df8c9bbac32
author: rothja
ms.author: jroth
ms.openlocfilehash: fd88799bce8f3b8a0842c9f2d2752eac062dc4b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037186"
---
# <a name="preparing-and-executing-commands"></a>Preparar y ejecutar comandos
Los comandos son instrucciones emitidas a un proveedor para realizar algunas operaciones en el origen de datos subyacente. Una instrucción SQL, por ejemplo, es un comando para el proveedor de datos de Microsoft SQL. En ADO, los comandos se representan normalmente mediante objetos de **comando** , aunque los comandos simples también se pueden emitir a través de objetos de **conjunto de registros** o de **conexión** .  
  
 Puede usar el objeto de **comando** para solicitar cualquier tipo de operación compatible desde el proveedor, suponiendo que el proveedor pueda interpretar correctamente la cadena de comandos. Una operación común para los proveedores de datos es consultar una base de datos y devolver registros en un objeto de **conjunto de registros** , que se puede considerar como un contenedor para contener el resultado y una herramienta para ver el resultado. Como con muchos objetos de ADO, algunas colecciones de objetos de **comando** , métodos o propiedades pueden generar errores cuando se hace referencia a ellos, dependiendo de la funcionalidad del proveedor.  
  
 Además de utilizar objetos de **comando** , puede utilizar el método **Execute** en el objeto **Connection** o el método **Open** en el objeto **Recordset** para emitir un comando y ejecutarlo. Sin embargo, debe usar un objeto de **comando** si necesita volver a usar un comando en el código o si necesita pasar información detallada de los parámetros con el comando. Estos escenarios se describen con más detalle más adelante en esta sección.  
  
> [!NOTE]
>  Algunos **comandos** pueden devolver un conjunto de resultados como una secuencia binaria o como un único **registro** en lugar de como un **conjunto de registros**, si es compatible con el proveedor. Además, algunos **comandos** no están diseñados para devolver ningún conjunto de resultados (por ejemplo, una consulta de actualización de SQL). En esta sección se tratará el escenario más típico, sin embargo: ejecutar los **comandos** que devuelven los resultados como un objeto de **conjunto de registros** . Para obtener más información sobre cómo devolver los **resultados en registros** s o **Stream** s, consulte [registros y secuencias](../../../ado/guide/data/records-and-streams.md).  
  
 Esta sección contiene los temas siguientes.  
  
-   [Información general sobre objetos de comando](../../../ado/guide/data/command-object-overview.md)  
  
-   [Crear y ejecutar un comando Simple](../../../ado/guide/data/creating-and-executing-a-simple-command.md)  
  
-   [Parámetros del objeto Command](../../../ado/guide/data/command-object-parameters.md)  
  
-   [Llamar a un procedimiento almacenado con un comando](../../../ado/guide/data/calling-a-stored-procedure-with-a-command.md)  
  
-   [Llamar a un procedimiento almacenado como un método en un objeto Connection](../../../ado/guide/data/calling-a-stored-procedure-as-a-method-on-a-connection-object.md)  
  
-   [Comandos con nombre](../../../ado/guide/data/named-commands.md)  
  
-   [Pasar parámetros a un comando con nombre](../../../ado/guide/data/passing-parameters-to-a-named-command.md)
