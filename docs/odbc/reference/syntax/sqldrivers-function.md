---
description: Función SQLDrivers
title: Función SQLDrivers | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDrivers
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDrivers
helpviewer_keywords:
- SQLDrivers function [ODBC]
ms.assetid: 6b5b7514-e9cb-4cfd-8b7a-ab51dfab9efa
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 905dfe1bc24872468868abef9f1c8cc2323d110a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195622"
---
# <a name="sqldrivers-function"></a>Función SQLDrivers
**Conformidad**  
 Versión introducida: ODBC 2,0 Standards Compliance: ODBC  
  
 **Resumen**  
 **SQLDrivers** muestra las descripciones de controladores y las palabras clave de atributos de controlador. Esta función solo la implementa el administrador de controladores.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLDrivers(  
     SQLHENV         EnvironmentHandle,  
     SQLUSMALLINT    Direction,  
     SQLCHAR *       DriverDescription,  
     SQLSMALLINT     BufferLength1,  
     SQLSMALLINT *   DescriptionLengthPtr,  
     SQLCHAR *       DriverAttributes,  
     SQLSMALLINT     BufferLength2,  
     SQLSMALLINT *   AttributesLengthPtr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *EnvironmentHandle*  
 Entradas Identificador de entorno.  
  
 *Dirección*  
 Entradas Determina si el administrador de controladores captura la siguiente descripción del controlador en la lista (SQL_FETCH_NEXT) o si la búsqueda se inicia desde el principio de la lista (SQL_FETCH_FIRST).  
  
 *DriverDescription*  
 Genere Puntero a un búfer en el que se va a devolver la descripción del controlador.  
  
 Si *DriverDescription* es null, *DescriptionLengthPtr* seguirá devolviendo el número total de caracteres (excepto el carácter de terminación null para los datos de caracteres) disponible para devolver en el búfer señalado por *DriverDescription*.  
  
 *BufferLength1*  
 Entradas Longitud del búfer **DriverDescription* , en caracteres.  
  
 *DescriptionLengthPtr*  
 Genere Puntero a un búfer en el que se va a devolver el número total de caracteres (excepto el carácter de terminación null) disponible para devolver en \* *DriverDescription*. Si el número de caracteres disponibles para devolver es mayor o igual que *BufferLength1*, la descripción del controlador en \* *DriverDescription* se trunca a *BufferLength1* menos la longitud de un carácter de terminación null.  
  
 *DriverAttributes*  
 Genere Puntero a un búfer en el que se va a devolver la lista de pares de valores de atributo de controlador (vea "comments").  
  
 Si *DriverAttributes* es null, *AttributesLengthPtr* devolverá el número total de bytes (sin incluir el carácter de terminación null para los datos de caracteres) disponible para devolver en el búfer señalado por *DriverAttributes*.  
  
 *BufferLength2*  
 Entradas Longitud del búfer de \* *DriverAttributes* , en caracteres. Si el valor de *\* DriverDescription* es una cadena Unicode (al llamar a **SQLDriversW**), el argumento *BufferLength* debe ser un número par.  
  
 *AttributesLengthPtr*  
 Genere Puntero a un búfer en el que se va a devolver el número total de bytes (sin incluir el byte de terminación nula) disponible para devolver en \* *DriverAttributes*. Si el número de bytes disponibles para devolver es mayor o igual que *BufferLength2*, la lista de pares de valores de atributo de \* *DriverAttributes* se trunca en *BufferLength2* menos la longitud del carácter de terminación null.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLDrivers** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_ENV y un *identificador* de *EnvironmentHandle*. En la tabla siguiente se enumeran los valores SQLSTATE que devuelve normalmente **SQLDrivers** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje de información específico del administrador de controladores (DM). (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Datos de cadena, truncados a la derecha|(DM) el búfer \* *DriverDescription* no era lo suficientemente grande como para devolver la descripción completa del controlador. Por lo tanto, la descripción se truncó. La longitud de la descripción completa del controlador se devuelve en \* *DescriptionLengthPtr*. (La función devuelve SQL_SUCCESS_WITH_INFO).<br /><br /> (DM) el búfer \* *DriverAttributes* no era lo suficientemente grande como para devolver la lista completa de pares de valores de atributo. Por lo tanto, la lista se truncó. La longitud de la lista sin truncar de pares de valores de atributo se devuelve en **AttributesLengthPtr*. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|(DM) el administrador de controladores no pudo asignar memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY010|Error de secuencia de función|Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|(DM) el valor especificado para el argumento *BufferLength1* era menor que 0.<br /><br /> (DM) el valor especificado para el argumento *BufferLength2* era menor que 0 o igual a 1.|  
|HY103|Código de recuperación no válido|(DM) el valor especificado para la *Dirección* del argumento no es igual a SQL_FETCH_FIRST o SQL_FETCH_NEXT.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
  
## <a name="comments"></a>Comentarios  
 **SQLDrivers** devuelve la descripción del controlador en el búfer de \* *DriverDescription* . Devuelve información adicional sobre el controlador en el búfer de \* *DriverAttributes* como una lista de pares de palabra clave y valor. Todas las palabras clave enumeradas en la información del sistema para los controladores se devolverán para todos los controladores, excepto para **CreateDSN**, que se usa para solicitar la creación de orígenes de datos y, por tanto, es opcional. Cada par finaliza con un byte nulo y la lista completa finaliza con un byte nulo (es decir, dos bytes nulos marcan el final de la lista). Por ejemplo, un controlador basado en archivo que usa la sintaxis de C puede devolver la siguiente lista de atributos ("\ 0" representa un carácter nulo):  
  
```  
FileUsage=1\0FileExtns=*.dbf\0\0  
```  
  
 Si \* *DriverAttributes* no es lo suficientemente grande como para contener la lista completa, se trunca la lista, **SQLDrivers** devuelve SQLSTATE 01004 (datos truncados) y la longitud de la lista (sin incluir el byte final de finalización de NULL) se devuelve en **AttributesLengthPtr*.  
  
 Las palabras clave del atributo driver se agregan desde la información del sistema cuando se instala el controlador. Para obtener más información, vea [instalar componentes ODBC](../../../odbc/reference/install/installing-odbc-components.md).  
  
 Una aplicación puede llamar varias veces a **SQLDrivers** para recuperar todas las descripciones de los controladores. El administrador de controladores recupera esta información de la información del sistema. Cuando no hay más descripciones de controladores, **SQLDrivers** devuelve SQL_NO_DATA. Si se llama a **SQLDrivers** con SQL_FETCH_NEXT inmediatamente después de que se devuelva SQL_NO_DATA, se devuelve la primera descripción del controlador. Para obtener información sobre cómo una aplicación usa la información devuelta por **SQLDrivers**, vea [elegir un origen de datos o un controlador](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md).  
  
 Si SQL_FETCH_NEXT se pasa a **SQLDrivers** la primera vez que se llama, **SQLDrivers** devuelve el primer nombre del origen de datos.  
  
 Dado que **SQLDrivers** se implementa en el administrador de controladores, se admite para todos los controladores independientemente del cumplimiento de los estándares de un controlador concreto.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Detección y enumeración de los valores necesarios para conectarse a un origen de datos|[Función SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|  
|Conectarse a un origen de datos|[Función SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|Devolver nombres de orígenes de datos|[Función SQLDataSources](../../../odbc/reference/syntax/sqldatasources-function.md)|  
|Conectar con un origen de datos mediante una cadena de conexión o un cuadro de diálogo|[Función SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
