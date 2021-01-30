---
description: Función SQLSetEnvAttr
title: Función SQLSetEnvAttr | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetEnvAttr
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLSetEnvAttr
helpviewer_keywords:
- SQLSetEnvAttr function [ODBC]
ms.assetid: 0343241c-4b15-4d4b-aa2b-2e8ab5215cd2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ed611f343275c2d777d4f390154b160aa6346094
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200961"
---
# <a name="sqlsetenvattr-function"></a>Función SQLSetEnvAttr
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 3,0: ISO 92  
  
 **Resumen**  
 **SQLSetEnvAttr** establece atributos que rigen aspectos de los entornos.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLSetEnvAttr(  
     SQLHENV      EnvironmentHandle,  
     SQLINTEGER   Attribute,  
     SQLPOINTER   ValuePtr,  
     SQLINTEGER   StringLength);  
```  
  
## <a name="arguments"></a>Argumentos  
 *EnvironmentHandle*  
 Entradas Identificador de entorno.  
  
 *Atributo*  
 Entradas Atributo que se va a establecer, mostrado en "Comentarios".  
  
 *ValuePtr*  
 Entradas Puntero al valor que se va a asociar al *atributo*. Dependiendo del valor del *atributo*, *ValuePtr* será un valor entero de 32 bits o apuntará a una cadena de caracteres terminada en NULL.  
  
 *StringLength*  
 Entradas Si *ValuePtr* apunta a una cadena de caracteres o a un búfer binario, este argumento debe ser la longitud de **ValuePtr*. En el caso de los datos de cadenas de caracteres, este argumento debe contener el número de bytes de la cadena.  
  
 Si *ValuePtr* es un entero, *StringLength* se omite.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLSetEnvAttr** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_ENV y un *identificador* de *EnvironmentHandle*. En la tabla siguiente se enumeran los valores SQLSTATE que devuelve normalmente **SQLSetEnvAttr** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario. Si un controlador no admite un atributo de entorno, solo se puede devolver el error durante el tiempo de conexión.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01S02|Valor de opción cambiado|El controlador no admitía el valor especificado en *ValuePtr* y sustituyó un valor similar. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY009|Uso no válido de puntero nulo|El argumento de atributo identificó un atributo de entorno que requería un valor de cadena y el argumento *ValuePtr* era un puntero nulo.|  
|HY010|Error de secuencia de función|(DM) se ha asignado un identificador de conexión en *EnvironmentHandle*.<br /><br /> No se ha establecido el **SQL_ATTR_ODBC_VERSION** (DM) con **SQLSetEnvAttr** y el *atributo* no es igual a **SQL_ATTR_ODBC_VERSION**. No es necesario establecer **SQL_ATTR_ODBC_VERSION** explícitamente si se usa **SQLAllocHandleStd**.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY024|Valor de atributo no válido|Dado el valor de *atributo* especificado, se especificó un valor no válido en *ValuePtr*.|  
|HY090|Longitud de búfer o cadena no válida|El argumento *StringLength* era menor que 0 pero no se SQL_NTS.|  
|HY092|Identificador de opción/atributo no válido|(DM) el valor especificado para el *atributo* argument no era válido para la versión de ODBC admitida por el controlador.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|El valor especificado para el *atributo* argument era un atributo de entorno ODBC válido para la versión de ODBC admitida por el controlador, pero no era compatible con el controlador.<br /><br /> (DM) el argumento de *atributo* se SQL_ATTR_OUTPUT_NTS y se SQL_FALSE *ValuePtr* .|  
  
## <a name="comments"></a>Comentarios  
 Una aplicación solo puede llamar a **SQLSetEnvAttr** si no se ha asignado ningún identificador de conexión en el entorno. Todos los atributos de entorno establecidos correctamente por la aplicación para el entorno se conservan hasta que se llama a **SQLFreeHandle** en el entorno. Se puede asignar más de un identificador de entorno simultáneamente en ODBC *3. x*.  
  
 El formato de la información que se establece a través de *ValuePtr* depende del *atributo* especificado. **SQLSetEnvAttr** aceptará información de atributos en uno de dos formatos diferentes: una cadena de caracteres terminada en null o un valor entero de 32 bits. El formato de cada se indica en la descripción del atributo.  
  
 No hay ningún atributo de entorno específico del controlador.  
  
 No se pueden establecer los atributos de conexión mediante una llamada a **SQLSetEnvAttr**. Si intenta hacerlo, se devolverá SQLSTATE HY092 (identificador de opción o atributo no válido).  
  
|*Atributo*|Contenido de *ValuePtr*|  
|-----------------|-------------------------|  
|SQL_ATTR_CONNECTION_POOLING (ODBC 3,8)|Un valor SQLUINTEGER que incluya de 32 bits que habilita o deshabilita la agrupación de conexiones en el nivel de entorno. Se usan los siguientes valores:<br /><br /> SQL_CP_OFF = la agrupación de conexiones está desactivada. Este es el valor predeterminado.<br /><br /> SQL_CP_ONE_PER_DRIVER = se admite un único grupo de conexiones para cada controlador. Cada conexión de un grupo está asociada a un controlador.<br /><br /> SQL_CP_ONE_PER_HENV = se admite un único grupo de conexiones para cada entorno. Cada conexión de un grupo está asociada a un entorno.<br /><br /> SQL_CP_DRIVER_AWARE = use la característica de reconocimiento del grupo de conexiones del controlador, si está disponible. Si el controlador no admite el reconocimiento del grupo de conexiones, SQL_CP_DRIVER_AWARE se omite y se usa SQL_CP_ONE_PER_HENV. Para obtener más información, consulte [agrupación de conexiones compatible con controladores](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md). En un entorno en el que algunos controladores admitan y algunos controladores no admitan el reconocimiento del grupo de conexiones, SQL_CP_DRIVER_AWARE pueden habilitar la característica de reconocimiento del grupo de conexiones en esos controladores auxiliares, pero equivale a establecer en SQL_CP_ONE_PER_HENV en los controladores que no admiten la característica de reconocimiento del grupo de conexiones.<br /><br /> La agrupación de conexiones se habilita llamando a **SQLSetEnvAttr** para establecer el atributo de SQL_ATTR_CONNECTION_POOLING en SQL_CP_ONE_PER_DRIVER o SQL_CP_ONE_PER_HENV. Esta llamada debe realizarse antes de que la aplicación asigne el entorno compartido para el que se va a habilitar la agrupación de conexiones. El identificador de entorno en la llamada a **SQLSetEnvAttr** se establece en null, lo que hace que SQL_ATTR_CONNECTION_POOLING un atributo de nivel de proceso. Una vez habilitada la agrupación de conexiones, la aplicación asigna un entorno compartido implícito llamando a **SQLAllocHandle** con el argumento *InputHandle* establecido en SQL_HANDLE_ENV.<br /><br /> Una vez que se ha habilitado la agrupación de conexiones y se ha seleccionado un entorno compartido para una aplicación, no se puede restablecer SQL_ATTR_CONNECTION_POOLING para ese entorno, porque se llama a **SQLSetEnvAttr** con un identificador de entorno nulo al establecer este atributo. Si se establece este atributo mientras la agrupación de conexiones ya está habilitada en un entorno compartido, el atributo solo afecta a los entornos compartidos que se asignan posteriormente.<br /><br /> También es posible habilitar la agrupación de conexiones en un entorno. Tenga en cuenta lo siguiente acerca de la agrupación de conexiones de entorno:<br /><br /> -La habilitación de la agrupación de conexiones en un identificador NULL es un atributo de nivel de proceso. Los entornos asignados posteriormente serán un entorno compartido y heredarán la configuración de la agrupación de conexiones de nivel de proceso.<br />-Después de que se haya asignado un entorno, una aplicación puede cambiar la configuración del grupo de conexiones.<br />-Si está habilitada la agrupación de conexiones de entorno y el controlador de la conexión usa la agrupación de controladores, la agrupación de entornos tiene preferencia.<br /><br /> SQL_ATTR_CONNECTION_POOLING se implementa en el administrador de controladores. No es necesario que un controlador implemente SQL_ATTR_CONNECTION_POOLING. Las aplicaciones ODBC 2,0 y 3,0 pueden establecer este atributo de entorno.<br /><br /> Para obtener más información, consulte este artículo sobre la [agrupación de conexiones ODBC](../../../odbc/reference/develop-app/driver-manager-connection-pooling.md).|  
|SQL_ATTR_CP_MATCH (ODBC 3,0)|Un valor SQLUINTEGER que incluya de 32 bits que determina cómo se elige una conexión de un grupo de conexiones. Cuando se llama a **SQLConnect** o **SQLDriverConnect** , el administrador de controladores determina qué conexión se reutiliza desde el grupo. El administrador de controladores intenta hacer coincidir las opciones de conexión de la llamada y los atributos de conexión establecidos por la aplicación con las palabras clave y los atributos de conexión de las conexiones del grupo. El valor de este atributo determina el nivel de precisión de los criterios de coincidencia.<br /><br /> Los valores siguientes se usan para establecer el valor de este atributo:<br /><br /> SQL_CP_STRICT_MATCH = solo se reutilizan las conexiones que coinciden exactamente con las opciones de conexión de la llamada y los atributos de conexión establecidos por la aplicación. Este es el valor predeterminado.<br /><br /> SQL_CP_RELAXED_MATCH = se pueden usar conexiones con palabras clave de cadena de conexión coincidentes. Las palabras clave deben coincidir, pero no todos los atributos de conexión deben coincidir.<br /><br /> Para obtener más información acerca del modo en que el administrador de controladores realiza la conexión a una conexión agrupada, vea [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md). Para obtener más información sobre la agrupación de conexiones, vea [agrupación de conexiones ODBC](../../../odbc/reference/develop-app/driver-manager-connection-pooling.md).|  
|SQL_ATTR_ODBC_VERSION (ODBC 3,0)|Entero de 32 bits que determina si determinada funcionalidad exhibe el comportamiento de ODBC *2. x* o el comportamiento de ODBC *3. x* . Los valores siguientes se usan para establecer el valor de este atributo:<br /><br /> SQL_OV_ODBC3_80 = el controlador y el administrador de controladores presentan el siguiente comportamiento de ODBC 3,8:<br /><br /> -El controlador devuelve y espera códigos ODBC *3. x* para fecha, hora y marca de tiempo.<br />-El controlador devuelve códigos SQLSTATE de ODBC *3. x* cuando se llama a **SQLError**, **SQLGetDiagField** o **SQLGetDiagRec** .<br />-El argumento *nombrecatálogo* en una llamada a **SQLTables** acepta un patrón de búsqueda.<br />-El administrador de controladores admite la extensibilidad de tipos de datos de C. Para obtener más información sobre la extensibilidad de tipos de datos de C, vea [tipos de datos de c en ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md).<br /><br /> Para obtener más información, vea [What's New in ODBC 3,8](../../../odbc/reference/what-s-new-in-odbc-3-8.md).<br /><br /> SQL_OV_ODBC3 = el controlador y el administrador de controladores presentan el siguiente comportamiento de ODBC *3. x* :<br /><br /> -El controlador devuelve y espera códigos ODBC *3. x* para fecha, hora y marca de tiempo.<br />-El controlador devuelve códigos SQLSTATE de ODBC *3. x* cuando se llama a **SQLError**, **SQLGetDiagField** o **SQLGetDiagRec** .<br />-El argumento *nombrecatálogo* en una llamada a **SQLTables** acepta un patrón de búsqueda.<br />-El administrador de controladores no admite la extensibilidad de tipos de datos de C.<br /><br /> SQL_OV_ODBC2 = el controlador y el administrador de controladores presentan el siguiente comportamiento de ODBC *2. x* . Esto es especialmente útil para una aplicación ODBC *2. x* que trabaja con un controlador ODBC *3. x* .<br /><br /> -El controlador devuelve y espera códigos ODBC *2. x* para fecha, hora y marca de tiempo.<br />-El controlador devuelve códigos SQLSTATE de ODBC *2. x* cuando se llama a **SQLError**, **SQLGetDiagField** o **SQLGetDiagRec** .<br />-El argumento *nombrecatálogo* en una llamada a **SQLTables** no acepta un patrón de búsqueda.<br />-El administrador de controladores no admite la extensibilidad de tipos de datos de C.<br /><br /> Una aplicación debe establecer este atributo de entorno antes de llamar a cualquier función que tenga un argumento SQLHENV, o bien la llamada devolverá SQLSTATE HY010 (error de secuencia de función). Es específico del controlador si existe un comportamiento adicional para estas marcas de entorno.<br /><br /> -Para obtener más información, vea [declarar la versión de ODBC de la aplicación](../../../odbc/reference/develop-app/declaring-the-application-s-odbc-version.md) y [los cambios de comportamiento](../../../odbc/reference/develop-app/behavioral-changes.md).|  
|SQL_ATTR_OUTPUT_NTS (ODBC 3,0)|Entero de 32 bits que determina cómo devuelve el controlador los datos de cadena. Si SQL_TRUE, el controlador devuelve datos de cadena terminados en NULL. Si SQL_FALSE, el controlador no devuelve datos de cadena terminados en NULL.<br /><br /> Este atributo tiene como valor predeterminado SQL_TRUE. Una llamada a **SQLSetEnvAttr** para establecerla en SQL_TRUE devuelve SQL_SUCCESS. Una llamada a **SQLSetEnvAttr** para establecerla en SQL_FALSE devuelve SQL_ERROR y SQLSTATE HYC00 (característica opcional no implementada).|  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Asignación de un identificador|[Función SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Devolver el valor de un atributo de entorno|[Función SQLGetEnvAttr](../../../odbc/reference/syntax/sqlgetenvattr-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Novedades de ODBC 3.8](../../../odbc/reference/what-s-new-in-odbc-3-8.md)
