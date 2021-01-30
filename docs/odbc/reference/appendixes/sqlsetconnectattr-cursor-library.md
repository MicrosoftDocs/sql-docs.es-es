---
description: SQLSetConnectAttr (biblioteca de cursores)
title: SQLSetConnectAttr (biblioteca de cursores) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetConnectAttr function [ODBC], Cursor Library
ms.assetid: 6f70bbd0-a057-49ef-8b05-4c80b58fc6e6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 02f702991a4e03816f3e4a30a233d098198bfefe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202665"
---
# <a name="sqlsetconnectattr-cursor-library"></a>SQLSetConnectAttr (biblioteca de cursores)
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 En este tema se describe el uso de la función **SQLSetConnectAttr** en la biblioteca de cursores. Para obtener información general sobre **SQLSetConnectAttr**, consulte [SQLSetConnectAttr (función](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)).  
  
 Una aplicación llama a **SQLSetConnectAttr** con el atributo SQL_ATTR_ODBC_CURSORS para especificar si se utiliza siempre la biblioteca de cursores, que se usa si el controlador no admite cursores desplazables, o si no se usa nunca. La biblioteca de cursores supone que un controlador admite cursores desplazables si devuelve SQL_CA1_RELATIVE para el tipo de información de SQL_STATIC_CURSOR_ATTRIBUTES1 en **SQLGetInfo**.  
  
 La aplicación debe llamar a **SQLSetConnectAttr** para especificar el uso de la biblioteca de cursores después de llamar a **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_DBC para asignar la conexión y antes de que se conecte al origen de datos. Si una aplicación llama a **SQLSetConnectAttr** con el SQL_ATTR_ODBC_CURSORS atributo mientras la conexión sigue activa, la biblioteca de cursores devuelve un error.  
  
 Para establecer un atributo de instrucción admitido por la biblioteca de cursores para todas las instrucciones asociadas a una conexión, una aplicación debe llamar a **SQLSetConnectAttr** para ese atributo de instrucción una vez que se conecta al origen de datos y antes de abrir el cursor. Si una aplicación llama a **SQLSetConnectAttr** con un atributo de instrucción y se abre un cursor en una instrucción asociada a la conexión, el atributo de instrucción no se aplicará a esa instrucción hasta que el cursor se cierre y se vuelva a abrir.
