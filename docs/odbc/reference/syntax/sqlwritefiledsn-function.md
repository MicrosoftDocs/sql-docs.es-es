---
description: Función SQLWriteFileDSN
title: Función SQLWriteFileDSN | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLWriteFileDSN
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLWriteFileDSN
helpviewer_keywords:
- SQLWriteFileDSN [ODBC]
ms.assetid: 9e18f56f-1061-416b-83d4-ffeec42ab5a9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f778c2669a55e3f63c1946d2b4323ae64242c533
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191666"
---
# <a name="sqlwritefiledsn-function"></a>Función SQLWriteFileDSN
**Conformidad**  
 Versión introducida: ODBC 3,0  
  
 **Resumen**  
 **SQLWriteFileDSN** escribe información en un DSN de archivo.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLWriteFileDSN(  
     LPCSTR     lpszFileName,  
     LPCSTR     lpszAppName,  
     LPCSTR     lpszKeyName,  
     LPCSTR     lpszString);  
```  
  
## <a name="arguments"></a>Argumentos  
 *lpszFileName*  
 Entradas Puntero al nombre del DSN de archivo. Una extensión DSN se anexa a todos los nombres de archivo que aún no tienen una extensión DSN.  
  
 *lpszAppName*  
 Entradas Puntero al nombre de la aplicación. Es "ODBC" para la sección ODBC.  
  
 *lpszKeyName*  
 Entradas Puntero al nombre de la clave que se va a leer. Vea "Comentarios" para ver las palabras clave reservadas.  
  
 *lpszString*  
 Genere Apunta a la cadena asociada a la clave que se va a escribir. La longitud máxima de la cadena a la que apunta este argumento es de 32.767 bytes.  
  
## <a name="returns"></a>Devoluciones  
 La función devuelve TRUE si es correcto, FALSE si se produce un error.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLWriteFileDSN** devuelve false, se puede obtener un valor de *\* pfErrorCode* asociado mediante una llamada a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Error general del instalador|Se produjo un error en el que no había ningún error específico del instalador.|  
|ODBC_ERROR_INVALID_PATH|Ruta de instalación no válida|La ruta de acceso del nombre de archivo especificado en el argumento *lpszFileName* no era válida.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Tipo de solicitud no válido|El argumento *lpszAppName*, *lpszKeyName* o *lpszString* era null.|  
  
## <a name="comments"></a>Comentarios  
 ODBC reserva el nombre de sección [ODBC] en el que se va a almacenar la información de conexión. Las palabras clave reservadas para esta sección son las mismas que las reservadas para una cadena de conexión en **SQLDriverConnect**. (Para obtener más información, consulte la descripción de la función [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) ).  
  
 Las aplicaciones pueden usar estas palabras clave reservadas para escribir información directamente en un DSN de archivo. Si una aplicación desea crear o modificar la cadena de conexión sin DSN asociada a un DSN de archivo, puede llamar a **SQLWriteFileDSN** para cualquiera de las palabras clave de cadena de conexión reservada en la sección [ODBC].  
  
 Si el argumento *lpszString* es un puntero nulo, la palabra clave a la que apunta el argumento *lpszKeyName* se eliminará del archivo. DSN. Si los argumentos *lpszString* y *lpszKeyName* son punteros nulos, la sección señalada por el argumento *lpszAppName* se eliminará del archivo. DSN.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Leer información de los DSN de archivo|[SQLReadFileDSN](../../../odbc/reference/syntax/sqlreadfiledsn-function.md)|
