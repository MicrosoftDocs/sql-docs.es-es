---
description: Función SQLGetDiagRec
title: SQLGetDiagRec (función) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetDiagRec
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetDiagRec
helpviewer_keywords:
- SQLGetDiagRec function [ODBC]
ms.assetid: ebdbac93-3d68-438f-8416-ef1f08e04269
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 93b5b85d6c20a58d314dcb7f7c53f391c15a1491
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194177"
---
# <a name="sqlgetdiagrec-function"></a>Función SQLGetDiagRec
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 3,0: ISO 92  
  
 **Resumen**  
 **SQLGetDiagRec** devuelve los valores actuales de varios campos de un registro de diagnóstico que contiene información de error, de advertencia y de estado. A diferencia de **SQLGetDiagField**, que devuelve un campo de diagnóstico por llamada, **SQLGetDiagRec** devuelve varios campos usados comúnmente de un registro de diagnóstico, incluido SQLSTATE, el código de error nativo y el texto del mensaje de diagnóstico.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLGetDiagRec(  
     SQLSMALLINT     HandleType,  
     SQLHANDLE       Handle,  
     SQLSMALLINT     RecNumber,  
     SQLCHAR *       SQLState,  
     SQLINTEGER *    NativeErrorPtr,  
     SQLCHAR *       MessageText,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   TextLengthPtr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *HandleType*  
 Entradas Identificador de tipo de identificador que describe el tipo de identificador para el que se requieren diagnósticos. Debe ser una de las siguientes:  
  
-   SQL_HANDLE_DBC  
  
-   SQL_HANDLE_DBC_INFO_TOKEN  
  
-   SQL_HANDLE_DESC  
  
-   SQL_HANDLE_ENV  
  
-   SQL_HANDLE_STMT  
  
 El identificador de SQL_HANDLE_DBC_INFO_TOKEN solo lo utiliza el administrador de controladores y el controlador. Las aplicaciones no deben usar este tipo de identificador. Para obtener más información acerca de SQL_HANDLE_DBC_INFO_TOKEN, vea [desarrollar el reconocimiento de Connection-Pool en un controlador ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).  
  
 *Handle*  
 Entradas Identificador de la estructura de datos de diagnóstico, del tipo indicado por *HandleType*. Si *HandleType* es SQL_HANDLE_ENV, *Handle* puede ser un identificador de entorno compartido o no compartido.  
  
 *RecNumber*  
 Entradas Indica el registro de estado del que la aplicación busca información. Los registros de estado se numeran a partir de 1.  
  
 *SQLState*  
 Genere Puntero a un búfer en el que se va a devolver un código SQLSTATE de cinco caracteres (y terminación NULL) para el registro de diagnóstico *RecNumber*. Los dos primeros caracteres indican la clase; los tres siguientes indican la subclase. Esta información se encuentra en el campo diagnóstico de SQL_DIAG_SQLSTATE. Para obtener más información, vea [SQLSTATEs](../../../odbc/reference/develop-app/sqlstates.md).  
  
 *NativeErrorPtr*  
 Genere Puntero a un búfer en el que se va a devolver el código de error nativo, específico del origen de datos. Esta información se encuentra en el campo diagnóstico de SQL_DIAG_NATIVE.  
  
 *MessageText*  
 Genere Puntero a un búfer en el que se va a devolver la cadena de texto del mensaje de diagnóstico. Esta información se encuentra en el campo diagnóstico de SQL_DIAG_MESSAGE_TEXT. Para el formato de la cadena, vea [mensajes de diagnóstico](../../../odbc/reference/develop-app/diagnostic-messages.md).  
  
 Si *MessageText* es null, *TextLengthPtr* seguirá devolviendo el número total de caracteres (excepto el carácter de terminación null para los datos de caracteres) disponible para devolver en el búfer señalado por *MessageText*.  
  
 *BufferLength*  
 Entradas Longitud del búfer **MessageText* en caracteres. No hay una longitud máxima del texto del mensaje de diagnóstico.  
  
 *TextLengthPtr*  
 Genere Puntero a un búfer en el que se va a devolver el número total de caracteres (excepto el número de caracteres necesarios para el carácter de terminación null) disponible para devolver en *\* MessageText*. Si el número de caracteres disponibles para devolver es mayor que *BufferLength*, el texto del mensaje de diagnóstico en *\* MessageText* se trunca a *BufferLength* menos la longitud de un carácter de terminación null.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 **SQLGetDiagRec** no envía registros de diagnóstico para sí mismo. Usa los siguientes valores devueltos para notificar el resultado de su propia ejecución:  
  
-   SQL_SUCCESS: la función devolvió correctamente la información de diagnóstico.  
  
-   SQL_SUCCESS_WITH_INFO: el \* búfer de *MessageText* era demasiado pequeño para contener el mensaje de diagnóstico solicitado. No se generó ningún registro de diagnóstico. Para determinar que se ha producido un truncamiento, la aplicación debe comparar *BufferLength* con el número real de bytes disponible, que se escribe en **StringLengthPtr*.  
  
-   SQL_INVALID_HANDLE: el identificador indicado por *HandleType* y el *identificador* no era un identificador válido.  
  
-   SQL_ERROR: se produjo una de las siguientes acciones:  
  
    -   *RecNumber* era negativo o 0.  
  
    -   *BufferLength* era menor que cero.  
  
    -   Al utilizar la notificación asincrónica, no se completó la operación asincrónica en el identificador.  
  
-   SQL_NO_DATA: *RecNumber* era mayor que el número de registros de diagnóstico que existían para el identificador especificado en el *identificador.* La función también devuelve SQL_NO_DATA para cualquier *RecNumber* positiva si no hay ningún registro de diagnóstico para el *identificador*.  
  
## <a name="comments"></a>Comentarios  
 Una aplicación normalmente llama a **SQLGetDiagRec** cuando una llamada anterior a una función ODBC ha devuelto SQL_ERROR o SQL_SUCCESS_WITH_INFO. Sin embargo, dado que cualquier función ODBC puede publicar cero o más registros de diagnóstico cada vez que se llama, una aplicación puede llamar a **SQLGetDiagRec** después de cualquier llamada a función de ODBC. Una aplicación puede llamar a **SQLGetDiagRec** varias veces para devolver algunos o todos los registros de la estructura de datos de diagnóstico. ODBC no impone ningún límite en el número de registros de diagnóstico que se pueden almacenar en un momento dado.  
  
 **SQLGetDiagRec** no se puede usar para devolver campos del encabezado de la estructura de datos de diagnóstico. (El argumento *RecNumber* debe ser mayor que 0). La aplicación debe llamar a **SQLGetDiagField** para este propósito.  
  
 **SQLGetDiagRec** solo recupera la información de diagnóstico asociada más recientemente con el identificador especificado en el argumento de *identificador* . Si la aplicación llama a otra función ODBC, excepto **SQLGetDiagRec**, **SQLGetDiagField** o **SQLError**, se pierde toda la información de diagnóstico de las llamadas anteriores en el mismo identificador.  
  
 Una aplicación puede examinar todos los registros de diagnóstico mediante bucles, incrementando *RecNumber*, siempre y cuando **SQLGetDiagRec** devuelva SQL_SUCCESS. Las llamadas a **SQLGetDiagRec** no son destructivas para los campos de encabezado y registro. La aplicación puede llamar a **SQLGetDiagRec** de nuevo más tarde para recuperar un campo de un registro siempre que no se haya llamado a ninguna otra función, excepto **SQLGetDiagRec**, **SQLGetDiagField** o **SQLError**, en el período de tiempo. La aplicación también puede recuperar un recuento del número total de registros de diagnóstico disponibles mediante una llamada a **SQLGetDiagField** para recuperar el valor del campo SQL_DIAG_NUMBER y, a continuación, llamar a **SQLGetDiagRec** tantas veces.  
  
 Para obtener una descripción de los campos de la estructura de datos de diagnóstico, vea [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md). Para obtener más información, vea [usar SQLGetDiagRec y SQLGetDiagField](../../../odbc/reference/develop-app/using-sqlgetdiagrec-and-sqlgetdiagfield.md) e [implementar SQLGetDiagRec y SQLGetDiagField](../../../odbc/reference/develop-app/implementing-sqlgetdiagrec-and-sqlgetdiagfield.md).  
  
 La llamada a una API distinta de la que se ejecuta de forma asincrónica generará el HY010 "error de secuencia de función". Sin embargo, el registro de error no se puede recuperar antes de que se complete la operación asincrónica.  
  
## <a name="handletype-argument"></a>Argumento HandleType  
 Cada tipo de controlador puede tener información de diagnóstico asociada. El argumento *HandleType* denota el tipo de identificador del argumento de *identificador* .  
  
 Algunos campos de encabezado y registro no se pueden devolver para los identificadores de entorno, conexión, instrucción y descriptor. Los identificadores para los que un campo no es aplicable se indican en las secciones "campos de encabezado" y "campos de registro" en [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).  
  
 Una llamada a **SQLGetDiagRec** devolverá SQL_INVALID_HANDLE si *HandleType* es SQL_HANDLE_SENV, que denota un identificador de entorno compartido. Sin embargo, si *HandleType* es SQL_HANDLE_ENV, *Handle* puede ser un identificador de entorno compartido o no compartido.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Obtención de un campo de un registro de diagnóstico o un campo del encabezado de diagnóstico|[Función SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Programa de ejemplo de ODBC](../../../odbc/reference/sample-odbc-program.md)
