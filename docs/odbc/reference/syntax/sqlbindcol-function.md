---
description: SQLBindCol (función)
title: SQLBindCol (función) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLBindCol
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLBindCol
helpviewer_keywords:
- SQLBindCol function [ODBC]
ms.assetid: 41a37655-84cd-423f-9daa-e0b47b88dc54
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d057b3e7b5842cafba2be18afa21f2d528175ffd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181094"
---
# <a name="sqlbindcol-function"></a>SQLBindCol (función)
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: ISO 92  
  
 **Resumen**  
 **SQLBindCol** enlaza los búferes de datos de la aplicación a las columnas del conjunto de resultados.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLBindCol(  
      SQLHSTMT       StatementHandle,  
      SQLUSMALLINT   ColumnNumber,  
      SQLSMALLINT    TargetType,  
      SQLPOINTER     TargetValuePtr,  
      SQLLEN         BufferLength,  
      SQLLEN *       StrLen_or_IndPtr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *ColumnNumber*  
 Entradas Número de la columna del conjunto de resultados que se va a enlazar. Las columnas se numeran al aumentar el orden de las columnas empezando por 0, donde la columna 0 es la columna de marcador. Si no se utilizan marcadores, es decir, el atributo de instrucción SQL_ATTR_USE_BOOKMARKS está establecido en SQL_UB_OFF-los números de columna empiezan en 1.  
  
 *TargetType*  
 Entradas Identificador del tipo de datos C del \* búfer *TargetValuePtr* . Cuando se recuperan datos del origen de datos con **SQLFetch**, **SQLFetchScroll**, **SQLBulkOperations** o **SQLSetPos**, el controlador convierte los datos a este tipo. Cuando envía datos al origen de datos con **SQLBulkOperations** o **SQLSetPos**, el controlador convierte los datos de este tipo. Para obtener una lista de tipos de datos e identificadores de tipo de C válidos, vea la sección tipos de datos de [c](../../../odbc/reference/appendixes/c-data-types.md) en el Apéndice D: tipos de datos.  
  
 Si el argumento *TargetType* es un tipo de datos de intervalo, se usan para los datos la precisión inicial del intervalo predeterminado (2) y la precisión de segundos del intervalo predeterminado (6), tal como se establece en los campos SQL_DESC_DATETIME_INTERVAL_PRECISION y SQL_DESC_PRECISION de ARD, respectivamente. Si el argumento *TargetType* es SQL_C_NUMERIC, se usa la precisión predeterminada (definida por el controlador) y la escala predeterminada (0), como se establece en los campos SQL_DESC_PRECISION y SQL_DESC_SCALE de ARD, para los datos. Si una precisión o escala predeterminada no es adecuada, la aplicación debe establecer explícitamente el campo de descriptor adecuado mediante una llamada a **SQLSetDescField** o **SQLSetDescRec**.  
  
 También puede especificar un tipo de datos de C extendido. Para obtener más información, vea el tema sobre [tipos de datos C en ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md).  
  
 *TargetValuePtr*  
 [Entrada/salida diferida] Puntero al búfer de datos que se va a enlazar a la columna. **SQLFetch** y **SQLFetchScroll** devuelven datos en este búfer. **SQLBulkOperations** devuelve datos en este búfer cuando se SQL_FETCH_BY_BOOKMARK la *operación* ; Recupera datos de este búfer cuando la *operación* es SQL_ADD o SQL_UPDATE_BY_BOOKMARK. **SQLSetPos** devuelve datos en este búfer cuando se SQL_REFRESH la *operación* ; Recupera datos de este búfer cuando se SQL_UPDATE la *operación* .  
  
 Si *TargetValuePtr* es un puntero nulo, el controlador desenlaza el búfer de datos de la columna. Una aplicación puede Desenlazar todas las columnas llamando a **SQLFreeStmt** con la opción SQL_UNBIND. Una aplicación puede desenlazar el búfer de datos de una columna pero todavía tiene un búfer de longitud/indicador enlazado para la columna, si el argumento *TargetValuePtr* de la llamada a **SQLBindCol** es un puntero nulo, pero el argumento *StrLen_or_IndPtr* es un valor válido.  
  
 *BufferLength*  
 Entradas Longitud del búfer de \* *TargetValuePtr* en bytes.  
  
 El controlador usa *BufferLength* para evitar escribir más allá del final del \* búfer de *TargetValuePtr* cuando devuelve datos de longitud variable, como datos de caracteres o binarios. Observe que el controlador cuenta el carácter de terminación NULL cuando devuelve datos de caracteres a \* *TargetValuePtr*. \*Por lo tanto, *TargetValuePtr* debe contener espacio para el carácter de terminación null o el controlador truncará los datos.  
  
 Cuando el controlador devuelve datos de longitud fija, como un entero o una estructura de fecha, el controlador omite *BufferLength* y asume que el búfer es lo suficientemente grande como para contener los datos. Por lo tanto, es importante que la aplicación asigne un búfer suficientemente grande para los datos de longitud fija o que el controlador escriba más allá del final del búfer.  
  
 **SQLBindCol** devuelve SQLSTATE HY090 (longitud de búfer o cadena no válida) cuando *BufferLength* es menor que 0 pero no cuando *BufferLength* es 0. Sin embargo, si *TargetType* especifica un tipo de carácter, una aplicación no debe establecer *BufferLength* en 0, porque los controladores compatibles con ISO CLI devuelven SQLSTATE HY090 (cadena o longitud de búfer no válida) en ese caso.  
  
 *StrLen_or_IndPtr*  
 [Entrada/salida diferida] Puntero al búfer de longitud/indicador que se va a enlazar a la columna. **SQLFetch** y **SQLFetchScroll** devuelven un valor en este búfer. **SQLBulkOperations** recupera un valor de este búfer cuando la *operación* es SQL_ADD, SQL_UPDATE_BY_BOOKMARK o SQL_DELETE_BY_BOOKMARK. **SQLBulkOperations** devuelve un valor en este búfer cuando se SQL_FETCH_BY_BOOKMARK la *operación* . **SQLSetPos** devuelve un valor en este búfer cuando se SQL_REFRESH la *operación* ; Recupera un valor de este búfer cuando se SQL_UPDATE la *operación* .  
  
 **SQLFetch**, **SQLFetchScroll**, **SQLBulkOperations** y **SQLSetPos** pueden devolver los siguientes valores en el búfer de longitud/indicador:  
  
-   La longitud de los datos disponibles para devolver  
  
-   SQL_NO_TOTAL  
  
-   SQL_NULL_DATA  
  
 La aplicación puede incluir los siguientes valores en el búfer de longitud/indicador para su uso con **SQLBulkOperations** o **SQLSetPos**:  
  
-   La longitud de los datos que se envían  
  
-   SQL_NTS  
  
-   SQL_NULL_DATA  
  
-   SQL_DATA_AT_EXEC  
  
-   Resultado de la macro SQL_LEN_DATA_AT_EXEC  
  
-   SQL_COLUMN_IGNORE  
  
 Si el búfer del indicador y el búfer de longitud son búferes independientes, el búfer del indicador solo puede devolver SQL_NULL_DATA, mientras que el búfer de longitud puede devolver todos los demás valores.  
  
 Para obtener más información, vea [función SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md), [función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md), [función SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)y [uso de valores de longitud/indicador](../../../odbc/reference/develop-app/using-length-and-indicator-values.md).  
  
 Si *StrLen_or_IndPtr* es un puntero nulo, no se usa ninguna longitud ni valor de indicador. Se trata de un error al capturar datos y los datos son NULL.  
  
 Consulte la [información de ODBC 64](../../../odbc/reference/odbc-64-bit-information.md)bits, si la aplicación se ejecutará en un sistema operativo de 64 bits.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLBindCol** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que suele devolver **SQLBindCol** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|07006|Infracción de atributo de tipo de datos restringido|(DM) el argumento *ColumnNumber* era 0 y el argumento *TargetType* no se SQL_C_BOOKMARK ni SQL_C_VARBOOKMARK.|  
|07009|Índice de descriptor no válido|El valor especificado para el argumento *ColumnNumber* ha superado el número máximo de columnas del conjunto de resultados.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY003|Tipo de búfer de aplicación no válido|El *TargetType* del argumento no era un tipo de datos válido ni SQL_C_DEFAULT.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a **SQLBindCol** .<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|(DM) el valor especificado para el argumento *BufferLength* era menor que 0.<br /><br /> (DM) el controlador era ODBC 2. *x* , el argumento *ColumnNumber* se estableció en 0 y el valor especificado para el argumento *BufferLength* no era igual a 4.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|El controlador o el origen de datos no admite la conversión especificada por la combinación del argumento *TargetType* y el tipo de datos SQL específico del controlador de la columna correspondiente.<br /><br /> El argumento *ColumnNumber* era 0 y el controlador no admite marcadores.<br /><br /> El controlador solo admite ODBC 2. *x* y el argumento *TargetType* era uno de los siguientes:<br /><br /> SQL_C_NUMERIC SQL_C_SBIGINT SQL_C_UBIGINT<br /><br /> y cualquiera de los tipos de datos de Interval C enumerados en los [tipos de datos de c](../../../odbc/reference/appendixes/c-data-types.md) en los tipos de datos Apéndice D:.<br /><br /> El controlador solo es compatible con las versiones ODBC anteriores a 3,50 y el argumento *TargetType* se SQL_C_GUID.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
  
## <a name="comments"></a>Comentarios  
 **SQLBindCol** se utiliza para asociar o *enlazar* columnas del conjunto de resultados a búferes de datos y búferes de longitud/indicador en la aplicación. Cuando la aplicación llama a **SQLFetch**, **SQLFetchScroll** o **SQLSetPos** para capturar datos, el controlador devuelve los datos de las columnas enlazadas en los búferes especificados; para obtener más información, vea [función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md). Cuando la aplicación llama a **SQLBulkOperations** para actualizar o insertar una fila o **SQLSetPos** para actualizar una fila, el controlador recupera los datos de las columnas enlazadas de los búferes especificados; para obtener más información, vea [función SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md) o [función SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md). Para obtener más información sobre el enlace, vea [recuperar resultados (Basic)](../../../odbc/reference/develop-app/retrieving-results-basic.md).  
  
 Tenga en cuenta que no es necesario enlazar columnas para recuperar datos de ellas. Una aplicación también puede llamar a **SQLGetData** para recuperar datos de columnas. Aunque es posible enlazar algunas columnas de una fila y llamar a **SQLGetData** para otras, esto está sujeto a algunas restricciones. Para obtener más información, consulte [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md).  
  
## <a name="binding-unbinding-and-rebinding-columns"></a>Enlazar, Desenlazar y reenlazar columnas  
 Una columna se puede enlazar, desenlazar o volver a enlazar en cualquier momento, incluso después de que se hayan recuperado los datos del conjunto de resultados. El nuevo enlace surte efecto la próxima vez que se llama a una función que utiliza enlaces. Por ejemplo, supongamos que una aplicación enlaza las columnas de un conjunto de resultados y llama a **SQLFetch**. El controlador devuelve los datos en los búferes enlazados. Ahora Supongamos que la aplicación enlaza las columnas a un conjunto diferente de búferes. El controlador no coloca los datos de la fila recién capturada en los búferes recién enlazados. En su lugar, espera hasta que se llama a **SQLFetch** de nuevo y, a continuación, coloca los datos para la siguiente fila de los búferes recién enlazados.  
  
> [!NOTE]  
>  El atributo de instrucción SQL_ATTR_USE_BOOKMARKS debe establecerse siempre antes de enlazar una columna a la columna 0. Esto no es necesario, pero se recomienda encarecidamente.  
  
## <a name="binding-columns"></a>Enlazar columnas  
 Para enlazar una columna, una aplicación llama a **SQLBindCol** y pasa el número de columna, el tipo, la dirección y la longitud de un búfer de datos, así como la dirección de un búfer de longitud/indicador. Para obtener información sobre cómo se usan estas direcciones, vea "direcciones de búfer", más adelante en esta sección. Para obtener más información acerca de cómo enlazar columnas, vea [usar SQLBindCol](../../../odbc/reference/develop-app/using-sqlbindcol.md).  
  
 El uso de estos búferes se aplaza; es decir, la aplicación los enlaza en **SQLBindCol** pero el controlador accede a ellos desde otras funciones, es decir, **SQLBulkOperations**, **SQLFetch**, **SQLFetchScroll** o **SQLSetPos**. Es responsabilidad de la aplicación asegurarse de que los punteros especificados en **SQLBindCol** siguen siendo válidos siempre que el enlace permanezca en vigor. Si la aplicación permite que estos punteros dejen de ser válidos; por ejemplo, libera un búfer y, a continuación, llama a una función que espera que sea válido, las consecuencias son indefinidas. Para obtener más información, vea [búferes diferidos](../../../odbc/reference/develop-app/deferred-buffers.md).  
  
 El enlace permanece en vigor hasta que se reemplaza por un nuevo enlace, se desenlaza la columna o se libera la instrucción.  
  
## <a name="unbinding-columns"></a>Desenlazar columnas  
 Para desenlazar una sola columna, una aplicación llama a **SQLBindCol** con *ColumnNumber* establecido en el número de esa columna y *TargetValuePtr* establecido en un puntero nulo. Si *ColumnNumber* hace referencia a una columna sin enlazar, **SQLBindCol** sigue devolverá SQL_SUCCESS.  
  
 Para desenlazar todas las columnas, una aplicación llama a **SQLFreeStmt** con *fOption* establecido en SQL_UNBIND. Esto también se puede lograr si se establece el campo de SQL_DESC_COUNT de ARD en cero.  
  
## <a name="rebinding-columns"></a>Reenlazar columnas  
 Una aplicación puede realizar cualquiera de las dos operaciones para cambiar un enlace:  
  
-   Llame a **SQLBindCol** para especificar un nuevo enlace para una columna que ya esté enlazada. El controlador sobrescribe el enlace anterior con el nuevo.  
  
-   Especifique el desplazamiento que se va a agregar a la dirección del búfer especificado por la llamada de enlace a **SQLBindCol**. Para obtener más información, vea la sección siguiente, "desplazamientos de enlace".  
  
## <a name="binding-offsets"></a>Desplazamientos de enlace  
 Un desplazamiento de enlace es un valor que se agrega a las direcciones de los búferes de datos y longitud/indicador (tal y como se especifica en el argumento *TargetValuePtr* y *StrLen_or_IndPtr* ) antes de que se desreferencian. Cuando se usan desplazamientos, los enlaces son una "plantilla" de cómo se diseñan los búferes de la aplicación y la aplicación puede moverla a diferentes áreas de memoria cambiando el desplazamiento. Dado que el mismo desplazamiento se agrega a cada dirección de cada enlace, los desplazamientos relativos entre los búferes de diferentes columnas deben ser iguales en cada conjunto de búferes. Siempre es true cuando se usa el enlace de modo de fila; la aplicación debe diseñar cuidadosamente sus búferes para que esto sea cierto cuando se usa el enlace de modo de columna.  
  
 El uso de un desplazamiento de enlace tiene básicamente el mismo efecto que reenlazar una columna llamando a **SQLBindCol**. La diferencia es que una nueva llamada a **SQLBindCol** especifica nuevas direcciones para el búfer de datos y el búfer de longitud/indicador, mientras que el uso de un desplazamiento de enlace no cambia las direcciones, sino que solo agrega un desplazamiento a ellas. La aplicación puede especificar un nuevo desplazamiento cada vez que lo desee, y este desplazamiento siempre se agrega a las direcciones enlazadas originalmente. En concreto, si el desplazamiento se establece en 0 o si el atributo de instrucción está establecido en un puntero nulo, el controlador utiliza las direcciones enlazadas originalmente.  
  
 Para especificar un desplazamiento de enlace, la aplicación establece el atributo de instrucción SQL_ATTR_ROW_BIND_OFFSET_PTR en la dirección de un búfer SQLINTEGER donde. Antes de que la aplicación llame a una función que usa enlaces, coloca un desplazamiento en bytes en este búfer. Para determinar la dirección del búfer que se va a usar, el controlador agrega el desplazamiento a la dirección en el enlace. La suma de la dirección y el desplazamiento debe ser una dirección válida, pero no es necesario que la dirección a la que se agrega el desplazamiento sea válida. Para obtener más información sobre cómo se usan los desplazamientos de enlace, vea "direcciones de búfer" más adelante en esta sección.  
  
## <a name="binding-arrays"></a>Enlazar matrices  
 Si el tamaño del conjunto de filas (el valor del atributo de instrucción SQL_ATTR_ROW_ARRAY_SIZE) es mayor que 1, la aplicación enlaza matrices de búferes en lugar de búferes únicos. Para obtener más información, vea [cursores de bloque](../../../odbc/reference/develop-app/block-cursors.md).  
  
 La aplicación puede enlazar matrices de dos maneras:  
  
-   Enlazar una matriz a cada columna. Esto se conoce como enlace de modo de *columna* porque cada estructura de datos (matriz) contiene datos de una sola columna.  
  
-   Defina una estructura que contenga los datos de una fila completa y enlace una matriz de estas estructuras. Esto se conoce como *enlace de* modo de fila porque cada estructura de datos contiene los datos de una sola fila.  
  
 Cada matriz de búferes debe tener al menos tantos elementos como el tamaño del conjunto de filas.  
  
> [!NOTE]  
>  Una aplicación debe comprobar que la alineación es válida. Para obtener más información acerca de las consideraciones de alineación, vea [alignment](../../../odbc/reference/develop-app/alignment.md).  
  
## <a name="column-wise-binding"></a>El enlace  
 En el enlace de modo de columna, la aplicación enlaza datos separados y matrices de longitud/indicador a cada columna.  
  
 Para usar el enlace de modo de columna, la aplicación establece primero el atributo de instrucción SQL_ATTR_ROW_BIND_TYPE en SQL_BIND_BY_COLUMN. (Este es el valor predeterminado). Para cada columna que se va a enlazar, la aplicación realiza los siguientes pasos:  
  
1.  Asigna una matriz de búferes de datos.  
  
2.  Asigna una matriz de búferes de longitud/indicador.  
  
    > [!NOTE]  
    >  Si la aplicación escribe directamente en los descriptores cuando se usa el enlace de modo de columna, se pueden usar matrices independientes para los datos de longitud y indicador.  
  
3.  Llama a **SQLBindCol** con los argumentos siguientes:  
  
    -   *TargetType* es el tipo de un único elemento en la matriz de búferes de datos.  
  
    -   *TargetValuePtr* es la dirección de la matriz de búferes de datos.  
  
    -   *BufferLength* es el tamaño de un único elemento en la matriz de búferes de datos. El argumento *BufferLength* se omite cuando los datos son datos de longitud fija.  
  
    -   *StrLen_or_IndPtr* es la dirección de la matriz de longitud/indicador.  
  
 Para obtener más información sobre cómo se usa esta información, vea "direcciones de búfer" más adelante en esta sección. Para obtener más información sobre el enlace de modo de columna, vea [enlace de](../../../odbc/reference/develop-app/column-wise-binding.md)modo de columna.  
  
## <a name="row-wise-binding"></a>El enlace  
 En el enlace de modo de fila, la aplicación define una estructura que contiene datos y búferes de longitud/indicador para cada columna que se va a enlazar.  
  
 Para usar el enlace de modo de fila, la aplicación realiza los pasos siguientes:  
  
1.  Define una estructura que contiene una sola fila de datos (incluidos los búferes de datos y de longitud/indicador) y asigna una matriz de estas estructuras.  
  
    > [!NOTE]  
    >  Si la aplicación escribe directamente en los descriptores cuando se usa el enlace de modo de fila, se pueden usar campos independientes para los datos de longitud y indicador.  
  
2.  Establece el atributo de instrucción SQL_ATTR_ROW_BIND_TYPE en el tamaño de la estructura que contiene una sola fila de datos o en el tamaño de una instancia de un búfer al que se enlazarán las columnas de resultados. La longitud debe incluir espacio para todas las columnas enlazadas, y cualquier relleno de la estructura o búfer, para asegurarse de que cuando la dirección de una columna enlazada se incrementa con la longitud especificada, el resultado apunta al principio de la misma columna en la fila siguiente. Al usar el operador *sizeof* en ANSI C, se garantiza este comportamiento.  
  
3.  Llama a **SQLBindCol** con los siguientes argumentos para cada columna que se va a enlazar:  
  
    -   *TargetType* es el tipo del miembro de búfer de datos que se va a enlazar a la columna.  
  
    -   *TargetValuePtr* es la dirección del miembro de búfer de datos en el primer elemento de la matriz.  
  
    -   *BufferLength* es el tamaño del miembro de búfer de datos.  
  
    -   *StrLen_or_IndPtr* es la dirección del miembro de longitud/indicador que se va a enlazar.  
  
 Para obtener más información sobre cómo se usa esta información, vea "direcciones de búfer" más adelante en esta sección. Para obtener más información sobre el enlace de modo de columna, vea [enlace de](../../../odbc/reference/develop-app/row-wise-binding.md)modo de fila.  
  
## <a name="buffer-addresses"></a>Direcciones de búfer  
 La *dirección del búfer* es la dirección real de los datos o el búfer de longitud/indicador. El controlador calcula la dirección del búfer justo antes de escribir en los búferes (por ejemplo, durante el tiempo de captura). Se calcula a partir de la fórmula siguiente, que usa las direcciones especificadas en los argumentos *TargetValuePtr* y *StrLen_or_IndPtr* , el desplazamiento de enlace y el número de fila:  
  
 *Dirección*  +  enlazada *Desplazamiento de enlace* + ((*número de fila* -1) x tamaño de *elemento*)  
  
 donde las variables de la fórmula se definen como se describe en la tabla siguiente.  
  
|Variable|Descripción|  
|--------------|-----------------|  
|*Dirección enlazada*|En el caso de los búferes de datos, la dirección especificada con el argumento *TargetValuePtr* en **SQLBindCol**.<br /><br /> En el caso de los búferes de longitud/indicador, la dirección especificada con el argumento *StrLen_or_IndPtr* en **SQLBindCol**. Para obtener más información, vea "comentarios adicionales" en la sección "descriptores y SQLBindCol".<br /><br /> Si la dirección enlazada es 0, no se devuelve ningún valor de datos, aunque la dirección calculada por la fórmula anterior sea distinto de cero.|  
|*Desplazamiento de enlace*|Si se utiliza el enlace de modo de fila, el valor almacenado en la dirección especificada con el atributo de instrucción SQL_ATTR_ROW_BIND_OFFSET_PTR.<br /><br /> Si se usa el enlace de modo de columna o si el valor del atributo de instrucción SQL_ATTR_ROW_BIND_OFFSET_PTR es un puntero nulo, el *desplazamiento de enlace* es 0.|  
|*Número de fila*|Número basado en 1 de la fila del conjunto de filas. En el caso de las capturas de una sola fila, que son el valor predeterminado, es 1.|  
|*Tamaño del elemento*|Tamaño de un elemento de la matriz enlazada.<br /><br /> Si se usa el enlace de modo de columna, es **sizeof (sqlinteger donde)** para los búferes de longitud/indicador. En el caso de los búferes de datos, es el valor del argumento *BufferLength* de **SQLBindCol** si el tipo de datos es de longitud variable y el tamaño del tipo de datos si el tipo de datos es de longitud fija.<br /><br /> Si se usa el enlace de modo de fila, este es el valor del atributo de la instrucción SQL_ATTR_ROW_BIND_TYPE para los búferes de datos y longitud/indicador.|  
  
## <a name="descriptors-and-sqlbindcol"></a>Descriptores y SQLBindCol  
 En las secciones siguientes se describe cómo **SQLBindCol** interactúa con descriptores.  
  
> [!CAUTION]  
>  Llamar a **SQLBindCol** para una instrucción puede afectar a otras instrucciones. Esto sucede cuando el ARD asociado a la instrucción se asigna explícitamente y también está asociado a otras instrucciones. Dado que **SQLBindCol** modifica el descriptor, las modificaciones se aplican a todas las instrucciones a las que está asociado este descriptor. Si este no es el comportamiento necesario, la aplicación debe desasociar Este descriptor de las demás instrucciones antes de llamar a **SQLBindCol**.  
  
## <a name="argument-mappings"></a>Asignaciones de argumentos  
 Conceptualmente, **SQLBindCol** realiza los pasos siguientes en la secuencia:  
  
1.  Llama a **SQLGetStmtAttr** para obtener el identificador de ARD.  
  
2.  Llama a **SQLGetDescField** para obtener el campo de SQL_DESC_COUNT de este descriptor y, si el valor del argumento *ColumnNumber* supera el valor de SQL_DESC_COUNT, llama a **SQLSetDescField** para aumentar el valor de SQL_DESC_COUNT a *ColumnNumber*.  
  
3.  Llama a **SQLSetDescField** varias veces para asignar valores a los campos siguientes de ARD:  
  
    -   Establece SQL_DESC_TYPE y SQL_DESC_CONCISE_TYPE en el valor de *TargetType*, salvo que si *TargetType* es uno de los identificadores concisos de un subtipo DateTime o Interval, establece SQL_DESC_TYPE en SQL_DATETIME o SQL_INTERVAL, respectivamente; establece SQL_DESC_CONCISE_TYPE en el identificador conciso; y establece SQL_DESC_DATETIME_INTERVAL_CODE en el subcódigo de fecha y hora correspondiente.  
  
    -   Establece uno o varios SQL_DESC_LENGTH, SQL_DESC_PRECISION, SQL_DESC_SCALE y SQL_DESC_DATETIME_INTERVAL_PRECISION, según corresponda para *TargetType*.  
  
    -   Establece el campo de SQL_DESC_OCTET_LENGTH en el valor de *BufferLength*.  
  
    -   Establece el campo de SQL_DESC_DATA_PTR en el valor de *TargetValuePtr*.  
  
    -   Establece el SQL_DESC_INDICATOR_PTR campo en el valor de *StrLen_or_IndPtr*. (Vea el párrafo siguiente).  
  
    -   Establece el SQL_DESC_OCTET_LENGTH_PTR campo en el valor de *StrLen_or_IndPtr*. (Vea el párrafo siguiente).  
  
 La variable a la que hace referencia el argumento *StrLen_or_IndPtr* se usa para la información de indicador y de longitud. Si una captura encuentra un valor null para la columna, almacena SQL_NULL_DATA en esta variable; de lo contrario, almacena la longitud de los datos en esta variable. Si se pasa un puntero nulo como *StrLen_or_IndPtr* se impide que la operación de captura devuelva la longitud de los datos, pero se produce un error en la captura si encuentra un valor NULL y no tiene ninguna manera de devolver SQL_NULL_DATA.  
  
 Si se produce un error en la llamada a **SQLBindCol** , el contenido de los campos de descriptor que habría establecido en ARD no está definido y el valor del campo de SQL_DESC_COUNT de ARD no cambia.  
  
## <a name="implicit-resetting-of-count-field"></a>Restablecimiento implícito del campo de recuento  
 **SQLBindCol** establece SQL_DESC_COUNT en el valor del argumento *ColumnNumber* solo cuando esto aumentaría el valor de SQL_DESC_COUNT. Si el valor del argumento *TargetValuePtr* es un puntero nulo y el valor del argumento *ColumnNumber* es igual a SQL_DESC_COUNT (es decir, al desenlazar la columna enlazada más alta), SQL_DESC_COUNT se establece en el número de la columna límite restante más alta.  
  
## <a name="cautions-regarding-sql_default"></a>Precauciones respecto a SQL_DEFAULT  
 Para recuperar los datos de la columna correctamente, la aplicación debe determinar correctamente la longitud y el punto inicial de los datos en el búfer de la aplicación. Cuando la aplicación especifica un *TargetType* explícito, se detectan fácilmente los malentendidos de las aplicaciones. Sin embargo, cuando la aplicación especifica un *TargetType* de SQL_DEFAULT, **SQLBindCol** se puede aplicar a una columna de un tipo de datos diferente del que desea la aplicación, ya sea de los cambios en los metadatos o aplicando el código a una columna diferente. En este caso, es posible que la aplicación no determine siempre el inicio o la longitud de los datos de la columna capturada. Esto puede provocar errores de datos no notificados o infracciones de memoria.  
  
## <a name="code-example"></a>Ejemplo de código  
 En el ejemplo siguiente, una aplicación ejecuta una instrucción **Select** en la tabla Customers para devolver un conjunto de resultados de los ID. de cliente, los nombres y los números de teléfono, ordenados por nombre. A continuación, llama a **SQLBindCol** para enlazar las columnas de datos a los búferes locales. Por último, la aplicación captura cada fila de datos con **SQLFetch** e imprime el nombre, el identificador y el número de teléfono de cada cliente.  
  
 Para obtener más ejemplos de código, vea [función SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md), función [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md), [función SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)y [función SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
```cpp  
// SQLBindCol_ref.cpp  
// compile with: odbc32.lib  
#include <windows.h>  
#include <stdio.h>  
  
#define UNICODE  
#include <sqlext.h>  
  
#define NAME_LEN 50  
#define PHONE_LEN 60
  
void show_error() {  
   printf("error\n");  
}  
  
int main() {  
   SQLHENV henv;  
   SQLHDBC hdbc;  
   SQLHSTMT hstmt = 0;  
   SQLRETURN retcode;  
   SQLWCHAR szName[NAME_LEN], szPhone[PHONE_LEN], sCustID[NAME_LEN];  
   SQLLEN cbName = 0, cbCustID = 0, cbPhone = 0;  
  
   // Allocate environment handle  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
  
   // Set the ODBC version environment attribute  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
      retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
      // Allocate connection handle  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
         retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
  
         // Set login timeout to 5 seconds  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
            SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
            // Connect to data source  
            retcode = SQLConnect(hdbc, (SQLWCHAR*) L"NorthWind", SQL_NTS, (SQLWCHAR*) NULL, 0, NULL, 0);  
  
            // Allocate statement handle  
            if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {   
               retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);   
  
               retcode = SQLExecDirect(hstmt, (SQLWCHAR *) L"SELECT CustomerID, ContactName, Phone FROM CUSTOMERS ORDER BY 2, 1, 3", SQL_NTS);  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
  
                  // Bind columns 1, 2, and 3  
                  retcode = SQLBindCol(hstmt, 1, SQL_C_WCHAR, &sCustID, 100, &cbCustID);  
                  retcode = SQLBindCol(hstmt, 2, SQL_C_WCHAR, szName, NAME_LEN, &cbName);  
                  retcode = SQLBindCol(hstmt, 3, SQL_C_WCHAR, szPhone, PHONE_LEN, &cbPhone);   
  
                  // Fetch and print each row of data. On an error, display a message and exit.  
                  for (int i=0 ; ; i++) {  
                     retcode = SQLFetch(hstmt);  
                     if (retcode == SQL_ERROR || retcode == SQL_SUCCESS_WITH_INFO)  
                        show_error();  
                     if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
                     {
                        //replace wprintf with printf
                        //%S with %ls
                        //warning C4477: 'wprintf' : format string '%S' requires an argument of type 'char *'
                        //but variadic argument 2 has type 'SQLWCHAR *'
                        //wprintf(L"%d: %S %S %S\n", i + 1, sCustID, szName, szPhone);  
                        printf("%d: %ls %ls %ls\n", i + 1, sCustID, szName, szPhone);  
                    }    
                     else  
                        break;  
                  }  
               }  
  
               // Process data  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
                  SQLCancel(hstmt);  
                  SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
               }  
  
               SQLDisconnect(hdbc);  
            }  
  
            SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
         }  
      }  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
   }  
}  
```  
  
 Vea también el [programa ODBC de ejemplo](../../../odbc/reference/sample-odbc-program.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Devolver información acerca de una columna de un conjunto de resultados|[SQLDescribeCol (función)](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Obtener un bloque de datos o desplazarse por un conjunto de resultados|[Función SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Obtener varias filas de datos|[Función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Liberar búferes de columna en la instrucción|[Función SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|Capturar parte o toda una columna de datos|[Función SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
|Devolver el número de columnas del conjunto de resultados|[SQLNumResultCols (función)](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
