---
description: Utilizar marcadores
title: Usar marcadores | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- bookmarks [ADO]
- Recordset object [ADO]
ms.assetid: cca244e6-84f8-4394-bca9-f7a819b8f4df
author: rothja
ms.author: jroth
ms.openlocfilehash: f07ad3e4f5dc69da31a74af96842b1c8aab97eda
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036775"
---
# <a name="using-bookmarks"></a>Utilizar marcadores
A menudo resulta útil volver directamente a un registro específico después de moverse por el conjunto de **registros** sin tener que desplazarse por cada registro y comparar los valores. Por ejemplo, si intenta buscar un registro con el método **Find** pero la búsqueda no devuelve ningún registro, se coloca automáticamente en cualquier extremo del **conjunto de registros**. Si el proveedor lo admite, se pueden usar marcadores para marcar su lugar antes de usar el método **Buscar** para que pueda volver a su ubicación. Un marcador es un valor de tipo **Variant** que identifica de forma única un registro en un objeto de **conjunto de registros** .  
  
 También puede utilizar una matriz de marcadores variante con el método de **filtro de conjunto de registros** para filtrar por un conjunto de registros seleccionado. Para obtener más información sobre esta técnica, vea filtrar los resultados en el tema [trabajar con conjuntos de registros](../../../ado/guide/data/working-with-recordsets.md), más adelante en esta sección.  
  
 Puede usar la propiedad **Bookmark** para obtener un marcador para un registro o establecer el registro actual de un objeto de **conjunto de registros** en el registro identificado por un marcador válido. En el código siguiente se usa la propiedad **Bookmark** para establecer un marcador y, después, volver al registro marcado después de pasar a otros registros. Para determinar si el **conjunto de registros** admite marcadores, use el método **Supports** .  
  
```  
'BeginBookmarkEg  
Dim varBookmark As Variant  
Dim blnCanBkmrk As Boolean  
  
objRs.Open strSQL, strConnStr, adOpenStatic, adLockOptimistic, adCmdText  
  
If objRs.RecordCount > 4 Then  
    objRs.Move 4                       ' move to the fifth record  
    blnCanBkmrk = objRs.Supports(adBookmark)  
    If blnCanBkmrk = True Then  
        varBookmark = objRs.Bookmark   ' record the bookmark  
        objRs.MoveLast                 ' move to a different record  
        objRs.Bookmark = varBookmark   ' return to the bookmarked (sixth) record  
    End If  
End If  
'EndBookmarkEg  
```  
  
 El método [Supports](../../../ado/reference/ado-api/supports-method.md) se trata con más detalle más adelante.  
  
 Excepto en el caso de los **conjuntos de registros** clonados, los marcadores son únicos en el **conjunto de registros** en el que se crearon, aunque se use el mismo comando. Esto significa que no puede usar un **marcador** Obtenido de un **conjunto de registros** para desplace al mismo registro en un segundo **conjunto de registros** abierto con el mismo comando.
