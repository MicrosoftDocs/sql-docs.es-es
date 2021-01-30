---
description: Función SQLSetConnectAttrForDbcInfo
title: Función SQLSetConnectAttrForDbcInfo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetConnectAttrForDbcInfo function [ODBC]
ms.assetid: a28fadb9-b998-472a-b252-709507e92005
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2ca64e1309c939f4b39b8b9a020a1debb1cc10b9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192439"
---
# <a name="sqlsetconnectattrfordbcinfo-function"></a>Función SQLSetConnectAttrForDbcInfo
**Conformidad**  
 Versión introducida: ODBC 3,81 Standards Compliance: ODBC  
  
 **Resumen**  
 **SQLSetConnectAttrForDbcInfo** es igual que **SQLSetConnectAttr**, pero establece el atributo en el token de información de conexión en lugar de en el identificador de conexión.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp
  
SQLRETURN  SQLSetConnectAttrForDbcInfo(  
                SQLHDBC_INFO_TOKEN    hDbcInfoToken,  
                SQLINTEGER            Attribute,  
                SQLPOINTER            ValuePtr,  
                SQLINTEGER            StringLength );  
```  
  
## <a name="arguments"></a>Argumentos  
 *hDbcInfoToken*  
 Entradas Identificador de token.  
  
 *Atributo*  
 Entradas Atributo que se va a establecer. La lista de atributos válidos es específica del controlador y la misma que para [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md).  
  
 *ValuePtr*  
 Entradas Puntero al valor que se va a asociar al *atributo*. Dependiendo del valor del *atributo*, *ValuePtr* será un valor entero sin signo de 32 bits o apuntará a una cadena de caracteres terminada en NULL. Tenga en cuenta que si el argumento de *atributo* es un valor específico del controlador, el valor de *ValuePtr* puede ser un entero con signo.  
  
 *StringLength*  
 Entradas Si el *atributo* es un atributo definido por ODBC y *ValuePtr* apunta a una cadena de caracteres o a un búfer binario, este argumento debe ser la longitud de **ValuePtr*. En el caso de los datos de cadenas de caracteres, este argumento debe contener el número de bytes de la cadena.  
  
 Si el *atributo* es un atributo definido por ODBC y *ValuePtr* es un entero, *StringLength* se omite.  
  
 Si el *atributo* es un atributo definido por el controlador, la aplicación indica la naturaleza del atributo para el administrador de controladores mediante el establecimiento del argumento *StringLength* . *StringLength* puede tener los siguientes valores:  
  
-   Si *ValuePtr* es un puntero a una cadena de caracteres, *StringLength* es la longitud de la cadena o SQL_NTS.  
  
-   Si *ValuePtr* es un puntero a un búfer binario, la aplicación coloca el resultado de la macro SQL_LEN_BINARY_ATTR (*length*) en *StringLength*. Esto coloca un valor negativo en *StringLength*.  
  
-   Si *ValuePtr* es un puntero a un valor distinto de una cadena de caracteres o una cadena binaria, *StringLength* debe tener el valor SQL_IS_POINTER.  
  
-   Si *ValuePtr* contiene un valor de longitud fija, *StringLength* es SQL_IS_INTEGER o SQL_IS_UINTEGER, según corresponda.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Igual que [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md), con la excepción de que el administrador de controladores usará un **HandleType** de SQL_HANDLE_DBC_INFO_TOKEN y un **identificador** de *hDbcInfoToken*.  
  
## <a name="remarks"></a>Observaciones  
 **SQLSetConnectAttrForDbcInfo** es igual que **SQLSetConnectAttr**, pero establece el atributo en el token de información de conexión, en lugar de en el identificador de conexión. Por ejemplo, si **SQLSetConnectAttr** no reconoce un atributo, **SQLSetConnectAttrForDbcInfo** también debe devolver SQL_ERROR para ese atributo.  
  
 Cada vez que el controlador devuelve SQL_ERROR o SQL_INVALID_HANDLE, el controlador debe omitir este atributo para calcular el identificador del grupo. Además, el administrador de controladores obtendrá la información de diagnóstico de *hDbcInfoToken* y devolverá SQL_SUCCESS_WITH_INFO a la aplicación en [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) y [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md). Por lo tanto, una aplicación puede recuperar detalles sobre el motivo por el que no se pueden establecer algunos atributos.  
  
 Las aplicaciones no deben llamar directamente a esta función. Un controlador ODBC que admita la agrupación de conexiones compatible con controladores debe implementar esta función.  
  
 Incluya sqlspi. h para el desarrollo del controlador ODBC.  
  
## <a name="see-also"></a>Consulte también  
 [Desarrollar un controlador ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Agrupación de conexiones compatible con controladores](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Desarrollar el conocimiento de la agrupación de conexiones en un controlador ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
