---
description: Saltar a un registro
title: Saltar a un registro | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- record jumping [ADO]
- jumping to record [ADO]
ms.assetid: 6caf6299-2eea-4d34-9b0e-b75aab07b740
author: rothja
ms.author: jroth
ms.openlocfilehash: 1a8a34ac8cafffe9f22595a76c5389369290a2b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032727"
---
# <a name="jumping-to-a-record"></a>Saltar a un registro
El método [Move](../../reference/ado-api/move-method-ado.md) permite desplazarse hacia delante o hacia atrás en el **conjunto** de registros un número especificado de registros utilizando la sintaxis siguiente:  
  
```  
oRs.Move NumRecords, Start  
```  
  
## <a name="remarks"></a>Observaciones  
 El método **Move** es compatible con todos los objetos de **conjunto de registros** .  
  
 Si el argumento *NumRecords* es mayor que cero, la posición del registro actual se desplaza hacia delante (hacia el final del **conjunto de registros**). Si *NumRecords* es menor que cero, la posición del registro actual se desplaza hacia atrás (hacia el principio del **conjunto de registros**).  
  
 Si la llamada a **Move** movera la posición del registro actual a un punto anterior al primer registro, ADO establece el registro actual en la posición anterior al primer registro del **conjunto de registros** (**BOF** es **true**). Si se intenta retroceder cuando la propiedad **BOF** ya es **true** , se genera un error.  
  
 Si la llamada a **Move** movera la posición del registro actual a un punto después del último registro, ADO establece el registro actual en la posición posterior al último registro del **conjunto de registros** (**EOF** es **true**). Un intento de avanzar cuando la propiedad **EOF** ya es **true** genera un error.  
  
 Al llamar al método **Move** desde un objeto **Recordset** vacío, se genera un error.  
  
 Si pasa un marcador en el argumento *Start* , el movimiento es relativo al registro con este marcador, suponiendo que el objeto de **conjunto de registros** admita marcadores. Un marcador se obtiene mediante la propiedad [Bookmark](../../reference/ado-api/bookmark-property-ado.md) . Si no se especifica, el movimiento es relativo al registro actual.  
  
 Si está utilizando la propiedad **CacheSize** para almacenar en caché localmente los registros del proveedor, al pasar un argumento *NumRecords* que mueve la posición del registro actual fuera del grupo actual de registros almacenados en caché, ADO recupera un nuevo grupo de registros, empezando por el registro de destino. La propiedad **CacheSize** determina el tamaño del grupo recién recuperado y el registro de destino es el primer registro recuperado.