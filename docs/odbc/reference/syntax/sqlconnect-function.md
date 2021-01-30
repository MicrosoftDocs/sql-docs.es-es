---
description: Función SQLConnect
title: Función SQLConnect | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLConnect
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLConnect
helpviewer_keywords:
- SQLConnect function [ODBC]
ms.assetid: 59075e46-a0ca-47bf-972a-367b08bb518d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 90d982db52e0656fba25eb54065ea84b751a7b18
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206140"
---
# <a name="sqlconnect-function"></a>Función SQLConnect
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: ISO 92  
  
 **Resumen**  
 **SQLConnect** establece conexiones con un controlador y un origen de datos. El identificador de conexión hace referencia al almacenamiento de toda la información sobre la conexión al origen de datos, incluido el estado, el estado de la transacción y la información del error.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLConnect(  
     SQLHDBC        ConnectionHandle,  
     SQLCHAR *      ServerName,  
     SQLSMALLINT    NameLength1,  
     SQLCHAR *      UserName,  
     SQLSMALLINT    NameLength2,  
     SQLCHAR *      Authentication,  
     SQLSMALLINT    NameLength3);  
```  
  
## <a name="arguments"></a>Argumentos  
 *ConnectionHandle*  
 [Entrada] Identificador de conexión.  
  
 *ServerName*  
 Entradas Nombre del origen de datos. Los datos pueden estar ubicados en el mismo equipo que el programa o en otro equipo de una red. Para obtener información sobre cómo una aplicación elige un origen de datos, vea [elegir un origen de datos o un controlador](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md).  
  
 *NameLength1*  
 Entradas Longitud de **ServerName* en caracteres.  
  
 *UserName*  
 Entradas Identificador de usuario.  
  
 *NameLength2*  
 Entradas Longitud de **nombre de usuario* en caracteres.  
  
 *Autenticación*  
 Entradas Cadena de autenticación (normalmente la contraseña).  
  
 *NameLength3*  
 Entradas Longitud de **autenticación* en caracteres.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_INVALID_HANDLE o SQL_STILL_EXECUTING.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLConnect** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_DBC y un *identificador* de *ConnectionHandle*. En la tabla siguiente se enumeran los valores SQLSTATE que devuelve normalmente **SQLConnect** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01S02|Valor de opción cambiado|El controlador no admitía el valor especificado del argumento *ValuePtr* en **SQLSetConnectAttr** y sustituyó un valor similar. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08001|El cliente no puede establecer la conexión|El controlador no pudo establecer una conexión con el origen de datos.|  
|08002|Nombre de conexión en uso|(DM) el *ConnectionHandle* especificado ya se ha utilizado para establecer una conexión con un origen de datos y la conexión todavía estaba abierta o el usuario estaba buscando una conexión.|  
|08004|El servidor rechazó la conexión|El origen de datos rechazó el establecimiento de la conexión por motivos definidos por la implementación.|  
|08S01|Error de vínculo de comunicación|Error en el vínculo de comunicación entre el controlador y el origen de datos al que el controlador estaba intentando conectar antes de que la función finalizara el procesamiento.|  
|28000|Especificación de autorización no válida|El valor especificado para el *nombre de usuario* del argumento o el valor especificado para la *autenticación* de argumento infringió las restricciones definidas por el origen de datos.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|(DM) el administrador de controladores no pudo asignar memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *ConnectionHandle*. Se llamó a la función **SQLConnect** y antes de completar la ejecución, se llamó a la [función SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md) en *ConnectionHandle* y, a continuación, se llamó de nuevo a la función **SQLConnect** en el *ConnectionHandle*.<br /><br /> O bien, se llamó a la función **SQLConnect** y antes de completar la ejecución, se llamó a **SQLCancelHandle** en *ConnectionHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *ConnectionHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|(DM) el valor especificado para el argumento *NameLength1*, *NameLength2* o *NameLength3* era menor que 0 pero no es igual a SQL_NTS.<br /><br /> (DM) el valor especificado para el argumento *NameLength1* superó la longitud máxima de un nombre de origen de datos.|  
|HYT00|Tiempo de espera agotado|El período de tiempo de espera de consulta expiró antes de que se completara la conexión al origen de datos. El período de tiempo de espera se establece mediante **SQLSetConnectAttr**, SQL_ATTR_LOGIN_TIMEOUT.|  
|HY114|El controlador no admite la ejecución de función asincrónica de nivel de conexión|(DM) la aplicación habilitó la operación asincrónica en el identificador de conexión antes de realizar la conexión. Sin embargo, el controlador no admite operaciones asincrónicas en el identificador de conexión.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador especificado por el nombre del origen de datos no admite la función.|  
|IM002|No se encontró el origen de datos y no se especificó ningún controlador predeterminado|(DM) el nombre del origen de datos especificado en el argumento *ServerName* no se encontró en la información del sistema, ni tenía una especificación de controlador predeterminada.|  
|IM003|El controlador especificado no se pudo conectar a|(DM) el controlador enumerado en la especificación de origen de datos de la información del sistema no se encontró o no se pudo conectar a por algún otro motivo.|  
|IM004|Error de SQLAllocHandle del controlador en SQL_HANDLE_ENV|(DM) durante **SQLConnect**, el administrador de controladores llamó a la función **SQLAllocHandle** del controlador con un *HandleType* de SQL_HANDLE_ENV y el controlador devolvió un error.|  
|IM005|Error de SQLAllocHandle del controlador en SQL_HANDLE_DBC|(DM) durante **SQLConnect**, el administrador de controladores llamó a la función **SQLAllocHandle** del controlador con un *HandleType* de SQL_HANDLE_DBC y el controlador devolvió un error.|  
|IM006|Error de SQLSetConnectAttr del controlador|Durante **SQLConnect**, el administrador de controladores llamó a la función **SQLSetConnectAttr** del controlador y el controlador devolvió un error. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|IM009|No se puede conectar a la DLL de traducción|El controlador no pudo conectarse a la DLL de traducción que se especificó para el origen de datos.|  
|IM010|El nombre del origen de datos es demasiado largo|(DM) *\* ServerName* tenía más de SQL_MAX_DSN_LENGTH caracteres.|  
|IM014|El DSN especificado contiene una incoherencia de arquitectura entre el controlador y la aplicación|(DM) 32: la aplicación de bits usa un DSN que se conecta a un controlador de 64 bits; o viceversa.|  
|IM015|Error de SQLConnect del controlador en SQL_HANDLE_DBC_INFO_HANDLE|Si un controlador devuelve SQL_ERROR, el administrador de controladores devolverá SQL_ERROR a la aplicación y se producirá un error en la conexión.<br /><br /> Para obtener más información acerca de SQL_HANDLE_DBC_INFO_TOKEN, vea [desarrollar el reconocimiento de Connection-Pool en un controlador ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
|S1118|El controlador no admite la notificación asincrónica|Cuando el controlador no admite la notificación asincrónica, no se puede establecer SQL_ATTR_ASYNC_DBC_EVENT o SQL_ATTR_ASYNC_DBC_RETCODE_PTR.|  
  
## <a name="comments"></a>Comentarios  
 Para obtener información sobre por qué una aplicación usa **SQLConnect**, consulte [conectar con SQLConnect](../../../odbc/reference/develop-app/connecting-with-sqlconnect.md).  
  
 El administrador de controladores no se conecta a un controlador hasta que la aplicación llama a una función (**SQLConnect**, **SQLDriverConnect** o **SQLBrowseConnect**) para conectarse al controlador. Hasta ese momento, el administrador de controladores trabaja con sus propios identificadores y administra la información de conexión. Cuando la aplicación llama a una función de conexión, el administrador de controladores comprueba si un controlador está conectado actualmente a para el *ConnectionHandle* especificado:  
  
-   Si un controlador no está conectado a, el administrador de controladores se conecta al controlador y llama a **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_ENV, **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_DBC, **SQLSetConnectAttr** (si la aplicación especificó algún atributo de conexión) y la función de conexión en el controlador. El administrador de controladores devuelve SQLSTATE IM006 (error de **SQLSetConnectOption** del controlador) y SQL_SUCCESS_WITH_INFO para la función de conexión si el controlador devolvió un error para **SQLSetConnectAttr**. Para obtener más información, vea [conectarse a un origen de datos o a un controlador](../../../odbc/reference/develop-app/connecting-to-a-data-source-or-driver.md).  
  
-   Si el controlador especificado ya está conectado a en *ConnectionHandle*, el administrador de controladores solo llama a la función de conexión del controlador. En este caso, el controlador debe asegurarse de que todos los atributos de conexión para el *ConnectionHandle* mantienen su configuración actual.  
  
-   Si un controlador diferente está conectado a, el administrador de controladores llama a **SQLFreeHandle** con un *HandleType* de SQL_HANDLE_DBC y, a continuación, si no hay otro controlador conectado a en ese entorno, llama a **SQLFreeHandle** con un *HandleType* de SQL_HANDLE_ENV en el controlador conectado y, a continuación, desconecta ese controlador. A continuación, realiza las mismas operaciones que cuando un controlador no está conectado a.  
  
 A continuación, el controlador asigna identificadores e inicializa.  
  
 Cuando la aplicación llama a **SQLDisconnect**, el administrador de controladores llama a **SQLDisconnect** en el controlador. Sin embargo, no desconecta el controlador. Esto mantiene el controlador en la memoria para las aplicaciones que se conectan y desconectan repetidamente de un origen de datos. Cuando la aplicación llama a **SQLFreeHandle** con un *HandleType* de SQL_HANDLE_DBC, el administrador de controladores llama a **SQLFreeHandle** con un *HandleType* de SQL_HANDLE_DBC y, después, **SQLFreeHandle** con un *HandleType* de SQL_HANDLE_ENV en el controlador y, a continuación, desconecta el controlador.  
  
 Una aplicación ODBC puede establecer más de una conexión.  
  
## <a name="driver-manager-guidelines"></a>Directrices para el administrador de controladores  
 El contenido de **ServerName* afecta a la forma en que el administrador de controladores y un controlador funcionan juntos para establecer una conexión a un origen de datos.  
  
-   Si \* *ServerName* contiene un nombre de origen de datos válido, el administrador de controladores busca la especificación de origen de datos correspondiente en la información del sistema y se conecta al controlador asociado. El administrador de controladores pasa cada argumento de **SQLConnect** al controlador.  
  
-   Si no se puede encontrar el nombre del origen de datos o *ServerName* es un puntero nulo, el administrador de controladores busca la especificación de origen de datos predeterminada y se conecta al controlador asociado. El administrador de controladores pasa al controlador los argumentos de *nombre de usuario* y *autenticación* sin modificar y "predeterminado" para el argumento *ServerName* .  
  
-   Si el argumento *ServerName* es "default", el administrador de controladores busca la especificación de origen de datos predeterminada y se conecta al controlador asociado. El administrador de controladores pasa cada argumento de **SQLConnect** al controlador.  
  
-   Si no se puede encontrar el nombre del origen de datos o si *ServerName* es un puntero nulo y la especificación del origen de datos predeterminada no existe, el administrador de controladores devuelve SQL_ERROR con SQLSTATE IM002 (no se encontró el nombre del origen de datos y no se ha especificado ningún controlador predeterminado).  
  
 Una vez que se conecta a mediante el administrador de controladores, un controlador puede buscar su especificación de origen de datos correspondiente en la información del sistema y usar la información específica del controlador de la especificación para completar su conjunto de información de conexión requerida.  
  
 Si se especifica una biblioteca de traducción predeterminada en la información del sistema para el origen de datos, el controlador se conecta a ella. Se puede conectar una biblioteca de traducción diferente a mediante una llamada a **SQLSetConnectAttr** con el atributo SQL_ATTR_TRANSLATE_LIB. Se puede especificar una opción de traducción mediante una llamada a **SQLSetConnectAttr** con el atributo SQL_ATTR_TRANSLATE_OPTION.  
  
 Si un controlador admite **SQLConnect**, la sección de la palabra clave driver de la información del sistema para el controlador debe contener la palabra clave **ConnectFunctions** con el primer carácter establecido en "Y".  
  
### <a name="connection-pooling"></a>Agrupar conexiones  
 La agrupación de conexiones permite a una aplicación volver a usar una conexión que ya se ha creado. Cuando la agrupación de conexiones está habilitada y se llama a **SQLConnect** , el administrador de controladores intenta establecer la conexión con una conexión que forma parte de un grupo de conexiones en un entorno que se ha designado para la agrupación de conexiones. Este entorno es un entorno compartido que usan todas las aplicaciones que usan las conexiones en el grupo.  
  
 La agrupación de conexiones está habilitada antes de que se asigne el entorno mediante una llamada a **SQLSetEnvAttr** para establecer SQL_ATTR_CONNECTION_POOLING en SQL_CP_ONE_PER_DRIVER (que especifica un máximo de un grupo por controlador) o SQL_CP_ONE_PER_HENV (que especifica un máximo de un grupo por entorno). En este caso, se llama a **SQLSetEnvAttr** con *EnvironmentHandle* establecido en null, lo que convierte el atributo en un atributo de nivel de proceso. Si SQL_ATTR_CONNECTION_POOLING está establecido en SQL_CP_OFF, la agrupación de conexiones está deshabilitada.  
  
 Una vez habilitada la agrupación de conexiones, se llama a **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_ENV para asignar un entorno. El entorno asignado por esta llamada es un entorno compartido porque se ha habilitado la agrupación de conexiones. Sin embargo, el entorno que se usará no se determinará hasta que se llame a **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_DBC.  
  
 Se llama a **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_DBC para asignar una conexión. El administrador de controladores intenta encontrar un entorno compartido existente que coincida con los atributos de entorno establecidos por la aplicación. Si no existe ningún entorno de este tipo, se crea uno como un *entorno compartido* implícito. Si se encuentra un entorno compartido coincidente, se devuelve el identificador de entorno a la aplicación y se incrementa su recuento de referencias.  
  
 Sin embargo, la conexión que se utilizará no se determinará hasta que se llame a **SQLConnect** . En ese momento, el administrador de controladores intenta encontrar una conexión existente en el grupo de conexiones que coincida con los criterios solicitados por la aplicación. Estos criterios incluyen las opciones de conexión solicitadas en la llamada a **SQLConnect** (los valores de *ServerName*, *username* y las palabras clave de *autenticación* ) y cualquier atributo de conexión establecido desde **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_DBC se llamó a. El administrador de controladores comprueba estos criterios en función de las palabras clave de conexión y los atributos correspondientes de las conexiones del grupo. Si se encuentra una coincidencia, se usa la conexión en el grupo. Si no se encuentra ninguna coincidencia, se crea una nueva conexión.  
  
 Si el atributo de entorno SQL_ATTR_CP_MATCH está establecido en SQL_CP_STRICT_MATCH, la coincidencia debe ser exacta para una conexión del grupo que se va a usar. Si el atributo de entorno SQL_ATTR_CP_MATCH está establecido en SQL_CP_RELAXED_MATCH, las opciones de conexión de la llamada a **SQLConnect** deben coincidir, pero no todos los atributos de conexión deben coincidir.  
  
 Las siguientes reglas se aplican cuando se llama a un atributo de conexión, establecido por la aplicación antes de que se llame a **SQLConnect** , no coincide con el atributo de conexión de la conexión en el Grupo:  
  
-   Si el atributo de conexión debe establecerse antes de que se realice la conexión:  
  
     Si SQL_ATTR_CP_MATCH es SQL_CP_STRICT_MATCH, SQL_ATTR_PACKET_SIZE en la conexión agrupada debe ser idéntico al atributo establecido por la aplicación. Si SQL_CP_RELAXED_MATCH, los valores de SQL_ATTR_PACKET_SIZE pueden ser diferentes.  
  
     El valor de SQL_ATTR_LOGIN_VALUE no afecta a la coincidencia.  
  
-   Si el atributo de conexión se puede establecer antes o después de realizar la conexión:  
  
     Si la aplicación no ha establecido el atributo de conexión pero se ha establecido en la conexión del grupo y existe un valor predeterminado, el atributo de conexión de la conexión agrupada se vuelve a establecer en el valor predeterminado y se declara una coincidencia. Si no hay ningún valor predeterminado, la conexión agrupada no se considera una coincidencia.  
  
     Si la aplicación ha establecido el atributo de conexión pero no se ha establecido en la conexión del grupo, el atributo de conexión del grupo se cambia a ese conjunto por la aplicación y se declara una coincidencia.  
  
     Si la aplicación ha establecido el atributo de conexión y también se ha establecido en la conexión del grupo pero los valores son diferentes, se usa el valor del atributo de conexión de la aplicación y se declara una coincidencia.  
  
-   Si los valores de los atributos de conexión específicos del controlador no son idénticos y SQL_ATTR_CP_MATCH está establecido en SQL_CP_STRICT_MATCH, no se utiliza la conexión en el grupo.  
  
 Cuando la aplicación llama a **SQLDisconnect** para desconectarse, la conexión se devuelve al grupo de conexiones y está disponible para su reutilización.  
  
### <a name="optimizing-connection-pooling-performance"></a>Optimizar el rendimiento de la agrupación de conexiones  
 Cuando intervienen las transacciones distribuidas, es posible optimizar el rendimiento de la agrupación de conexiones mediante **SQL_DTC_TRANSITION_COST**, que es una máscara de sqluinteger que incluya. Las transiciones a las que se hace referencia son las transiciones del atributo de conexión SQL_ATTR_ENLIST_IN_DTC van del valor 0 a un valor distinto de cero y viceversa. Se trata de una conexión que se va a no dar de alta en una transacción distribuida a dada de alta en una transacción distribuida y viceversa. Según la forma en que el controlador haya implementado la inscripción (estableciendo el atributo de conexión SQL_ATTR_ENLIST_IN_DTC), estas transiciones pueden ser costosas y, por tanto, deben evitarse para mejorar el rendimiento.  
  
 El valor devuelto por el controlador contiene cualquier combinación de los bits siguientes:  
  
-   **SQL_DTC_ENLIST_EXPENSIVE**, cuando se establece, implica que la transición entre cero y distinto de cero es significativamente más cara que una transición de distinto de cero a otro valor distinto de cero (da de alta una conexión dada de alta previamente en su siguiente transacción).  
  
-   **SQL_DTC_UNENLIST_EXPENSIVE**, cuando se establece, implica que la transición entre cero y cero es significativamente más cara que el uso de una conexión cuyo atributo SQL_ATTR_ENLIST_IN_DTC ya está establecido en cero.  
  
 Existe un equilibrio entre el rendimiento y el uso de la conexión. Si un controlador indica que una o más de estas transiciones son costosas, el concentrador de conexiones del administrador de controladores responde a esto manteniendo más conexiones en el grupo. Algunas de las conexiones del grupo son preferibles para el uso no transaccional y otras se prefieren para el uso transaccional. Sin embargo, si el controlador indica que estas transiciones no son costosas, se pueden usar menos conexiones, quizás alternar entre el uso no transaccional y el de transacciones.  
  
 Los controladores que no admiten SQL_ATTR_ENLIST_IN_DTC no necesitan admitir SQL_DTC_TRANSITION_COST. En el caso de los controladores que admiten SQL_ATTR_ENLIST_IN_DTC pero no SQL_DTC_TRANSITION_COST, se supone que las transiciones no son costosas, como si el controlador devolviera 0 (sin conjuntos de bits) para este valor.  
  
 Aunque SQL_DTC_TRANSITION_COST se presentó en ODBC 3,5, ODBC 2. el controlador *x* también puede ser compatible porque el administrador de controladores consultará esta información independientemente de la versión del controlador.  
  
### <a name="code-example"></a>Ejemplo de código  
 En el ejemplo siguiente, una aplicación asigna controladores de entorno y de conexión. Después, se conecta al origen de datos SalesOrders con el identificador de usuario JohnS y la contraseña sésamo y procesa los datos. Cuando ha terminado de procesar los datos, se desconecta del origen de datos y libera los identificadores.  
  
```cpp  
// SQLConnect_ref.cpp  
// compile with: odbc32.lib  
#include <windows.h>  
#include <sqlext.h>  
  
int main() {  
   SQLHENV henv;  
   SQLHDBC hdbc;  
   SQLHSTMT hstmt;  
   SQLRETURN retcode;  
  
   SQLCHAR * OutConnStr = (SQLCHAR * )malloc(255);  
   SQLSMALLINT * OutConnStrLen = (SQLSMALLINT *)malloc(255);  
  
   // Allocate environment handle  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
  
   // Set the ODBC version environment attribute  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
      retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (void*)SQL_OV_ODBC3, 0);   
  
      // Allocate connection handle  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
         retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
  
         // Set login timeout to 5 seconds  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
            SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
            // Connect to data source  
            retcode = SQLConnect(hdbc, (SQLCHAR*) "NorthWind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
  
            // Allocate statement handle  
            if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
               retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);   
  
               // Process data  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
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
  
### <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Asignación de un identificador|[Función SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Detección y enumeración de los valores necesarios para conectarse a un origen de datos|[Función SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|  
|Desconectar de un origen de datos|[Función SQLDisconnect](../../../odbc/reference/syntax/sqldisconnect-function.md)|  
|Conectar con un origen de datos mediante una cadena de conexión o un cuadro de diálogo|[Función SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|Devolver el valor de un atributo de conexión|[Función SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Establecer un atributo de conexión|[Función SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
