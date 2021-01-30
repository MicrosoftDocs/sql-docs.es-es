---
description: Función SQLDataSources
title: Función SQLDataSources | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDataSources
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDataSources
helpviewer_keywords:
- SQLDataSources function [ODBC]
ms.assetid: 3f63b1b4-e70e-44cd-96c6-6878d50d0117
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f22039a9e5f1443f828523d67a4cfa6c10496b23
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206102"
---
# <a name="sqldatasources-function"></a>Función SQLDataSources
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: ISO 92  
  
 **Resumen**  
 **SQLDataSources** devuelve información sobre un origen de datos. Esta función solo la implementa el administrador de controladores.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLDataSources(  
     SQLHENV          EnvironmentHandle,  
     SQLUSMALLINT     Direction,  
     SQLCHAR *        ServerName,  
     SQLSMALLINT      BufferLength1,  
     SQLSMALLINT *    NameLength1Ptr,  
     SQLCHAR *        Description,  
     SQLSMALLINT      BufferLength2,  
     SQLSMALLINT *    NameLength2Ptr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *EnvironmentHandle*  
 Entradas Identificador de entorno.  
  
 *Dirección*  
 Entradas Determina en qué origen de datos devuelve información el administrador de controladores. Puede ser:  
  
 SQL_FETCH_NEXT (para capturar el siguiente nombre del origen de datos en la lista), SQL_FETCH_FIRST (para capturar desde el principio de la lista), SQL_FETCH_FIRST_USER (para capturar el primer DSN de usuario) o SQL_FETCH_FIRST_SYSTEM (para capturar el primer DSN del sistema).  
  
 Cuando *Direction* se establece en SQL_FETCH_FIRST, las llamadas subsiguientes a **SQLDataSources** con la *Dirección* establecida en SQL_FETCH_NEXT devuelven DSN de usuario y del sistema. Cuando *Direction* se establece en SQL_FETCH_FIRST_USER, todas las llamadas subsiguientes a **SQLDataSources** con la *Dirección* establecida en SQL_FETCH_NEXT solo devuelven DSN de usuario. Cuando *Direction* se establece en SQL_FETCH_FIRST_SYSTEM, todas las llamadas subsiguientes a **SQLDataSources** con *Direction* establecidas en SQL_FETCH_NEXT solo devuelven DSN del sistema.  
  
 *ServerName*  
 Genere Puntero a un búfer en el que se va a devolver el nombre del origen de datos.  
  
 Si *ServerName* es null, *NameLength1Ptr* seguirá devolviendo el número total de caracteres (excepto el carácter de terminación null para los datos de caracteres) disponible para devolver en el búfer señalado por *ServerName*.  
  
 *BufferLength1*  
 Entradas Longitud del búfer **ServerName* , en caracteres; no es necesario que sea mayor que SQL_MAX_DSN_LENGTH más el carácter de terminación null.  
  
 *NameLength1Ptr*  
 Genere Puntero a un búfer en el que se va a devolver el número total de caracteres (excepto el carácter de terminación de NULL) disponible para devolver en \* *ServerName*. Si el número de caracteres disponibles para devolver es mayor o igual que *BufferLength1*, el nombre del origen de datos en \* *ServerName* se trunca a *BufferLength1* menos la longitud de un carácter de terminación null.  
  
 *Descripción*  
 Genere Puntero a un búfer en el que se va a devolver la descripción del controlador asociado al origen de datos. Por ejemplo, dBASE o SQL Server.  
  
 Si *Description* es null, *NameLength2Ptr* seguirá devolviendo el número total de caracteres (excepto el carácter de terminación null para los datos de caracteres) disponible para devolver en el búfer señalado por la *Descripción*.  
  
 *BufferLength2*  
 Entradas Longitud en caracteres del búfer **Description* .  
  
 *NameLength2Ptr*  
 Genere Puntero a un búfer en el que se va a devolver el número total de caracteres (excepto el carácter de terminación null) disponible para devolver en la \* *Descripción*. Si el número de caracteres disponibles para devolver es mayor o igual que *BufferLength2*, la descripción del controlador en la \* *Descripción* se trunca a *BufferLength2* menos la longitud de un carácter de terminación null.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLDataSources** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_ENV y un *identificador* de *EnvironmentHandle*. En la tabla siguiente se enumeran los valores SQLSTATE que devuelve normalmente **SQLDataSources** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje de información específico del administrador de controladores (DM). (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Datos de cadena, truncados a la derecha|(DM) buffer \* *ServerName* no era lo suficientemente grande como para devolver el nombre del origen de datos completo. Por lo tanto, el nombre se truncó. La longitud del nombre del origen de datos completo se devuelve en \* *NameLength1Ptr*. (La función devuelve SQL_SUCCESS_WITH_INFO).<br /><br /> (DM) la \* *Descripción* del búfer no era lo suficientemente grande como para devolver la descripción completa del controlador. Por lo tanto, la descripción se truncó. La longitud de la descripción del origen de datos no truncado se devuelve en **NameLength2Ptr*. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|HY000|Error general|(DM) se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|(DM) el administrador de controladores no pudo asignar memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY010|Error de secuencia de función|Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|(DM) el valor especificado para el argumento *BufferLength1* era menor que 0.<br /><br /> (DM) el valor especificado para el argumento *BufferLength2* era menor que 0.|  
|HY103|Código de recuperación no válido|(DM) el valor especificado para la *Dirección* del argumento no es igual a SQL_FETCH_FIRST, SQL_FETCH_FIRST_USER, SQL_FETCH_FIRST_SYSTEM o SQL_FETCH_NEXT.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
  
## <a name="comments"></a>Comentarios  
 Dado que **SQLDataSources** se implementa en el administrador de controladores, se admite para todos los controladores independientemente del cumplimiento de los estándares de un controlador concreto.  
  
 Una aplicación puede llamar varias veces a **SQLDataSources** para recuperar todos los nombres de orígenes de datos. El administrador de controladores recupera esta información de la información del sistema. Cuando no hay más nombres de orígenes de datos, el administrador de controladores devuelve SQL_NO_DATA. Si se llama a **SQLDataSources** con SQL_FETCH_NEXT inmediatamente después de que se devuelva SQL_NO_DATA, se devolverá el primer nombre del origen de datos. Para obtener información sobre cómo una aplicación usa la información devuelta por **SQLDataSources**, vea [elegir un origen de datos o un controlador](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md).  
  
 Si SQL_FETCH_NEXT se pasa a **SQLDataSources** la primera vez que se llama, devolverá el primer nombre del origen de datos.  
  
 El controlador determina cómo se asignan los nombres de orígenes de datos a los orígenes de datos reales.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Detección y enumeración de los valores necesarios para conectarse a un origen de datos|[Función SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|  
|Conectarse a un origen de datos|[Función SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|Conectar con un origen de datos mediante una cadena de conexión o un cuadro de diálogo|[Función SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|Devolver descripciones y atributos de controladores|[Función SQLDrivers](../../../odbc/reference/syntax/sqldrivers-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
