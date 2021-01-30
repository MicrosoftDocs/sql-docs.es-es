---
description: Función SQLBrowseConnect
title: Función SQLBrowseConnect | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLBrowseConnect
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLBrowseConnect
helpviewer_keywords:
- SQLBrowseConnect function [ODBC]
ms.assetid: b7f1be66-e6c7-4790-88ec-62b7662103c0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 33983c9fb41625fa6479c47045ca4a04738a1463
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181078"
---
# <a name="sqlbrowseconnect-function"></a>Función SQLBrowseConnect
**Conformidad**  
 Versión introducida: ODBC 1,0 Standards Compliance: ODBC  
  
 **Resumen**  
 **SQLBrowseConnect** admite un método iterativo de detectar y enumerar los atributos y los valores de atributo necesarios para conectarse a un origen de datos. Cada llamada a **SQLBrowseConnect** devuelve niveles sucesivos de atributos y valores de atributo. Cuando se han enumerado todos los niveles, se completa una conexión al origen de datos y **SQLBrowseConnect** devuelve una cadena de conexión completa. Un código de retorno de SQL_SUCCESS o SQL_SUCCESS_WITH_INFO indica que se ha especificado toda la información de conexión y ahora la aplicación está conectada al origen de datos.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLBrowseConnect(  
     SQLHDBC         ConnectionHandle,  
     SQLCHAR *       InConnectionString,  
     SQLSMALLINT     StringLength1,  
     SQLCHAR *       OutConnectionString,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   StringLength2Ptr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *ConnectionHandle*  
 [Entrada] Identificador de conexión.  
  
 *Inconnectionstring*  
 Entradas Examinar cadena de conexión de solicitud (vea "argumento *inconnectionstring* " en "Comentarios").  
  
 *StringLength1*  
 Entradas Longitud de **inconnectionstring* en caracteres.  
  
 *Outconnectionstring*  
 Genere Puntero a un búfer de caracteres en el que se va a devolver la cadena de conexión del resultado de la exploración (vea el "argumento *outconnectionstring* " en "comments").  
  
 Si *outconnectionstring* es null, *StringLength2Ptr* seguirá devolviendo el número total de caracteres (excepto el carácter de terminación null para los datos de caracteres) disponible para devolver en el búfer señalado por *outconnectionstring*.  
  
 *BufferLength*  
 Entradas Longitud, en caracteres, del búfer **outconnectionstring* .  
  
 *StringLength2Ptr*  
 Genere El número total de caracteres (excluyendo la terminación nula) disponible para devolver en \* *outconnectionstring*. Si el número de caracteres disponibles para devolver es mayor o igual que *BufferLength*, la cadena de conexión de \* *outconnectionstring* se trunca a *BufferLength* menos la longitud de un carácter de terminación null.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NEED_DATA, SQL_ERROR, SQL_INVALID_HANDLE o SQL_STILL_EXECUTING.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLBrowseConnect** devuelve SQL_ERROR, SQL_SUCCESS_WITH_INFO o SQL_NEED_DATA, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador de ConnectionHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que suele devolver **SQLBrowseConnect** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Datos de cadena, truncados a la derecha|El búfer \* *outconnectionstring* no era lo suficientemente grande como para devolver la cadena de conexión del resultado de la exploración completa, por lo que la cadena se truncó. El búfer **StringLength2Ptr* contiene la longitud de la cadena de conexión del resultado de la exploración sin truncar. (La función devuelve SQL_NEED_DATA).|  
|01S00|Atributo de cadena de conexión no válido|Se especificó una palabra clave de atributo no válido en la cadena de conexión de la solicitud de exploración (*inconnectionstring*). (La función devuelve SQL_NEED_DATA).<br /><br /> Se especificó una palabra clave de atributo en la cadena de conexión de la solicitud de exploración (*inconnectionstring*) que no se aplica al nivel de conexión actual. (La función devuelve SQL_NEED_DATA).|  
|01S02|Valor cambiado|El controlador no admitía el valor especificado del argumento *ValuePtr* en **SQLSetConnectAttr** y sustituyó un valor similar. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08001|El cliente no puede establecer la conexión|El controlador no pudo establecer una conexión con el origen de datos.|  
|08002|Nombre de conexión en uso|(DM) la conexión especificada ya se ha utilizado para establecer una conexión con un origen de datos y la conexión estaba abierta.|  
|08004|El servidor rechazó la conexión|El origen de datos rechazó el establecimiento de la conexión por motivos definidos por la implementación.|  
|08S01|Error de vínculo de comunicación|Error en el vínculo de comunicación entre el controlador y el origen de datos al que el controlador estaba intentando conectar antes de que la función finalizara el procesamiento.|  
|28000|Especificación de autorización no válida|El identificador de usuario o la cadena de autorización, o ambos, según se especifica en la cadena de conexión de la solicitud de exploración (*inconnectionstring*), infringiendo las restricciones definidas por el origen de datos.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|(DM) el administrador de controladores no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.<br /><br /> El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|Una operación asincrónica se canceló mediante una llamada a la [función SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md). A continuación, se llamó de nuevo a la función original en *ConnectionHandle*.<br /><br /> Se canceló una operación llamando a **SQLCancelHandle** en *ConnectionHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *ConnectionHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|(DM) el valor especificado para el argumento *StringLength1* era menor que 0 y no era igual a SQL_NTS.<br /><br /> (DM) el valor especificado para el argumento *BufferLength* era menor que 0.|  
|HY114|El controlador no admite la ejecución de función asincrónica de nivel de conexión|(DM) la aplicación habilitó la operación asincrónica en el identificador de conexión antes de realizar la conexión. Sin embargo, el controlador no admite la operación asincrónica en el identificador de conexión.|  
|HYT00|Tiempo de espera agotado|Expiró el tiempo de espera de inicio de sesión antes de que se completara la conexión al origen de datos. El período de tiempo de espera se establece mediante **SQLSetConnectAttr**, SQL_ATTR_LOGIN_TIMEOUT.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador correspondiente al nombre del origen de datos especificado no admite la función.|  
|IM002|No se encontró el origen de datos y no se especificó ningún controlador predeterminado|(DM) el nombre del origen de datos especificado en la cadena de conexión de la solicitud de exploración (*inconnectionstring*) no se encontró en la información del sistema, ni tenía una especificación de controlador predeterminada.<br /><br /> No se encontró el origen de datos ODBC de (DM) y la información de controlador predeterminada en la información del sistema.|  
|IM003|No se pudo cargar el controlador especificado|(DM) el controlador enumerado en la especificación del origen de datos en la información del sistema o especificado por la palabra clave **driver** no se encontró o no se pudo cargar por algún otro motivo.|  
|IM004|Error de **SQLAllocHandle** del controlador en SQL_HANDLE _ENV|(DM) durante **SQLBrowseConnect**, el administrador de controladores llamó a la función **SQLAllocHandle** del controlador con un *HandleType* de SQL_HANDLE_ENV y el controlador devolvió un error.|  
|IM005|Error de **SQLAllocHandle** del controlador en SQL_HANDLE_DBC|(DM) durante **SQLBrowseConnect**, el administrador de controladores llamó a la función **SQLAllocHandle** del controlador con un *HandleType* de SQL_HANDLE_DBC y el controlador devolvió un error.|  
|IM006|Error de **SQLSetConnectAttr** del controlador|(DM) durante **SQLBrowseConnect**, el administrador de controladores llamó a la función **SQLSetConnectAttr** del controlador y el controlador devolvió un error.|  
|IM009|No se puede cargar la DLL de traducción|El controlador no pudo cargar la DLL de traducción que se especificó para el origen de datos o para la conexión.|  
|IM010|El nombre del origen de datos es demasiado largo|(DM) el valor de atributo de la palabra clave DSN era más largo que SQL_MAX_DSN_LENGTH caracteres.|  
|IM011|El nombre del controlador es demasiado largo|(DM) el valor de atributo para la palabra clave DRIVER tenía más de 255 caracteres.|  
|IM012|Error de sintaxis de palabra clave DRIVER|(DM) el par palabra clave-valor de la palabra clave DRIVER contenía un error de sintaxis.|  
|IM014|El DSN especificado contiene una incoherencia de arquitectura entre el controlador y la aplicación|(DM) 32: la aplicación de bits usa un DSN que se conecta a un controlador de 64 bits; o viceversa.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
|S1118|El controlador no admite la notificación asincrónica|Cuando el controlador no admite la notificación asincrónica, no se puede establecer SQL_ATTR_ASYNC_DBC_EVENT o SQL_ATTR_ASYNC_DBC_RETCODE_PTR.|  
  
## <a name="inconnectionstring-argument"></a>Argumento inconnectionstring  
 Una cadena de conexión de solicitud de examen tiene la siguiente sintaxis:  
  
 *Connection-String* :: = *atributo*[ `;` ] &#124;  `;` *cadena de conexión de* atributo;<br>
 *Attribute* :: = *Attribute-keyword* `=` *Attribute-Value* &#124; `DRIVER=` [ `{` ]*atributo-valor*[ `}` ]<br>
 *Attribute-keyword* :: = `DSN` &#124; `UID` &#124; de la `PWD` *palabra clave-Attribute-Defined del controlador* &#124;<br>
 *Attribute-Value* :: = *cadena de caracteres*<br>
 *driver-Defined-Attribute-keyword* :: = *Identifier*<br>
  
 donde *la cadena de caracteres* tiene cero o más caracteres; el *identificador* tiene uno o más caracteres; *Attribute-keyword* no distingue mayúsculas de minúsculas; *Attribute-Value* puede distinguir mayúsculas de minúsculas; y el valor de la palabra clave **DSN** no se compone únicamente de espacios en blanco. Debido a la gramática de la cadena de conexión y al archivo de inicialización, las palabras clave y los valores de atributo que contienen los caracteres **[] {} (),;? \* =! @** debe evitarse. Debido a la gramática de la información del sistema, las palabras clave y los nombres de los orígenes de datos no pueden contener el carácter de barra diagonal inversa ( \\ ). Para un ODBC 2. controlador *x* , se requieren llaves alrededor del valor de atributo para la palabra clave driver.  
  
 Si alguna palabra clave se repite en la cadena de conexión de la solicitud de exploración, el controlador usa el valor asociado a la primera aparición de la palabra clave. Si las palabras clave **DSN** y **driver** están incluidas en la misma cadena de conexión de solicitud de examen, el administrador de controladores y el controlador usan la palabra clave la que aparece en primer lugar.  
  
 Para obtener información sobre cómo una aplicación elige un origen de datos o un controlador, vea [elegir un origen de datos o un controlador](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md).  
  
## <a name="outconnectionstring-argument"></a>Outconnectionstring (argumento)  
 La cadena de conexión de resultados de examen es una lista de atributos de conexión. Un atributo de conexión consta de una palabra clave de atributo y un valor de atributo correspondiente. La cadena de conexión del resultado de la exploración tiene la siguiente sintaxis:  
  
 *Connection-String* :: = *atributo*[ `;` ] &#124;  `;` *cadena de conexión de* atributo<br>
 *Attribute::* = [ `*` ] Attribute *-palabra clave* Attribute `=` *-Value*<br>
 *Attribute-keyword* :: = *ODBC-Attribute-palabra clave* &#124; *driver-Attribute-keyword*<br>
 *ODBC-Attribute-keyword* = { `UID` &#124; `PWD` } [ `:` *identificador adaptado*] *controlador-atributo-atributo-palabra clave* :: = *identificador*[ `:` *localizador-identificador*] *atributo-valor* :: = `{` *atributo-valor-lista* `}` &#124; `?` (las llaves son literales; son devueltas por el controlador).<br>
 *Attribute-Value-List* :: = *cadena de caracteres* [ `:` *cadena de caracteres localizados*] &#124; *cadena de caracteres* [cadena `:` *de caracteres localizados*] `,` *atributo-valor-lista*<br>
  
 donde cadena *de* caracteres y *cadena de caracteres localizados* tienen cero o más caracteres; el *identificador* y *el identificador localizado* tienen uno o más caracteres; *Attribute-keyword* no distingue mayúsculas de minúsculas; y *Attribute-Value* pueden distinguir entre mayúsculas y minúsculas. Debido a la gramática de la cadena de conexión y al archivo de inicialización, las palabras clave, los identificadores localizados y los valores de atributo que contienen los caracteres **[] {} (),;? \* =! @** debe evitarse. Debido a la gramática de la información del sistema, las palabras clave y los nombres de los orígenes de datos no pueden contener el carácter de barra diagonal inversa ( \\ ).  
  
 La sintaxis de la cadena de conexión de resultados de examen se usa según las siguientes reglas semánticas:  
  
-   Si un asterisco ( \* ) precede a una *palabra clave de atributo*, el *atributo* es opcional y se puede omitir en la siguiente llamada a **SQLBrowseConnect**.  
  
-   Las palabras clave de atributo **UID** y **pwd** tienen el mismo significado que el definido en **SQLDriverConnect**.  
  
-   Una *palabra clave de atributo definido por el controlador* designa el tipo de atributo para el que se puede proporcionar un valor de atributo. Por ejemplo, podría tratarse de un **servidor**, una **base de datos**, un **host** o un **DBMS**.  
  
-   Las palabras clave de *atributo ODBC* y *driver-Attribute-Keywords* incluyen una versión localizada o descriptiva de la palabra clave. Esto podría ser utilizado por las aplicaciones como una etiqueta en un cuadro de diálogo. Sin embargo, se debe usar **UID**, **pwd** o el *identificador* solo al pasar una cadena de solicitud de examen al controlador.  
  
-   {*Attribute-Value-List*} es una enumeración de los valores reales válidos para la *palabra clave de atributo* correspondiente. Tenga en cuenta que las llaves ( {} ) no indican una lista de opciones; las devuelve el controlador. Por ejemplo, podría ser una lista de nombres de servidor o una lista de nombres de base de datos.  
  
-   Si el *valor del atributo* es un signo de interrogación (?), un solo valor corresponde a la *palabra clave de atributo*. Por ejemplo, UID = JohnS; PWD = sésamo.  
  
-   Cada llamada a **SQLBrowseConnect** solo devuelve la información necesaria para satisfacer el siguiente nivel del proceso de conexión. El controlador asocia información de estado con el identificador de conexión para que siempre se pueda determinar el contexto en cada llamada.  
  
## <a name="using-sqlbrowseconnect"></a>Usar SQLBrowseConnect  
 **SQLBrowseConnect** requiere una conexión asignada. El administrador de controladores carga el controlador que se especificó en o que corresponde al nombre del origen de datos especificado en la cadena de conexión de la solicitud de exploración inicial; para obtener información sobre cuándo se produce esto, consulte la sección "Comentarios" en la [función SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md). El controlador puede establecer una conexión con el origen de datos durante el proceso de exploración. Si **SQLBrowseConnect** devuelve SQL_ERROR, se terminan las conexiones pendientes y la conexión se devuelve a un estado no conectado.  
  
> [!NOTE]  
>  **SQLBrowseConnect** no admite la agrupación de conexiones. Si se llama a **SQLBrowseConnect** mientras la agrupación de conexiones está habilitada, se devolverá SQLSTATE HY000 (error general).  
  
 Cuando se llama a **SQLBrowseConnect** por primera vez en una conexión, la cadena de conexión de la solicitud de exploración debe contener la palabra clave **DSN** o la palabra clave **driver** . Si la cadena de conexión de la solicitud de exploración contiene la palabra clave **DSN** , el administrador de controladores busca una especificación de origen de datos correspondiente en la información del sistema:  
  
-   Si el administrador de controladores encuentra la especificación de origen de datos correspondiente, carga la DLL del controlador asociada; el controlador puede recuperar información sobre el origen de datos a partir de la información del sistema.  
  
-   Si el administrador de controladores no encuentra la especificación de origen de datos correspondiente, busca la especificación de origen de datos predeterminada y carga la DLL del controlador asociada. el controlador puede recuperar información acerca del origen de datos predeterminado de la información del sistema. "DEFAULT" se pasa al controlador del DSN.  
  
-   Si el administrador de controladores no encuentra la especificación de origen de datos correspondiente y no hay ninguna especificación de origen de datos predeterminada, devuelve SQL_ERROR con SQLSTATE IM002 (origen de datos no encontrado y no se ha especificado ningún controlador predeterminado).  
  
 Si la cadena de conexión de la solicitud de exploración contiene la palabra clave **driver** , el administrador de controladores carga el controlador especificado. no intenta ubicar un origen de datos en la información del sistema. Dado que la palabra clave **driver** no utiliza información de la información del sistema, el controlador debe definir palabras clave suficientes para que un controlador pueda conectarse a un origen de datos solo mediante la información de las cadenas de conexión de la solicitud de exploración.  
  
 En cada llamada a **SQLBrowseConnect**, la aplicación especifica los valores de atributo de conexión en la cadena de conexión de la solicitud de exploración. El controlador devuelve niveles sucesivos de atributos y valores de atributo en la cadena de conexión del resultado de la exploración. devuelve SQL_NEED_DATA siempre y cuando haya atributos de conexión que todavía no se hayan enumerado en la cadena de conexión de la solicitud de exploración. La aplicación usa el contenido de la cadena de conexión del resultado de la exploración para generar la cadena de conexión de la solicitud de exploración para la siguiente llamada a **SQLBrowseConnect**. Todos los atributos obligatorios (los no precedidos por un asterisco en el argumento *outconnectionstring* ) deben incluirse en la siguiente llamada a **SQLBrowseConnect**. Tenga en cuenta que la aplicación no puede usar el contenido de las cadenas de conexión de resultados de examen anteriores al compilar la cadena de conexión de la solicitud de exploración actual. es decir, no puede especificar valores diferentes para los atributos establecidos en los niveles anteriores.  
  
 Cuando se han enumerado todos los niveles de conexión y sus atributos asociados, el controlador devuelve SQL_SUCCESS, se completa la conexión al origen de datos y se devuelve una cadena de conexión completa a la aplicación. La cadena de conexión es adecuada para su uso, junto con **SQLDriverConnect**, con la opción SQL_DRIVER_NOPROMPT para establecer otra conexión. No obstante, no se puede usar la cadena de conexión completa en otra llamada a **SQLBrowseConnect**. Si se llama de nuevo a **SQLBrowseConnect** , toda la secuencia de llamadas tendría que repetirse.  
  
 **SQLBrowseConnect** también devuelve SQL_NEED_DATA si hay errores irrecuperables recuperables durante el proceso de exploración; por ejemplo, una contraseña no válida o una palabra clave de atributo suministrada por la aplicación. Cuando se devuelve SQL_NEED_DATA y la cadena de conexión del resultado de la exploración no cambia, se produce un error y la aplicación puede llamar a **SQLGetDiagRec** para devolver el valor de SQLSTATE para los errores en tiempo de exploración. Esto permite que la aplicación corrija el atributo y continúe el examen.  
  
 Una aplicación puede finalizar el proceso de exploración en cualquier momento llamando a **SQLDisconnect**. El controlador terminará las conexiones pendientes y devolverá la conexión a un estado no conectado.  
  
 Si las operaciones asincrónicas están habilitadas en el identificador de conexión, **SQLBrowseConnect** también podría devolver SQL_STILL_EXECUTING. Cuando devuelve SQL_NEED_DATA, una aplicación debe usar **SQLDisconnect** para cancelar el proceso de exploración. Si **SQLBrowseConnect** devuelve SQL_STILL_EXECUTING, una aplicación debe usar **SQLCancelHandle** para cancelar la operación en curso. Llamar a **SQLCancelHandle** después de que la función devuelva SQL_NEED_DATA no tiene ningún efecto.  
  
 Para obtener más información, consulte [conectar con SQLBrowseConnect](../../../odbc/reference/develop-app/connecting-with-sqlbrowseconnect.md).  
  
 Si un controlador admite **SQLBrowseConnect**, la sección de la palabra clave driver de la información del sistema para el controlador debe contener la palabra clave **ConnectFunctions** con el tercer carácter establecido en "Y".  
  
## <a name="code-example"></a>Ejemplo de código  
  
> [!NOTE]  
>  Si se va a conectar a un proveedor de origen de datos que admite la autenticación de Windows, debe especificar `Trusted_Connection=yes` en lugar de la información de ID. de usuario y contraseña en la cadena de conexión.  
  
 En el ejemplo siguiente, una aplicación llama a **SQLBrowseConnect** varias veces. Cada vez que **SQLBrowseConnect** devuelve SQL_NEED_DATA, pasa información sobre los datos que necesita en \* *outconnectionstring*. La aplicación pasa *outconnectionstring* a su rutina **GetUserInput** (no se muestra). **GetUserInput** analiza la información, crea y muestra un cuadro de diálogo y devuelve la información especificada por el usuario en \* *inconnectionstring*. La aplicación pasa la información del usuario al controlador en la siguiente llamada a **SQLBrowseConnect**. Una vez que la aplicación ha proporcionado toda la información necesaria para que el controlador se conecte al origen de datos, **SQLBrowseConnect** devuelve SQL_SUCCESS y la aplicación continúa.  
  
 Para obtener un ejemplo más detallado de la conexión a un controlador de SQL Server llamando a **SQLBrowseConnect**, consulte [SQL Server ejemplo de exploración](../../../odbc/reference/develop-app/sql-server-browsing-example.md).  
  
 Por ejemplo, para conectarse al origen de datos sales, pueden producirse las acciones siguientes. En primer lugar, la aplicación pasa la siguiente cadena a **SQLBrowseConnect**:  
  
```  
"DSN=Sales"  
```  
  
 El administrador de controladores carga el controlador asociado al origen de datos sales. A continuación, llama a la función **SQLBrowseConnect** del controlador con los mismos argumentos que recibió de la aplicación. El controlador devuelve la siguiente cadena en **outconnectionstring*:  
  
```  
"HOST:Server={red,blue,green};UID:ID=?;PWD:Password=?"  
```  
  
 La aplicación pasa esta cadena a su rutina **GetUserInput** , que crea un cuadro de diálogo que pide al usuario que seleccione el servidor rojo, azul o verde y que escriba un identificador de usuario y una contraseña. La rutina pasa la siguiente información especificada por el usuario de nuevo en \* *inconnectionstring*, que la aplicación pasa a **SQLBrowseConnect**:  
  
```  
"HOST=red;UID=Smith;PWD=Sesame"  
```  
  
 **SQLBrowseConnect** usa esta información para conectarse al servidor rojo como Smith con la contraseña sésamo y, a continuación, devuelve la siguiente cadena en **outconnectionstring*:  
  
```  
"*DATABASE:Database={SalesEmployees,SalesGoals,SalesOrders}"  
```  
  
 La aplicación pasa esta cadena a su rutina **GetUserInput** , que crea un cuadro de diálogo que pide al usuario que seleccione una base de datos. El usuario selecciona EmpData y la aplicación llama a **SQLBrowseConnect** una hora final con esta cadena:  
  
```  
"DATABASE=SalesOrders"  
```  
  
 Esta es la parte final de la información que el controlador necesita para conectarse al origen de datos; **SQLBrowseConnect** devuelve SQL_SUCCESS y **outconnectionstring* contiene la cadena de conexión completada:  
  
```cpp  
// SQLBrowseConnect_Function.cpp  
// compile with: odbc32.lib  
#include <windows.h>  
#include <sqltypes.h>  
#include <sqlext.h>  
  
#define BRWS_LEN 100  
SQLHENV henv;  
SQLHDBC hdbc;  
SQLHSTMT hstmt;  
SQLRETURN retcode;  
SQLCHAR szConnStrIn[BRWS_LEN], szConnStrOut[BRWS_LEN];  
SQLSMALLINT cbConnStrOut;  
  
void GetUserInput(SQLCHAR * szConnStrOut, SQLCHAR * szConnStrIn) {}  
  
int main() {  
   // Allocate the environment handle.  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);        
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
  
      // Set the version environment attribute.  
      retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
  
         // Allocate the connection handle.  
         retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
            // Call SQLBrowseConnect until it returns a value other than SQL_NEED_DATA   
            // (pass data source name the first time).  If SQL_NEED_DATA is returned, call GetUserInput   
            // (not shown) to build a dialog from the values in szConnStrOut.  The user-supplied values   
            // are returned in szConnStrIn, which is passed in the next call to SQLBrowseConnect.  
  
            strcpy_s((char*)szConnStrIn, _countof(szConnStrIn), "DSN=Sales");  
            do {  
               retcode = SQLBrowseConnect(hdbc, szConnStrIn, SQL_NTS,  
                  szConnStrOut, BRWS_LEN, &cbConnStrOut);  
               if (retcode == SQL_NEED_DATA)  
                  GetUserInput(szConnStrOut, szConnStrIn);  
            } while (retcode == SQL_NEED_DATA);  
  
            if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO){  
  
               // Allocate the statement handle.  
               retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
                  // Process data after successful connection  
                  SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
               SQLDisconnect(hdbc);  
            }  
         }  
         SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
      }  
   }  
   SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
```  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Asignación de un identificador de conexión|[Función SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Conectarse a un origen de datos|[Función SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|Desconectar de un origen de datos|[Función SQLDisconnect](../../../odbc/reference/syntax/sqldisconnect-function.md)|  
|Conectar con un origen de datos mediante una cadena de conexión o un cuadro de diálogo|[Función SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|Devolver descripciones y atributos de controladores|[Función SQLDrivers](../../../odbc/reference/syntax/sqldrivers-function.md)|  
|Liberar un identificador de conexión|[Función SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
