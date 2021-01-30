---
description: Función SQLSetConnectAttr
title: SQLSetConnectAttr, función | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetConnectAttr
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetConnectAttr
helpviewer_keywords:
- SQLSetConnectAttr function [ODBC]
ms.assetid: 97fc7445-5a66-4eb9-8e77-10990b5fd685
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 06935fe7eca6f19a8c6fcc052cd3a9503144a296
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192446"
---
# <a name="sqlsetconnectattr-function"></a>Función SQLSetConnectAttr
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 3,0: ISO 92  
  
 **Resumen**  
 **SQLSetConnectAttr** establece atributos que rigen aspectos de las conexiones.  
  
> [!NOTE]
>  Para obtener más información sobre lo que el administrador de controladores asigna a esta función cuando una aplicación ODBC 3 *. x* está trabajando con un controlador ODBC 2 *. x* , consulte [asignación de funciones de reemplazo para mantener la compatibilidad con versiones anteriores de las aplicaciones](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLSetConnectAttr(  
     SQLHDBC       ConnectionHandle,  
     SQLINTEGER    Attribute,  
     SQLPOINTER    ValuePtr,  
     SQLINTEGER    StringLength);  
```  
  
## <a name="arguments"></a>Argumentos  
 *ConnectionHandle*  
 [Entrada] Identificador de conexión.  
  
 *Atributo*  
 Entradas Atributo que se va a establecer, mostrado en "Comentarios".  
  
 *ValuePtr*  
 Entradas Puntero al valor que se va a asociar al *atributo*. Dependiendo del valor del *atributo*, *ValuePtr* será un valor entero sin signo o apuntará a una cadena de caracteres terminada en NULL. Tenga en cuenta que el tipo entero del argumento de *atributo* puede que no tenga una longitud fija; vea la sección Comentarios para obtener información detallada.  
  
 *StringLength*  
 Entradas Si el *atributo* es un atributo definido por ODBC y *ValuePtr* apunta a una cadena de caracteres o a un búfer binario, este argumento debe ser la longitud de **ValuePtr*. En el caso de los datos de cadenas de caracteres, este argumento debe contener el número de bytes de la cadena.  
  
 Si el *atributo* es un atributo definido por ODBC y *ValuePtr* es un entero, *StringLength* se omite.  
  
 Si el *atributo* es un atributo definido por el controlador, la aplicación indica la naturaleza del atributo para el administrador de controladores mediante el establecimiento del argumento *StringLength* . *StringLength* puede tener los siguientes valores:  
  
-   Si *ValuePtr* es un puntero a una cadena de caracteres, *StringLength* es la longitud de la cadena o SQL_NTS.  
  
-   Si *ValuePtr* es un puntero a un búfer binario, la aplicación coloca el resultado de la macro SQL_LEN_BINARY_ATTR (*length*) en *StringLength*. Esto coloca un valor negativo en *StringLength*.  
  
-   Si *ValuePtr* es un puntero a un valor distinto de una cadena de caracteres o una cadena binaria, *StringLength* debe tener el valor SQL_IS_POINTER.  
  
-   Si *ValuePtr* contiene un valor de longitud fija, *StringLength* es SQL_IS_INTEGER o SQL_IS_UINTEGER, según corresponda.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_INVALID_HANDLE o SQL_STILL_EXECUTING.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLSetConnectAttr** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_DBC y un *identificador* de *ConnectionHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que suele devolver **SQLSetConnectAttr** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
 El controlador puede devolver SQL_SUCCESS_WITH_INFO para proporcionar información sobre el resultado de establecer una opción.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01S02|Valor de opción cambiado|El controlador no admitía el valor especificado en *ValuePtr* y sustituyó un valor similar. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08002|Nombre de conexión en uso|Se SQL_ATTR_ODBC_CURSORS el argumento de *atributo* y el controlador ya estaba conectado al origen de datos.|  
|08003|Conexión no abierta|(DM) se especificó un valor de *atributo* que requería una conexión abierta, pero el *ConnectionHandle* no estaba en un estado conectado.|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|24000|Estado de cursor no válido|Se SQL_ATTR_CURRENT_CATALOG el argumento de *atributo* y hay un conjunto de resultados pendiente.|  
|25000|Operación no válida en una transacción local|Una conexión estaba en una transacción local al intentar darse de alta en una conexión de transacciones distribuidas (DTC) estableciendo el atributo de conexión SQL_ATTR_ENLIST_IN_DTC.<br /><br /> Una conexión ya está dada de alta en un DTC.<br /><br /> Se ha dado de alta una conexión en una conexión de transacción distribuida y se ha iniciado una transacción local estableciendo SQL_ATTR_AUTOCOMMIT en SQL_AUTOCOMMIT_OFF.|  
|3D000|Nombre de catálogo no válido|Se SQL_CURRENT_CATALOG el argumento de *atributo* y el nombre de catálogo especificado no era válido.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *ConnectionHandle*. Se llamó a la función **SQLSetConnectAttr** y antes de completar la ejecución, se llamó a la [función SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md) en *ConnectionHandle* y, a continuación, se llamó de nuevo a la función **SQLSetConnectAttr** en *ConnectionHandle*.<br /><br /> O bien, se llamó a la función  **SQLSetConnectAttr** y antes de completar la ejecución, se llamó a **SQLCancelHandle** en *ConnectionHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY009|Uso no válido de puntero nulo|El argumento de *atributo* identificó un atributo de conexión que requería un valor de cadena y el argumento *ValuePtr* era un puntero nulo.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para un *StatementHandle* asociado a *ConnectionHandle* y que todavía se estaba ejecutando cuando se llamó a **SQLSetConnectAttr** .<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *ConnectionHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para uno de los identificadores de instrucciones asociados a *ConnectionHandle* y se devolvieron SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para un *StatementHandle* asociado con *ConnectionHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.<br /><br /> Se llamó a (DM) **SQLBrowseConnect** para *ConnectionHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de que **SQLBrowseConnect** devolviera SQL_SUCCESS_WITH_INFO o SQL_SUCCESS.|  
|HY011|El atributo no se puede establecer ahora|Se SQL_ATTR_TXN_ISOLATION el argumento de *atributo* y se abrió una transacción.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY024|Valor de atributo no válido|Dado el valor de *atributo* especificado, se especificó un valor no válido en *ValuePtr*. (El administrador de controladores devuelve este SQLSTATE solo para los atributos de conexión y de instrucción que aceptan un conjunto discreto de valores, como SQL_ATTR_ACCESS_MODE o SQL_ATTR_ASYNC_ENABLE. Para todos los demás atributos de conexión y de instrucción, el controlador debe comprobar el valor especificado en *ValuePtr*).<br /><br /> El argumento de *atributo* era SQL_ATTR_TRACEFILE o SQL_ATTR_TRANSLATE_LIB y *ValuePtr* era una cadena vacía.|  
|HY090|Longitud de búfer o cadena no válida|*(DM) \* ValuePtr* es una cadena de caracteres y el argumento *StringLength* era menor que 0 pero no se SQL_NTS.|  
|HY092|Identificador de opción/atributo no válido|(DM) el valor especificado para el *atributo* argument no era válido para la versión de ODBC admitida por el controlador.<br /><br /> (DM) el valor especificado para el *atributo* argument era un atributo de solo lectura.|  
|HY114|El controlador no admite la ejecución de funciones asincrónicas en el nivel de conexión|(DM) una aplicación intentó habilitar la ejecución de función asincrónica con SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE para un controlador que no admite operaciones de conexión asincrónicas.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HY121|La biblioteca de cursores y la agrupación de Driver-Aware no se pueden habilitar al mismo tiempo|Para obtener más información, consulte [agrupación de conexiones compatible con controladores](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md).|  
|HYC00|Característica opcional no implementada|El valor especificado para el *atributo* argument era una conexión ODBC válida o un atributo de instrucción para la versión de ODBC admitida por el controlador, pero no era compatible con el controlador.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *ConnectionHandle* no admite la función.|  
|IM009|No se puede cargar la DLL de traducción|El controlador no pudo cargar la DLL de traducción que se especificó para la conexión. Este error solo se puede devolver cuando el *atributo* es SQL_ATTR_TRANSLATE_LIB.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
|S1118|El controlador no admite la notificación asincrónica|Se estableció SQL_ATTR_ASYNC_DBC_EVENT (después de realizar la conexión), pero el controlador no admite la notificación asincrónica.|  
  
 Cuando el *atributo* es un atributo de instrucción, **SQLSetConnectAttr** puede devolver cualquier SQLSTATEs devuelto por **SQLSetStmtAttr**.  
  
## <a name="comments"></a>Comentarios  
 Para obtener información general sobre los atributos de conexión, consulte [atributos de conexión](../../../odbc/reference/develop-app/connection-attributes.md).  
  
 Los atributos definidos actualmente y la versión de ODBC en la que se introdujeron se muestran en la tabla que aparece más adelante en esta sección. se espera que se definan más atributos para aprovechar los distintos orígenes de datos. ODBC reserva un intervalo de atributos; los programadores de controladores deben reservar valores para su propio uso específico del controlador desde Open Group.  
  
> [!NOTE]
>  La capacidad de establecer atributos de instrucción en el nivel de conexión llamando a **SQLSetConnectAttr** está en desuso en ODBC 3 *. x*. Las aplicaciones ODBC 3 *. x* nunca deben establecer atributos de instrucción en el nivel de conexión. Los atributos de la instrucción ODBC 3 *. x* no se pueden establecer en el nivel de conexión, a excepción de los atributos SQL_ATTR_METADATA_ID y SQL_ATTR_ASYNC_ENABLE, que son atributos de conexión y de instrucción, y se pueden establecer en el nivel de conexión o en el nivel de instrucción.  
> 
>  Los controladores ODBC 3 *. x* solo necesitan admitir esta funcionalidad si deben funcionar con aplicaciones ODBC 2 *. x* que establecen las opciones de la instrucción ODBC 2 *. x* en el nivel de conexión. Para obtener más información, consulte [asignación de SQLSetConnectOption](../../../odbc/reference/appendixes/sqlsetconnectoption-mapping.md) en el Apéndice G: instrucciones del controlador para la compatibilidad con versiones anteriores.  
  
 Una aplicación puede llamar a **SQLSetConnectAttr** en cualquier momento entre el momento en que se asigna y se libera la conexión. Todos los atributos de conexión y de instrucción establecidos correctamente por la aplicación para la conexión se conservan hasta que se llama a **SQLFreeHandle** en la conexión. Por ejemplo, si una aplicación llama a **SQLSetConnectAttr** antes de conectarse a un origen de datos, el atributo persiste incluso si se produce un error en **SQLSetConnectAttr** en el controlador cuando la aplicación se conecta al origen de datos. Si una aplicación establece un atributo específico del controlador, el atributo se conserva aunque la aplicación se conecte a un controlador diferente en la conexión.  
  
 Algunos atributos de conexión solo se pueden establecer antes de que se realice una conexión; otras se pueden establecer solo después de que se haya establecido una conexión. En la tabla siguiente se indican los atributos de conexión que se deben establecer antes o después de realizar una conexión. *Indica que* el atributo se puede establecer antes o después de la conexión.  
  
|Atributo|¿Establecer antes o después de la conexión?|  
|---------------|-------------------------------------|  
|SQL_ATTR_ACCESS_MODE|[1]|  
|SQL_ATTR_ASYNC_DBC_EVENT|Es posible usar el|  
|SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE|[4]|  
|SQL_ATTR_ASYNC_DBC_PCALLBACK|Es posible usar el|  
|SQL_ATTR_ASYNC_DBC_PCONTEXT|Es posible usar el|  
|SQL_ATTR_ASYNC_ENABLE|[2]|  
|SQL_ATTR_AUTO_IPD|Es posible usar el|  
|SQL_ATTR_AUTOCOMMIT|[5]|  
|SQL_ATTR_CONNECTION_DEAD|Después|  
|SQL_ATTR_CONNECTION_TIMEOUT|Es posible usar el|  
|SQL_ATTR_CURRENT_CATALOG|[1]|  
|SQL_ATTR_DBC_INFO_TOKEN|Después|  
|SQL_ATTR_ENLIST_IN_DTC|Después|  
|SQL_ATTR_LOGIN_TIMEOUT|Antes|  
|SQL_ATTR_METADATA_ID|Es posible usar el|  
|SQL_ATTR_ODBC_CURSORS|Antes|  
|SQL_ATTR_PACKET_SIZE|Antes|  
|SQL_ATTR_QUIET_MODE|Es posible usar el|  
|SQL_ATTR_TRACE|Es posible usar el|  
|SQL_ATTR_TRACEFILE|Es posible usar el|  
|SQL_ATTR_TRANSLATE_LIB|Después|  
|SQL_ATTR_TRANSLATE_OPTION|Después|  
|SQL_ATTR_TXN_ISOLATION|[3]|  
  
 [1] SQL_ATTR_ACCESS_MODE y SQL_ATTR_CURRENT_CATALOG se pueden establecer antes o después de la conexión, en función del controlador. Sin embargo, las aplicaciones interoperables las establecen antes de la conexión porque algunos controladores no admiten el cambio de estos después de la conexión.  
  
 [2] SQL_ATTR_ASYNC_ENABLE debe establecerse antes de que haya una instrucción activa.  
  
 [3] SQL_ATTR_TXN_ISOLATION solo se puede establecer si no hay transacciones abiertas en la conexión. Algunos atributos de conexión admiten la sustitución de un valor similar si el origen de datos no admite el valor especificado en \* *ValuePtr*. En tales casos, el controlador devuelve SQL_SUCCESS_WITH_INFO y SQLSTATE 01S02 (valor de opción cambiado). Por ejemplo, si el *atributo* es SQL_ATTR_PACKET_SIZE y \* *ValuePtr* supera el tamaño de paquete máximo, el controlador sustituye el tamaño máximo. Para determinar el valor sustituido, una aplicación llama a **SQLGetConnectAttr**.  
  
 [4] si se establece SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE antes de que se abra una conexión, el administrador de controladores establecerá el atributo del controlador cuando se cargue el controlador durante una llamada a **SQLBrowseConnect**, **SQLConnect** o **SQLDriverConnect**. Antes de realizar una llamada a **SQLBrowseConnect**, **SQLConnect** o **SQLDriverConnect**, el administrador de controladores no sabe a qué controlador debe conectarse y no sabe si el controlador admite operaciones de conexión asincrónicas. Por lo tanto, el administrador de controladores siempre devuelve SQL_SUCCESS. Sin embargo, en caso de que el controlador no admita operaciones de conexión asincrónicas, se producirá un error en la llamada a **SQLBrowseConnect**, **SQLConnect** o **SQLDriverConnect** .  
  
 [5] cuando SQL_ATTR_AUTOCOMMIT está establecido en FALSE, las aplicaciones deben llamar a SQLEndTran (SQL_ROLLBACK) si alguna API devuelve SQL_ERROR para garantizar la coherencia transaccional.  
  
 El formato de la información establecida en el búfer de \* *ValuePtr* depende del *atributo* especificado. **SQLSetConnectAttr** aceptará información de atributos en uno de dos formatos diferentes: una cadena de caracteres terminada en null o un valor entero. El formato de cada se indica en la descripción del atributo. Las cadenas de caracteres a las que apunta el argumento *ValuePtr* de **SQLSetConnectAttr** tienen una longitud de *StringLength* bytes.  
  
 El argumento *StringLength* se omite si el atributo define la longitud, como es el caso de todos los atributos introducidos en ODBC 2 *. x* o anterior.  
  
|*Atributo*|Contenido de *ValuePtr*|  
|-----------------|-------------------------|  
|SQL_ATTR_ACCESS_MODE (ODBC 1,0)|Valor de SQLUINTEGER que incluya. El controlador o el origen de datos utiliza SQL_MODE_READ_ONLY como un indicador de que no es necesario que la conexión admita instrucciones SQL que hagan que se produzcan actualizaciones. Este modo se puede usar para optimizar las estrategias de bloqueo, la administración de transacciones u otras áreas según sea necesario para el controlador o el origen de datos. No es necesario que el controlador impida que se envíen estas instrucciones al origen de datos. El comportamiento del controlador y el origen de datos cuando se le pide que procese instrucciones SQL que no son de solo lectura durante una conexión de solo lectura está definido por la implementación. SQL_MODE_READ_WRITE es el valor predeterminado.|  
|SQL_ATTR_ASYNC_DBC_EVENT (ODBC 3,8)|Un valor de SQLPOINTER que es un identificador de evento.<br /><br /> La notificación de la finalización de las funciones asincrónicas se habilita llamando a **SQLSetConnectAttr** con el atributo SQL_ATTR_ASYNC_STMT_EVENT y especificando el identificador del evento. **Nota:**  El método de notificación no es compatible con la biblioteca de cursores. Una aplicación recibirá un mensaje de error si intenta habilitar la biblioteca de cursores mediante SQLSetConnectAttr, cuando se habilite el método de notificación.|  
|SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE (ODBC 3,8)|Un valor SQLUINTEGER que incluya que habilita o deshabilita la ejecución asincrónica de las funciones seleccionadas en el identificador de conexión. Para obtener más información, vea [ejecución asincrónica (método de sondeo)](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md).<br /><br /> SQL_ASYNC_DBC_ENABLE_ON = habilitar la operación asincrónica para las funciones relacionadas con la conexión especificadas.<br /><br /> SQL_ASYNC_DBC_ENABLE_OFF = (valor predeterminado) deshabilitar la operación asincrónica para las funciones relacionadas con la conexión especificadas.<br /><br /> Establecer SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE es siempre sincrónico (es decir, nunca devolverá SQL_STILL_EXECUTING).<br /><br /> La ejecución asincrónica de las operaciones de instrucción se habilita con SQL_ATTR_ASYNC_ENABLE.|  
|SQL_ATTR_ASYNC_DBC_PCALLBACK (ODBC 3,8)|Valor de SQLPOINTER que apunta a la estructura de contexto.<br /><br /> Solo el administrador de controladores puede llamar a la función **SQLSetStmtAttr** de un controlador con este atributo.|  
|SQL_ATTR_ASYNC_DBC_PCONTEXT (ODBC 3,8)|Valor de SQLPOINTER que apunta a la estructura de contexto.<br /><br /> Solo el administrador de controladores puede llamar a la función **SQLSetStmtAttr** de un controlador con este atributo.|  
|SQL_ATTR_ASYNC_ENABLE (ODBC 3,0)|Un valor SQLULEN que especifica si una función llamada con una instrucción en la conexión especificada se ejecuta de forma asincrónica:<br /><br /> SQL_ASYNC_ENABLE_OFF = deshabilitar la compatibilidad con la ejecución asincrónica en el nivel de conexión para las operaciones de instrucción (el valor predeterminado).<br /><br /> SQL_ASYNC_ENABLE_ON = habilitar la compatibilidad de ejecución asincrónica en el nivel de conexión para las operaciones de instrucción.<br /><br /> Este atributo se puede establecer si **SQLGetInfo** con el tipo de información SQL_ASYNC_MODE devuelve SQL_AM_CONNECTION o SQL_AM_STATEMENT.|  
|SQL_ATTR_AUTO_IPD (ODBC 3,0)|Un valor SQLUINTEGER que incluya de solo lectura que especifica si se admite el rellenado automático de IPD después de una llamada a **SQLPrepare** :<br /><br /> SQL_TRUE = rellenado automático de IPD después de que la llamada a **SQLPrepare** sea compatible con el controlador.<br /><br /> SQL_FALSE = rellenado automático de IPD después de que la llamada a **SQLPrepare** no sea compatible con el controlador. Los servidores que no admiten instrucciones preparadas no podrán rellenar el IPD automáticamente.<br /><br /> Si se devuelve SQL_TRUE para el atributo de conexión SQL_ATTR_AUTO_IPD, se puede establecer el atributo de instrucción SQL_ATTR_ENABLE_AUTO_IPD para activar o desactivar el rellenado automático de IPD. Si SQL_ATTR_AUTO_IPD es SQL_FALSE, SQL_ATTR_ENABLE_AUTO_IPD no se puede establecer en SQL_TRUE. El valor predeterminado de SQL_ATTR_ENABLE_AUTO_IPD es igual al valor de SQL_ATTR_AUTO_IPD.<br /><br /> Este atributo de conexión puede ser devuelto por **SQLGetConnectAttr** , pero **SQLSetConnectAttr** no puede establecerlo.|  
|SQL_ATTR_AUTOCOMMIT (ODBC 1,0)|Un valor SQLUINTEGER que incluya que especifica si se debe usar el modo de confirmación automática o manual:<br /><br /> SQL_AUTOCOMMIT_OFF = el controlador usa el modo de confirmación manual y la aplicación debe confirmar o revertir las transacciones explícitamente con **SQLEndTran**.<br /><br /> SQL_AUTOCOMMIT_ON = el controlador utiliza el modo de confirmación automática. Cada instrucción se confirma inmediatamente después de ejecutarse. Este es el valor predeterminado. Cualquier transacción abierta en la conexión se confirma cuando se establece SQL_ATTR_AUTOCOMMIT en SQL_AUTOCOMMIT_ON para cambiar del modo de confirmación manual al modo de confirmación automática.<br /><br /> Para obtener más información, vea [modo de confirmación](../../../odbc/reference/develop-app/commit-mode.md). **Importante:**  Algunos orígenes de datos eliminan los planes de acceso y cierran los cursores de todas las instrucciones de una conexión cada vez que se confirma una instrucción; el modo de confirmación automática puede hacer que esto suceda después de ejecutar cada instrucción que no es de consulta o cuando se cierra el cursor para una consulta. Para obtener más información, vea los tipos de información SQL_CURSOR_COMMIT_BEHAVIOR y SQL_CURSOR_ROLLBACK_BEHAVIOR en [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) y el [efecto de las transacciones en cursores y instrucciones preparadas](../../../odbc/reference/develop-app/effect-of-transactions-on-cursors-and-prepared-statements.md). <br /><br /> Cuando un lote se ejecuta en modo de confirmación automática, son posibles dos cosas. Todo el lote se puede tratar como una unidad autovalidable, o cada instrucción de un lote se trata como una unidad confirmable. Algunos orígenes de datos pueden admitir ambos comportamientos y pueden proporcionar una forma de elegir uno u otro. Está definido por el controlador si un lote se trata como una unidad confirmable o si cada instrucción individual dentro del lote es confirmable.|  
|SQL_ATTR_CONNECTION_DEAD<br /><br /> (ODBC 3,5)|Un valor SQLUINTEGER que incluya de solo lectura que indica el estado de la conexión. Si SQL_CD_TRUE, se ha perdido la conexión. Si SQL_CD_FALSE, la conexión sigue activa.|  
|SQL_ATTR_CONNECTION_TIMEOUT (ODBC 3,0)|Un valor SQLUINTEGER que incluya que corresponde al número de segundos que se debe esperar para que se complete cualquier solicitud de la conexión antes de volver a la aplicación. El controlador debe devolver SQLSTATE HYT00 (tiempo de espera expirado) siempre que sea posible agotar el tiempo de espera en una situación no asociada a la ejecución de la consulta o al inicio de sesión.<br /><br /> Si *ValuePtr* es igual a 0 (el valor predeterminado), no hay tiempo de espera.|  
|SQL_ATTR_CURRENT_CATALOG (ODBC 2,0)|Cadena de caracteres que contiene el nombre del catálogo que va a usar el origen de datos. Por ejemplo, en SQL Server, el catálogo es una base de datos, por lo que el controlador envía una instrucción **use** _Database_ al origen de datos, donde *Database* es la base de datos especificada en \* *ValuePtr*. Para un controlador de un solo nivel, el catálogo podría ser un directorio, por lo que el controlador cambia su directorio actual al directorio especificado en **ValuePtr*.|  
|SQL_ATTR_DBC_INFO_TOKEN (ODBC 3,8|Un valor de SQLPOINTER que se usa para volver a establecer el token de información de conexión en el identificador DBC cuando el parámetro de [SQLRateConnection](../../../odbc/reference/syntax/sqlrateconnection-function.md)( \* *pRating*) no es igual a 100.<br /><br /> SQL_ATTR_DBC_INFO_TOKEN es solo de conjunto. No es posible usar **SQLGetConnectAttr** o **SQLGetConnectOption** para recuperar este valor. **SQLSetConnectAttr** del administrador de controladores no aceptará SQL_ATTR_DBC_INFO_TOKEN, ya que una aplicación no debe establecer este atributo.<br /><br /> Si un controlador devuelve SQL_ERROR después de establecer SQL_ATTR_DBC_INFO_TOKEN, se liberará la conexión obtenida del grupo. El administrador de controladores intentará obtener otra conexión del grupo. Para obtener más información, consulte [desarrollo de Connection-Pool reconocimiento en un controlador ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md) .|  
|SQL_ATTR_ENLIST_IN_DTC (ODBC 3,0)|Valor SQLPOINTER que especifica si se debe usar el controlador ODBC en las transacciones distribuidas coordinadas por los servicios de componentes de Microsoft.<br /><br /> Pase un objeto de transacción OLE de DTC que especifique la transacción que se va a exportar a SQL Server, o SQL_DTC_DONE para finalizar la Asociación de DTC de la conexión.<br /><br /> El cliente llama al método OLE ITransactionDispenser:: BeginTransaction de Microsoft Coordinador de transacciones distribuidas (MS DTC) para iniciar una transacción de MS DTC y crear un objeto de transacción de MS DTC que representa la transacción. A continuación, la aplicación llama a SQLSetConnectAttr con la opción SQL_ATTR_ENLIST_IN_DTC para asociar el objeto de transacción a la conexión ODBC. Toda la actividad de base de datos relacionada se realizará bajo la protección de la transacción MS DTC. La aplicación llama a SQLSetConnectAttr con SQL_DTC_DONE para finalizar la asociación DTC de la conexión. Para obtener más información, consulte la documentación de MS DTC.|  
|SQL_ATTR_LOGIN_TIMEOUT (ODBC 1,0)|Un valor SQLUINTEGER que incluya que corresponde al número de segundos que se debe esperar para que se complete una solicitud de inicio de sesión antes de volver a la aplicación. El valor predeterminado es dependiente del controlador. Si *ValuePtr* es 0, el tiempo de espera está deshabilitado y un intento de conexión esperará indefinidamente.<br /><br /> Si el tiempo de espera especificado supera el tiempo de espera de inicio de sesión máximo en el origen de datos, el controlador sustituye ese valor y devuelve SQLSTATE 01S02 (valor de opción cambiado).|  
|SQL_ATTR_METADATA_ID (ODBC 3,0)|Un valor SQLUINTEGER que incluya que determina cómo se tratan los argumentos de cadena de las funciones de catálogo.<br /><br /> Si SQL_TRUE, el argumento de cadena de las funciones de catálogo se tratan como identificadores. El caso no es significativo. En el caso de las cadenas no delimitadas, el controlador quita los espacios finales y la cadena se dobla a mayúsculas. En el caso de las cadenas delimitadas, el controlador quita los espacios iniciales o finales y toma literalmente lo que haya entre los delimitadores. Si uno de estos argumentos se establece en un puntero nulo, la función devuelve SQL_ERROR y SQLSTATE HY009 (uso no válido de puntero nulo).<br /><br /> Si SQL_FALSE, los argumentos de cadena de las funciones de catálogo no se tratan como identificadores. El caso es significativo. Pueden contener un modelo de búsqueda de cadenas o no, dependiendo del argumento.<br /><br /> El valor predeterminado es SQL_FALSE.<br /><br /> El argumento *TableType* de **SQLTables**, que toma una lista de valores, no se ve afectado por este atributo.<br /><br /> También se puede establecer SQL_ATTR_METADATA_ID en el nivel de instrucción. (Es el único atributo de conexión que también es un atributo de instrucción).<br /><br /> Para obtener más información, vea [argumentos en funciones de catálogo](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).|  
|SQL_ATTR_ODBC_CURSORS (ODBC 2,0)|Un valor SQLULEN que especifica el modo en que el administrador de controladores utiliza la biblioteca de cursores ODBC:<br /><br /> SQL_CUR_USE_IF_NEEDED = el administrador de controladores solo usa la biblioteca de cursores ODBC si es necesario. Si el controlador admite la opción SQL_FETCH_PRIOR en **SQLFetchScroll**, el administrador de controladores usa las capacidades de desplazamiento del controlador. De lo contrario, utiliza la biblioteca de cursores ODBC.<br /><br /> SQL_CUR_USE_ODBC = el administrador de controladores utiliza la biblioteca de cursores ODBC.<br /><br /> SQL_CUR_USE_DRIVER = el administrador de controladores usa las capacidades de desplazamiento del controlador. Esta es la configuración predeterminada.<br /><br /> Para obtener más información acerca de la biblioteca de cursores ODBC, vea [Apéndice F: biblioteca de cursores ODBC](../../../odbc/reference/appendixes/appendix-f-odbc-cursor-library.md). **ADVERTENCIA:**  La biblioteca de cursores se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.|  
|SQL_ATTR_PACKET_SIZE (ODBC 2,0)|Un valor SQLUINTEGER que incluya que especifica el tamaño de paquete de red en bytes. **Nota:**  Muchos orígenes de datos no admiten esta opción o solo pueden devolver, pero no establecer el tamaño de paquete de red. <br /><br /> Si el tamaño especificado supera el tamaño de paquete máximo o es menor que el tamaño de paquete mínimo, el controlador sustituye ese valor y devuelve SQLSTATE 01S02 (valor de opción cambiado).<br /><br /> Si la aplicación establece el tamaño de los paquetes después de que ya se haya realizado una conexión, el controlador devolverá SQLSTATE HY011 (el atributo no se puede establecer ahora).|  
|SQL_ATTR_QUIET_MODE (ODBC 2,0)|Identificador de ventana (HWND).<br /><br /> Si el identificador de ventana es un puntero nulo, el controlador no muestra ningún cuadro de diálogo.<br /><br /> Si el identificador de ventana no es un puntero nulo, debe ser el identificador de ventana principal de la aplicación. Este es el valor predeterminado. El controlador utiliza este identificador para mostrar los cuadros de diálogo. **Nota:**  El atributo de conexión SQL_ATTR_QUIET_MODE no se aplica a los cuadros de diálogo que se muestran en **SQLDriverConnect**.|  
|SQL_ATTR_TRACE (ODBC 1,0)|Un valor SQLUINTEGER que incluya que indica al administrador de controladores si se realiza el seguimiento:<br /><br /> SQL_OPT_TRACE_OFF = seguimiento desactivado (valor predeterminado)<br /><br /> SQL_OPT_TRACE_ON = seguimiento activado<br /><br /> Cuando el seguimiento está activado, el administrador de controladores escribe cada llamada a función ODBC en el archivo de seguimiento. **Nota:**  Cuando el seguimiento está activado, el administrador de controladores puede devolver SQLSTATE IM013 (error del archivo de seguimiento) desde cualquier función. <br /><br /> Una aplicación especifica un archivo de seguimiento con la opción SQL_ATTR_TRACEFILE. Si el archivo ya existe, el administrador de controladores anexa al archivo. De lo contrario, crea el archivo. Si el seguimiento está activado y no se ha especificado ningún archivo de seguimiento, el administrador de controladores escribe en el archivo SQL. Inicie sesión en el directorio raíz.<br /><br /> Una aplicación puede establecer la variable **ODBCSharedTraceFlag** para habilitar el seguimiento de forma dinámica. Después, el seguimiento se habilita para todas las aplicaciones ODBC que se están ejecutando actualmente. Si una aplicación desactiva el seguimiento, se desactiva solo para esa aplicación.<br /><br /> Si la palabra clave **Trace** de la información del sistema está establecida en 1 cuando una aplicación llama a **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_ENV, el seguimiento está habilitado para todos los identificadores. Solo está habilitada para la aplicación que llamó a **SQLAllocHandle**.<br /><br /> La llamada a **SQLSetConnectAttr** con un *atributo* de SQL_ATTR_TRACE no requiere que el argumento *ConnectionHandle* sea válido y no devolverá SQL_ERROR si *ConnectionHandle* es NULL. Este atributo se aplica a todas las conexiones.|  
|SQL_ATTR_TRACEFILE (ODBC 1,0)|Cadena de caracteres terminada en null que contiene el nombre del archivo de seguimiento.<br /><br /> El valor predeterminado del atributo SQL_ATTR_TRACEFILE se especifica con **la palabra clave de la información** del sistema. Para obtener más información, vea [subclave ODBC](../../../odbc/reference/install/odbc-subkey.md).<br /><br /> La llamada a **SQLSetConnectAttr** con un *atributo* de SQL_ATTR_TRACEFILE no requiere que el argumento *ConnectionHandle* sea válido y no devolverá SQL_ERROR si *ConnectionHandle* no es válido. Este atributo se aplica a todas las conexiones.|  
|SQL_ATTR_TRANSLATE_LIB (ODBC 1,0)|Cadena de caracteres terminada en null que contiene el nombre de una biblioteca que contiene las funciones **SQLDriverToDataSource** y **SQLDataSourceToDriver** a las que el controlador tiene acceso para realizar tareas como la traducción del juego de caracteres. Esta opción solo se puede especificar si el controlador se ha conectado al origen de datos. La configuración de este atributo se conservará entre las conexiones. Para obtener más información sobre la traducción de datos [, vea la](../../../odbc/reference/develop-app/translation-dlls.md) referencia sobre las [funciones DLL](../../../odbc/reference/syntax/translation-dll-api-reference.md)de traducción y de traducción.|  
|SQL_ATTR_TRANSLATE_OPTION (ODBC 1,0)|Valor de marca de 32 bits que se pasa a la DLL de traducción. Este atributo solo se puede especificar si el controlador se ha conectado al origen de datos. Para obtener información sobre la traducción de datos, vea [archivos dll de traducción](../../../odbc/reference/develop-app/translation-dlls.md).|  
|SQL_ATTR_TXN_ISOLATION (ODBC 1,0)|Máscara de bits de 32 bits que establece el nivel de aislamiento de transacción para la conexión actual. Una aplicación debe llamar a **SQLEndTran** para confirmar o revertir todas las transacciones abiertas en una conexión, antes de llamar a **SQLSetConnectAttr** con esta opción.<br /><br /> Los valores válidos para *ValuePtr* se pueden determinar mediante una llamada a **SQLGetInfo** con *InfoType* igual a SQL_TXN_ISOLATION_OPTIONS.<br /><br /> Para obtener una descripción de los niveles de aislamiento de transacción, vea la descripción del tipo de información SQL_DEFAULT_TXN_ISOLATION en [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) y en [los niveles de aislamiento de transacción](../../../odbc/reference/develop-app/transaction-isolation-levels.md).|  
  
 [1] solo se puede llamar a estas funciones de forma asincrónica si el descriptor es un descriptor de implementación, no un descriptor de la aplicación.  
  
## <a name="code-example"></a>Ejemplo de código  
 Vea [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Asignación de un identificador|[Función SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Devolver el valor de un atributo de conexión|[Función SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
