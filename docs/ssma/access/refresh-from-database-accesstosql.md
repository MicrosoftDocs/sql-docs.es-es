---
description: Actualizar desde la base de datos (AccessToSQL)
title: Actualizar desde la base de datos (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 3b671f49-c4cc-44fd-801e-e738a8c79415
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: df61abefdab6642c098cbf456cd6147417cb2dd7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066310"
---
# <a name="refresh-from-database-accesstosql"></a>Actualizar desde la base de datos (AccessToSQL)
El cuadro de diálogo **actualizar desde base de datos** permite seleccionar los objetos que se van a actualizar desde la base de datos de Access. Las filas del cuadro de diálogo están codificadas por colores en función del estado de los metadatos:  
  
-   Si los metadatos del objeto han cambiado localmente y en la base de datos de Access, la fila es azul.  
  
-   Si los metadatos del objeto han cambiado en la base de datos de Access pero no en SSMA, la fila es amarilla.  
  
-   Si los metadatos del objeto han cambiado localmente, pero no en la base de datos de Access, la fila es verde.  
  
-   Si el objeto es nuevo en la base de datos de Access, la fila es de color rosa.  
  
Puede especificar la configuración de actualización de objetos predeterminada en el cuadro de diálogo **configuración del proyecto** . Para obtener más información, vea [configuración del proyecto &#40;cargar objetos&#41; &#40;AccessToSQL&#41;](../../ssma/access/project-settings-loading-objects-accesstosql.md)  
  
Para tener acceso al cuadro **de diálogo actualizar desde base** de datos, haga clic con el botón secundario en cualquier nodo de **base** de datos en el explorador de metadatos y haga clic en **actualizar desde base de** datos  
  
