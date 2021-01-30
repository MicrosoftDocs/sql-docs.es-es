---
description: Función SQLReadFileDSN
title: Función SQLReadFileDSN | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLReadFileDSN
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLReadFileDSN
helpviewer_keywords:
- SQLReadFileDSN function [ODBC]
ms.assetid: ead464aa-cdc3-47dd-a0c0-997711205d31
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7a04747258f612a86746642ae11fb06e9b742662
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192537"
---
# <a name="sqlreadfiledsn-function"></a>Función SQLReadFileDSN
**Conformidad**  
 Versión introducida: ODBC 3,0  
  
 **Resumen**  
 **SQLReadFileDSN** Lee información de un DSN de archivo.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLReadFileDSN(  
     LPCSTR   lpszFileName,  
     LPCSTR   lpszAppName,  
     LPCSTR   lpszKeyName,  
     LPSTR    lpszString,  
     WORD     cbString,  
     WORD *   pcbString);  
```  
  
## <a name="arguments"></a>Argumentos  
 *lpszFileName*  
 Entradas Puntero al búfer de datos que contiene el nombre del archivo. DSN. Una extensión. DSN se anexa a todos los nombres de archivo que aún no tienen una extensión. DSN. El valor de *\* lpszFileName* debe ser una cadena terminada en NULL.  
  
 *lpszAppName*  
 Entradas Puntero al búfer de datos que contiene el nombre de la aplicación. Es "ODBC" para la sección ODBC. El valor de *\* lpszAppName* debe ser una cadena terminada en NULL.  
  
 *lpszKeyName*  
 Entradas Puntero al búfer de datos que contiene el nombre de la clave que se va a leer. Vea "Comentarios" para ver las palabras clave reservadas. El valor de *\* lpszAppName* debe ser una cadena terminada en NULL.  
  
 *lpszString*  
 Genere Puntero al búfer de datos que contiene la cadena asociada a la clave que se va a leer.  
  
 Si *\* lpszFileName* es un nombre de archivo. DSN válido, pero el argumento *lpszAppName* es un puntero nulo y el argumento *lpszKeyName* es un puntero nulo, *\* lpszString* contiene una lista de aplicaciones válidas. Si *\* lpszFileName* es un nombre de archivo. DSN válido y *\* lpszAppName* es un nombre de aplicación válido, pero el argumento *lpszKeyName* es un puntero nulo, *\* lpszString* contiene una lista de palabras clave reservadas válidas en la sección correspondiente del archivo DSN, delimitada por signos de punto y coma. Si *\* lpszFileName* es un nombre de archivo. DSN válido, pero *\* lpszAppName* es un puntero nulo y el argumento *lpszKeyName* es un puntero nulo, *\* lpszString* contiene una lista de las secciones del archivo DSN, delimitadas por signos de punto y coma.  
  
 *cbString*  
 Entradas Longitud del búfer de *\* lpszString* .  
  
 *pcbString*  
 Genere Número total de bytes disponibles que se van a devolver en *\* lpszString*. Si el número de bytes disponibles para devolver es mayor o igual que *cbString*, la cadena de salida de *\* LpszString* se trunca en *cbString* menos el carácter de terminación de NULL. El argumento *pcbString* puede ser un puntero nulo.  
  
## <a name="returns"></a>Devoluciones  
 La función devuelve TRUE si es correcto, FALSE si se produce un error.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLReadFileDSN** devuelve false, se puede obtener un valor de *\* pfErrorCode* asociado mediante una llamada a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Error general del instalador|Se produjo un error en el que no había ningún error específico del instalador.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longitud de búfer no válida|El argumento *lpszString* era null.<br /><br /> El argumento *cbString* era menor o igual que 0.|  
|ODBC_ERROR_INVALID_PATH|Ruta de instalación no válida|La ruta de acceso del nombre de archivo especificado en el argumento *lpszFileName* no era válida.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Tipo de solicitud no válido|El argumento *lpszAppName* era null, mientras que el argumento *lpszKeyName* era válido.|  
|ODBC_ERROR_OUT_OF_MEM|No hay memoria suficiente|El instalador no pudo realizar la función debido a una falta de memoria.|  
|ODBC_ERROR_OUTPUT_STRING_TRUNCATED|Cadena de salida truncada|La cadena devuelta en *\* lpszString* se truncó porque el valor de *cbString* era menor o igual que el valor de *\* pcbString*.|  
|ODBC_ERROR_REQUEST_FAILED|Error en la solicitud|La palabra clave no existía en el DSN de archivo.|  
  
## <a name="comments"></a>Comentarios  
 ODBC reserva el nombre de sección [ODBC] en el que se va a almacenar la información de conexión. Las palabras clave reservadas para esta sección son las mismas que las reservadas para una cadena de conexión en **SQLDriverConnect**. (Para obtener más información, consulte la descripción de la función [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) ).  
  
 Las aplicaciones pueden usar estas palabras clave reservadas para leer la información de un DSN de archivo. Si una aplicación desea averiguar la cadena de conexión sin DSN asociada a un DSN de archivo, puede llamar a **SQLReadFileDSN** para cualquiera de las palabras clave de cadena de conexión reservada en la sección [ODBC]. La cadena de conexión completa que se pasa en una conexión sin DSN es una combinación de todas las palabras clave (reservadas y específicas del controlador) en la sección [ODBC].  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Escribir información en un DSN de archivo|[SQLWriteFileDSN](../../../odbc/reference/syntax/sqlwritefiledsn-function.md)|
