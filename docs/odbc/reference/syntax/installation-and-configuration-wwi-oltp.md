---
description: Función SQLSetDriverConnectInfo
title: Función SQLSetDriverConnectInfo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetDriverConnectInfo function [ODBC]
ms.assetid: bfd4dfc2-fbca-4ef3-81e5-2706f2389256
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: de0a21b8fa3e620f4a6884311c10c93c36518849
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174687"
---
# <a name="sqlsetdriverconnectinfo-function"></a>Función SQLSetDriverConnectInfo
**Conformidad**  
 Versión introducida: ODBC 3,81 Standards Compliance: ODBC  
  
 **Resumen**  
 **SQLSetDriverConnectInfo** se usa para establecer la cadena de conexión en el token de información de conexión para la llamada **SQLDriverConnect** de una aplicación.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp
  
SQLRETURN SQLSetDriverConnectInfo(  
                SQLHDBC_INFO_TOKEN   hDbcInfoToken,  
                WCHAR *              InConnectionString,  
                SQLSMALLINT          StringLength1 );  
```  
  
## <a name="arguments"></a>Argumentos  
 *TokenHandle*  
 Entradas Identificador de token.  
  
 *Inconnectionstring*  
 Entradas Una cadena de conexión completa (vea la sintaxis en "Comentarios" en [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)), una cadena de conexión parcial o una cadena vacía.  
  
 *StringLength1*  
 Entradas Longitud de **inconnectionstring*, en caracteres si la cadena es Unicode, o bytes si la cadena es ANSI o DBCS.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Igual que [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) relacionado con cualquier error de validación de entrada, salvo que el administrador de controladores usará una **HandleType** de SQL_HANDLE_DBC_INFO_TOKEN y un **identificador** de *hDbcInfoToken*.  
  
## <a name="remarks"></a>Observaciones  
 Cada vez que un controlador devuelve SQL_ERROR o SQL_INVALID_HANDLE, el administrador de controladores devuelve el error a la aplicación (en [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) o [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)).  
  
 Cada vez que un controlador devuelve SQL_SUCCESS_WITH_INFO, el administrador de controladores obtendrá la información de diagnóstico de *hDbcInfoToken* y devolverá SQL_SUCCESS_WITH_INFO a la aplicación en [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) y [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
 Las aplicaciones no deben llamar directamente a esta función. Un controlador ODBC que admita la agrupación de conexiones compatible con controladores debe implementar esta función.  
  
 Incluya sqlspi. h para el desarrollo del controlador ODBC.  
  
## <a name="see-also"></a>Consulte también  
 [Desarrollar un controlador ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Agrupación de conexiones compatible con controladores](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Desarrollar el conocimiento de la agrupación de conexiones en un controlador ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
