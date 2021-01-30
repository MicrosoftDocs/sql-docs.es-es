---
title: SQLDriverConnect (función) | Microsoft Docs
description: La función SQLDriverConnect forma parte del estándar de la API de ODBC y esta documentación de referencia proporciona información sobre su sintaxis.
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDriverConnect
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDriverConnect
helpviewer_keywords:
- SQLDriverConnect function [ODBC]
ms.assetid: e299be1d-5c74-4ede-b6a3-430eb189134f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 58c9f2071825c6c7d8f873f300e8e5c3f46af9b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195636"
---
# <a name="sqldriverconnect-function"></a>Función SQLDriverConnect
**Conformidad**  
 Versión introducida: ODBC 1,0 Standards Compliance: ODBC  
  
 **Resumen**  
 **SQLDriverConnect** es una alternativa a **SQLConnect**. Admite orígenes de datos que requieren más información de conexión que los tres argumentos de **SQLConnect**, cuadros de diálogo para solicitar al usuario toda la información de conexión y orígenes de datos que no están definidos en la información del sistema. Para obtener más información, consulte [conectar con SQLDriverConnect](../develop-app/connecting-with-sqldriverconnect.md).  

## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLDriverConnect(  
     SQLHDBC         ConnectionHandle,  
     SQLHWND         WindowHandle,  
     SQLCHAR *       InConnectionString,  
     SQLSMALLINT     StringLength1,  
     SQLCHAR *       OutConnectionString,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   StringLength2Ptr,  
     SQLUSMALLINT    DriverCompletion);  
```  
  
## <a name="arguments"></a>Argumentos  
 *ConnectionHandle*  
 [Entrada] Identificador de conexión.  
  
 *WindowHandle*  
 Entradas Identificador de ventana. La aplicación puede pasar el identificador de la ventana primaria, si procede, o un puntero nulo si el identificador de ventana no es aplicable o **SQLDriverConnect** no presentará ningún cuadro de diálogo.  
  
 *Inconnectionstring*  
 Entradas Una cadena de conexión completa (vea la sintaxis en "Comentarios"), una cadena de conexión parcial o una cadena vacía.  
  
 *StringLength1*  
 Entradas Longitud de **inconnectionstring*, en caracteres si la cadena es Unicode, o bytes si la cadena es ANSI o DBCS.  
  
 *Outconnectionstring*  
 Genere Puntero a un búfer para la cadena de conexión completada. Cuando la conexión se realiza correctamente con el origen de datos de destino, este búfer contiene la cadena de conexión completada. Las aplicaciones deben asignar al menos 1.024 caracteres para este búfer.  
  
 Si *outconnectionstring* es null, *StringLength2Ptr* seguirá devolviendo el número total de caracteres (excepto el carácter de terminación null para los datos de caracteres) disponible para devolver en el búfer señalado por *outconnectionstring*.  
  
 *BufferLength*  
 Entradas Longitud del búfer de **outconnectionstring* , en caracteres.  
  
 *StringLength2Ptr*  
 Genere Puntero a un búfer en el que se va a devolver el número total de caracteres (excepto el carácter de terminación null) disponible para devolver en \* *outconnectionstring*. Si el número de caracteres disponibles para devolver es mayor o igual que *BufferLength*, la cadena de conexión completada en \* *outconnectionstring* se trunca a *BufferLength* menos la longitud de un carácter de terminación null.  
  
 *DriverCompletion*  
 Entradas Marca que indica si el administrador de controladores o el controlador debe solicitar más información de conexión:  
  
 SQL_DRIVER_PROMPT, SQL_DRIVER_COMPLETE, SQL_DRIVER_COMPLETE_REQUIRED o SQL_DRIVER_NOPROMPT.  
  
 (Para obtener más información, vea "Comentarios").  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR, SQL_INVALID_HANDLE o SQL_STILL_EXECUTING.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLDriverConnect** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *FHandleType* de SQL_HANDLE_DBC y un *hHandle* de *ConnectionHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que se devuelven normalmente por **SQLDriverConnect** y se explica cada uno en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Datos de cadena, truncados a la derecha|El búfer \* *outconnectionstring* no era lo suficientemente grande como para devolver la cadena de conexión completa, por lo que se truncó la cadena de conexión. La longitud de la cadena de conexión sin truncar se devuelve en **StringLength2Ptr*. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01S00|Atributo de cadena de conexión no válido|Se especificó una palabra clave de atributo no válido en la cadena de conexión (*inconnectionstring*), pero el controlador se pudo conectar al origen de datos de todos modos. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01S02|Valor de opción cambiado|El controlador no admitía el valor especificado al que apunta el argumento *ValuePtr* en **SQLSetConnectAttr** y sustituyó un valor similar. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01S08|Error al guardar el DSN de archivo|La cadena de *\* inconnectionstring* contiene una palabra clave **FILEDSN** , pero no se guardó el archivo. DSN. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01S09|Palabra clave no válida|(DM) la cadena de *\* inconnectionstring* contenía una palabra clave **SaveFile** pero no un **controlador** o una palabra clave **FILEDSN** . (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08001|El cliente no puede establecer la conexión|El controlador no pudo establecer una conexión con el origen de datos.|  
|08002|Nombre de conexión en uso|(DM) el *ConnectionHandle* especificado ya se ha utilizado para establecer una conexión con un origen de datos y la conexión todavía estaba abierta.|  
|08004|El servidor rechazó la conexión|El origen de datos rechazó el establecimiento de la conexión por motivos definidos por la implementación.|  
|08S01|Error de vínculo de comunicación|Error en el vínculo de comunicación entre el controlador y el origen de datos al que el controlador estaba intentando conectar antes de que la función **SQLDriverConnect** completara el procesamiento.|  
|28000|Especificación de autorización no válida|El identificador de usuario o la cadena de autorización, o ambos, según se especifica en la cadena de conexión (*inconnectionstring*), infringió las restricciones definidas por el origen de datos.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* szMessageText* describe el error y su causa.|  
|HY000|Error general: DSN de archivo no válido|(DM) la cadena en **Inconnectionstring* contenía una palabra clave FILEDSN, pero no se encontró el nombre del archivo. DSN.|  
|HY000|Error general: no se puede crear el búfer de archivos|(DM) la cadena en **Inconnectionstring* contenía una palabra clave FILEDSN, pero el archivo. DSN era ilegible.|  
|HY001|Error de asignación de memoria|El administrador de controladores no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función **SQLDriverConnect** .<br /><br /> El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *ConnectionHandle*. Se llamó a la función y antes de completar la ejecución, se llamó a la [función SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md) en *ConnectionHandle* y, a continuación, se llamó de nuevo a la función **SQLDriverConnect** en el *ConnectionHandle*.<br /><br /> O bien, se llamó a la función **SQLDriverConnect** y antes de completar la ejecución, se llamó a **SQLCancelHandle** en *ConnectionHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY010|Error de secuencia de función|(DM) se llamó a otra función que se ejecuta de forma asincrónica (no **SQLDriverConnect**) para *ConnectionHandle* y que todavía se estaba ejecutando cuando se llamó a la función **SQLDriverConnect** .|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada a la función **SQLDriverConnect** porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|(DM) el valor especificado para el argumento *StringLength1* era menor que 0 y no era igual a SQL_NTS.<br /><br /> (DM) el valor especificado para el argumento *BufferLength* era menor que 0.|  
|HY092|Identificador de opción/atributo no válido|(DM) el argumento *DriverCompletion* se SQL_DRIVER_PROMPT y el argumento *WindowHandle* era un puntero nulo.|  
|HY110|Finalización de controlador no válida|(DM) el valor especificado para el argumento *DriverCompletion* no es igual a SQL_DRIVER_PROMPT, SQL_DRIVER_COMPLETE, SQL_DRIVER_COMPLETE_REQUIRED o SQL_DRIVER_NOPROMPT.<br /><br /> La agrupación de conexiones (DM) estaba habilitada y el valor especificado para el argumento *DriverCompletion* no es igual a SQL_DRIVER_NOPROMPT.|  
|HYC00|Característica opcional no implementada|El controlador no admite la versión del comportamiento de ODBC que solicitó la aplicación.|  
|HYT00|Tiempo de espera agotado|Expiró el tiempo de espera de inicio de sesión antes de que se completara la conexión al origen de datos. El período de tiempo de espera se establece mediante **SQLSetConnectAttr**, SQL_ATTR_LOGIN_TIMEOUT.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador correspondiente al nombre del origen de datos especificado no admite la función.|  
|IM002|No se encontró el origen de datos y no se especificó ningún controlador predeterminado|(DM) el nombre del origen de datos especificado en la cadena de conexión (*inconnectionstring*) no se encontró en la información del sistema y no había ninguna especificación de controlador predeterminada.<br /><br /> No se encontró el origen de datos ODBC de (DM) y la información de controlador predeterminada en la información del sistema.|  
|IM003|No se pudo cargar el controlador especificado|(DM) el controlador enumerado en la especificación del origen de datos en la información del sistema o especificado por la palabra clave **driver** no se encontró o no se pudo cargar por algún otro motivo.|  
|IM004|Error de **SQLAllocHandle** del controlador en SQL_HANDLE_ENV|(DM) durante **SQLDriverConnect**, el administrador de controladores llamó a la función **SQLAllocHandle** del controlador con un *fHandleType* de SQL_HANDLE_ENV y el controlador devolvió un error.|  
|IM005|Error de **SQLAllocHandle** del controlador en SQL_HANDLE_DBC.|(DM) durante **SQLDriverConnect**, el administrador de controladores llamó a la función **SQLAllocHandle** del controlador con un *fHandleType* de SQL_HANDLE_DBC y el controlador devolvió un error.|  
|IM006|Error de **SQLSetConnectAttr** del controlador|(DM) durante **SQLDriverConnect**, el administrador de controladores llamó a la función **SQLSetConnectAttr** del controlador y el controlador devolvió un error.|  
|IM007|No se especificó ningún origen de datos o controlador; cuadro de diálogo prohibido|No se especificó ningún nombre o controlador de origen de datos en la cadena de conexión y *DriverCompletion* se SQL_DRIVER_NOPROMPT.|  
|IM008|Error de diálogo|El controlador intentó mostrar el cuadro de diálogo de inicio de sesión y se produjo un error.<br /><br /> *WindowHandle* era un puntero nulo y *DriverCompletion* no se SQL_DRIVER_NO_PROMPT.|  
|IM009|No se puede cargar la DLL de traducción|El controlador no pudo cargar la DLL de traducción que se especificó para el origen de datos o para la conexión.|  
|IM010|El nombre del origen de datos es demasiado largo|(DM) el valor de atributo de la palabra clave DSN era más largo que SQL_MAX_DSN_LENGTH caracteres.|  
|IM011|El nombre del controlador es demasiado largo|(DM) el valor de atributo para la palabra clave **driver** tenía más de 255 caracteres.|  
|IM012|Error de sintaxis de palabra clave DRIVER|(DM) el par palabra clave-valor de la palabra clave **driver** contenía un error de sintaxis.<br /><br /> (DM) la cadena de *\* inconnectionstring* contiene una palabra clave **FILEDSN** , pero el archivo. DSN no contenía una palabra clave de **controlador** o una palabra clave de **DSN** .|  
|IM014|El DSN especificado contiene una incoherencia de arquitectura entre el controlador y la aplicación|(DM) 32: la aplicación de bits usa un DSN que se conecta a un controlador de 64 bits; o viceversa.|  
|IM015|Error de SQLDriverConnect del controlador en SQL_HANDLE_DBC_INFO_HANDLE|Si un controlador devuelve SQL_ERROR, el administrador de controladores devolverá SQL_ERROR a la aplicación y se producirá un error en la conexión.<br /><br /> Para obtener más información acerca de SQL_HANDLE_DBC_INFO_TOKEN, vea [desarrollar el reconocimiento de Connection-Pool en un controlador ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
|S1118|El controlador no admite la notificación asincrónica|Cuando el controlador no admite la notificación asincrónica, no se puede establecer SQL_ATTR_ASYNC_DBC_EVENT o SQL_ATTR_ASYNC_DBC_RETCODE_PTR.|  
  
## <a name="comments"></a>Comentarios  
 Una cadena de conexión tiene la siguiente sintaxis:  
  
 *Connection-String* :: = *Empty-String*[;] &#124; *atributo*[;] &#124; *atributo*; *cadena de conexión*  
  
 *Empty-String* :: =*Attribute* :: = Attribute *-Keyword* = *atributo-value* &#124; driver = [{]*Attribute-Value*[}]  
  
 *Attribute-keyword* :: = DSN &#124; UID &#124; pwd &#124; *Defined-Attribute-keyword*  
  
 *Attribute-Value* :: = *cadena de caracteres*  
  
 *driver-Defined-Attribute-keyword* :: = *Identifier*  
  
 donde *la cadena de caracteres* tiene cero o más caracteres; el *identificador* tiene uno o más caracteres; *Attribute-keyword* no distingue mayúsculas de minúsculas; *Attribute-Value* puede distinguir mayúsculas de minúsculas; y el valor de la palabra clave **DSN** no se compone únicamente de espacios en blanco.  
  
 Debido a la gramática de la cadena de conexión y al archivo de inicialización, las palabras clave y los valores de atributo que contienen los caracteres **[] {} (),;? \* se debe evitar =! @** no incluido entre llaves. El valor de la palabra clave **DSN** no puede constar solo de espacios en blanco y no debe contener espacios en blanco iniciales. Debido a la gramática de la información del sistema, las palabras clave y los nombres de los orígenes de datos no pueden contener el carácter de barra diagonal inversa ( \\ ).  
  
 Las aplicaciones no tienen que agregar llaves alrededor del valor del atributo después de la palabra clave **driver** , a menos que el atributo contenga un punto y coma (;), en cuyo caso las llaves son necesarias. Si el valor de atributo que el controlador recibe incluye llaves, el controlador no debe quitarlos, pero deben formar parte de la cadena de conexión devuelta.  
  
 Un valor de cadena de conexión o DSN entre llaves ( {} ) que contiene cualquiera de los caracteres **[] {} (),;? \* =! @** se pasa intacto al controlador. Sin embargo, cuando se usan estos caracteres en una palabra clave, el administrador de controladores devuelve un error al trabajar con DSN de archivo, pero pasa la cadena de conexión al controlador para las cadenas de conexión normales. Evite el uso de llaves incrustadas en un valor de palabra clave.  
  
 La cadena de conexión puede incluir cualquier número de palabras clave definidas por el controlador. Dado que la palabra clave **driver** no utiliza información de la información del sistema, el controlador debe definir palabras clave suficientes para que un controlador pueda conectarse a un origen de datos solo mediante la información de la cadena de conexión. (Para obtener más información, vea "instrucciones del controlador", más adelante en esta sección). El controlador define qué palabras clave son necesarias para conectarse al origen de datos.  
  
 En la tabla siguiente se describen los valores de atributo de las palabras clave **DSN**, **FILEDSN**, **driver**, **UID**, **pwd** y **SaveFile** .  
  
|Palabra clave|Descripción del valor de atributo|  
|-------------|---------------------------------|  
|**DSN**|Nombre de un origen de datos devuelto por **SQLDataSources** o el cuadro de diálogo orígenes de datos de **SQLDriverConnect**.|  
|**FILEDSN**|Nombre de un archivo. DSN a partir del cual se generará una cadena de conexión para el origen de datos. Estos orígenes de datos se denominan orígenes de datos de archivo.|  
|**DISPOSITIVO**|Descripción del controlador devuelto por la función **SQLDrivers** . Por ejemplo, RDB o SQL Server.|  
|**UID**|Un id. de usuario.|  
|**PWD**|La contraseña correspondiente al identificador de usuario o una cadena vacía si no hay ninguna contraseña para el ID. de usuario (PWD =;).|  
|**SAVEFILE**|Nombre de archivo de un archivo. DSN en el que se deben guardar los valores de atributo de las palabras clave utilizadas para establecer la conexión actual.|  
  
 Para obtener información sobre cómo una aplicación elige un origen de datos o un controlador, vea [elegir un origen de datos o un controlador](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md).  
  
 Si alguna palabra clave se repite en la cadena de conexión, el controlador utiliza el valor asociado a la primera aparición de la palabra clave. Si las palabras clave **DSN** y **driver** están incluidas en la misma cadena de conexión, el administrador de controladores y la palabra clave use la que aparece en primer lugar.  
  
 Las palabras clave **FILEDSN** y **DSN** se excluyen mutuamente: la palabra clave que aparece primero se usa y se omite la que aparece en segundo lugar. Por otro lado, las palabras clave **FILEDSN** y **driver** no son mutuamente excluyentes. Si aparece alguna palabra clave en una cadena de conexión con **FILEDSN**, se usa el valor de atributo de la palabra clave en la cadena de conexión, en lugar del valor de atributo de la misma palabra clave en el archivo. DSN.  
  
 Si se usa la palabra clave **FILEDSN** , se usan las palabras clave especificadas en un archivo. DSN para crear una cadena de conexión. (Para obtener más información, vea "orígenes de datos de archivo", más adelante en esta sección). La palabra clave **UID** es opcional. se puede crear un archivo. DSN solo con la palabra clave **driver** . La palabra clave **pwd** no se almacena en un archivo. DSN. El directorio predeterminado para guardar y cargar un archivo. DSN será una combinación de la ruta de acceso especificada por **CommonFileDir** en HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ Windows\CurrentVersion y "ODBC\DataSources". (Si CommonFileDir eran "C:\Archivos de Programa\archivos files", el directorio predeterminado sería "C:\Archivos de Programa\archivos Files\ODBC\Data sources").  
  
> [!NOTE]  
>  Un archivo. DSN se puede manipular directamente llamando a las funciones [SQLReadFileDSN](../../../odbc/reference/syntax/sqlreadfiledsn-function.md) y [SQLWriteFileDSN](../../../odbc/reference/syntax/sqlwritefiledsn-function.md) en el archivo DLL del instalador.  
  
 Si se utiliza la palabra clave **SaveFile** , los valores de atributo de las palabras clave utilizadas para establecer la conexión actual se guardarán como un archivo. DSN con el nombre del valor de atributo de la palabra clave **SaveFile** . La palabra clave **SaveFile** debe utilizarse junto con la palabra clave **driver** , la palabra clave **FILEDSN** o ambas, o la función devuelve SQL_SUCCESS_WITH_INFO con SQLSTATE 01S09 (palabra clave no válida). La palabra clave **SaveFile** debe aparecer antes que la palabra clave **driver** en la cadena de conexión, o bien los resultados serán indefinidos.  
  
## <a name="driver-manager-guidelines"></a>Directrices para el administrador de controladores  
 El administrador de controladores crea una cadena de conexión que se va a pasar al controlador en el argumento *inconnectionstring* de la función **SQLDriverConnect** del controlador. El administrador de controladores no modifica el argumento *inconnectionstring* que le pasa la aplicación.  
  
 La acción del administrador de controladores se basa en el valor del argumento *DriverCompletion* :  
  
-   SQL_DRIVER_PROMPT: Si la cadena de conexión no contiene la palabra clave **driver**, **DSN** o **FILEDSN** , el administrador de controladores muestra el cuadro de diálogo orígenes de datos. Crea una cadena de conexión a partir del nombre del origen de datos devuelto por el cuadro de diálogo y cualquier otra palabra clave pasada por la aplicación. Si el nombre del origen de datos devuelto por el cuadro de diálogo está vacío, el administrador de controladores especifica la palabra clave-valor par DSN = default. (Este cuadro de diálogo no mostrará ningún origen de datos con el nombre "default").  
  
-   SQL_DRIVER_COMPLETE o SQL_DRIVER_COMPLETE_REQUIRED: Si la cadena de conexión especificada por la aplicación incluye la palabra clave **DSN** , el administrador de controladores copia la cadena de conexión especificada por la aplicación. De lo contrario, realiza las mismas acciones que cuando se SQL_DRIVER_PROMPT *DriverCompletion* .  
  
-   SQL_DRIVER_NOPROMPT: el administrador de controladores copia la cadena de conexión especificada por la aplicación.  
  
 Si la cadena de conexión especificada por la aplicación contiene la palabra clave **driver** , el administrador de controladores copia la cadena de conexión especificada por la aplicación.  
  
 Mediante el uso de la cadena de conexión que ha construido, el administrador de controladores determina qué controlador usar, se conecta a ese controlador y pasa la cadena de conexión que ha creado al controlador; para obtener más información acerca de la interacción del administrador de controladores y el controlador, consulte la sección "Comentarios" en la [función SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md). Si la cadena de conexión no contiene la palabra clave **driver** , el administrador de controladores determina qué controlador usar de la manera siguiente:  
  
1.  Si la cadena de conexión contiene la palabra clave **DSN** , el administrador de controladores recupera el controlador asociado con el origen de datos de la información del sistema.  
  
2.  Si la cadena de conexión no contiene la palabra clave **DSN** o no se encuentra el origen de datos, el administrador de controladores recupera el controlador asociado al origen de datos predeterminado de la información del sistema. (Para obtener más información, vea [subclave predeterminada](../../../odbc/reference/install/default-subkey.md)). El administrador de controladores cambia el valor de la palabra clave **DSN** en la cadena de conexión a "default".  
  
3.  Si la palabra clave **DSN** de la cadena de conexión se establece en "default", el administrador de controladores recupera el controlador asociado al origen de datos predeterminado de la información del sistema.  
  
4.  Si no se encuentra el origen de datos y no se encuentra el origen de datos predeterminado, el administrador de controladores devuelve SQL_ERROR con SQLSTATE IM002 (no se encontró el origen de datos y no se ha especificado ningún controlador predeterminado).  
  
## <a name="file-data-sources"></a>Orígenes de datos de archivo  
 Si la cadena de conexión especificada por la aplicación en la llamada a **SQLDriverConnect** contiene la palabra clave **FILEDSN** y esta palabra clave no se sustituye por la palabra clave **DSN** o **driver** , el administrador de controladores crea una cadena de conexión con la información del archivo. DSN y el argumento *inconnectionstring* . El administrador de controladores continúa de la siguiente manera:  
  
1.  Comprueba si el nombre de archivo del archivo. DSN es válido. Si no es así, devuelve SQL_ERROR con SQLSTATE IM014 (nombre del DSN de archivo no válido). Si el nombre de archivo es una cadena vacía ("") y no se especifica SQL_DRIVER_NOPROMPT, se muestra el cuadro **de diálogo Abrir archivo** . Si el nombre de archivo contiene una ruta de acceso válida pero no un nombre de archivo o un nombre de archivo no válido y no se especifica SQL_DRIVER_NOPROMPT, el cuadro de diálogo **Abrir archivo** se muestra con el directorio actual establecido en el especificado en el nombre de archivo. Si el nombre de archivo es una cadena vacía ("") o el nombre de archivo contiene una ruta de acceso válida pero no un nombre de archivo o un nombre de archivo no válido y se especifica SQL_DRIVER_NOPROMPT, se devuelve SQL_ERROR con SQLSTATE IM014 (nombre del DSN de archivo no válido).  
  
2.  Lee todas las palabras clave de la sección [ODBC] del archivo. DSN. Si la palabra clave **driver** no está presente, devuelve SQL_ERROR con SQLSTATE IM012 (error de sintaxis de palabra clave driver), excepto cuando el archivo. DSN no es compartible y, por tanto, solo contiene la palabra clave **DSN** .  
  
     Si el origen de datos de archivo no es compartible, el administrador de controladores lee el valor de la palabra clave **DSN** y se conecta según sea necesario para el origen de datos de usuario o del sistema al que apunta el origen de datos de archivo que no se pueden compartir. No se realizan los pasos 3 a 5.  
  
3.  Crea una cadena de conexión para el controlador. La cadena de conexión del controlador es la Unión de las palabras clave especificadas en el archivo. DSN y las especificadas en la cadena de conexión de la aplicación original. Las reglas para la construcción de la cadena de conexión del controlador donde se superponen las palabras clave son las siguientes:  
  
    -   Si la palabra clave **driver** existe en la cadena de conexión de la aplicación y los controladores especificados por las palabras clave del **controlador** no son los mismos en el archivo. DSN y en la cadena de conexión de la aplicación, se omite la información del controlador en el archivo. DSN y se usa la información del controlador en la cadena de conexión de la aplicación. Si los controladores especificados por la palabra clave **driver** son los mismos en el archivo. DSN y en la cadena de conexión de la aplicación, donde se superponen todas las palabras clave, las especificadas en la cadena de conexión de la aplicación tienen prioridad sobre las especificadas en el archivo. DSN.  
  
    -   En la nueva cadena de conexión, se elimina la palabra clave **FILEDSN** .  
  
4.  Carga el controlador examinando la entrada del registro HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBCINST.INI\\<nombre del controlador \> \Driver, donde \<Driver Name> se especifica mediante la palabra clave **driver** .  
  
5.  Pasa el controlador a la nueva cadena de conexión.  
  
 Para obtener ejemplos de archivos. DSN, consulte [conectarse con orígenes de datos de archivo](../../../odbc/reference/develop-app/connecting-using-file-data-sources.md).  
  
## <a name="savefile-keyword"></a>SAVEFILE (palabra clave)  
 Si la cadena de conexión especificada por la aplicación contiene la palabra clave **SaveFile** , el administrador de controladores guarda la cadena de conexión en un archivo. DSN. El administrador de controladores continúa de la siguiente manera:  
  
1.  Comprueba si el nombre de archivo del archivo. DSN incluido como valor de atributo de la palabra clave **SaveFile** es válido. Si no es así, devuelve SQL_ERROR con SQLSTATE IM014 (nombre del DSN de archivo no válido). La validez del nombre de archivo está determinada por las reglas de nomenclatura del sistema estándar. Si el nombre de archivo es una cadena vacía ("") y el argumento *DriverCompletion* no se SQL_DRIVER_NOPROMPT, el nombre de archivo es válido. Si el nombre de archivo ya existe, si *DriverCompletion* es SQL_DRIVER_NOPROMPT, se sobrescribe el archivo. Si *DriverCompletion* es SQL_DRIVER_PROMPT, SQL_DRIVER_COMPLETE o SQL_DRIVER_COMPLETE_REQUIRED, un cuadro de diálogo solicita al usuario que especifique si se debe sobrescribir el archivo. Si no se especifica ninguno, se muestra el cuadro **de diálogo Guardar archivo** .  
  
2.  Si el controlador devuelve SQL_SUCCESS y el nombre de archivo no es una cadena vacía, el administrador de controladores escribe la información de conexión devuelta en el argumento *Outconnectionstring* en el archivo especificado con el formato especificado en la sección "cadenas de conexión" anteriormente en esta sección.  
  
3.  Si el controlador devuelve SQL_SUCCESS y el nombre de archivo era una cadena vacía (""), el administrador de controladores llama al cuadro de diálogo común **de guardar archivo** con el *hWnd* especificado y escribe la información de conexión devuelta en *outconnectionstring* en el archivo especificado en el cuadro de diálogo File-Save común con el formato especificado en la sección "cadenas de conexión" anteriormente en esta sección.  
  
4.  Si el controlador devuelve SQL_SUCCESS, devuelve el argumento *outconnectionstring* que contiene la cadena de conexión a la aplicación.  
  
5.  Si el controlador devuelve SQL_SUCCESS_WITH_INFO o SQL_ERROR, el administrador de controladores devuelve el SQLSTATE a la aplicación.  
  
## <a name="driver-guidelines"></a>Instrucciones del controlador  
 El controlador comprueba si la cadena de conexión que el administrador de controladores le ha pasado contiene la palabra clave **DSN** o **driver** . Si la cadena de conexión contiene la palabra clave **driver** , el controlador no puede recuperar información sobre el origen de datos de la información del sistema. Si la cadena de conexión contiene la palabra clave **DSN** o no contiene la palabra clave **DSN** o **driver** , el controlador puede recuperar información sobre el origen de datos de la información del sistema como se indica a continuación:  
  
1.  Si la cadena de conexión contiene la palabra clave **DSN** , el controlador recupera la información del origen de datos especificado.  
  
2.  Si la cadena de conexión no contiene la palabra clave **DSN** , no se encuentra el origen de datos especificado o la palabra clave **DSN** está establecida en "default", el controlador recupera la información del origen de datos predeterminado.  
  
 El controlador utiliza cualquier información recuperada de la información del sistema para aumentar la información que se le pasa en la cadena de conexión. Si la información de la información del sistema duplica la información de la cadena de conexión, el controlador utiliza la información de la cadena de conexión.  
  
 Según el valor de *DriverCompletion*, el controlador solicita al usuario la información de conexión, como el identificador de usuario y la contraseña, y se conecta al origen de datos:  
  
-   SQL_DRIVER_PROMPT: el controlador muestra un cuadro de diálogo con los valores de la cadena de conexión y la información del sistema (si los hay) como valores iniciales. Cuando el usuario sale del cuadro de diálogo, el controlador se conecta al origen de datos. También crea una cadena de conexión a partir del valor de la palabra clave **DSN** o **driver** en \* *inconnectionstring* y la información devuelta por el cuadro de diálogo. Coloca esta cadena de conexión en el búfer **outconnectionstring* .  
  
-   SQL_DRIVER_COMPLETE o SQL_DRIVER_COMPLETE_REQUIRED: Si la cadena de conexión contiene suficiente información y esa información es correcta, el controlador se conecta al origen de datos y copia \* *inconnectionstring* en \* *outconnectionstring*. Si falta alguna información o es incorrecta, el controlador realiza las mismas acciones que cuando se SQL_DRIVER_PROMPT *DriverCompletion* , excepto que si *DriverCompletion* es SQL_DRIVER_COMPLETE_REQUIRED, el controlador deshabilita los controles para cualquier información no necesaria para conectarse al origen de datos.  
  
-   SQL_DRIVER_NOPROMPT: Si la cadena de conexión contiene suficiente información, el controlador se conecta al origen de datos y copia \* *inconnectionstring* en \* *outconnectionstring*. De lo contrario, el controlador devuelve SQL_ERROR para **SQLDriverConnect**.  
  
 Si la conexión se realiza correctamente con el origen de datos, el controlador también establece \* *StringLength2Ptr* en la longitud de la cadena de conexión de salida que está disponible para devolver en **outconnectionstring*.  
  
 Si el usuario cancela un cuadro de diálogo que presenta el administrador de controladores o el controlador, **SQLDriverConnect** devuelve SQL_NO_DATA.  
  
 Para obtener información sobre el modo en que el administrador de controladores y el controlador interactúan durante el proceso de conexión, consulte [función SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
 Si un controlador admite **SQLDriverConnect**, la sección de la palabra clave driver de la información del sistema para el controlador debe contener la palabra clave **ConnectFunctions** con el segundo carácter establecido en "Y".  
  
## <a name="connecting-when-connection-pooling-is-enabled"></a>Conexión cuando la agrupación de conexiones está habilitada  
 La agrupación de conexiones permite a una aplicación volver a usar una conexión que ya se ha creado. Cuando se llama a **SQLDriverConnect** , el administrador de controladores intenta realizar la conexión mediante una conexión que forma parte de un grupo de conexiones en un entorno que se ha designado para la agrupación de conexiones. Para obtener más información sobre la agrupación de conexiones, vea [función SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
 Una aplicación puede establecer SQL_ATTR_RESET_CONNECTION antes de llamar a SQLDisconnect en una conexión en la que está habilitada la agrupación. Para obtener más información, consulte [SQLSetConnectAttr (función](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)).  
  
 Las siguientes restricciones se aplican cuando una aplicación llama a **SQLDriverConnect** para conectarse a una conexión agrupada:  
  
-   No se realiza ningún procesamiento de agrupación de conexiones cuando se especifica la palabra clave **SaveFile** en la cadena de conexión.  
  
-   Si la agrupación de conexiones está habilitada, solo se puede llamar a **SQLDriverConnect** con un argumento *DriverCompletion* de SQL_DRIVER_NOPROMPT; Si se llama a **SQLDriverConnect** con cualquier otro *DriverCompletion*, se devolverá SQLSTATE HY110 (finalización del controlador no válida).  
  
## <a name="connection-attributes"></a>Atributos de conexión  
 El atributo de conexión SQL_ATTR_LOGIN_TIMEOUT, establecido mediante **SQLSetConnectAttr**, define el número de segundos que se esperará a que el controlador complete una solicitud de inicio de sesión con una conexión correcta antes de volver a la aplicación. Si al usuario se le pide que complete la cadena de conexión, comienza un período de espera para cada solicitud de inicio de sesión cuando el controlador inicia el proceso de conexión.  
  
 De forma predeterminada, el controlador abre la conexión en el modo de SQL_MODE_READ_WRITE acceso. Para establecer el modo de acceso en SQL_MODE_READ_ONLY, la aplicación debe llamar a **SQLSetConnectAttr** con el atributo SQL_ATTR_ACCESS_MODE antes de llamar a **SQLDriverConnect**.  
  
 Si se especifica una biblioteca de traducción predeterminada en la información del sistema para el origen de datos, el controlador la carga. Se puede cargar una biblioteca de traducción diferente llamando a **SQLSetConnectAttr** con el atributo SQL_ATTR_TRANSLATE_LIB. Se puede especificar una opción de traducción mediante una llamada a **SQLSetConnectAttr** con la opción SQL_ATTR_TRANSLATE_OPTION.  
  
 Para obtener más información, consulte [conectar con SQLDriverConnect](../../../odbc/reference/develop-app/connecting-with-sqldriverconnect.md).  
  
```cpp  
// SQLDriverConnect_ref.cpp  
// compile with: odbc32.lib user32.lib  
#include <windows.h>  
#include <sqlext.h>  
  
int main() {  
   SQLHENV henv;  
   SQLHDBC hdbc;  
   SQLHSTMT hstmt;  
   SQLRETURN retcode;  
  
   SQLCHAR OutConnStr[255];  
   SQLSMALLINT OutConnStrLen;  
  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
  
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
  
            retcode = SQLDriverConnect( // SQL_NULL_HDBC  
               hdbc,   
               desktopHandle,   
               (SQLCHAR*)"driver=SQL Server",   
               _countof("driver=SQL Server"),  
               OutConnStr,  
               255,   
               &OutConnStrLen,  
               SQL_DRIVER_PROMPT );  
  
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
  
 Vea también el [programa ODBC de ejemplo](../../../odbc/reference/sample-odbc-program.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Asignación de un identificador|[Función SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Detección y enumeración de los valores necesarios para conectarse a un origen de datos|[Función SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|  
|Conectarse a un origen de datos|[Función SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|Desconectar de un origen de datos|[Función SQLDisconnect](../../../odbc/reference/syntax/sqldisconnect-function.md)|  
|Devolver descripciones y atributos de controladores|[Función SQLDrivers](../../../odbc/reference/syntax/sqldrivers-function.md)|  
|Liberar un identificador|[Función SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|Establecer un atributo de conexión|[Función SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
