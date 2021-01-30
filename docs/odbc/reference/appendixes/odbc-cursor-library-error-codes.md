---
description: Códigos de error de la biblioteca de cursores ODBC
title: Códigos de error de la biblioteca de cursores ODBC | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- cursor library [ODBC], error codes
- error codes [ODBC], cursor library
- ODBC cursor library [ODBC], error codes
ms.assetid: 9713480e-8744-4f37-a630-20871590d4a1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6c9a4d7a68ddaf9f569a04acd8be81b993a636b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193255"
---
# <a name="odbc-cursor-library-error-codes"></a>Códigos de error de la biblioteca de cursores ODBC
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura del componente de Microsoft Data Access. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. En su lugar, use los cursores de controlador y de servidor.  
  
 La biblioteca de cursores ODBC devuelve el siguiente SQLSTATEs, además de los enumerados en referencia de la [API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md).  
  
> [!NOTE]  
>  La biblioteca de cursores no ordena los registros de estado. el administrador de controladores y ODBC 3. los controladores *x* son responsables de la ordenación de los registros de estado.  
  
|SQLSTATE|Descripción|Se puede devolver desde|  
|--------------|-----------------|--------------------------|  
|01000|El cursor no es actualizable.|**SQLFetch**<br /><br /> **SQLFetchScroll**|  
|01000|No se utiliza la biblioteca de cursores. Error de carga.|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**|  
|01000|No se utiliza la biblioteca de cursores. Compatibilidad con controladores insuficientes.|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**|  
|01000|No se utiliza la biblioteca de cursores. La versión no coincide con el administrador de controladores.|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**|  
|01000|El controlador devolvió SQL_SUCCESS_WITH_INFO. Se perdió el mensaje de advertencia.|**SQLFetch**<br /><br /> **SQLFetchScroll**|  
|S1000|Error general: no se puede crear el búfer de archivos.|**SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**|  
|S1000|Error general: no se puede leer el búfer de archivos.|**SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**|  
|S1000|Error general: no se puede escribir en el búfer de archivos.|**SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**|  
|S1000|Error general: no se puede cerrar o quitar el búfer de archivos.|**SQLFreeHandle**<br /><br /> **SQLFreeStmt**|  
|SL001|No se puede realizar la solicitud posicionada porque no se enlazaron columnas que se pueden buscar.|**SQLExecDirect**<br /><br /> **SQLGetData**<br /><br /> **SQLPrepare**|  
|SL002|No se pudo realizar la solicitud posicionada porque el conjunto de resultados fue creado por una condición de combinación.|**SQLExecute**<br /><br /> **SQLExecDirect**<br /><br /> **SQLGetData**|  
|SL003|El búfer enlazado supera el tamaño máximo del segmento.|**SQLFetch**<br /><br /> **SQLFetchScroll**|  
|SL004|Un conjunto de resultados no fue generado por una instrucción **Select** .|**SQLGetData**|  
|SL005|**La instrucción SELECT** contiene una cláusula Group by.|**SQLGetData**|  
|SL006|Las matrices de parámetros no se admiten con las solicitudes posicionadas.|**SQLPrepare**<br /><br /> **SQLExecDirect**|  
|SL008|No se permite **SQLGetData** en un cursor de solo avance (no almacenado en búfer).|**SQLGetData**|  
|SL009|No había columnas enlazadas antes de llamar a **SQLFetch** o **SQLFetchScroll**.|**SQLFetch**<br /><br /> **SQLFetchScroll**|  
|SL010|**SQLBindCol** devolvió SQL_ERROR durante un intento de enlazar a un búfer interno.|**SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**|  
|SL011|La opción de instrucción solo es válida después de llamar a **SQLFetch** o **SQLFetchScroll**.|**SQLGetStmtAttr**|  
|SL012|Los enlaces de instrucción no se pueden cambiar mientras se abre un cursor.|**SQLBindCol**<br /><br /> **SQLFreeHandle**<br /><br /> **SQLFreeStmt**<br /><br /> **SQLSetStmtAttr**|  
|SL014|Se emitió una solicitud posicionada y no se almacenaron en búfer todos los campos de número de columnas.|**SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLPrepare**|  
|SL015|**SQLFetch** y **SQLFetchScroll** no se pueden mezclar.|**SQLExtendedFetch**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**|
