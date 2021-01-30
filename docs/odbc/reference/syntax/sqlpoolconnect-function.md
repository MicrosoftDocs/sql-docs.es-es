---
description: Función SQLPoolConnect
title: Función SQLPoolConnect | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLPoolConnect function [ODBC]
ms.assetid: 41322737-890d-4a81-aed2-06cc3d546962
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8317e159a5a438bb22e8077f3a6635a79f9baefd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204554"
---
# <a name="sqlpoolconnect-function"></a>Función SQLPoolConnect
**Conformidad**  
 Versión introducida: ODBC 3,8 Standards Compliance: ODBC  
  
 **Resumen**  
 **SQLPoolConnect** se usa para crear una nueva conexión si no se puede volver a usar una conexión en el grupo.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp
  
SQLRETURN  SQLPoolConnect(  
                SQLHDBC              hDbc,  
                SQLHDBC_INFO_TOKEN   hDbcInfoToken,  
                WCHAR *              wszOutConnectString,  
                SQLSMALLINT          cchConnectStringBuffer,  
                SQLSMALLINT *        cchConnectStringLen );  
```  
  
## <a name="arguments"></a>Argumentos  
 *hDbc*  
 Entradas Identificador de la conexión.  
  
 *hDbcInfoToken*  
 Entradas Identificador de token para la nueva solicitud de conexión de la aplicación.  
  
 *wszOutConnectString*  
 Genere Puntero a un búfer para la cadena de conexión completada. Cuando la conexión se realiza correctamente con el origen de datos de destino, este búfer contiene la cadena de conexión completada. Las aplicaciones deben asignar al menos 1.024 caracteres para este búfer.  
  
 Si *wszOutConnectString* es null, *cchConnectStringLen* seguirá devolviendo el número total de caracteres (excepto el carácter de terminación null para los datos de caracteres) disponible para devolver en el búfer señalado por *wszOutConnectString*.  
  
 *cchConnectStringBuffer*  
 Entradas Longitud del búfer **wszOutConnectString* , en caracteres.  
  
 *cchConnectStringLen*  
 Genere Puntero a un búfer en el que se va a devolver el número total de caracteres (excepto el carácter de terminación null) disponible para devolver en \* *wszOutConnectString*. Si el número de caracteres disponibles para devolver es mayor o igual que *cchConnectStringBuffer*, la cadena de conexión completada en \* *WszOutConnectString* se trunca a *cchConnectStringBuffer* menos la longitud de un carácter de terminación null.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Similar a [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) para cualquier error de validación de entrada, salvo que el administrador de controladores usará una **HandleType** de SQL_HANDLE_DBC_INFO_TOKEN y un **identificador** de *hDbcInfoToken*.  
  
## <a name="remarks"></a>Observaciones  
 El administrador de controladores garantiza que el identificador de HENV principal de *hDbc* y *hDbcInfoToken* es el mismo.  
  
 A diferencia de [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md), no hay ningún argumento *DriverCompletion* para pedir a los usuarios que escriban información de conexión. No se permite un cuadro de diálogo de solicitud en el escenario de agrupación.  
  
 Las aplicaciones no deben llamar directamente a esta función. Un controlador ODBC que admita la agrupación de conexiones compatible con controladores debe implementar esta función.  
  
 Cada vez que un controlador devuelve SQL_ERROR o SQL_INVALID_HANDLE, el administrador de controladores devuelve el error a la aplicación (en [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) o [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)).  
  
 Cada vez que un controlador devuelve SQL_SUCCESS_WITH_INFO, el administrador de controladores obtendrá la información de diagnóstico de *hDbcInfoToken* y devolverá SQL_SUCCESS_WITH_INFO a la aplicación en [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) y [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
 Cuando una aplicación usa [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md), *wszOutConnectString* será un búfer nulo (los tres últimos parámetros se establecerán en NULL, 0, null). De lo contrario, el controlador debe devolver la cadena de conexión de salida, que se devolverá a la llamada a la [función SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) de la aplicación.  
  
 Incluya sqlspi. h para el desarrollo del controlador ODBC.  
  
## <a name="see-also"></a>Consulte también  
 [Desarrollar un controlador ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Agrupación de conexiones compatible con controladores](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Desarrollar el conocimiento de la agrupación de conexiones en un controlador ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
