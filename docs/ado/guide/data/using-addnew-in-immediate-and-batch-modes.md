---
title: Usar AddNew en los modos inmediato y por lotes | Microsoft Docs
description: Explica cómo usar AddNew en los modos inmediato y por lotes.
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- AddNew method [ADO]
- ADO, editing data
- ADO, adding data
- editing data [ADO], AddNew method
ms.assetid: ed314bb9-e188-4658-a68c-a2abc49610be
author: rothja
ms.author: jroth
ms.openlocfilehash: b68d4870c7c256495bb16b7bfba7176d60445ea7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036795"
---
# <a name="using-addnew-in-immediate-and-batch-modes"></a>Uso de AddNew de inmediato y modos de lote
El comportamiento del método **AddNew** depende del modo de actualización del objeto de **conjunto de registros** y de si se pasan los argumentos *FieldList* y *Values* .  
  
 En el modo de actualización inmediata (en el que el proveedor escribe los cambios en el origen de datos subyacente una vez que se llama al método **Update** ), al llamar al método **AddNew** sin argumentos se establece la propiedad **EditMode** en **adEditAdd.** El proveedor almacena en caché los cambios de los valores de campo de forma local. La llamada al método **Update** expone el nuevo registro en la base de datos y restablece la propiedad **EditMode** en **adEditNone.** Si se pasan los argumentos *FieldList* y *Values* , ADO envía inmediatamente el nuevo registro a la base de datos (no se necesita ninguna llamada de **actualización** ); el valor de la propiedad **EditMode** no cambia (**adEditNone**).  
  
 En el modo de actualización por lotes, al llamar al método **AddNew** sin argumentos, se establece la propiedad **EditMode** en **adEditAdd**. El proveedor almacena en caché los cambios de los valores de campo de forma local. Al llamar al método **Update** se agrega el nuevo registro al **conjunto de registros** actual y se restablece la propiedad **EditMode** en **adEditNone**, pero el proveedor no publica los cambios en la base de datos subyacente hasta que se llama al método **UpdateBatch** . Si se pasan los argumentos *FieldList* y *Values* , ADO envía el nuevo registro al proveedor para su almacenamiento en una memoria caché; debe llamar al método **UpdateBatch** para publicar el nuevo registro en la base de datos subyacente. Para obtener más información acerca de **Update** y **UpdateBatch**, vea [actualizar y conservar datos](../../../ado/guide/data/updating-and-persisting-data.md).
