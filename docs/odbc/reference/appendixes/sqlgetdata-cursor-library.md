---
description: SQLGetData (biblioteca de cursores)
title: SQLGetData (biblioteca de cursores) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLGetData function [ODBC], Cursor Library
ms.assetid: ff40c9c0-b847-4426-a099-1bff47e6e872
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1e0cb6a73fb294e8b47ab82c988f8d07a4a220df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202787"
---
# <a name="sqlgetdata-cursor-library"></a>SQLGetData (biblioteca de cursores)
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 En este tema se describe el uso de la función **SQLGetData** en la biblioteca de cursores. Para obtener información general acerca de **SQLGetData**, vea [función SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md).  
  
 La biblioteca de cursores implementa **SQLGetData** creando primero una instrucción **Select** con una cláusula **Where** que enumera los valores almacenados en su caché para cada columna enlazada en la fila actual. A continuación, ejecuta la instrucción **Select** para volver a seleccionar la fila y llama a **SQLGetData** en el controlador para recuperar los datos del origen de datos (en lugar de la memoria caché).  
  
> [!CAUTION]  
>  La cláusula **Where** construida por la biblioteca de cursores para identificar la fila actual puede no identificar filas, identificar una fila diferente o identificar más de una fila. Para obtener más información, vea [crear instrucciones de búsqueda](../../../odbc/reference/appendixes/constructing-searched-statements.md).  
  
 Si el atributo de instrucción SQL_ATTR_USE_BOOKMARKS está establecido en SQL_UB_VARIABLE, se puede llamar a **SQLGetData** en la columna 0 para devolver los datos del marcador.  
  
 Las llamadas a **SQLGetData** están sujetas a las siguientes restricciones:  
  
-   No se puede llamar a **SQLGetData** para cursores de solo avance.  
  
-   Solo se puede llamar a **SQLGetData** cuando se cumplen las condiciones siguientes: una instrucción **Select** genera el conjunto de resultados. la instrucción **Select** no contenía una cláusula join, **Union** o **Group by** ; y cualquier columna que utilizara un alias o una expresión en la lista de selección no estaba enlazada con **SQLBindCol**.  
  
-   Si el controlador solo admite una instrucción activa, la biblioteca de cursores captura el resto del conjunto de resultados antes de ejecutar la instrucción **Select** y llamar a **SQLGetData**.
