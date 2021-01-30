---
description: Función SQLColAttribute
title: Función SQLColAttribute | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLColAttribute
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLColAttribute
helpviewer_keywords:
- SQLColAttribute function [ODBC]
ms.assetid: 8c45c598-cb01-4789-a571-e93619a18ed9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ce0bd87f2a4e2a9306bb5fa94add7fb1ee583dfa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208504"
---
# <a name="sqlcolattribute-function"></a>Función SQLColAttribute
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 3,0: ISO 92  
  
 **Resumen**  
 **SQLColAttribute** devuelve información del descriptor de una columna de un conjunto de resultados. La información del descriptor se devuelve como una cadena de caracteres, un valor dependiente del descriptor o un valor entero.  
  
> [!NOTE]  
>  Para obtener más información sobre lo que el administrador de controladores asigna a esta función cuando se trata de un ODBC 3. la aplicación *x* está trabajando con ODBC 2. controlador *x* , consulte [asignación de funciones de reemplazo para la compatibilidad con versiones anteriores de las aplicaciones](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLColAttribute (  
      SQLHSTMT        StatementHandle,  
      SQLUSMALLINT    ColumnNumber,  
      SQLUSMALLINT    FieldIdentifier,  
      SQLPOINTER      CharacterAttributePtr,  
      SQLSMALLINT     BufferLength,  
      SQLSMALLINT *   StringLengthPtr,  
      SQLLEN *        NumericAttributePtr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *ColumnNumber*  
 Entradas Número del registro en el IRD desde el que se va a recuperar el valor del campo. Este argumento corresponde al número de columnas de los datos de resultados, ordenados secuencialmente en el orden creciente de las columnas, empezando por 1. Las columnas se pueden describir en cualquier orden.  
  
 Se puede especificar la columna 0 en este argumento, pero todos los valores excepto SQL_DESC_TYPE y SQL_DESC_OCTET_LENGTH devolverán valores no definidos.  
  
 *FieldIdentifier*  
 Entradas Identificador del descriptor. Este identificador define el campo de IRD que debe consultarse (por ejemplo, SQL_COLUMN_TABLE_NAME).  
  
 *CharacterAttributePtr*  
 Genere Puntero a un búfer en el que se va a devolver el valor del campo *FieldIdentifier* de la fila *ColumnNumber* de IRD, si el campo es una cadena de caracteres. De lo contrario, el campo no se utiliza.  
  
 Si *CharacterAttributePtr* es null, *StringLengthPtr* devolverá el número total de bytes (sin incluir el carácter de terminación null para los datos de caracteres) disponible para devolver en el búfer señalado por *CharacterAttributePtr*.  
  
 *BufferLength*  
 Entradas Si *FieldIdentifier* es un campo definido por ODBC y *CharacterAttributePtr* apunta a una cadena de caracteres o a un búfer binario, este argumento debe ser la longitud de \* *CharacterAttributePtr*. Si *FieldIdentifier* es un campo definido por ODBC y \* *CharacterAttribute* PTR es un entero, se omite este campo. Si *\* CharacterAttributePtr* es una cadena Unicode (al llamar a **SQLColAttributeW**), el argumento *BufferLength* debe ser un número par. Si *FieldIdentifier* es un campo definido por el controlador, la aplicación indica la naturaleza del campo en el administrador de controladores estableciendo el argumento *BufferLength* . *BufferLength* puede tener los siguientes valores:  
  
-   Si *CharacterAttributePtr* es un puntero a un puntero, *BufferLength* debe tener el valor SQL_IS_POINTER.  
  
-   Si *CharacterAttributePtr* es un puntero a una cadena de caracteres, el valor de *BufferLength* es la longitud del búfer.  
  
-   Si *CharacterAttributePtr* es un puntero a un búfer binario, la aplicación coloca el resultado de la macro SQL_LEN_BINARY_ATTR (*length*) en *BufferLength*. Esto coloca un valor negativo en *BufferLength*.  
  
-   Si *CharacterAttributePtr* es un puntero a un tipo de datos de longitud fija, *BufferLength* debe ser uno de los siguientes: SQL_IS_INTEGER, SQL_IS_UINTEGER, SQL_IS_SMALLINT o SQL_IS_USMALLINT.  
  
 *StringLengthPtr*  
 Genere Puntero a un búfer en el que se va a devolver el número total de bytes (sin incluir el byte de terminación null para los datos de caracteres) disponible para devolver en **CharacterAttributePtr*.  
  
 En el caso de los datos de caracteres, si el número de bytes disponibles para devolver es mayor o igual que *BufferLength*, la información del descriptor de \* *CharacterAttributePtr* se trunca a *BufferLength* menos la longitud de un carácter de terminación NULL y el controlador termina en NULL.  
  
 En el caso de todos los demás tipos de datos, se omite el valor de *BufferLength* y el controlador asume que el tamaño de **CharacterAttributePtr* es 32 bits.  
  
 *NumericAttributePtr*  
 Genere Puntero a un búfer entero en el que se va a devolver el valor del campo *FieldIdentifier* de la fila *ColumnNumber* de IRD, si el campo es un tipo de descriptor numérico, como SQL_DESC_COLUMN_LENGTH. De lo contrario, el campo no se utiliza. Tenga en cuenta que algunos controladores solo pueden escribir el menor 32 bits o 16 bits de un búfer y dejar el bit de orden superior sin cambios. Por lo tanto, las aplicaciones deben inicializar el valor en 0 antes de llamar a esta función.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLColAttribute** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLColAttribute** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Datos de cadena, truncados a la derecha|El búfer \* *CharacterAttributePtr* no era lo suficientemente grande como para devolver el valor completo de la cadena, por lo que el valor de la cadena se truncó. La longitud del valor de cadena no truncado se devuelve en **StringLengthPtr*. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|07005|Instrucción preparada no es una *especificación de cursor*|La instrucción asociada a *StatementHandle* no devolvió un conjunto de resultados y *FieldIdentifier* no se SQL_DESC_COUNT. No había columnas para describir.|  
|07009|Índice de descriptor no válido|(DM) el valor especificado para *ColumnNumber* era igual a 0 y el atributo de instrucción SQL_ATTR_USE_BOOKMARKS era SQL_UB_OFF.<br /><br /> El valor especificado para el argumento *ColumnNumber* era mayor que el número de columnas del conjunto de resultados.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagField** de la estructura de datos de diagnóstico describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle*. A continuación, se llamó de nuevo a la función en *StatementHandle*.<br /><br /> Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función aynchronous todavía se estaba ejecutando cuando se llamó a SQLColAttribute.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) se llamó a la función antes de llamar a **SQLPrepare**, **SQLExecDirect** o una función de catálogo para el *StatementHandle*.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|(DM) *\* CharacterAttributePtr* es una cadena de caracteres y *BufferLength* era menor que 0 pero no es igual a SQL_NTS.|  
|HY091|Identificador de campo descriptor no válido|El valor especificado para el argumento *FieldIdentifier* no era uno de los valores definidos y no era un valor definido por la implementación.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Controlador no compatible|El valor especificado para el argumento *FieldIdentifier* no era compatible con el controlador.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
 Cuando se llama después de **SQLPrepare** y antes de **SQLExecute**, **SQLColAttribute** puede devolver cualquier SQLSTATE que pueda ser devuelto por **SQLPrepare** o **SQLExecute**, dependiendo de si el origen de datos evalúa la instrucción SQL asociada a *StatementHandle*.  
  
 Por motivos de rendimiento, una aplicación no debe llamar a **SQLColAttribute** antes de ejecutar una instrucción.  
  
## <a name="comments"></a>Comentarios  
 Para obtener información sobre cómo usan las aplicaciones la información devuelta por **SQLColAttribute**, consulte [metadatos del conjunto de resultados](../../../odbc/reference/develop-app/result-set-metadata.md).  
  
 **SQLColAttribute** devuelve información en \* *NumericAttributePtr* o en \* *CharacterAttributePtr*. La información de enteros se devuelve en \* *NumericAttributePtr* como un valor SQLLEN; todos los demás formatos de información se devuelven en \* *CharacterAttributePtr*. Cuando se devuelve información en \* *NumericAttributePtr*, el controlador omite *CharacterAttributePtr*, *BufferLength* y *StringLengthPtr*. Cuando se devuelve información en \* *CharacterAttributePtr*, el controlador omite *NumericAttributePtr*.  
  
 **SQLColAttribute** devuelve valores de los campos de descriptor de IRD. Se llama a la función con un identificador de instrucción en lugar de un identificador de descriptor. Los valores devueltos por **SQLColAttribute** para los valores de *FieldIdentifier* que se enumeran más adelante en esta sección también se pueden recuperar llamando a **SQLGetDescField** con el identificador IRD adecuado.  
  
 Los campos de descriptor definidos actualmente, la versión de ODBC en la que se introdujeron y los argumentos en los que se devuelve información para ellos se muestran más adelante en esta sección. los controladores pueden definir más tipos de descriptores para aprovechar los distintos orígenes de datos.  
  
 ODBC 3. el controlador *x* debe devolver un valor para cada uno de los campos de descriptor. Si un campo descriptor no se aplica a un controlador o a un origen de datos y, a menos que se indique lo contrario, el controlador devuelve 0 en \* *StringLengthPtr* o una cadena vacía en **CharacterAttributePtr*.  
  
## <a name="backward-compatibility"></a>Backward Compatibility  
 ODBC 3. la función *x* **SQLCOLATTRIBUTE** reemplaza a ODBC 2 en desuso.  función x **SQLColAttributes**. Al asignar **SQLColAttributes** a **SQLColAttribute** (cuando se trata de un ODBC 2.** la aplicación x está trabajando con un ODBC 3. *x* ) o asignar **SQLColAttribute** a **SQLColAttributes** (cuando se trata de un ODBC 3.** la aplicación x está trabajando con ODBC 2. *x* ), el administrador de controladores pasa el valor de *FieldIdentifier* a, lo asigna a un nuevo valor o devuelve un error, como se indica a continuación:  
  
> [!NOTE]  
>  El prefijo usado en los valores de *FieldIdentifier* en ODBC 3. *x* se ha cambiado de la utilizada en ODBC 2. *x*. El nuevo prefijo es "SQL_DESC"; el prefijo anterior era "SQL_COLUMN".  
  
-   Si el valor **#define** de ODBC 2. *x* *FieldIdentifier* es igual que el valor **#define** de ODBC 3. *x* *FieldIdentifier*, el valor de la llamada de función se acaba de pasar a través de.  
  
-   Los valores **#define** de ODBC 2. *x* *FieldIdentifiers* SQL_COLUMN_LENGTH, SQL_COLUMN_PRECISION y SQL_COLUMN_SCALE son diferentes de los valores **#define** de ODBC 3. *x* *FieldIdentifiers* SQL_DESC_PRECISION, SQL_DESC_SCALE y SQL_DESC_LENGTH. ODBC 2. el controlador *x* solo es compatible con ODBC 2. valores *x* . ODBC 3. el controlador *x* debe admitir los valores "SQL_COLUMN" y "SQL_DESC" para estos tres *FieldIdentifiers*. Estos valores son diferentes porque la precisión, la escala y la longitud se definen de forma diferente en ODBC 3. *x* que estaban en ODBC 2. *x*. Para obtener más información, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).  
  
-   Si el valor **#define** de ODBC 2. *x* *FieldIdentifier* es diferente del valor **#define** de ODBC 3. *x* *FieldIdentifier*, como ocurre con los valores Count, Name y Nullable, el valor de la llamada de función se asigna al valor correspondiente. Por ejemplo, SQL_COLUMN_COUNT se asigna a SQL_DESC_COUNT y SQL_DESC_COUNT se asigna a SQL_COLUMN_COUNT, dependiendo de la dirección de la asignación.  
  
-   Si *FieldIdentifier* es un nuevo valor de ODBC 3. *x*, para el que no hay ningún valor correspondiente en ODBC 2. *x*, no se asignará cuando se trata de un ODBC 3. la aplicación *x* lo usa en una llamada a **SQLColAttribute** en ODBC 2. *x* y la llamada devolverá SQLSTATE HY091 (identificador de campo de descriptor no válido).  
  
 En la tabla siguiente se enumeran los tipos de descriptores devueltos por **SQLColAttribute**. El tipo de los valores de *NumericAttributePtr* es **SQLLEN \** _.  
  
|_FieldIdentifier *|Información<br /><br /> se devuelve en|Descripción|  
|-----------------------|---------------------------------|-----------------|  
|SQL_DESC_AUTO_UNIQUE_VALUE (ODBC 1,0)|*NumericAttributePtr*|SQL_TRUE si la columna es una columna de incremento automático.<br /><br /> SQL_FALSE si la columna no es una columna de incremento automático o no es numérica.<br /><br /> Este campo solo es válido para las columnas de tipos de datos numéricos. Una aplicación puede insertar valores en una fila que contenga una columna de incremento automático, pero normalmente no puede actualizar los valores de la columna.<br /><br /> Cuando se realiza una inserción en una columna de incremento automático, se inserta un valor único en la columna en el momento de la inserción. El incremento no está definido, pero es específico del origen de datos. Una aplicación no debe suponer que una columna de incremento automático se inicia en un punto determinado o se incrementa en un determinado valor.|  
|SQL_DESC_BASE_COLUMN_NAME (ODBC 3,0)|*CharacterAttributePtr*|Nombre de la columna base para la columna del conjunto de resultados. Si no existe un nombre de columna base (como en el caso de las columnas que son expresiones), esta variable contiene una cadena vacía.<br /><br /> Esta información se devuelve desde el campo SQL_DESC_BASE_COLUMN_NAME registro de IRD, que es un campo de solo lectura.|  
|SQL_DESC_BASE_TABLE_NAME (ODBC 3,0)|*CharacterAttributePtr*|Nombre de la tabla base que contiene la columna. Si el nombre de la tabla base no se puede definir o no es aplicable, esta variable contiene una cadena vacía.<br /><br /> Esta información se devuelve desde el campo SQL_DESC_BASE_TABLE_NAME registro de IRD, que es un campo de solo lectura.|  
|SQL_DESC_CASE_SENSITIVE (ODBC 1,0)|*NumericAttributePtr*|SQL_TRUE si la columna se trata como con distinción de mayúsculas y minúsculas en las intercalaciones y comparaciones.<br /><br /> SQL_FALSE si la columna no se trata con distinción de mayúsculas y minúsculas en las intercalaciones y comparaciones, o no es un carácter.|  
|SQL_DESC_CATALOG_NAME (ODBC 2,0)|*CharacterAttributePtr*|Catálogo de la tabla que contiene la columna. El valor devuelto es definido por la implementación si la columna es una expresión o si la columna forma parte de una vista. Si el origen de datos no admite catálogos o no se puede determinar el nombre del catálogo, se devuelve una cadena vacía. Este campo de registro VARCHAR no está limitado a 128 caracteres.|  
|SQL_DESC_CONCISE_TYPE (ODBC 1,0)|*NumericAttributePtr*|El tipo de datos conciso.<br /><br /> En el caso de los tipos de datos datetime e Interval, este campo devuelve el tipo de datos conciso; por ejemplo, SQL_TYPE_TIME o SQL_INTERVAL_YEAR. (Para obtener más información, vea [identificadores y descriptores de tipos de datos](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md) en el Apéndice D: tipos de datos).<br /><br /> Esta información se devuelve desde el campo registro de SQL_DESC_CONCISE_TYPE de IRD.|  
|SQL_DESC_COUNT (ODBC 1,0)|*NumericAttributePtr*|El número de columnas disponibles en el conjunto de resultados. Esto devuelve 0 si no hay ninguna columna en el conjunto de resultados. Se omite el valor del argumento *ColumnNumber* .<br /><br /> Esta información se devuelve desde el campo de encabezado SQL_DESC_COUNT de IRD.|  
|SQL_DESC_DISPLAY_SIZE (ODBC 1,0)|*NumericAttributePtr*|Número máximo de caracteres necesarios para mostrar los datos de la columna. Para obtener más información sobre el tamaño de la presentación, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación en el](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) Apéndice D: tipos de datos.|  
|SQL_DESC_FIXED_PREC_SCALE (ODBC 1,0)|*NumericAttributePtr*|SQL_TRUE si la columna tiene una precisión fija y una escala distinto de cero que son específicas del origen de datos.<br /><br /> SQL_FALSE si la columna no tiene una precisión fija y una escala que no sea cero y que sean específicas del origen de datos.|  
|SQL_DESC_LABEL (ODBC 2,0)|*CharacterAttributePtr*|Etiqueta o título de la columna. Por ejemplo, una columna denominada EmpName podría tener la etiqueta nombre de empleado o podría estar etiquetada con un alias.<br /><br /> Si una columna no tiene una etiqueta, se devuelve el nombre de la columna. Si la columna no tiene etiqueta y no tiene nombre, se devuelve una cadena vacía.|  
|SQL_DESC_LENGTH (ODBC 3,0)|*NumericAttributePtr*|Valor numérico que es la longitud máxima o real de caracteres de una cadena de caracteres o un tipo de datos binario. Es la longitud de caracteres máxima para un tipo de datos de longitud fija o la longitud de caracteres real para un tipo de datos de longitud variable. Su valor siempre excluye el byte de terminación null que finaliza la cadena de caracteres.<br /><br /> Esta información se devuelve desde el campo registro de SQL_DESC_LENGTH de IRD.<br /><br /> Para obtener más información acerca de la longitud, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación en el](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) Apéndice D: tipos de datos.|  
|SQL_DESC_LITERAL_PREFIX (ODBC 3,0)|*CharacterAttributePtr*|Este campo de registro VARCHAR (128) contiene el carácter o los caracteres que el controlador reconoce como prefijo para un literal de este tipo de datos. Este campo contiene una cadena vacía para un tipo de datos para el que no se puede aplicar un prefijo literal. Para obtener más información, vea [prefijos literales y](../../../odbc/reference/develop-app/literal-prefixes-and-suffixes.md)sufijos.|  
|SQL_DESC_LITERAL_SUFFIX (ODBC 3,0)|*CharacterAttributePtr*|Este campo de registro VARCHAR (128) contiene el carácter o los caracteres que el controlador reconoce como sufijo de un literal de este tipo de datos. Este campo contiene una cadena vacía para un tipo de datos para el que no se puede aplicar un sufijo literal. Para obtener más información, vea [prefijos literales y](../../../odbc/reference/develop-app/literal-prefixes-and-suffixes.md)sufijos.|  
|SQL_DESC_LOCAL_TYPE_NAME (ODBC 3,0)|*CharacterAttributePtr*|Este campo de registro VARCHAR (128) contiene cualquier nombre localizado (lenguaje nativo) para el tipo de datos que puede ser diferente del nombre normal del tipo de datos. Si no hay ningún nombre localizado, se devuelve una cadena vacía. Este campo solo es para fines de presentación. El juego de caracteres de la cadena depende de la configuración regional y suele ser el juego de caracteres predeterminado del servidor.|  
|SQL_DESC_NAME (ODBC 3,0)|*CharacterAttributePtr*|El alias de columna, si se aplica. Si no se aplica el alias de columna, se devuelve el nombre de la columna. En cualquier caso, SQL_DESC_UNNAMED se establece en SQL_NAMED. Si no hay un nombre de columna o un alias de columna, se devuelve una cadena vacía y SQL_DESC_UNNAMED se establece en SQL_UNNAMED.<br /><br /> Esta información se devuelve desde el campo registro de SQL_DESC_NAME de IRD.|  
|SQL_DESC_NULLABLE (ODBC 3,0)|*NumericAttributePtr*|SQL_ aceptan valores NULL si la columna puede tener valores NULL; SQL_NO_NULLS si la columna no tiene valores NULL; o SQL_NULLABLE_UNKNOWN si no se sabe si la columna acepta valores NULL.<br /><br /> Esta información se devuelve desde el campo registro de SQL_DESC_NULLABLE de IRD.|  
|SQL_DESC_NUM_PREC_RADIX (ODBC 3,0)|*NumericAttributePtr*|Si el tipo de datos del campo SQL_DESC_TYPE es un tipo de datos numérico aproximado, este campo SQLINTEGER donde contiene un valor de 2 porque el campo SQL_DESC_PRECISION contiene el número de bits. Si el tipo de datos del campo SQL_DESC_TYPE es un tipo de datos numérico exacto, este campo contiene un valor de 10 porque el campo SQL_DESC_PRECISION contiene el número de dígitos decimales. Este campo se establece en 0 para todos los tipos de datos no numéricos.|  
|SQL_DESC_OCTET_LENGTH (ODBC 3,0)|*NumericAttributePtr*|La longitud, en bytes, de una cadena de caracteres o un tipo de datos binario. En el caso de los tipos de caracteres de longitud fija o binarios, se trata de la longitud real en bytes. En el caso de los tipos de caracteres de longitud variable o binarios, es la longitud máxima en bytes. Este valor no incluye el terminador null.<br /><br /> Esta información se devuelve desde el campo registro de SQL_DESC_OCTET_LENGTH de IRD.<br /><br /> Para obtener más información acerca de la longitud, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación en el](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) Apéndice D: tipos de datos.|  
|SQL_DESC_PRECISION (ODBC 3,0)|*NumericAttributePtr*|Un valor numérico que para un tipo de datos numérico denota la precisión aplicable. En el caso de los tipos de datos SQL_TYPE_TIME, SQL_TYPE_TIMESTAMP y todos los tipos de datos de intervalo que representan un intervalo de tiempo, su valor es la precisión aplicable del componente de fracciones de segundo.<br /><br /> Esta información se devuelve desde el campo registro de SQL_DESC_PRECISION de IRD.|  
|SQL_DESC_SCALE (ODBC 3,0)|*NumericAttributePtr*|Un valor numérico que es la escala aplicable para un tipo de datos numérico. En el caso de los tipos de datos DECIMAL y NUMERIC, se trata de la escala definida. No está definido para el resto de tipos de datos.<br /><br /> Esta información se devuelve del campo de registro de escala de IRD.|  
|SQL_DESC_SCHEMA_NAME (ODBC 2,0)|*CharacterAttributePtr*|El esquema de la tabla que contiene la columna. El valor devuelto es definido por la implementación si la columna es una expresión o si la columna forma parte de una vista. Si el origen de datos no admite esquemas o no se puede determinar el nombre del esquema, se devuelve una cadena vacía. Este campo de registro VARCHAR no está limitado a 128 caracteres.|  
|SQL_DESC_SEARCHABLE (ODBC 1,0)|*NumericAttributePtr*|SQL_PRED_NONE si la columna no se puede usar en una cláusula WHERE. (Es igual que el valor de SQL_UNSEARCHABLE en ODBC 2. *x*).<br /><br /> SQL_PRED_CHAR si la columna se puede utilizar en una cláusula WHERE, pero solo con el predicado LIKE. (Es igual que el valor de SQL_LIKE_ONLY en ODBC 2. *x*).<br /><br /> SQL_PRED_BASIC si la columna se puede utilizar en una cláusula WHERE con todos los operadores de comparación excepto LIKE. (Es igual que el valor de SQL_EXCEPT_LIKE en ODBC 2. *x*).<br /><br /> SQL_PRED_SEARCHABLE si la columna se puede utilizar en una cláusula WHERE con cualquier operador de comparación.<br /><br /> Las columnas de tipo SQL_LONGVARCHAR y SQL_LONGVARBINARY normalmente devuelven SQL_PRED_CHAR.|  
|SQL_DESC_TABLE_NAME (ODBC 2,0)|*CharacterAttributePtr*|El nombre de la tabla que contiene la columna. El valor devuelto es definido por la implementación si la columna es una expresión o si la columna forma parte de una vista.<br /><br /> Si no se puede determinar el nombre de la tabla, se devuelve una cadena vacía.|  
|SQL_DESC_TYPE (ODBC 3,0)|*NumericAttributePtr*|Valor numérico que especifica el tipo de datos SQL.<br /><br /> Cuando *ColumnNumber* es igual a 0, se devuelve SQL_BINARY para los marcadores de longitud variable y se devuelve SQL_INTEGER para los marcadores de longitud fija.<br /><br /> En el caso de los tipos de datos datetime e Interval, este campo devuelve el tipo de datos detallado: SQL_DATETIME o SQL_INTERVAL. (Para obtener más información, vea [identificadores y descriptores de tipos de datos](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md) en el Apéndice D: tipos de datos.<br /><br /> Esta información se devuelve desde el campo registro de SQL_DESC_TYPE de IRD. **Nota:**  Para trabajar con ODBC 2. *x* , use SQL_DESC_CONCISE_TYPE en su lugar.|  
|SQL_DESC_TYPE_NAME (ODBC 1,0)|*CharacterAttributePtr*|Nombre del tipo de datos dependiente del origen de datos; por ejemplo, "CHAR", "VARCHAR", "MONEY", "LONG VARBINARY" o "CHAR () FOR BIT DATA".<br /><br /> Si el tipo es desconocido, se devuelve una cadena vacía.|  
|SQL_DESC_UNNAMED (ODBC 3,0)|*NumericAttributePtr*|SQL_NAMED o SQL_UNNAMED. Si el SQL_DESC_NAME campo del IRD contiene un alias de columna o un nombre de columna, se devuelve SQL_NAMED. Si no hay un nombre de columna o un alias de columna, se devuelve SQL_UNNAMED.<br /><br /> Esta información se devuelve desde el campo registro de SQL_DESC_UNNAMED de IRD.|  
|SQL_DESC_UNSIGNED (ODBC 1,0)|*NumericAttributePtr*|SQL_TRUE si la columna es sin signo (o no numérica).<br /><br /> SQL_FALSE si la columna está firmada.|  
|SQL_DESC_UPDATABLE (ODBC 1,0)|*NumericAttributePtr*|Los valores de las constantes definidas describen la columna:<br /><br /> SQL_ATTR_READONLY SQL_ATTR_WRITE SQL_ATTR_READWRITE_UNKNOWN<br /><br /> SQL_DESC_UPDATABLE describe la actualización de la columna en el conjunto de resultados, no la columna de la tabla base. La capacidad de actualización de la columna base en la que se basa la columna del conjunto de resultados puede ser diferente del valor de este campo. El hecho de que una columna sea actualizable puede basarse en el tipo de datos, los privilegios de usuario y la definición del conjunto de resultados. Si no está claro si una columna es actualizable, se debe devolver SQL_ATTR_READWRITE_UNKNOWN.|  
  
 **SQLColAttribute** es una alternativa extensible a **SQLDescribeCol**. **SQLDescribeCol** devuelve un conjunto fijo de información de descriptor basado en SQL ANSI-89. **SQLColAttribute** permite el acceso al conjunto más amplio de información de descriptores disponible en las extensiones de proveedor ANSI SQL-92 y DBMS.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar un búfer a una columna de un conjunto de resultados|[SQLBindCol (función)](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Devolver información acerca de una columna de un conjunto de resultados|[SQLDescribeCol (función)](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Obtener un bloque de datos o desplazarse por un conjunto de resultados|[Función SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Obtener varias filas de datos|[Función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
  
## <a name="example"></a>Ejemplo  
 El código de ejemplo siguiente no libera identificadores ni conexiones. Vea la [función SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md), el [programa ODBC de ejemplo](../../../odbc/reference/sample-odbc-program.md)y la [función SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md) para ver ejemplos de código para liberar identificadores e instrucciones.  
  
```cpp  
// SQLColAttibute.cpp  
// compile with: user32.lib odbc32.lib  
  
#define UNICODE  
  
#include <windows.h>  
#include <sqlext.h>  
#include <strsafe.h>  
  
struct DataBinding {  
   SQLSMALLINT TargetType;  
   SQLPOINTER TargetValuePtr;  
   SQLINTEGER BufferLength;  
   SQLLEN StrLen_or_Ind;  
};  
  
void printStatementResult(SQLHSTMT hstmt) {  
   int bufferSize = 1024, i;  
   SQLRETURN retCode;  
   SQLSMALLINT numColumn = 0, bufferLenUsed;
   
   retCode = SQLNumResultCols(hstmt, &numColumn);  
   
   SQLPOINTER* columnLabels = (SQLPOINTER *)malloc( numColumn * sizeof(SQLPOINTER*) );  
   struct DataBinding* columnData = (struct DataBinding*)malloc( numColumn * sizeof(struct DataBinding) );  
  
   printf( "Columns from that table:\n" );  
   for ( i = 0 ; i < numColumn ; i++ ) {  
      columnLabels[i] = (SQLPOINTER)malloc( bufferSize*sizeof(char) );  
  
      retCode = SQLColAttribute(hstmt, (SQLUSMALLINT)i + 1, SQL_DESC_LABEL, columnLabels[i], (SQLSMALLINT)bufferSize, &bufferLenUsed, NULL);  
      wprintf( L"Column %d: %s\n", i, (wchar_t*)columnLabels[i] );  
   }  
  
   // allocate memory for the binding  
   for ( i = 0 ; i < numColumn ; i++ ) {  
      columnData[i].TargetType = SQL_C_CHAR;  
      columnData[i].BufferLength = (bufferSize+1);  
      columnData[i].TargetValuePtr = malloc( sizeof(unsigned char)*columnData[i].BufferLength );  
   }  
  
   // setup the binding   
   for ( i = 0 ; i < numColumn ; i++ ) {  
      retCode = SQLBindCol(hstmt, (SQLUSMALLINT)i + 1, columnData[i].TargetType,   
         columnData[i].TargetValuePtr, columnData[i].BufferLength, &(columnData[i].StrLen_or_Ind));  
   }  
  
   printf( "Data from that table:\n" );  
   // fetch the data and print out the data  
   for ( retCode = SQLFetch(hstmt) ; retCode == SQL_SUCCESS || retCode == SQL_SUCCESS_WITH_INFO ; retCode = SQLFetch(hstmt) ) {  
      int j;  
      for ( j = 0 ; j < numColumn ; j++ )  
         wprintf( L"%s: %hs\n", columnLabels[j], columnData[j].TargetValuePtr );  
      printf( "\n" );  
   }  
   printf( "\n" );   
}  
  
int main() {  
   int bufferSize = 1024, i, count = 1, numCols = 5;  
   wchar_t firstTableName[1024], * dbName = (wchar_t *)malloc( sizeof(wchar_t)*bufferSize ), * userName = (wchar_t *)malloc( sizeof(wchar_t)*bufferSize );  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
   SQLWCHAR connStrbuffer[1024];  
   SQLSMALLINT connStrBufferLen, bufferLen;  
   SQLRETURN retCode;  
  
   SQLHENV henv = NULL;   // Environment     
   SQLHDBC hdbc = NULL;   // Connection handle  
   SQLHSTMT hstmt = NULL;   // Statement handle  
  
   struct DataBinding* catalogResult = (struct DataBinding*) malloc( numCols * sizeof(struct DataBinding) );  
   SQLWCHAR* selectAllQuery = (SQLWCHAR *)malloc( sizeof(SQLWCHAR) * bufferSize );  
  
   // connect to database  
   retCode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retCode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLCHAR *)(void*)SQL_OV_ODBC3, -1);  
   retCode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
   retCode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)10, 0);  
   retCode = SQLDriverConnect(hdbc, desktopHandle, L"Driver={SQL Server}", SQL_NTS, connStrbuffer, 1025, &connStrBufferLen, SQL_DRIVER_PROMPT);  
   retCode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   // display the database information  
   retCode = SQLGetInfo(hdbc, SQL_DATABASE_NAME, dbName, (SQLSMALLINT)bufferSize, (SQLSMALLINT *)&bufferLen);  
   retCode = SQLGetInfo(hdbc, SQL_USER_NAME, userName, (SQLSMALLINT)bufferSize, &bufferLen);  
  
   for ( i = 0 ; i < numCols ; i++ ) {  
      catalogResult[i].TargetType = SQL_C_CHAR;  
      catalogResult[i].BufferLength = (bufferSize + 1);  
      catalogResult[i].TargetValuePtr = malloc( sizeof(unsigned char)*catalogResult[i].BufferLength );  
   }  
  
   // Set up the binding. This can be used even if the statement is closed by closeStatementHandle  
   for ( i = 0 ; i < numCols ; i++ )  
      retCode = SQLBindCol(hstmt, (SQLUSMALLINT)i + 1, catalogResult[i].TargetType, catalogResult[i].TargetValuePtr, catalogResult[i].BufferLength, &(catalogResult[i].StrLen_or_Ind));  
  
   retCode = SQLTables( hstmt, (SQLWCHAR*)SQL_ALL_CATALOGS, SQL_NTS, L"", SQL_NTS, L"", SQL_NTS, L"", SQL_NTS );  
   retCode = SQLFreeStmt(hstmt, SQL_CLOSE);  
  
   retCode = SQLTables( hstmt, dbName, SQL_NTS, userName, SQL_NTS, L"%", SQL_NTS, L"TABLE", SQL_NTS );  
  
   for ( retCode = SQLFetch(hstmt) ; retCode == SQL_SUCCESS || retCode == SQL_SUCCESS_WITH_INFO ; retCode = SQLFetch(hstmt), ++count )  
      if ( count == 1 )  
         StringCchPrintfW( firstTableName, 1024, L"%hs", catalogResult[2].TargetValuePtr );  
   retCode = SQLFreeStmt(hstmt, SQL_CLOSE);  
  
   wprintf( L"Select all data from the first table (%s)\n", firstTableName );  
   StringCchPrintfW( selectAllQuery, bufferSize, L"SELECT * FROM %s", firstTableName );  
  
   retCode = SQLExecDirect(hstmt, selectAllQuery, SQL_NTS);  
   printStatementResult(hstmt);  
}  
```  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Programa de ejemplo de ODBC](../../../odbc/reference/sample-odbc-program.md)
