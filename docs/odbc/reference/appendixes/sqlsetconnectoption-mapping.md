---
description: Asignación de SQLSetConnectOption
title: Asignación de SQLSetConnectOption | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetConnectOption function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLSetConnectOption
ms.assetid: a1b325cf-0c42-41c1-b141-b5a4fee7e708
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 919220fed87878bf0bcf23359f0714f702f9a6a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202657"
---
# <a name="sqlsetconnectoption-mapping"></a>Asignación de SQLSetConnectOption
Cuando se trata de un ODBC 2. la aplicación *x* llama a **SQLSetConnectOption** a través de un controlador ODBC 3 *. x* , la llamada a  
  
```  
SQLSetConnectOption(hdbc, fOption, vParam)  
```  
  
 el resultado será el siguiente:  
  
-   Si *fOption* indica un atributo de conexión definido por ODBC que requiere una cadena, el administrador de controladores llama a  
  
    ```  
    SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, SQL_NTS)  
    ```  
  
-   Si *fOption* indica un atributo de conexión definido por ODBC que devuelve un valor entero de 32 bits, el administrador de controladores llama a  
  
    ```  
    SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, 0)  
    ```  
  
-   Si *fOption* indica un atributo de conexión definido por el controlador, el administrador de controladores llama a  
  
    ```  
    SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, BufferLength)  
    ```  
  
 En los tres casos anteriores, el argumento *ConnectionHandle* se establece en el valor de *hdbc*, el argumento del *atributo* se establece en el valor de *fOption* y el argumento *ValuePtr* se establece en el mismo valor que *vParam*.  
  
 Dado que el administrador de controladores no sabe si el atributo de conexión definido por el controlador necesita una cadena o un valor entero de 32 bits, tiene que pasar un valor válido para el argumento *BufferLength* de **SQLSetConnectAttr**. Si el controlador tiene una semántica especial definida para los atributos de conexión definidos por el controlador y debe llamarse mediante **SQLSetConnectOption**, debe admitir **SQLSetConnectOption**.  
  
 Si ODBC 2. la aplicación *x* llama a **SQLSetConnectOption** para establecer una opción de instrucción específica del controlador en un controlador ODBC 3 *. x* y la opción se definió en ODBC 2. *x* versión del controlador, se debe definir una nueva constante de manifiesto para la opción en el controlador ODBC 3 *. x* . Si se usa la constante de manifiesto antigua en la llamada a **SQLSetConnectOption**, el administrador de controladores llamará a **SQLSetConnectAttr** con el argumento **StringLength** establecido en 0.  
  
 Para un controlador ODBC 3 *. x* , el administrador de controladores ya no comprueba si *fOption* se encuentra entre SQL_CONN_OPT_MIN y SQL_CONN_OPT_MAX, o es mayor que SQL_CONNECT_OPT_DRVR_START.  
  
## <a name="setting-statement-options-on-the-connection-level"></a>Establecer opciones de instrucción en el nivel de conexión  
 En ODBC 2. *x*, una aplicación podría llamar a **SQLSetConnectOption** para establecer una opción de instrucción. Cuando esto se hace, el controlador establece la opción de instrucción como un valor predeterminado para cualquier instrucción que se asigne posteriormente para esa conexión. Está definido por el controlador si el controlador establece la opción de instrucción para las instrucciones existentes asociadas a la conexión especificada.  
  
 Esta capacidad está en desuso en ODBC 3 *. x*. Los controladores ODBC 3 *. x* solo necesitan admitir la configuración de ODBC 2. atributos de la instrucción *x* en el nivel de conexión si desean trabajar con ODBC 2. aplicaciones *x* que lo hacen. Las aplicaciones ODBC 3 *. x* nunca deben establecer atributos de instrucción en el nivel de conexión. Los atributos de la instrucción ODBC 3 *. x* no se pueden establecer en el nivel de conexión, a excepción de los atributos SQL_ATTR_METADATA_ID y SQL_ATTR_ASYNC_ENABLE, que son atributos de conexión y de instrucción, y se pueden establecer en el nivel de conexión o en el nivel de instrucción.
