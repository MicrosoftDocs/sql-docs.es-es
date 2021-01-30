---
description: Lo que hace el controlador
title: Lo que hace el controlador | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- scrollable cursors [ODBC]
- cursors [ODBC], backward compatibility
- compatibility [ODBC], cursors
- backward compatibility [ODBC], cursors
- block cursors [ODBC]
ms.assetid: 75dcdea6-ff6b-4ac8-aa11-a1f9edbeb8e6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 37c6c84b74e6b89dc9b31a342e9c47c8a63eb030
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158173"
---
# <a name="what-the-driver-does"></a>Lo que hace el controlador
En la tabla siguiente se resumen las funciones y los atributos de instrucción que debe implementar un controlador ODBC *3. x* para los cursores de bloque y desplazables.  
  
|Función o<br /><br /> atributo de instrucción|Comentarios|  
|-----------------------------------------|--------------|  
|SQL_ATTR_ROW_STATUS_PTR|Establece la dirección de la matriz de estado de fila rellena por **SQLFetch** y **SQLFetchScroll**. La matriz también se rellena con **SQLSetPos** si se llama a **SQLSetPos** en el estado de instrucción S6. Si se llama a **SQLSetPos** en el estado S7, esta matriz no se rellena, pero se rellena la matriz a la que apunta el argumento *RowStatusArray* de **SQLExtendedFetch** . Para obtener más información, vea [transiciones de instrucciones](../../../odbc/reference/appendixes/statement-transitions.md) en el Apéndice B: tablas de transición de estado de ODBC.|  
|SQL_ATTR_ROWS_FETCHED_PTR|Establece la dirección del búfer en el que **SQLFetch** y **SQLFetchScroll** devuelven el número de filas recuperadas. Si se llama a **SQLExtendedFetch** , no se rellena este búfer, pero el argumento *RowCountPtr* apunta al número de filas recuperadas.|  
|SQL_ATTR_ROW_ARRAY_SIZE|Establece el tamaño del conjunto de filas usado por **SQLFetch** y **SQLFetchScroll**.|  
|SQL_ROWSET_SIZE|Establece el tamaño del conjunto de filas usado por **SQLExtendedFetch**. Los controladores ODBC *3. x* implementan esto si desean trabajar con aplicaciones ODBC *2. x* que llaman a **SQLExtendedFetch** o **SQLSetPos**.|  
|**SQLBulkOperations**|Si un controlador *ODBC 3. x* debe trabajar con aplicaciones ODBC *2. x* que usan **sqlsetpos** con una *operación* de SQL_ADD, el controlador debe admitir **SQLSetPos** con una *operación* de SQL_ADD además de **SQLBulkOperations** con una *operación* de SQL_ADD.|  
|**SQLExtendedFetch**|Devuelve el conjunto de filas especificado. Los controladores ODBC *3. x* implementan esto si desean trabajar con aplicaciones ODBC *2. x* que llaman a **SQLExtendedFetch** o **SQLSetPos**. Estos son los detalles de la implementación:<br /><br /> -El controlador recupera el tamaño del conjunto de filas a partir del valor del atributo de instrucción SQL_ROWSET_SIZE.<br />-El controlador recupera la dirección de la matriz de estado de fila del argumento *RowStatusArray* , no del atributo de instrucción SQL_ATTR_ROW_STATUS_PTR. El argumento *RowStatusArray* en una llamada a **SQLExtendedFetch** no debe ser un puntero nulo. (Tenga en cuenta que en ODBC *3. x*, el atributo de instrucción SQL_ATTR_ROW_STATUS_PTR puede ser un puntero nulo).<br />-El controlador recupera la dirección del búfer de filas capturadas del argumento *RowCountPtr* , no del atributo de instrucción SQL_ATTR_ROWS_FETCHED_PTR.<br />-El controlador devuelve SQLSTATE 01S01 (error en la fila) para indicar que se ha producido un error mientras una llamada a **SQLExtendedFetch** ha capturado filas. Un controlador ODBC *3. x* debe devolver SQLSTATE 01S01 (error in row) solo cuando se llama a **SQLExtendedFetch** , no cuando se llama a **SQLFetch** o **SQLFetchScroll** . Para mantener la compatibilidad con versiones anteriores, cuando **SQLExtendedFetch** devuelve SQLSTATE 01S01 (error en la fila), el administrador de controladores no ordena los registros de estado en la cola de errores de acuerdo con las reglas descritas en la sección "secuencia de registros de estado" de [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).|  
|**SQLFetch**|Devuelve el conjunto de filas siguiente. Estos son los detalles de la implementación:<br /><br /> -El controlador recupera el tamaño del conjunto de filas a partir del valor del atributo de instrucción SQL_ATTR_ROW_ARRAY_SIZE.<br />-El controlador recupera la dirección de la matriz de estado de fila del atributo de instrucción SQL_ATTR_ROW_STATUS_PTR.<br />-El controlador recupera la dirección del búfer de filas capturadas del atributo de instrucción SQL_ATTR_ROWS_FETCHED_PTR.<br />-La aplicación puede mezclar llamadas entre **SQLFetchScroll** y **SQLFetch**.<br />-   **SQLFetch** devuelve marcadores si la columna 0 está enlazada.<br />-   Se puede llamar a **SQLFetch** para devolver más de una fila.<br />-El controlador no devuelve SQLSTATE 01S01 (error en la fila) para indicar que se ha producido un error mientras una llamada a **SQLFetch** ha capturado filas.|  
|**SQLFetchScroll**|Devuelve el conjunto de filas especificado. Estos son los detalles de la implementación:<br /><br /> -El controlador recupera el tamaño del conjunto de filas del atributo de instrucción SQL_ATTR_ROW_ARRAY_SIZE.<br />-El controlador recupera la dirección de la matriz de estado de fila del atributo de instrucción SQL_ATTR_ROW_STATUS_PTR.<br />-El controlador recupera la dirección del búfer de filas capturadas del atributo de instrucción SQL_ATTR_ROWS_FETCHED_PTR.<br />-La aplicación puede mezclar llamadas entre **SQLFetchScroll** y **SQLFetch**.<br />-El controlador no devuelve SQLSTATE 01S01 (error en la fila) para indicar que se ha producido un error mientras una llamada a **SQLFetchScroll** ha capturado filas.|  
|**SQLSetPos**|Realiza varias operaciones de posición. Estos son los detalles de la implementación:<br /><br /> -Esto se puede llamar en los Estados de instrucción S6 o S7. Para obtener más información, vea [transiciones de instrucciones](../../../odbc/reference/appendixes/statement-transitions.md) en el Apéndice B: tablas de transición de estado de ODBC.<br />-Si se llama a esta en el estado de instrucción S5 o S6, el controlador recupera el tamaño del conjunto de filas del atributo de instrucción SQL_ATTR_ROWS_FETCHED_PTR y la dirección de la matriz de estado de fila del atributo de instrucción SQL_ATTR_ROW_STATUS_PTR.<br />-Si se llama a este método en el estado de instrucción S7, el controlador recupera el tamaño del conjunto de filas del atributo de instrucción SQL_ROWSET_SIZE y la dirección de la matriz de estado de fila del argumento *RowStatusArray* de **SQLExtendedFetch**.<br />-El controlador devuelve SQLSTATE 01S01 (error in row) para indicar que se ha producido un error mientras se capturaban filas mediante una llamada a **SQLSetPos** para realizar una operación masiva cuando se llama a la función en el estado S7. Para mantener la compatibilidad con versiones anteriores, si **SQLSetPos** devuelve SQLSTATE 01S01 (error en la fila), el administrador de controladores no ordena los registros de estado en la cola de errores de acuerdo con las reglas descritas en la sección "secuencia de registros de estado" de [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).<br />-Si el controlador debe funcionar con aplicaciones ODBC *2. x* que llaman a **SQLSetPos** con un argumento de *operación* de SQL_ADD, el controlador debe admitir **SQLSetPos** con un argumento de *operación* de SQL_ADD.|
