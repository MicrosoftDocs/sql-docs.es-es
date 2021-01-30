---
description: Función SQLCompleteAsync
title: Función SQLCompleteAsync | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
f1_keywords:
- SQLCompleteAsync
helpviewer_keywords:
- SQLCompleteAsync function [ODBC]
ms.assetid: 1b97c46a-d2e5-4540-8239-9d975e5321c6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ad9a84bcc5f8ae716394925d4935c384d29794c2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207108"
---
# <a name="sqlcompleteasync-function"></a>Función SQLCompleteAsync
**Conformidad**  
 Versión introducida: cumplimiento de estándares de ODBC 3,8: ninguno  
  
 **Resumen**  
 **SQLCompleteAsync** se puede usar para determinar cuándo se completa una función asincrónica mediante el procesamiento basado en notificaciones o en el sondeo. Para obtener más información acerca de las operaciones asincrónicas, vea [ejecución asincrónica](../../../odbc/reference/develop-app/asynchronous-execution.md).  
  
 **SQLCompleteAsync** solo se implementa en el administrador de controladores ODBC.  
  
 En el modo de procesamiento asincrónico basado en notificaciones, se debe llamar a **SQLCompleteAsync** después de que el administrador de controladores genere el objeto de evento que se usa para la notificación. **SQLCompleteAsync** completa el procesamiento asincrónico y la función asincrónica generará un código de retorno.  
  
 En el modo de procesamiento asincrónico basado en sondeo, **SQLCompleteAsync** es una alternativa a llamar a la función asincrónica original, sin necesidad de especificar los argumentos en la llamada de función asincrónica original. Se puede usar **SQLCompleteAsync** independientemente de si está habilitada la biblioteca de cursores ODBC.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLCompleteAsync(  
      SQLSMALLINT HandleType,  
      SQLHANDLE   Handle,  
      RETCODE *   AsyncRetCodePtr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *HandleType*  
 Entradas Tipo del identificador en el que se va a completar el procesamiento asincrónico. Los valores válidos son SQL_HANDLE_DBC o SQL_HANDLE_STMT.  
  
 *Handle*  
 Entradas Identificador en el que se va a completar el procesamiento asincrónico. Si el *identificador* no es un identificador válido del tipo especificado por *HandleType*, **SQLCompleteAsync** devuelve SQL_INVALID_HANDLE.  
  
 Si el *identificador* no es un identificador válido del tipo especificado por *HandleType*, **SQLCompleteAsync** devuelve SQL_INVALID_HANDLE.  
  
 *AsyncRetCodePtr*  
 Genere Puntero a un búfer que contendrá el código de retorno de la API asincrónica. Si *AsyncRetCodePtr* es null, **SQLCompleteAsync** devuelve SQL_ERROR.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_ERROR, SQL_NO_DATA o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Si **SQLCompleteAsync** devuelve SQL_SUCCESS, una aplicación debe obtener el código de retorno de la función asincrónica del búfer al que apunta *AsyncRetCodePtr*. El SQLSTATE asociado, si existe, se puede obtener llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un identificador de instrucción o un *HandleType* de SQL_HANDLE_DBC y un identificador de conexión. Estos registros de diagnóstico están asociados a la función asincrónica, no a esta función **SQLCompleteAsync** .  
  
 **SQLCompleteAsync** devuelve un código distinto de SQL_SUCCESS para indicar que no se llama a **SQLCompleteAsync** correctamente. En este caso, **SQLCompleteAsync** no publicará ningún registro de diagnóstico. Los códigos de retorno posibles son:  
  
-   SQL_INVALID_HANDLE: el identificador indicado por *HandleType* y el *identificador* no es un identificador válido.  
  
-   SQL_ERROR: *AsyncRetCodePtr* es null o el procesamiento asincrónico no está habilitado en el identificador.  
  
-   SQL_NO_DATA: en el modo de notificación, una operación asincrónica no está en curso o el administrador de controladores no ha notificado a la aplicación. En el modo de sondeo, una operación asincrónica no está en curso.  
  
## <a name="comments"></a>Comentarios  
 En el modo de procesamiento asincrónico basado en sondeo, *AsyncRetCodePtr* podría ser SQL_STILL_EXECUTING cuando **SQLCompleteAsync** devuelve SQL_SUCCESS. La aplicación debe seguir sondeando hasta que no se SQL_STILL_EXECUTING *AsyncRetCodePtr* . En el modo de procesamiento asincrónico basado en notificaciones, *AsyncRetCodePtr* nunca se SQL_STILL_EXECUTING.  
  
## <a name="see-also"></a>Consulte también  
 [Ejecución asincrónica (método de sondeo)](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md)
