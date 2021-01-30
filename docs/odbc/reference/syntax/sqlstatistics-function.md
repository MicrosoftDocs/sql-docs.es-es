---
description: Función SQLStatistics
title: SQLStatistics (función) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLStatistics
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLStatistics
helpviewer_keywords:
- SQLStatistics function [ODBC]
ms.assetid: 45210682-cfea-4e5d-9951-bcf1cbe10f41
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 711efa4797855e42b278e519106c38d62fda1817
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191747"
---
# <a name="sqlstatistics-function"></a>Función SQLStatistics
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: ISO 92  
  
 **Resumen**  
 **SQLStatistics** recupera una lista de estadísticas sobre una sola tabla y los índices asociados a la tabla. El controlador devuelve la información como un conjunto de resultados.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLStatistics(  
     SQLHSTMT        StatementHandle,  
     SQLCHAR *       CatalogName,  
     SQLSMALLINT     NameLength1,  
     SQLCHAR *       SchemaName,  
     SQLSMALLINT     NameLength2,  
     SQLCHAR *       TableName,  
     SQLSMALLINT     NameLength3,  
     SQLUSMALLINT    Unique,  
     SQLUSMALLINT    Reserved);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *Nombrecatálogo*  
 Entradas Nombre del catálogo. Si un controlador admite catálogos para algunas tablas, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, una cadena vacía ("") indica las tablas que no tienen catálogos. *Nombrecatálogo* no puede contener un patrón de búsqueda de cadenas.  
  
 Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *nombrecatálogo* se trata como un identificador y su caso no es significativo. Si se SQL_FALSE, *nombrecatálogo* es un argumento normal; se trata literalmente y su caso es significativo. Para obtener más información, vea [argumentos en funciones de catálogo](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 Entradas Longitud en caracteres de **nombrecatálogo*.  
  
 *Equivale*  
 Entradas Nombre del esquema. Si un controlador admite esquemas para algunas tablas, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, una cadena vacía ("") indica las tablas que no tienen esquemas. *SchemaName* no puede contener un patrón de búsqueda de cadenas.  
  
 Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *SchemaName* se trata como un identificador y su caso no es significativo. Si se SQL_FALSE, *SchemaName* es un argumento normal; se trata literalmente y su caso es significativo.  
  
 *NameLength2*  
 Entradas Longitud en caracteres de **SchemaName*.  
  
 *TableName*  
 Entradas Nombre de la tabla. Este argumento no puede ser un puntero nulo. *SchemaName* no puede contener un patrón de búsqueda de cadenas.  
  
 Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *TableName* se trata como un identificador y su caso no es significativo. Si se SQL_FALSE, *TableName* es un argumento normal; se trata literalmente y su caso es significativo.  
  
 *NameLength3*  
 Entradas Longitud en caracteres de **TableName*.  
  
 *Único*  
 Entradas Tipo de índice: SQL_INDEX_UNIQUE o SQL_INDEX_ALL.  
  
 *Reserved*  
 Entradas Indica la importancia de las columnas de CARDINALidad y páginas en el conjunto de resultados. Las siguientes opciones afectan únicamente a la devolución de las columnas de CARDINALidad y páginas. la información del índice se devuelve aunque no se devuelvan la CARDINALidad y las páginas.  
  
 SQL_ENSURE solicita que el controlador recupere las estadísticas de forma incondicional. (Los controladores que solo se ajustan al estándar Open Group y no admiten extensiones ODBC no podrán admitir SQL_ENSURE).  
  
 SQL_QUICK solicita que el controlador recupere la CARDINALidad y las páginas solo si están disponibles en el servidor. En este caso, el controlador no garantiza que los valores sean actuales. (Las aplicaciones que se escriben en el estándar de grupo abierto siempre obtendrán SQL_QUICK comportamiento de los controladores compatibles con ODBC *3. x*).  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLStatistics** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que suele devolver **SQLStatistics** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|24000|Estado de cursor no válido|Un cursor estaba abierto en el *StatementHandle* y se ha llamado a **SQLFetch** o **SQLFetchScroll** . Este error lo devuelve el administrador de controladores si **SQLFetch** o **sqlfetchscroll** no ha devuelto SQL_NO_DATA y lo devuelve el controlador si **SQLFetch** o **sqlfetchscroll** ha devuelto SQL_NO_DATA.<br /><br /> Un cursor estaba abierto en el *StatementHandle*, pero no se ha llamado a **SQLFetch** o **SQLFetchScroll** .|  
|40001|Error de serialización|La transacción se revirtió debido a un interbloqueo de recursos con otra transacción.|  
|40003|Finalización de instrucciones desconocida|No se pudo establecer la conexión asociada durante la ejecución de esta función y no se puede determinar el estado de la transacción.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle* y, a continuación, se llamó de nuevo a la función en *StatementHandle*.<br /><br /> Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY009|Uso no válido de puntero nulo|El argumento *TableName* era un puntero nulo.<br /><br /> El atributo de instrucción SQL_ATTR_METADATA_ID se estableció en SQL_TRUE, el argumento *nombrecatálogo* era un puntero nulo y el SQL_CATALOG_NAME *InfoType* devuelve los nombres de catálogo que se admiten.<br /><br /> (DM) el atributo de instrucción SQL_ATTR_METADATA_ID se estableció en SQL_TRUE y el argumento *SchemaName* era un puntero nulo.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función **SQLStatistics** .<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|(DM) el valor de uno de los argumentos de longitud de nombre era menor que 0 pero no es igual a SQL_NTS.<br /><br /> El valor de uno de los argumentos de longitud de nombre ha superado el valor de longitud máxima para el nombre correspondiente.|  
|HY100|Tipo de opción de unicidad fuera del intervalo|(DM) se especificó un valor *único* no válido.|  
|HY101|Tipo de opción de precisión fuera del intervalo|(DM) se especificó un valor *reservado* no válido.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|Se ha especificado un catálogo y el controlador o el origen de datos no admiten catálogos.<br /><br /> Se ha especificado un esquema y el controlador o el origen de datos no admiten esquemas.<br /><br /> La combinación de los valores actuales de los atributos de instrucción SQL_ATTR_CONCURRENCY y SQL_ATTR_CURSOR_TYPE no era compatible con el controlador o el origen de datos.<br /><br /> El atributo de instrucción SQL_ATTR_USE_BOOKMARKS se estableció en SQL_UB_VARIABLE y el atributo de instrucción SQL_ATTR_CURSOR_TYPE se estableció en un tipo de cursor para el que el controlador no admite marcadores.|  
|HYT00|Tiempo de espera agotado|El período de tiempo de espera de consulta expiró antes de que el origen de datos devolviera el conjunto de resultados solicitado. El período de tiempo de espera se establece a través de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
## <a name="comments"></a>Comentarios  
 **SQLStatistics** devuelve información acerca de una sola tabla como un conjunto de resultados estándar, ordenados por NON_UNIQUE, tipo, INDEX_QUALIFIER, INDEX_NAME y ORDINAL_POSITION. El conjunto de resultados combina la información de estadísticas (en las columnas CARDINALidad y páginas del conjunto de resultados) de la tabla con información sobre cada índice. Para obtener información sobre cómo se puede usar esta información, vea [usos de los datos del catálogo](../../../odbc/reference/develop-app/uses-of-catalog-data.md).  
  
 Para determinar las longitudes reales de las columnas TABLE_CAT, TABLE_SCHEM, TABLE_NAME y COLUMN_NAME, una aplicación puede llamar a **SQLGetInfo** con las opciones SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN, SQL_MAX_TABLE_NAME_LEN y SQL_MAX_COLUMN_NAME_LEN.  
  
> [!NOTE]  
>  Para obtener más información sobre el uso general, los argumentos y los datos devueltos de las funciones de catálogo de ODBC, vea [funciones de catálogo](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 Se ha cambiado el nombre de las siguientes columnas para ODBC *3. x*. Los cambios de nombre de columna no afectan a la compatibilidad con versiones anteriores porque las aplicaciones se enlazan por número de columna.  
  
|Columna ODBC 2,0|Columna ODBC *3. x*|  
|---------------------|-----------------------|  
|TABLE_QUALIFIER|TABLE_CAT|  
|TABLE_OWNER|TABLE_SCHEM|  
|SEQ_IN_INDEX|ORDINAL_POSITION|  
|COLLATION|ASC_OR_DESC|  
  
 En la tabla siguiente se enumeran las columnas del conjunto de resultados. El controlador puede definir columnas adicionales más allá de la columna 13 (FILTER_CONDITION). Una aplicación debe obtener acceso a las columnas específicas del controlador contando desde el final del conjunto de resultados en lugar de especificar una posición ordinal explícita. Para obtener más información, vea [datos devueltos por las funciones de catálogo](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nombre de la columna|Número de columna|Tipo de datos|Comentarios|  
|-----------------|-------------------|---------------|--------------|  
|TABLE_CAT (ODBC 1,0)|1|Varchar|Nombre del catálogo de la tabla a la que se aplica la estadística o el índice; ES NULL si no es aplicable al origen de datos. Si un controlador admite catálogos para algunas tablas, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, devuelve una cadena vacía ("") para las tablas que no tienen catálogos.|  
|TABLE_SCHEM (ODBC 1,0)|2|Varchar|Nombre del esquema de la tabla a la que se aplica la estadística o el índice; ES NULL si no es aplicable al origen de datos. Si un controlador admite esquemas para algunas tablas, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, devuelve una cadena vacía ("") para las tablas que no tienen esquemas.|  
|TABLE_NAME (ODBC 1,0)|3|VARCHAR NOT NULL|Nombre de tabla de la tabla a la que se aplica la estadística o el índice.|  
|NON_UNIQUE (ODBC 1,0)|4|Smallint|Indica si el índice no permite valores duplicados:<br /><br /> SQL_TRUE si los valores de índice pueden ser no únicos.<br /><br /> SQL_FALSE si los valores de índice deben ser únicos.<br /><br /> Se devuelve NULL si el tipo es SQL_TABLE_STAT.|  
|INDEX_QUALIFIER (ODBC 1,0)|5|Varchar|Identificador que se utiliza para calificar el nombre del índice que realiza una **Drop index**; Se devuelve NULL si el origen de datos no admite un calificador de índice o si el tipo es SQL_TABLE_STAT. Si en esta columna se devuelve un valor distinto de NULL, se debe usar para calificar el nombre del índice en una instrucción **Drop index** ; de lo contrario, se debe usar el TABLE_SCHEM para calificar el nombre del índice.|  
|INDEX_NAME (ODBC 1,0)|6|Varchar|Nombre del índice; Se devuelve NULL si el tipo es SQL_TABLE_STAT.|  
|TIPO (ODBC 1,0)|7|Smallint no NULL|Tipo de información que se devuelve:<br /><br /> SQL_TABLE_STAT indica una estadística de la tabla (en la columna CARDINALidad o páginas).<br /><br /> SQL_INDEX_BTREE indica un índice de árbol B.<br /><br /> SQL_INDEX_CLUSTERED indica un índice clúster.<br /><br /> SQL_INDEX_CONTENT indica un índice de contenido.<br /><br /> SQL_INDEX_HASHED indica un índice hash.<br /><br /> SQL_INDEX_OTHER indica otro tipo de índice.|  
|ORDINAL_POSITION (ODBC 1,0)|8|Smallint|Número de secuencia de la columna en el índice (a partir de 1); Se devuelve NULL si el tipo es SQL_TABLE_STAT.|  
|COLUMN_NAME (ODBC 1,0)|9|Varchar|Nombre de la columna. Si la columna se basa en una expresión, como salario + beneficios, se devuelve la expresión; Si no se puede determinar la expresión, se devuelve una cadena vacía. Se devuelve NULL si el tipo es SQL_TABLE_STAT.|  
|ASC_OR_DESC (ODBC 1,0)|10|Char(1)|Secuencia de ordenación para la columna: "A" para ascendente; "D" para descendente; Se devuelve NULL si el origen de datos no admite la secuencia de ordenación de columnas o si el tipo es SQL_TABLE_STAT.|  
|CARDINALIDAD (ODBC 1,0)|11|Entero|Cardinalidad de la tabla o el índice; número de filas de la tabla si el tipo es SQL_TABLE_STAT; número de valores únicos del índice si el tipo no es SQL_TABLE_STAT; Se devuelve NULL si el valor no está disponible en el origen de datos.|  
|PÁGINAS (ODBC 1,0)|12|Entero|Número de páginas utilizadas para almacenar el índice o la tabla; número de páginas de la tabla si el tipo es SQL_TABLE_STAT; número de páginas del índice si el tipo no es SQL_TABLE_STAT; Se devuelve NULL si el valor no está disponible en el origen de datos o si no es aplicable al origen de datos.|  
|FILTER_CONDITION (ODBC 2,0)|13|Varchar|Si el índice es un índice filtrado, esta es la condición de filtro, como salario > 30000; Si no se puede determinar la condición de filtro, se trata de una cadena vacía.<br /><br /> NULL si el índice no es un índice filtrado, no se puede determinar si el índice es un índice filtrado o si el tipo es SQL_TABLE_STAT.|  
  
 Si la fila del conjunto de resultados corresponde a una tabla, el controlador establece el tipo en SQL_TABLE_STAT y establece NON_UNIQUE, INDEX_QUALIFIER, INDEX_NAME, ORDINAL_POSITION, COLUMN_NAME y ASC_OR_DESC en NULL. Si la CARDINALidad o las páginas no están disponibles en el origen de datos, el controlador las establece en NULL.  
  
## <a name="code-example"></a>Ejemplo de código  
 Para obtener un ejemplo de código de una función similar, vea [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar un búfer a una columna de un conjunto de resultados|[SQLBindCol (función)](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Capturar una sola fila o un bloque de datos en una dirección de solo avance.|[Función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Obtener un bloque de datos o desplazarse por un conjunto de resultados|[Función SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Devolver las columnas de claves externas|[Función SQLForeignKeys](../../../odbc/reference/syntax/sqlforeignkeys-function.md)|  
|Devolver las columnas de una clave principal|[Función SQLPrimaryKeys](../../../odbc/reference/syntax/sqlprimarykeys-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
