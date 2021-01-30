---
description: Función SQLExecDirect
title: Función SQLExecDirect | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLExecDirect
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLExecDirect
helpviewer_keywords:
- SQLExecDirect function [ODBC]
ms.assetid: 985fcee1-f204-425c-bdd1-deb0e7d7bbd9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 938d2aa1631041e9dbb7a51ea952ff7d60811fb4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179886"
---
# <a name="sqlexecdirect-function"></a>Función SQLExecDirect
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: ISO 92  
  
 **Resumen**  
 **SQLExecDirect** ejecuta una instrucción preparable. con los valores actuales de las variables de marcador de parámetro si existe algún parámetro en la instrucción. **SQLExecDirect** es la forma más rápida de enviar una instrucción SQL para ejecutarse una sola vez.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLExecDirect(  
     SQLHSTMT     StatementHandle,  
     SQLCHAR *    StatementText,  
     SQLINTEGER   TextLength);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *StatementText*  
 Entradas Instrucción SQL que se va a ejecutar.  
  
 *TextLength*  
 Entradas Longitud de **StatementText* en caracteres.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NEED_DATA, SQL_STILL_EXECUTING, SQL_ERROR, SQL_NO_DATA, SQL_INVALID_HANDLE o SQL_PARAM_DATA_AVAILABLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLExecDirect** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que se devuelven normalmente con **SQLExecDirect** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01001|Conflicto de operación de cursor|\**StatementText* contenía una instrucción UPDATE o DELETE posicionada, y no se actualizaron o eliminaron más de una fila. (Para obtener más información acerca de las actualizaciones de más de una fila, vea la descripción del *atributo* SQL_ATTR_SIMULATE_CURSOR en **SQLSetStmtAttr**).<br /><br /> (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01003|Se eliminó un valor NULL en la función set|El argumento *StatementText* contenía una función de conjunto (como **AVG**, **Max**, **min**, etc.), pero no la función de conjunto de **recuento** , y los valores de argumento NULL se eliminaban antes de aplicar la función. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Datos de cadena, truncados a la derecha|Los datos binarios o de cadena devueltos para un parámetro de entrada/salida o de salida dieron como resultado el truncamiento de datos binarios de caracteres que no están en blanco o no NULL. Si fuera un valor de cadena, se truncó a la derecha. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01006|Privilegio no revocado|\**StatementText* contenía una instrucción **REVOKE** y el usuario no tenía el privilegio especificado. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01007|Privilegio no concedido|*\* StatementText* era una instrucción **Grant** y no se pudo conceder al usuario el privilegio especificado.|  
|01S02|Valor de opción cambiado|Un atributo de instrucción especificado no era válido debido a condiciones de trabajo de implementación, por lo que un valor similar se sustituyó temporalmente. (Se puede llamar a **SQLGetStmtAttr** para determinar cuál es el valor sustituido temporalmente). El valor de sustitución es válido para *StatementHandle* hasta que se cierra el cursor, en cuyo punto el atributo de instrucción revierte a su valor anterior. Los atributos de instrucción que se pueden cambiar son:<br /><br /> SQL_ ATTR_CONCURRENCY SQL_ ATTR_CURSOR_TYPE SQL_ ATTR_KEYSET_SIZE SQL_ ATTR_MAX_LENGTH SQL_ ATTR_MAX_ROWS SQL_ ATTR_QUERY_TIMEOUT SQL_ ATTR_SIMULATE_CURSOR<br /><br /> (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01S07|Truncamiento fraccionario|Los datos devueltos para un parámetro de entrada/salida o de salida se truncaron de forma que la parte fraccionaria de un tipo de datos numéricos se truncara o la parte fraccionaria del componente de hora de un tipo de datos de hora, marca de tiempo o intervalo se truncara.<br /><br /> (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|07002|Campo de recuento incorrecto|El número de parámetros especificado en **SQLBindParameter** era menor que el número de parámetros de la instrucción SQL incluido en \* *StatementText*.<br /><br /> Se llamó a **SQLBindParameter** con *ParameterValuePtr* establecido en un puntero nulo, *StrLen_or_IndPtr* no se ha establecido en SQL_NULL_DATA o SQL_DATA_AT_EXEC y *InputOutputType* no establecido en SQL_PARAM_OUTPUT, de modo que el número de parámetros especificado en **SQLBindParameter** sea mayor que el número de parámetros de la instrucción SQL incluida en **StatementText*.|  
|07006|Infracción de atributo de tipo de datos restringido|El valor de datos identificado por el argumento *ValueType* en **SQLBindParameter** para el parámetro enlazado no se pudo convertir al tipo de datos identificado por el argumento *ParameterType* en **SQLBindParameter**.<br /><br /> El valor de datos devuelto para un parámetro enlazado como SQL_PARAM_INPUT_OUTPUT o SQL_PARAM_OUTPUT no se pudo convertir al tipo de datos identificado por el argumento *ValueType* en **SQLBindParameter**.<br /><br /> (Si no se pudieron convertir los valores de datos de una o más filas pero se devolvieron una o varias filas correctamente, esta función devuelve SQL_SUCCESS_WITH_INFO).|  
|07007|Infracción de valor de parámetro restringido|El tipo de parámetro SQL_PARAM_INPUT_OUTPUT_STREAM solo se usa para un parámetro que envía y recibe datos en partes. No se permite un búfer enlazado de entrada para este tipo de parámetro.<br /><br /> Este error se producirá cuando el tipo de parámetro sea SQL_PARAM_INPUT_OUTPUT y cuando el \* *StrLen_or_IndPtr* especificado en **SQLBindParameter** no sea igual a SQL_NULL_DATA, SQL_DEFAULT_PARAM, SQL_LEN_DATA_AT_EXEC (len) o SQL_DATA_AT_EXEC.|  
|07S01|Uso no válido del parámetro predeterminado|Se SQL_DEFAULT_PARAM un valor de parámetro, establecido con **SQLBindParameter**, y el parámetro correspondiente no tenía un valor predeterminado.|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|21S01|La lista de valores de inserción no coincide con la lista de columnas|\**StatementText* contenía una instrucción **Insert** y el número de valores que se van a insertar no coincidía con el grado de la tabla derivada.|  
|21S02|El grado de la tabla derivada no coincide con la lista de columnas|\**StatementText* contenía una instrucción **Create View** , y la lista de columnas incompleta (el número de columnas especificado para la vista en los argumentos de *identificador de columna* de la instrucción SQL) contenía más nombres que el número de columnas de la tabla derivada definido por el argumento de *especificación de consulta* de la instrucción SQL.|  
|22001|Datos de cadena, truncamiento derecho|La asignación de un valor de carácter o binario a una columna dio como resultado el truncamiento de datos de caracteres que no están en blanco o datos binarios no nulos.|  
|22002|Se requiere una variable de indicador pero no se ha proporcionado|Los datos NULL se enlazaron a un parámetro de salida cuya *StrLen_or_IndPtr* establecida por **SQLBindParameter** era un puntero nulo.|  
|22003|Valor numérico fuera del intervalo|**StatementText* contenía una instrucción SQL que contenía un parámetro o literal numérico enlazado, y el valor hacía que la parte entera (en oposición a fraccionaria) del número se truncara cuando se asignase a la columna de la tabla asociada.<br /><br /> La devolución de un valor numérico (como Numeric o String) para uno o varios parámetros de entrada/salida o de salida habría causado la parte entera (en oposición a fraccionario) del número que se va a truncar.|  
|22007|Formato de fecha y hora no válido|**StatementText* contenía una instrucción SQL que contenía una estructura de fecha, hora o marca de tiempo como parámetro enlazado, y el parámetro era, respectivamente, una fecha, una hora o una marca de tiempo no válidas.<br /><br /> Un parámetro de entrada/salida o de salida se ha enlazado a una estructura de fecha, hora o marca de tiempo de C, y un valor en el parámetro devuelto era, respectivamente, una fecha, una hora o una marca de tiempo no válidas. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|22008|Desbordamiento de campo de fecha y hora|**StatementText* contenía una instrucción SQL que contenía una expresión DateTime que, cuando se calculó, producía una estructura de fecha, hora o marca de tiempo que no era válida.<br /><br /> Una expresión DateTime calculada para un parámetro de entrada/salida o de salida dio como resultado una estructura de fecha, hora o marca de tiempo de C que no era válida.|  
|22012|División por cero|**StatementText* contenía una instrucción SQL que contenía una expresión aritmética que provocó la división por cero.<br /><br /> Una expresión aritmética calculada para un parámetro de entrada/salida o de salida dio como resultado una división por cero.|  
|22015|Desbordamiento de campo de intervalo|*\* StatementText* contenía un parámetro de intervalo o numérico exacto que, cuando se convierte en un tipo de datos SQL de intervalo, causó una pérdida de dígitos significativos.<br /><br /> *\* StatementText* contenía un parámetro Interval con más de un campo que, cuando se convierte en un tipo de datos numérico en una columna, no tenía ninguna representación en el tipo de datos Numeric.<br /><br /> *\* StatementText* contenía datos de parámetro que se asignaron a un tipo SQL de intervalo y no había ninguna representación del valor del tipo C en el tipo SQL de intervalo.<br /><br /> La asignación de un parámetro de entrada/salida o de salida que era un tipo numérico exacto o de intervalo SQL a un tipo de intervalo C provocó una pérdida de dígitos significativos.<br /><br /> Cuando se asigna un parámetro de entrada/salida o de salida a una estructura de intervalo C, no había ninguna representación de los datos en la estructura de datos de intervalo.|  
|22018|Valor de carácter no válido para la especificación de conversión|*\* StatementText* contenía un tipo de C que era un tipo numérico exacto o aproximado, un valor de fecha y hora o un tipo de datos de intervalo; el tipo SQL de la columna era un tipo de datos de caracteres y el valor de la columna no era un literal válido del tipo C enlazado.<br /><br /> Cuando se devolvió un parámetro de entrada/salida o de salida, el tipo SQL era un tipo numérico exacto o aproximado, un valor de fecha y hora o un intervalo de datos. se SQL_C_CHAR el tipo C; y el valor de la columna no era un literal válido del tipo SQL enlazado.|  
|22019|Carácter de escape no válido|\**StatementText* contenía una instrucción SQL que contenía un predicado **like** con un **escape** en la cláusula **Where** y la longitud del carácter de escape que sigue a **escape** no era igual a 1.|  
|22025|Secuencia de escape no válida|\**StatementText* contenía una instrucción SQL que contenía "**like** _Pattern Value_ **escape escape"** en la cláusula **Where** y el carácter que sigue al carácter _de escape en_ el valor de pattern no era uno de "%" o "_".|  
|23000|Infracción de la restricción de integridad|**StatementText* contenía una instrucción SQL que contenía un parámetro o un literal. El valor del parámetro era NULL para una columna definida como NOT NULL en la columna de la tabla asociada, se proporcionó un valor duplicado para una columna restringida a contener solo valores únicos, o bien se infringió alguna otra restricción de integridad.|  
|24000|Estado de cursor no válido|Un cursor se colocó en *StatementHandle* por **SQLFetch** o **SQLFetchScroll**. Este error lo devuelve el administrador de controladores si **SQLFetch** o **sqlfetchscroll** no ha devuelto SQL_NO_DATA y lo devuelve el controlador si **SQLFetch** o **sqlfetchscroll** ha devuelto SQL_NO_DATA.<br /><br /> Un cursor estaba abierto pero no se colocó en el *StatementHandle*.<br /><br /> **StatementText* contenía una instrucción UPDATE o DELETE posicionada y el cursor se colocó antes del inicio del conjunto de resultados o después del final del conjunto de resultados.|  
|34000|Nombre de cursor no válido|**StatementText* contenía una instrucción UPDATE o DELETE posicionada y el cursor al que hace referencia la instrucción que se está ejecutando no estaba abierto.|  
|3D000|Nombre de catálogo no válido|El nombre de catálogo especificado en *StatementText* no era válido.|  
|3F000|Nombre de esquema no válido|El nombre de esquema especificado en *StatementText* no era válido.|  
|40001|Error de serialización|La transacción se revirtió debido a un interbloqueo de recursos con otra transacción.|  
|40003|Finalización de instrucciones desconocida|No se pudo establecer la conexión asociada durante la ejecución de esta función y no se puede determinar el estado de la transacción.|  
|42000|Error de sintaxis o infracción de acceso|\**StatementText* contenía una instrucción SQL que no era preparable. o contenía un error de sintaxis.<br /><br /> El usuario no tenía permiso para ejecutar la instrucción SQL incluida en **StatementText*.|  
|42S01|La tabla o vista base ya existe|\**StatementText* contenía una instrucción **CREATE TABLE** o **Create View** , y el nombre de tabla o el nombre de vista especificado ya existe.|  
|42S02|No se encontró la tabla base o la vista|\**StatementText* contenía una instrucción **DROP TABLE** o **Drop View** , y el nombre de tabla o el nombre de vista especificados no existían.<br /><br /> \**StatementText* contenía una instrucción **ALTER TABLE** y el nombre de tabla especificado no existía.<br /><br /> \**StatementText* contenía una instrucción **Create View** y no existía un nombre de tabla o nombre de vista definido por la especificación de consulta.<br /><br /> \**StatementText* contenía una instrucción **Create index** y el nombre de tabla especificado no existía.<br /><br /> \**StatementText* contenía una instrucción **Grant** o **REVOKE** y el nombre de tabla o el nombre de vista especificados no existían.<br /><br /> \**StatementText* contenía una instrucción **Select** y no existía un nombre de tabla o nombre de vista especificado.<br /><br /> \**StatementText* contenía una instrucción **Delete**, **Insert** o **Update** , y el nombre de tabla especificado no existía.<br /><br /> \**StatementText* contenía una instrucción **CREATE TABLE** y una tabla especificada en una restricción (que hace referencia a una tabla distinta de la que se está creando) no existía.<br /><br /> \**StatementText* contenía una instrucción **Create Schema** y no existía un nombre de tabla o nombre de vista especificado.|  
|42S11|El índice ya existe|\**StatementText* contenía una instrucción **Create index** y el nombre de índice especificado ya existía.<br /><br /> \**StatementText* contenía una instrucción **Create Schema** y el nombre de índice especificado ya existía.|  
|42S12|No se encontró el índice|\**StatementText* contenía una instrucción **Drop index** y el nombre de índice especificado no existía.|  
|42S21|La columna ya existe|\**StatementText* contenía una instrucción **ALTER TABLE** y la columna especificada en la cláusula **Add** no es única o identifica una columna existente en la tabla base.|  
|42S22|Columna no encontrada|\**StatementText* contenía una instrucción **Create index** y uno o varios de los nombres de columna especificados en la lista de columnas no existían.<br /><br /> \**StatementText* contenía una instrucción **Grant** o **REVOKE** y no existía un nombre de columna especificado.<br /><br /> \**StatementText* contenía una instrucción **Select**, **Delete**, **Insert** o **Update** , y no existía un nombre de columna especificado.<br /><br /> \**StatementText* contenía una instrucción **CREATE TABLE** y una columna especificada en una restricción (que hace referencia a una tabla distinta de la que se está creando) no existía.<br /><br /> \**StatementText* contenía una instrucción **Create Schema** y no existía un nombre de columna especificado.|  
|44000|Infracción de WITH CHECK OPTION|El argumento *StatementText* contenía una instrucción **Insert** realizada en una tabla vista o una tabla derivada de la tabla vista que se creó especificando **with check Option**, de modo que una o varias filas afectadas por la instrucción **Insert** ya no estarán presentes en la tabla vista.<br /><br /> El argumento *StatementText* contenía una instrucción **Update** realizada en una tabla vista o una tabla derivada de la tabla vista que se creó especificando **with check Option**, de modo que una o varias filas afectadas por la instrucción **Update** ya no estarán presentes en la tabla vista.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle*. A continuación, se llamó de nuevo a la función en *StatementHandle*.<br /><br /> Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY009|Uso no válido de puntero nulo|(DM) **StatementText* era un puntero nulo.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función **SQLExecDirect** .<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|(DM) el argumento *textLength* era menor o igual que 0 pero no es igual a SQL_NTS.<br /><br /> Un valor de parámetro, establecido con **SQLBindParameter**, era un puntero nulo y el valor de longitud del parámetro no era 0, SQL_NULL_DATA, SQL_DATA_AT_EXEC, SQL_DEFAULT_PARAM o menor o igual que SQL_LEN_DATA_AT_EXEC_OFFSET.<br /><br /> Un valor de parámetro, establecido con **SQLBindParameter**, no era un puntero nulo. el tipo de datos C se SQL_C_BINARY o SQL_C_CHAR; y el valor de longitud del parámetro era menor que 0 pero no se SQL_NTS, SQL_NULL_DATA, SQL_DATA_AT_EXEC, SQL_DEFAULT_PARAM, o menor o igual que SQL_LEN_DATA_AT_EXEC_OFFSET.<br /><br /> Un valor de longitud de parámetro enlazado a **SQLBindParameter** se estableció en SQL_DATA_AT_EXEC; el tipo SQL era SQL_LONGVARCHAR, SQL_LONGVARBINARY o un tipo de datos específico del origen de datos largo; y el tipo de información SQL_NEED_LONG_DATA_LEN en **SQLGetInfo** era "Y".|  
|HY105|Tipo de parámetro no válido|El valor especificado para el argumento *InputOutputType* en **SQLBindParameter** se SQL_PARAM_OUTPUT y el parámetro era un parámetro de entrada.|  
|HY109|Posición del cursor no válida|\**StatementText* contenía una instrucción UPDATE o DELETE posicionada y el cursor estaba colocado (por **SQLSetPos** o **SQLFetchScroll**) en una fila que se ha eliminado o no se ha podido recuperar.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|La combinación de los valores actuales de los atributos de instrucción SQL_ATTR_CONCURRENCY y SQL_ATTR_CURSOR_TYPE no era compatible con el controlador o el origen de datos.<br /><br /> El atributo de instrucción SQL_ATTR_USE_BOOKMARKS se estableció en SQL_UB_VARIABLE y el atributo de instrucción SQL_ATTR_CURSOR_TYPE se estableció en un tipo de cursor para el que el controlador no admite marcadores.|  
|HYT00|Tiempo de espera agotado|El período de tiempo de espera de consulta expiró antes de que el origen de datos devolviera el conjunto de resultados. El período de tiempo de espera se establece a través de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
## <a name="comments"></a>Comentarios  
 La aplicación llama a **SQLExecDirect** para enviar una instrucción SQL al origen de datos. Para obtener más información acerca de la ejecución directa, vea [ejecución directa](../../../odbc/reference/develop-app/direct-execution-odbc.md). El controlador modifica la instrucción para utilizar la forma de SQL utilizada por el origen de datos y, a continuación, la envía al origen de datos. En concreto, el controlador modifica las secuencias de escape que se usan para definir ciertas características en SQL. Para obtener la sintaxis de las secuencias de escape, vea [secuencias de escape en ODBC](../../../odbc/reference/develop-app/escape-sequences-in-odbc.md).  
  
 La aplicación puede incluir uno o varios marcadores de parámetros en la instrucción SQL. Para incluir un marcador de parámetro, la aplicación inserta un signo de interrogación (?) en la instrucción SQL en la posición adecuada. Para obtener información sobre los parámetros, vea parámetros de la [instrucción](../../../odbc/reference/develop-app/statement-parameters.md).  
  
 Si la instrucción SQL es una instrucción **Select** y la aplicación llamada **SQLSetCursorName** para asociar un cursor a una instrucción, el controlador utiliza el cursor especificado. De lo contrario, el controlador genera un nombre de cursor.  
  
 Si el origen de datos está en modo de confirmación manual (que requiere el inicio de la transacción explícito) y aún no se ha iniciado una transacción, el controlador inicia una transacción antes de enviar la instrucción SQL. Para obtener más información, vea [modo de confirmación manual](../../../odbc/reference/develop-app/manual-commit-mode.md).  
  
 Si una aplicación utiliza **SQLExecDirect** para enviar una instrucción **commit** o **Rollback** , no se podrá interoperar entre productos DBMS. Para confirmar o revertir una transacción, una aplicación llama a **SQLEndTran**.  
  
 Si **SQLExecDirect** encuentra un parámetro de datos en ejecución, devuelve SQL_NEED_DATA. La aplicación envía los datos mediante **SQLParamData** y **SQLPutData**. Consulte [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md), [SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md), [SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)y [envío de datos largos](../../../odbc/reference/develop-app/sending-long-data.md).  
  
 Si **SQLExecDirect** ejecuta una instrucción UPDATE, INSERT o DELETE buscada que no afecta a ninguna fila del origen de datos, la llamada a **SQLExecDirect** devuelve SQL_NO_DATA.  
  
 Si el valor del atributo de instrucción SQL_ATTR_PARAMSET_SIZE es mayor que 1 y la instrucción SQL contiene al menos un marcador de parámetro, **SQLExecDirect** ejecutará la instrucción SQL una vez para cada conjunto de valores de parámetro de las matrices a las que apunta el argumento *ParameterValuePointer* en la llamada a **SQLBindParameter**. Para obtener más información, vea [matrices de valores de parámetro](../../../odbc/reference/develop-app/arrays-of-parameter-values.md).  
  
 Si los marcadores están activados y se ejecuta una consulta que no admite marcadores, el controlador debe intentar forzar el entorno a uno que admita marcadores cambiando un valor de atributo y devolviendo SQLSTATE 01S02 (valor de opción cambiado). Si no se puede cambiar el atributo, el controlador debe devolver SQLSTATE HY024 (valor de atributo no válido).  
  
> [!NOTE]  
>  Al utilizar la agrupación de conexiones, una aplicación no debe ejecutar instrucciones SQL que cambien la base de datos o el contexto de la base de datos, como la instrucción **use** _Database_ en SQL Server, que cambia el catálogo utilizado por un origen de datos.  
  
## <a name="code-example"></a>Ejemplo de código  
 Vea [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md), [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)y el [programa ODBC de ejemplo](../../../odbc/reference/sample-odbc-program.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar un búfer a una columna de un conjunto de resultados|[SQLBindCol (función)](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Ejecutar una operación de confirmación o reversión|[Función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md)|  
|Ejecutar una instrucción SQL preparada|[Función SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Obtener varias filas de datos|[Función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Obtener un bloque de datos o desplazarse por un conjunto de resultados|[Función SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Devolver un nombre de cursor|[Función SQLGetCursorName](../../../odbc/reference/syntax/sqlgetcursorname-function.md)|  
|Capturar parte o toda una columna de datos|[Función SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
|Devolver el siguiente parámetro para el que se van a enviar datos|[Función SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
|Preparar una instrucción para su ejecución|[Función SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
|Enviar datos de parámetros en tiempo de ejecución|[Función SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)|  
|Establecer un nombre de cursor|[Función SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
|Establecer un atributo de instrucción|[Función SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
