---
description: Función SQLColumnPrivileges
title: Función SQLColumnPrivileges | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLColumnPrivileges
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLColumnPrivileges
helpviewer_keywords:
- SQLColumnPrivileges function [ODBC]
ms.assetid: ef233d9a-6ed5-4986-9d42-5e0b1a79fb6e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 33bf7f6289ec7c7f3167372cbdd658d3db32f28e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208431"
---
# <a name="sqlcolumnprivileges-function"></a>Función SQLColumnPrivileges
**Conformidad**  
 Versión introducida: ODBC 1,0 Standards Compliance: ODBC  
  
 **Resumen**  
 **SQLColumnPrivileges** devuelve una lista de columnas y privilegios asociados para la tabla especificada. El controlador devuelve la información como un conjunto de resultados en el *StatementHandle* especificado.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLColumnPrivileges(  
     SQLHSTMT      StatementHandle,  
     SQLCHAR *     CatalogName,  
     SQLSMALLINT   NameLength1,  
     SQLCHAR *     SchemaName,  
     SQLSMALLINT   NameLength2,  
     SQLCHAR *     TableName,  
     SQLSMALLINT   NameLength3,  
     SQLCHAR *     ColumnName,  
     SQLSMALLINT   NameLength4);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *Nombrecatálogo*  
 Entradas Nombre del catálogo. Si un controlador admite nombres para algunos catálogos pero no para otros, como cuando el controlador recupera datos de distintos DBMS, una cadena vacía ("") denota los catálogos que no tienen nombres. *Nombrecatálogo* no puede contener un patrón de búsqueda de cadenas.  
  
 Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *nombrecatálogo* se trata como un identificador y su caso no es significativo. Si se SQL_FALSE, *nombrecatálogo* es un argumento normal; se trata literalmente y su caso es significativo. Para obtener más información, vea [argumentos en funciones de catálogo](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 Entradas Longitud en caracteres de **nombrecatálogo*.  
  
 *Equivale*  
 Entradas Nombre del esquema. Si un controlador admite esquemas para algunas tablas, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, una cadena vacía ("") denota las tablas que no tienen esquemas. *SchemaName* no puede contener un patrón de búsqueda de cadenas.  
  
 Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *SchemaName* se trata como un identificador. Si se SQL_FALSE, *SchemaName* es un argumento normal; se trata literalmente y su caso es significativo.  
  
 *NameLength2*  
 Entradas Longitud en caracteres de **SchemaName*.  
  
 *TableName*  
 Entradas Nombre de la tabla. Este argumento no puede ser un puntero nulo. *TableName* no puede contener un patrón de búsqueda de cadenas.  
  
 Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *TableName* se trata como un identificador y su caso no es significativo. Si se SQL_FALSE, *TableName* es un argumento normal; se trata literalmente y su caso es significativo.  
  
 *NameLength3*  
 Entradas Longitud en caracteres de **TableName*.  
  
 *ColumnName*  
 Entradas Patrón de búsqueda de cadenas para los nombres de columna.  
  
 Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *columnName* se trata como un identificador y su caso no es significativo. Si se SQL_FALSE, *columnName* es un argumento de valor de patrón; se trata literalmente y su caso es significativo.  
  
 *NameLength4*  
 Entradas Longitud en caracteres de **columnName*.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLColumnPrivileges** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLColumnPrivileges** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|24000|Estado de cursor no válido|Un cursor estaba abierto en el *StatementHandle* y se ha llamado a **SQLFetch** o **SQLFetchScroll** . Este error lo devuelve el administrador de controladores si **SQLFetch** o **sqlfetchscroll** no ha devuelto SQL_NO_DATA y lo devuelve el controlador si **SQLFetch** o **sqlfetchscroll** ha devuelto SQL_NO_DATA.<br /><br /> Un cursor estaba abierto en el *StatementHandle*, pero no se ha llamado a **SQLFetch** o **SQLFetchScroll** .|  
|40001|Error de serialización|La transacción se revirtió debido a un interbloqueo de recursos con otra transacción.|  
|40003|Finalización de instrucciones desconocida|No se pudo establecer la conexión asociada durante la ejecución de esta función y no se puede determinar el estado de la transacción.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle*. A continuación, se llamó de nuevo a la función en *StatementHandle*.<br /><br /> Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY009|Uso no válido de puntero nulo|El argumento *TableName* era un puntero nulo.<br /><br /> El atributo de instrucción SQL_ATTR_METADATA_ID se estableció en SQL_TRUE, el argumento *nombrecatálogo* era un puntero nulo y el SQL_CATALOG_NAME *InfoType* devuelve los nombres de catálogo que se admiten.<br /><br /> (DM) el atributo de instrucción SQL_ATTR_METADATA_ID se estableció en SQL_TRUE y el argumento *SchemaName* o *columnName* era un puntero nulo.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|(DM) el valor de uno de los argumentos de longitud de nombre era menor que 0 pero no es igual a SQL_NTS.|  
|||El valor de uno de los argumentos de longitud de nombre ha superado el valor de longitud máxima para el nombre correspondiente. (Vea "Comentarios").|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|Se ha especificado un nombre de catálogo y el controlador o el origen de datos no admiten catálogos.<br /><br /> Se ha especificado un nombre de esquema y el controlador o el origen de datos no admiten esquemas.<br /><br /> Se especificó un patrón de búsqueda de cadena para el nombre de columna y el origen de datos no admite patrones de búsqueda para ese argumento.<br /><br /> La combinación de los valores actuales de los atributos de instrucción SQL_CONCURRENCY y SQL_CURSOR_TYPE no era compatible con el controlador o el origen de datos.<br /><br /> El atributo de instrucción SQL_ATTR_USE_BOOKMARKS se estableció en SQL_UB_VARIABLE y el atributo de instrucción SQL_ATTR_CURSOR_TYPE se estableció en un tipo de cursor para el que el controlador no admite marcadores.|  
|HYT00|Tiempo de espera agotado|El período de tiempo de espera de consulta expiró antes de que el origen de datos devolviera el conjunto de resultados. El período de tiempo de espera se establece a través de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
## <a name="comments"></a>Comentarios  
 **SQLColumnPrivileges** devuelve los resultados como un conjunto de resultados estándar, ordenados por TABLE_CAT, TABLE_SCHEM, TABLE_NAME, column_name y Privilege.  
  
> [!NOTE]  
>  Es posible que **SQLColumnPrivileges** no devuelva privilegios para todas las columnas. Por ejemplo, es posible que un controlador no devuelva información sobre los privilegios de las pseudo columnas, como el ROWID de Oracle. Las aplicaciones pueden utilizar cualquier columna válida, independientemente de si la devuelve **SQLColumnPrivileges**.  
  
 Las longitudes de las columnas VARCHAR no se muestran en la tabla; las longitudes reales dependen del origen de datos. Para determinar las longitudes reales de las columnas CATALOG_NAME, SCHEMA_NAME, TABLE_NAME y COLUMN_NAME, una aplicación puede llamar a **SQLGetInfo** con las opciones SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN, SQL_MAX_TABLE_NAME_LEN y SQL_MAX_COLUMN_NAME_LEN.  
  
> [!NOTE]  
>  Para obtener más información sobre el uso general, los argumentos y los datos devueltos de las funciones de catálogo de ODBC, vea [funciones de catálogo](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 Se ha cambiado el nombre de las siguientes columnas para ODBC 3. *x*. Los cambios de nombre de columna no afectan a la compatibilidad con versiones anteriores porque las aplicaciones se enlazan por número de columna.  
  
|Columna ODBC 2,0|ODBC 3. columna *x*|  
|---------------------|-----------------------|  
|TABLE_QUALIFIER|TABLE_CAT|  
|TABLE_OWNER|TABLE_SCHEM|  
  
 En la tabla siguiente se enumeran las columnas del conjunto de resultados. El controlador puede definir columnas adicionales más allá de la columna 8 (IS_GRANTABLE). Una aplicación debe obtener acceso a las columnas específicas del controlador contando desde el final del conjunto de resultados en lugar de especificar una posición ordinal explícita. Para obtener más información, vea [datos devueltos por las funciones de catálogo](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nombre de la columna|Número de columna|Tipo de datos|Comentarios|  
|-----------------|-------------------|---------------|--------------|  
|TABLE_CAT (ODBC 1,0)|1|Varchar|Identificador del catálogo; ES NULL si no es aplicable al origen de datos. Si un controlador admite catálogos para algunas tablas, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, devuelve una cadena vacía ("") para las tablas que no tienen catálogos.|  
|TABLE_SCHEM (ODBC 1,0)|2|Varchar|Identificador de esquema; ES NULL si no es aplicable al origen de datos. Si un controlador admite esquemas para algunas tablas, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, devuelve una cadena vacía ("") para las tablas que no tienen esquemas.|  
|TABLE_NAME (ODBC 1,0)|3|VARCHAR NOT NULL|Identificador de tabla.|  
|COLUMN_NAME (ODBC 1,0)|4|VARCHAR NOT NULL|Nombre de la columna. El controlador devuelve una cadena vacía para una columna que no tiene un nombre.|  
|GRANTOR (ODBC 1,0)|5|Varchar|Nombre del usuario que concedió el privilegio. ES NULL si no es aplicable al origen de datos.<br /><br /> Para todas las filas en las que el valor de la columna GRANTEE sea el propietario del objeto, la columna de GRANTOR será "_SYSTEM".|  
|RECEPTOR (ODBC 1,0)|6|VARCHAR NOT NULL|Nombre del usuario al que se concedió el privilegio.|  
|PRIVILEGIO (ODBC 1,0)|7|VARCHAR NOT NULL|Identifica el privilegio de columna. Puede ser uno de los siguientes (u otros admitidos por el origen de datos cuando se define la implementación):<br /><br /> SELECT: el receptor tiene permiso para recuperar los datos de la columna.<br /><br /> INSERT: el receptor puede proporcionar datos para la columna en nuevas filas que se insertan en la tabla asociada.<br /><br /> UPDATE: el receptor puede actualizar los datos de la columna.<br /><br /> REFERENCIAS: el receptor tiene permiso para hacer referencia a la columna dentro de una restricción (por ejemplo, una restricción UNIQUE, referencial o de comprobación de tabla).|  
|IS_GRANTABLE (ODBC 1,0)|8|Varchar|Indica si el receptor tiene permiso para conceder el privilegio a otros usuarios; "Sí", "NO" o "NULL" si se desconoce o no es aplicable al origen de datos.<br /><br /> Un privilegio se puede conceder o no, pero no ambos. El conjunto de resultados devuelto por **SQLColumnPrivileges** nunca contendrá dos filas para las que todas las columnas excepto la columna IS_GRANTABLE contengan el mismo valor.|  
  
## <a name="code-example"></a>Ejemplo de código  
 Para obtener un ejemplo de código de una función similar, vea [SQLColumns (función](../../../odbc/reference/syntax/sqlcolumns-function.md)).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar un búfer a una columna de un conjunto de resultados|[SQLBindCol (función)](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Devolver las columnas de una tabla o tablas|[Función SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md)|  
|Obtener un bloque de datos o desplazarse por un conjunto de resultados|[Función SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Obtener varias filas de datos|[Función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Devolver privilegios para una tabla o tablas|[Función SQLTablePrivileges](../../../odbc/reference/syntax/sqltableprivileges-function.md)|  
|Devolver una lista de tablas de un origen de datos|[Función SQLTables](../../../odbc/reference/syntax/sqltables-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
