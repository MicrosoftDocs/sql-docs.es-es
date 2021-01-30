---
description: Asignación de SQLSetScrollOptions
title: Asignación de SQLSetScrollOptions | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetScrollOptions function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLSetScrollOptions
ms.assetid: a0fa4510-8891-4a61-a867-b2555bc35f05
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 105443a2b33a1903f1f22affef1c6ca70ee2cfdf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202597"
---
# <a name="sqlsetscrolloptions-mapping"></a>Asignación de SQLSetScrollOptions
Cuando una aplicación llama a **SQLSetScrollOptions** a través de un controlador ODBC *3. x* y el controlador no admite **SQLSetScrollOptions**, la llamada a  
  
```  
SQLSetScrollOptions(StatementHandle, Concurrency, KeysetSize, RowsetSize)  
```  
  
 el resultado será el siguiente:  
  
-   Una llamada a  
  
    ```  
    SQLGetInfo(ConnectionHandle, InfoType, InfoValuePtr, BufferLength, StringLengthPtr)  
    ```  
  
     con el argumento *InfoType* establecido en uno de los valores de la tabla siguiente, dependiendo del valor del argumento *KeysetSize* en **SQLSetScrollOptions**.  
  
    |*Argumento KeysetSize*|*Argumento InfoType*|  
    |---------------------------|-------------------------|  
    |SQL_SCROLL_FORWARD_ONLY|SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES2|  
    |SQL_SCROLL_STATIC|SQL_STATIC_CURSOR_ATTRIBUTES2|  
    |SQL_SCROLL_KEYSET_DRIVEN|SQL_KEYSET_CURSOR_ATTRIBUTES2|  
    |SQL_SCROLL_DYNAMIC|SQL_DYNAMIC_CURSOR_ATTRIBUTES2|  
    |Un valor mayor que el argumento *RowsetSize*|SQL_KEYSET_CURSOR_ATTRIBUTES2|  
  
     Si el valor del argumento *KeysetSize* no aparece en la tabla anterior, la llamada a **SQLSetScrollOptions** devuelve SQLSTATE S1107 (valor de fila fuera del intervalo) y no se realiza ninguno de los pasos siguientes.  
  
     A continuación, el administrador de controladores comprueba si se ha establecido el bit adecuado en el valor **InfoValuePtr* devuelto por la llamada a **SQLGetInfo**, según el valor del argumento de *simultaneidad* en **SQLSetScrollOptions**.  
  
    |Argumento de *simultaneidad*|Configuración de *InfoType*|  
    |----------------------------|------------------------|  
    |SQL_CONCUR_READ_ONLY|SQL_CA2_READ_ONLY_CONCURRENCY|  
    |SQL_CONCUR_LOCK|SQL_CA2_LOCK_CONCURRENCY|  
    |SQL_CONCUR_ROWVER|SQL_CA2_ROWVER_CONCURRENCY|  
    |SQL_CONCUR_VALUES|SQL_CA2_VALUES_CONCURRENCY|  
  
     Si el argumento de *simultaneidad* no es ninguno de los valores de la tabla anterior, la llamada a **SQLSetScrollOptions** devuelve SQLSTATE S1108 (opción de simultaneidad fuera del intervalo) y no se realiza ninguno de los pasos siguientes. Si el bit adecuado (como se indica en la tabla anterior) no se establece en **InfoValuePtr* en uno de los valores correspondientes al argumento de *simultaneidad* , la llamada a **SQLSetScrollOptions** devuelve SQLSTATE S1C00 (controlador no compatible) y no se realiza ninguno de los pasos siguientes.  
  
-   Una llamada a  
  
    ```  
    SQLSetStmtAttr(StatementHandle, SQL_ATTR_CURSOR_TYPE, ValuePtr, 0)  
    ```  
  
     con *\* ValuePtr* establecido en uno de los valores de la tabla siguiente, según el valor del argumento *KeysetSize* en **SQLSetScrollOptions**.  
  
    |Argumento *KeysetSize*|*\*ValuePtr*|  
    |---------------------------|------------------|  
    |SQL_SCROLL_FORWARD_ONLY|SQL_CURSOR_FORWARD_ONLY|  
    |SQL_SCROLL_STATIC|SQL_CURSOR_STATIC|  
    |SQL_SCROLL_KEYSET_DRIVEN|SQL_CURSOR_KEYSET_DRIVEN|  
    |SQL_SCROLL_DYNAMIC|SQL_CURSOR_DYNAMIC|  
    |Un valor mayor que el argumento *RowsetSize*|SQL_CURSOR_KEYSET_DRIVEN|  
  
-   Una llamada a  
  
    ```  
    SQLSetStmtAttr(StatementHandle, SQL_ATTR_CONCURRENCY, ValuePtr, 0)  
    ```  
  
     con *\* ValuePtr* establecido en el argumento de *simultaneidad* en **SQLSetScrollOptions**.  
  
-   Si el argumento *KeysetSize* de la llamada a **SQLSetScrollOptions** es positivo, una llamada a  
  
    ```  
    SQLSetStmtAttr(StatementHandle, SQL_ATTR_KEYSET_SIZE, ValuePtr, 0)  
    ```  
  
     con *\* ValuePtr* establecido en el argumento *KeysetSize* en **SQLSetScrollOptions**.  
  
-   Una llamada a  
  
    ```  
    SQLSetStmtAttr(StatementHandle, SQL_ROWSET_SIZE, ValuePtr, 0)  
    ```  
  
     con *\* ValuePtr* establecido en el argumento *RowsetSize* en **SQLSetScrollOptions**.  
  
    > [!NOTE]  
    >  Cuando el administrador de controladores asigna **SQLSetScrollOptions** para una aplicación que trabaja con un controlador ODBC *3. x* que no admite **SQLSetScrollOptions**, el administrador de controladores establece la opción de instrucción SQL_ROWSET_SIZE, no el atributo de instrucción SQL_ATTR_ROW_ARRAY_SIZE, en el argumento *RowsetSize* de **SQLSetScrollOption**. Como resultado, una aplicación no puede usar **SQLSetScrollOptions** al capturar varias filas mediante una llamada a **SQLFetch** o **SQLFetchScroll**. Solo se puede usar al capturar varias filas mediante una llamada a **SQLExtendedFetch**.
