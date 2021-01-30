---
description: Función SQLAllocHandle
title: SQLAllocHandle (función) | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLAllocHandle
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLAllocHandle
helpviewer_keywords:
- SQLAllocHandle function [ODBC]
ms.assetid: 6e7fe420-8cf4-4e72-8dad-212affaff317
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1d546ddfa5f7e18365fec4bdff08032a8bba749f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184330"
---
# <a name="sqlallochandle-function"></a>Función SQLAllocHandle
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 3,0: ISO 92  
  
 **Resumen**  
 **SQLAllocHandle** asigna un entorno, una conexión, una instrucción o un identificador de descriptor.  
  
> [!NOTE]  
>  Esta función es una función genérica para asignar identificadores que reemplazan las funciones de ODBC 2,0 **SQLAllocConnect**, **SQLAllocEnv** y **SQLAllocStmt**. Para permitir que las aplicaciones que llaman a **SQLAllocHandle** funcionen con ODBC 2. *x* , se asigna una llamada a **SQLAllocHandle** en el administrador de controladores a **SQLAllocConnect**, **SQLAllocEnv** o **SQLAllocStmt**, según corresponda. Para obtener más información, vea "Comentarios". Para obtener más información sobre lo que el administrador de controladores asigna a esta función cuando se trata de un ODBC 3. la aplicación *x* está trabajando con ODBC 2. controlador *x* , consulte [asignación de funciones de reemplazo para la compatibilidad con versiones anteriores de las aplicaciones](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLAllocHandle(  
      SQLSMALLINT   HandleType,  
      SQLHANDLE     InputHandle,  
      SQLHANDLE *   OutputHandlePtr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *HandleType*  
 Entradas El tipo de identificador que se va a asignar mediante **SQLAllocHandle**. Debe ser uno de los siguientes valores:   
  
-   SQL_HANDLE_DBC  
  
-   SQL_HANDLE_DBC_INFO_TOKEN  
  
-   SQL_HANDLE_DESC  
  
-   SQL_HANDLE_ENV  
  
-   SQL_HANDLE_STMT  
  
 El identificador de SQL_HANDLE_DBC_INFO_TOKEN solo lo utiliza el administrador de controladores y el controlador. Las aplicaciones no deben usar este tipo de identificador. Para obtener más información acerca de SQL_HANDLE_DBC_INFO_TOKEN, vea [desarrollar el reconocimiento de Connection-Pool en un controlador ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).  
  
 *InputHandle*  
 Entradas Identificador de entrada en cuyo contexto se va a asignar el nuevo identificador. Si *HandleType* es SQL_HANDLE_ENV, se SQL_NULL_HANDLE. Si *HandleType* es SQL_HANDLE_DBC, debe ser un identificador de entorno y, si es SQL_HANDLE_STMT o SQL_HANDLE_DESC, debe ser un identificador de conexión.  
  
 *OutputHandlePtr*  
 Genere Puntero a un búfer en el que se va a devolver el identificador a la estructura de datos recién asignada.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_INVALID_HANDLE o SQL_ERROR.  
  
 Al asignar un identificador que no sea un identificador de entorno, si **SQLAllocHandle** devuelve SQL_ERROR, establece *OutputHandlePtr* en SQL_NULL_HDBC, SQL_NULL_HSTMT o SQL_NULL_HDESC, según el valor de *HandleType*, a menos que el argumento de salida sea un puntero nulo. A continuación, la aplicación puede obtener información adicional de la estructura de datos de diagnóstico asociada al identificador en el argumento *InputHandle* .  
  
## <a name="environment-handle-allocation-errors"></a>Errores de asignación de control de entorno  
 La asignación de entorno se produce en el administrador de controladores y dentro de cada controlador. El error devuelto por **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_ENV depende del nivel en el que se produjo el error.  
  
 Si el administrador de controladores no puede asignar memoria para *\* OutputHandlePtr* cuando se llama a **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_ENV, o la aplicación proporciona un puntero nulo para *OutputHandlePtr*, **SQLAllocHandle** devuelve SQL_ERROR. El administrador de controladores establece **OutputHandlePtr* en SQL_NULL_HENV (a menos que la aplicación proporcione un puntero nulo, que devuelve SQL_ERROR). No hay ningún identificador con el que asociar información de diagnóstico adicional.  
  
 El administrador de controladores no llama a la función de asignación de identificador de entorno de nivel de controlador hasta que la aplicación llama a **SQLConnect**, **SQLBrowseConnect** o **SQLDriverConnect**. Si se produce un error en la función **SQLAllocHandle** de nivel de controlador, la función **SQLConnect**, **SQLBrowseConnect** o **SQLDriverConnect** de nivel de administrador de controladores devuelve SQL_ERROR. La estructura de datos de diagnóstico contiene SQLSTATE IM004 (error de **SQLAllocHandle** del controlador). El error se devuelve en un identificador de conexión.  
  
 Para obtener más información sobre el flujo de llamadas de función entre el administrador de controladores y un controlador, consulte [función SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLAllocHandle** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con los valores de *HandleType* y *Handle* adecuados establecidos en el valor de *InputHandle*. SQL_SUCCESS_WITH_INFO (pero no SQL_ERROR) se puede devolver para el argumento *OutputHandle* . En la tabla siguiente se enumeran los valores de SQLSTATE que suele devolver **SQLAllocHandle** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08003|Conexión no abierta|(DM) el argumento *HandleType* se SQL_HANDLE_STMT o SQL_HANDLE_DESC, pero no se abrió la conexión especificada por el argumento *InputHandle* . El proceso de conexión debe completarse correctamente (y la conexión debe estar abierta) para que el controlador asigne un identificador de instrucción o descriptor.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer **MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|(DM) el administrador de controladores no pudo asignar memoria para el identificador especificado.<br /><br /> El controlador no pudo asignar memoria para el identificador especificado.|  
|HY009|Uso no válido de puntero nulo|(DM) el argumento *OutputHandlePtr* era un puntero nulo.|  
|HY010|Error de secuencia de función|(DM) el argumento *HandleType* se SQL_HANDLE_DBC y no se ha llamado a **SQLSetEnvAttr** para establecer el atributo de entorno SQL_ODBC_VERSION.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica para **InputHandle** y que todavía se estaba ejecutando cuando se llamó a la función **SQLAllocHandle** con **HandleType** establecida en SQL_HANDLE_STMT o SQL_HANDLE_DESC.|  
|HY013|Error de administración de memoria|El argumento *HandleType* era SQL_HANDLE_DBC, SQL_HANDLE_STMT o SQL_HANDLE_DESC; no se pudo procesar la llamada a la función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY014|Límite en el número de identificadores superado|Se ha alcanzado el límite definido por el controlador para el número de identificadores que se pueden asignar para el tipo de identificador indicado por el argumento *HandleType* .|  
|HY092|Identificador de opción/atributo no válido|(DM) el argumento *HandleType* no era: SQL_HANDLE_ENV, SQL_HANDLE_DBC, SQL_HANDLE_STMT o SQL_HANDLE_DESC.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|Se SQL_HANDLE_DESC el argumento *HandleType* y el controlador era ODBC 2. controlador *x* .|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el argumento *HandleType* se SQL_HANDLE_STMT y el controlador no era un controlador ODBC válido.<br /><br /> (DM) el argumento *HandleType* se SQL_HANDLE_DESC y el controlador no admite la asignación de un identificador de descriptor.|  
  
## <a name="comments"></a>Comentarios  
 **SQLAllocHandle** se utiliza para asignar identificadores para entornos, conexiones, instrucciones y descriptores, tal como se describe en las secciones siguientes. Para obtener información general acerca de los identificadores, vea [identificadores](../../../odbc/reference/develop-app/handles.md).  
  
 Una aplicación puede asignar más de un entorno, conexión o identificador de instrucción a la vez si el controlador admite varias asignaciones. En ODBC, no se define ningún límite en el número de identificadores de entorno, conexión, instrucción o descriptor que se pueden asignar en un momento dado. Los controladores pueden imponer un límite en cuanto al número de un determinado tipo de identificador que se puede asignar a la vez; para obtener más información, consulte la documentación del controlador.  
  
 Si la aplicación llama a **SQLAllocHandle** con *\* OutputHandlePtr* establecido en un entorno, una conexión, una instrucción o un identificador de descriptor que ya existe, el controlador sobrescribe la información asociada con el *identificador*, a menos que la aplicación use la agrupación de conexiones (vea "asignar un atributo de entorno para la agrupación de conexiones" más adelante en esta sección). El administrador de controladores no comprueba si el *identificador* especificado en *\* OutputHandlePtr* ya se está usando, ni comprueba el contenido anterior de un identificador antes de sobrescribirlo.  
  
> [!NOTE]  
>  La programación de aplicaciones ODBC es incorrecta para llamar a **SQLAllocHandle** dos veces con la misma variable de aplicación definida para *\* OutputHandlePtr* sin llamar a **SQLFreeHandle** para liberar el identificador antes de reasignarlo. Si se sobrescriben los identificadores ODBC de tal manera, se podría producir un comportamiento incoherente o errores en la parte de los controladores ODBC.  
  
 En los sistemas operativos que admiten varios subprocesos, las aplicaciones pueden usar el mismo entorno, conexión, instrucción o identificador de descriptor en subprocesos diferentes. Por lo tanto, los controladores deben admitir el acceso multiproceso seguro a esta información; una manera de lograrlo, por ejemplo, es usar una sección crítica o un semáforo. Para obtener más información sobre los subprocesos, vea [multithreading](../../../odbc/reference/develop-app/multithreading.md).  
  
 **SQLAllocHandle** no establece el atributo de entorno SQL_ATTR_ODBC_VERSION cuando se llama para asignar un identificador de entorno. la aplicación debe establecer el atributo de entorno, o SQLSTATE HY010 (error de secuencia de función) se devolverá cuando se llame a **SQLAllocHandle** para asignar un identificador de conexión.  
  
 En el caso de las aplicaciones compatibles con los estándares, **SQLAllocHandle** se asigna a **SQLAllocHandleStd** en tiempo de compilación. La diferencia entre estas dos funciones es que **SQLAllocHandleStd** establece el atributo de entorno SQL_ATTR_ODBC_VERSION en SQL_OV_ODBC3 cuando se llama con el argumento *HandleType* establecido en SQL_HANDLE_ENV. Esto se hace porque las aplicaciones compatibles con los estándares siempre son ODBC 3. aplicaciones *x* . Además, los estándares no requieren que se registre la versión de la aplicación. Esta es la única diferencia entre estas dos funciones: de lo contrario, son idénticos. **SQLAllocHandleStd** se asigna a **SQLAllocHandle** dentro del administrador de controladores. Por lo tanto, los controladores de terceros no tienen que implementar **SQLAllocHandleStd**.  
  
 Las aplicaciones ODBC 3,8 deben usar:  
  
-   **SQLAllocHandle y no SQLAllocHandleStd** para asignar un identificador de entorno.  
  
-   **SQLSetEnvAttr** para establecer el atributo de entorno SQL_ATTR_ODBC_VERSION en SQL_OV_ODBC3_80.  
  
## <a name="allocating-an-environment-handle"></a>Asignar un identificador de entorno  
 Un identificador de entorno proporciona acceso a información global, como los identificadores de conexión válidos y los identificadores de conexión activos. Para obtener información general acerca de los identificadores de entorno, consulte [controladores de entorno](../../../odbc/reference/develop-app/environment-handles.md).  
  
 Para solicitar un identificador de entorno, una aplicación llama a **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_ENV y un *InputHandle* de SQL_NULL_HANDLE. El controlador asigna memoria para la información del entorno y devuelve el valor del identificador asociado en el argumento *\* OutputHandlePtr* . La aplicación pasa el valor *\* OutputHandle* en todas las llamadas subsiguientes que requieren un argumento de identificador de entorno. Para obtener más información, consulte [asignar el identificador de entorno](../../../odbc/reference/develop-app/allocating-the-environment-handle.md).  
  
 En el identificador de entorno de un administrador de controladores, si ya existe el identificador de entorno de un controlador, no se llama a **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_ENV en ese controlador cuando se establece una conexión, solo **sqlallochandle** con un *HandleType* de SQL_HANDLE_DBC. Si el identificador de entorno de un controlador no existe en el identificador de entorno del administrador de controladores, se llama a SQLAllocHandle con un HandleType de SQL_HANDLE_ENV y SQLAllocHandle con un HandleType de SQL_HANDLE_DBC en el controlador cuando el primer identificador de conexión del entorno está conectado al controlador.  
  
 Cuando el administrador de controladores procesa la función **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_ENV, comprueba la palabra clave **Trace** en la sección [ODBC] de la información del sistema. Si se establece en 1, el administrador de controladores habilita el seguimiento para la aplicación actual. Si se establece la marca de seguimiento, el seguimiento se inicia cuando se asigna el primer identificador de entorno y termina cuando se libera el último identificador de entorno. Para obtener más información, vea [configuración de orígenes de datos](../../../odbc/reference/install/configuring-data-sources.md).  
  
 Después de asignar un identificador de entorno, una aplicación debe llamar a **SQLSetEnvAttr** en el identificador de entorno para establecer el atributo de entorno SQL_ATTR_ODBC_VERSION. Si no se establece este atributo antes de llamar a **SQLAllocHandle** para asignar un identificador de conexión en el entorno, la llamada para asignar la conexión devolverá SQLSTATE HY010 (error de secuencia de función). Para obtener más información, vea [declarar la versión de ODBC de la aplicación](../../../odbc/reference/develop-app/declaring-the-application-s-odbc-version.md).  
  
## <a name="allocating-shared-environments-for-connection-pooling"></a>Asignación de entornos compartidos para la agrupación de conexiones  
 Los entornos se pueden compartir entre varios componentes en un único proceso. Varios componentes pueden usar un entorno compartido al mismo tiempo. Cuando un componente utiliza un entorno compartido, puede usar conexiones agrupadas, lo que le permite asignar y usar una conexión existente sin volver a crear la conexión.  
  
 Antes de asignar un entorno compartido que se pueda usar para la agrupación de conexiones, una aplicación debe llamar a **SQLSetEnvAttr** para establecer el atributo de entorno SQL_ATTR_CONNECTION_POOLING en SQL_CP_ONE_PER_DRIVER o SQL_CP_ONE_PER_HENV. En este caso, se llama a **SQLSetEnvAttr** con *EnvironmentHandle* establecido en null, lo que convierte el atributo en un atributo de nivel de proceso.  
  
 Una vez habilitada la agrupación de conexiones, una aplicación llama a **SQLAllocHandle** con el argumento *HandleType* establecido en SQL_HANDLE_ENV. El entorno asignado por esta llamada será un entorno compartido implícito porque se ha habilitado la agrupación de conexiones.  
  
 Cuando se asigna un entorno compartido, el entorno que se utilizará no se determinará hasta que se llame a **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_DBC. En ese momento, el administrador de controladores intenta encontrar un entorno existente que coincida con los atributos de entorno solicitados por la aplicación. Si no existe ningún entorno de este tipo, se crea uno como un entorno compartido. El administrador de controladores mantiene un recuento de referencias para cada entorno compartido; el recuento se establece en 1 cuando se crea el entorno por primera vez. Si se encuentra un entorno coincidente, el identificador de ese entorno se devuelve a la aplicación y se incrementa el recuento de referencias. Se puede usar un identificador de entorno asignado de esta manera en cualquier función ODBC que acepte un identificador de entorno como argumento de entrada.  
  
## <a name="allocating-a-connection-handle"></a>Asignar un identificador de conexión  
 Un identificador de conexión proporciona acceso a información como la instrucción y los identificadores de descriptor válidos en la conexión y si una transacción está abierta actualmente. Para obtener información general acerca de los identificadores de conexión, vea [identificadores de conexión](../../../odbc/reference/develop-app/connection-handles.md).  
  
 Para solicitar un identificador de conexión, una aplicación llama a **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_DBC. El argumento *InputHandle* se establece en el identificador de entorno que devolvió la llamada a **SQLAllocHandle** que asignó ese identificador. El controlador asigna memoria para la información de conexión y pasa el valor del identificador asociado de nuevo en *\* OutputHandlePtr*. La aplicación pasa el valor *\* OutputHandlePtr* en todas las llamadas subsiguientes que requieren un identificador de conexión. Para obtener más información, consulte [asignar un identificador de conexión](../../../odbc/reference/develop-app/allocating-a-connection-handle-odbc.md).  
  
 El administrador de controladores procesa la función **SQLAllocHandle** y llama a la función **SQLAllocHandle** del controlador cuando la aplicación llama a **SQLConnect**, **SQLBrowseConnect** o **SQLDriverConnect**. (Para obtener más información, vea [función SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)).  
  
 Si no se establece el atributo de entorno SQL_ATTR_ODBC_VERSION antes de llamar a **SQLAllocHandle** para asignar un identificador de conexión en el entorno, la llamada para asignar la conexión devolverá SQLSTATE HY010 (error de secuencia de función).  
  
 Cuando una aplicación llama a **SQLAllocHandle** con el argumento *InputHandle* establecido en SQL_HANDLE_DBC y también se establece en un identificador de entorno compartido, el administrador de controladores intenta encontrar un entorno compartido existente que coincida con los atributos de entorno establecidos por la aplicación. Si no existe ningún entorno de este tipo, se crea uno, con un recuento de referencias (mantenido por el administrador de controladores) de 1. Si se encuentra un entorno compartido coincidente, este identificador se devuelve a la aplicación y se incrementa su recuento de referencias.  
  
 El administrador de controladores no determina la conexión real que se utilizará hasta que se llame a **SQLConnect** o **SQLDriverConnect** . El administrador de controladores usa las opciones de conexión en la llamada a **SQLConnect** (o las palabras clave de conexión en la llamada a **SQLDriverConnect**) y los atributos de conexión establecidos después de la asignación de conexión para determinar qué conexión del grupo se debe usar. Para obtener más información, consulte [función SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
## <a name="allocating-a-statement-handle"></a>Asignar un identificador de instrucción  
 Un identificador de instrucción proporciona acceso a información de instrucciones, como mensajes de error, el nombre del cursor y la información de estado para el procesamiento de instrucciones SQL. Para obtener información general acerca de los identificadores de instrucciones, vea [identificadores de instrucciones](../../../odbc/reference/develop-app/statement-handles.md).  
  
 Para solicitar un identificador de instrucción, una aplicación se conecta a un origen de datos y, a continuación, llama a **SQLAllocHandle** antes de enviar instrucciones SQL. En esta llamada, *HandleType* debe establecerse en SQL_HANDLE_STMT y *InputHandle* debe establecerse en el identificador de conexión devuelto por la llamada a **SQLAllocHandle** que asignó ese identificador. El controlador asigna memoria para la información de la instrucción, asocia el identificador de instrucción a la conexión especificada y pasa el valor del identificador asociado de nuevo en *\* OutputHandlePtr*. La aplicación pasa el valor *\* OutputHandlePtr* en todas las llamadas subsiguientes que requieren un identificador de instrucción. Para obtener más información, vea [asignar un identificador de instrucción](../../../odbc/reference/develop-app/allocating-a-statement-handle-odbc.md).  
  
 Cuando se asigna el identificador de instrucción, el controlador asigna automáticamente un conjunto de cuatro descriptores y asigna los identificadores de estos descriptores a los atributos de instrucción SQL_ATTR_APP_ROW_DESC, SQL_ATTR_APP_PARAM_DESC, SQL_ATTR_IMP_ROW_DESC y SQL_ATTR_IMP_PARAM_DESC. Estos se conocen como descriptores asignados *implícitamente* . Para asignar explícitamente un descriptor de aplicación, vea la sección siguiente, "asignar un identificador de descriptor".  
  
## <a name="allocating-a-descriptor-handle"></a>Asignación de un identificador de descriptor  
 Cuando una aplicación llama a **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_DESC, el controlador asigna un descriptor de la aplicación. Estos se conocen como descriptores asignados *explícitamente* . La aplicación dirige a un controlador para que use un descriptor de aplicación asignado explícitamente en lugar de uno asignado automáticamente para un identificador de instrucción determinado mediante una llamada a la función **SQLSetStmtAttr** con el atributo SQL_ATTR_APP_ROW_DESC o SQL_ATTR_APP_PARAM_DESC. Un descriptor de implementación no se puede asignar explícitamente, ni se puede especificar un descriptor de implementación en una llamada de función **SQLSetStmtAttr** .  
  
 Los descriptores asignados explícitamente se asocian con un identificador de conexión en lugar de un identificador de instrucción (como los descriptores asignados automáticamente). Los descriptores permanecen asignados solo cuando una aplicación está realmente conectada a la base de datos. Dado que los descriptores asignados explícitamente están asociados a un identificador de conexión, una aplicación puede asociar un descriptor asignado explícitamente a más de una instrucción dentro de una conexión. Un descriptor de aplicación asignado implícitamente, por otro lado, no se puede asociar a más de un identificador de instrucción. (No se puede asociar con ningún identificador de instrucción que no sea el para el que se asignó). Los identificadores de descriptor asignados explícitamente pueden ser liberados explícitamente por la aplicación o llamando a **SQLFreeHandle** con un *HandleType* de SQL_HANDLE_DESC, o implícitamente cuando se cierra la conexión.  
  
 Cuando se libera el descriptor asignado explícitamente, el descriptor asignado implícitamente vuelve a estar asociado a la instrucción. (El atributo SQL_ATTR_APP_ROW_DESC o SQL_ATTR_APP_PARAM_DESC de la instrucción se vuelve a establecer en el identificador de descriptor asignado implícitamente). Esto se aplica a todas las instrucciones asociadas con el descriptor asignado explícitamente en la conexión.  
  
 Para obtener más información acerca de los descriptores, consulte [descriptores](../../../odbc/reference/develop-app/descriptors.md).  
  
## <a name="code-example"></a>Ejemplo de código  
 Vea el [programa ODBC de ejemplo](../../../odbc/reference/sample-odbc-program.md), la función [SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md), la [función SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)y la [función SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Ejecutar una instrucción SQL|[Función SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Ejecutar una instrucción SQL preparada|[Función SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Liberar un entorno, una conexión, una instrucción o un identificador de descriptor|[Función SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|Preparar una instrucción para su ejecución|[Función SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
|Establecer un atributo de conexión|[Función SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Establecer un campo de descriptor|[Función SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|Establecer un atributo de entorno|[Función SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|  
|Establecer un atributo de instrucción|[Función SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
