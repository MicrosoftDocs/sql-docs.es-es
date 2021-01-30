---
description: Asignación de SQLGetConnectOption
title: Asignación de SQLGetConnectOption | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLGetConnectOption
- SQLGetConnectOption function [ODBC], mapping
ms.assetid: e3792fe4-a955-473a-a297-c1b2403660c4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cdc5f2e9249e262d0d3da9a69607861bfe64bc25
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202794"
---
# <a name="sqlgetconnectoption-mapping"></a>Asignación de SQLGetConnectOption
Cuando una aplicación llama a **SQLGetConnectOption** a través de un controlador ODBC *3. x* , la llamada a  
  
```  
SQLGetConnectOption(hdbc, fOption, pvParam)   
```  
  
 se asigna de la siguiente manera:  
  
-   Si *fOption* indica una opción de conexión definida por ODBC que devuelve una cadena, el administrador de controladores llama a  
  
    ```  
    SQLGetConnectAttr(ConnectionHandle, Attribute, ValuePtr, BufferLength, NULL)  
    ```  
  
-   Si *fOption* indica una opción de conexión definida por ODBC que devuelve un valor entero de 32 bits, el administrador de controladores llama a  
  
    ```  
    SQLGetConnectAttr(ConnectionHandle, Attribute, ValuePtr, 0, NULL)  
    ```  
  
-   Si *fOption* indica una opción de instrucción definida por el controlador, el administrador de controladores llama a  
  
    ```  
    SQLGetConnectAttr(ConnectionHandle, Attribute, ValuePtr, BufferLength, NULL)  
    ```  
  
 En los tres casos anteriores, el argumento *ConnectionHandle* se establece en el valor de *hdbc*, el argumento del *atributo* se establece en el valor de *fOption* y el argumento *ValuePtr* se establece en el mismo valor que *pvParam*.  
  
 En el caso de las opciones de conexión de cadenas definidas por ODBC, el administrador de controladores establece el argumento *BufferLength* en la llamada a **SQLGetConnectAttr** en la longitud máxima predefinida (SQL_MAX_OPTION_STRING_LENGTH); en el caso de una opción de conexión que no sea de cadena, *BufferLength* se establece en 0.  
  
 Para un controlador ODBC *3. x* , el administrador de controladores ya no comprueba si la *opción* está entre SQL_CONN_OPT_MIN y SQL_CONN_OPT_MAX, o es mayor que SQL_CONNECT_OPT_DRVR_START. El controlador debe comprobar la validez de los valores de opción.
