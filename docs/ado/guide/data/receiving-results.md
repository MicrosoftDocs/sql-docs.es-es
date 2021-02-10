---
description: Recibir los resultados
title: Recibiendo resultados | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- receiving results [ADO]
- Recordset object [ADO], receiving results
ms.assetid: 791aa26e-7aae-477e-9f05-5cd46e1de095
author: rothja
ms.author: jroth
ms.openlocfilehash: 1cdcf6acb2eb9de033f86b9f34739a55dc0dc021
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032587"
---
# <a name="receiving-results"></a>Recibir los resultados
En la mayoría de los comandos de ADO se devuelve cierta información al autor de la llamada. En el caso de los comandos que devuelven conjuntos de filas, los resultados se reciben en un objeto de **conjunto de registros** , que es probablemente el más usado de los objetos ADO.  
  
 Hay varias maneras de recibir datos en un objeto de **conjunto de registros** de un origen de datos, como llamar a lo siguiente:  
  
-   [Connection.Exe(método)](../../../ado/guide/data/creating-and-executing-a-simple-command.md)  
  
-   [Command.Exe(método)](../../../ado/guide/data/creating-and-executing-a-simple-command.md)  
  
-   [Recordset. Open (método)](../../../ado/guide/data/creating-and-executing-a-simple-command.md)  
  
-   [Conexión. NamedCommand](../../../ado/guide/data/named-commands.md)  
  
-   [Conexión. StoredProcedure](../../../ado/guide/data/calling-a-stored-procedure-as-a-method-on-a-connection-object.md)  
  
 La recepción de datos en un objeto de **conjunto de registros** concluye el proceso de obtención de datos, con la participación de un objeto de **conexión** y un objeto de **comando** , implícita o explícitamente. En un sistema de aplicaciones cliente/servidor típico, todo el proceso de obtención de datos requiere un recorrido de ida y vuelta por la red para cada **conjunto de registros** rellenado.  
  
 Para recibir más de un conjunto de resultados, es necesario realizar varios viajes de ida y vuelta a través de la red, uno para cada conjunto de datos encapsulado en un objeto de **conjunto de registros** . En el caso de redes lentas o congestionadas, reducir el número de viajes de ida y vuelta puede ayudar a mejorar los rendimientos de la aplicación. Por lo tanto, algunos proveedores ofrecen compatibilidad para recibir varios **conjuntos de registros** en un solo recorrido de ida y vuelta. Esto se describe en el tema siguiente:  
  
-   [Recibir varios conjuntos de registros](../../../ado/guide/data/receiving-multiple-recordsets.md)
