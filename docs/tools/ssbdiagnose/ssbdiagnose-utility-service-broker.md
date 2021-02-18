---
title: utilidad ssbdiagnose (Service Broker)
description: La utilidad ssbdiagnose informa de la existencia de problemas en las conversaciones de Service Broker o en la configuración de los servicios de Service Broker.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- Service Broker, runtime reports
- Service Broker, command prompt utilities
- troubleshooting [Service Broker], conversations
- troubleshooting [Service Broker], configurations
- command prompt utilities [Service Broker]
- Service Broker, troubleshooting
- Service Broker, configuration reports
- Service Broker, tools
- troubleshooting [Service Broker], runtime
- conversations [Service Broker], troubleshooting
- troubleshooting [Service Broker], ssbdiagnose utility
- tools [Service Broker], ssbdiagnose
- Service Broker, ssbdiagnose utility
- ssbdiagnose
ms.assetid: 0c1636e8-a3db-438e-be4c-1ea40d1f4877
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: a5001821d5a396f65ee739341424a2eaae3dad78
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335910"
---
# <a name="ssbdiagnose-utility-service-broker"></a>utilidad ssbdiagnose (Service Broker)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

La utilidad **ssbdiagnose** informa de la existencia de problemas en las conversaciones de [!INCLUDE[ssSB](../../includes/sssb-md.md)] o en la configuración de los servicios de [!INCLUDE[ssSB](../../includes/sssb-md.md)] . Las comprobaciones de la configuración se pueden realizar en dos servicios o en un único servicio. La existencia de problemas se indica en la ventana del símbolo del sistema en forma de texto legible, o como XML con formato que se puede redirigir a un archivo o a otro programa.

## <a name="syntax"></a>Sintaxis  
  
```  
  
ssbdiagnose   
[ [ -XML ]  
    [ -LEVEL { ERROR | WARNING | INFO } ]  
  [-IGNORE error_id ] [ ...n]  
    [ <baseconnectionoptions> ]  
  { <configurationreport> | <runtimereport> }  
]  
| -?  
  
<configurationreport> ::=  
    CONFIGURATION  
  { [ FROM SERVICE service_name  
      [ <fromconnectionoptions> ]  
      [ MIRROR <mirrorconnectionoptions> ]  
    ]  
    [ TO SERVICE service_name[, broker_id ]  
      [ <toconnectionoptions> ]  
      [ MIRROR <mirrorconnectionoptions> ]  
    ]  
  }  
    ON CONTRACT contract_name  
  [ ENCRYPTION { ON | OFF | ANONYMOUS } ]  
  
<runtime_report> ::=  
    RUNTIME  
    [-SHOWEVENTS ]  
        [ -NEW  
         [ -ID { conversation_handle  
                | conversation_group_id  
                 | conversation_id  
                  }  
        ] [ ...n]  
        ]  
    [ -TIMEOUT timeout_interval ]  
    [ <runtimeconnectionoptions> ]  
  
<baseconnectionoptions> ::=  
  <connectionoptions>  
  
<fromconnectionoptions> ::=  
  <connectionoptions>  
  
<toconnectionoptions> ::=  
  <connectionoptions>  
  
<mirrorconnectionoptions> ::=  
  <connectionoptions>  
  
<runtimeconnectionoptions> ::=  
  [ CONNECT TO <connectionoptions> ] [ ...n]  
  
<connectionoptions> ::=  
    [ -E | { -U login_id [ -P password ] } ]  
  [ -S server_name[\instance_name] ]  
  [ -d database_name ]  
  [ -l login_timeout ]  
  
```  
  
## <a name="command-line-options"></a>Opciones de la línea de comandos  
 **-XML**  
 Especifica que la salida de **ssbdiagnose** se debe generar como XML con formato. Dicha salida se puede redirigir a un archivo o a otra aplicación. Si no se especifica **-XML** , la salida de **ssbdiagnose** se muestra como texto legible.  
  
 **-LEVEL** { **ERROR** | **WARNING** | **INFO**}  
 Especifica el nivel de los mensajes que se notificarán.  
  
 **ERROR**: notifica únicamente los mensajes de error.  
  
 **WARNING**: notifica los mensajes de advertencia y los de error.  
  
 **INFO**: notifica los mensajes de error, los de advertencia y los informativos.  
  
 El valor de configuración predeterminado es **WARNING**.  
  
 **-IGNORE** _error_id_  
 Especifica que los errores o los mensajes con el *error_id* especificado no se deben incluir en los informes. Puede especificar **-IGNORE** varias veces para suprimir varios identificadores de mensaje.  
  
 **\<baseconnectionoptions>**  
 Especifica la información de conexión base que se usa en **ssbdiagnose** cuando no se incluyen opciones de conexión en una cláusula determinada. La información de conexión proporcionada en una cláusula específica invalida la información de **baseconnectionoption** . Este proceso se realiza para cada parámetro de forma individual. Por ejemplo, si tanto **-S** como **-d** se especifican en **baseconnetionoptions** y solo se especifica **-d** en **toconnetionoptions**, **ssbdiagnose** usará -S de **baseconnetionoptions** y -d de **toconnetionoptions**.  
  
 **CONFIGURATION**  
 Solicita un informe de errores de configuración entre un par de servicios de [!INCLUDE[ssSB](../../includes/sssb-md.md)] o de un único servicio.  
  
 **FROM SERVICE** _service_name_  
 Especifica el servicio que inicia las conversaciones.  
  
 **\<fromconnectionoptions>**  
 Especifica la información requerida para conectar con la base de datos que contiene el servicio del iniciador. Si no se especifica **fromconnectionoptions** , **ssbdiagnose** usa la información de conexión de **baseconnectionoptions** para conectarse a la base de datos del iniciador. Si se especifica **fromconnectionoptions** , se debe incluir la base de datos que contiene el servicio del iniciador. Si **fromconnectionoptions** no se especifica, **baseconnectionoptions** debe especificar la base de datos del iniciador.  
  
 **TO SERVICE** _service_name_[, *broker_id* ]  
 Especifica el servicio que es el destino de las conversaciones.  
  
 *service_name*: especifica el nombre del servicio de destino.  
  
 *broker_id*: especifica el identificador de [!INCLUDE[ssSB](../../includes/sssb-md.md)] que identifica la base de datos de destino. *broker_id* es un GUID. Puede ejecutar la siguiente consulta en la base de datos de destino para encontrarlo:  
  
```  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID();  
```  
  
 **\<toconnectionoptions>**  
 Especifica la información requerida para conectar con la base de datos que contiene el servicio de destino. Si **toconnectionoptions** no se especifica, **ssbdiagnose** usa la información de conexión de **baseconnectionoptions** para conectarse a la base de datos de destino.  
  
 **MIRROR**  
 Especifica que el servicio de [!INCLUDE[ssSB](../../includes/sssb-md.md)] asociado se hospeda en una base de datos reflejada. **ssbdiagnose** comprueba que la ruta al servicio es una ruta reflejada, donde se ha especificado MIRROR_ADDRESS en CREATE ROUTE.  
  
 **\<mirrorconnectionoptions>**  
 Especifica la información requerida para conectar con la base de datos reflejada. Si no se especifica **mirrorconnectionoptions** , **ssbdiagnose** usa la información de conexión de **baseconnectionoptions** para conectarse a la base de datos reflejada.  
  
 **ON CONTRACT** _contract_name_  
 Indica que **ssbdiagnose** únicamente debe comprobar las configuraciones que usan el contrato especificado. Si no se especifica ON CONTRACT, **ssbdiagnose** incluye información sobre el contrato denominado DEFAULT.  
  
 **ENCRYPTION** { **ON** | **OFF** | **ANONYMOUS** }  
 Solicita que se compruebe si el diálogo está configurado correctamente para el nivel de cifrado especificado:  
  
 **ON**: valor predeterminado. Se ha configurado la seguridad de diálogo completa. Se han implementado los certificados en ambos extremos del diálogo, está presente un enlace de servicio remoto y la instrucción GRANT SEND para el servicio de destino especificó el usuario del iniciador.  
  
 **OFF**: no se ha configurado la seguridad de diálogo. No se ha implementado ningún certificado, no se ha creado ningún enlace de servicio remoto y la instrucción GRANT SEND para el servicio del iniciador ha especificado el rol **public** .  
  
 **ANONYMOUS**: se ha configurado la seguridad de diálogo anónima. Se ha implementado un certificado, el enlace de servicio remoto ha especificado la cláusula anónima y la instrucción GRANT SEND del servicio de destino ha especificado el rol **public** .  
  
 **RUNTIME**  
 Solicita un informe de los problemas que provocan errores en tiempo de ejecución en una conversación de [!INCLUDE[ssSB](../../includes/sssb-md.md)] . Si no se especifican **-NEW** ni **-ID** , **ssbdiagnose** supervisa todas las conversaciones en todas las bases de datos especificadas en las opciones de conexión. Si se especifican **-NEW** o **-ID** , **ssbdiagnose** genera una lista de los identificadores especificados en los parámetros.  
  
 Mientras **ssbdiagnose** se está ejecutando, registra todos los eventos de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] que indican errores de tiempo de ejecución. Registra los eventos que se producen en los identificadores especificados, además de los eventos del nivel de sistema. Si se encuentran errores de tiempo de ejecución, **ssbdiagnose** ejecuta un informe de la configuración asociada.  
  
 De forma predeterminada, los errores de tiempo de ejecución no se incluyen en el informe de salida, solo se incluyen los resultados del análisis de la configuración. Use **-SHOWEVENTS** para incluir los errores en tiempo de ejecución en el informe.  
  
 **-SHOWEVENTS**  
 Indica que **ssbdiagnose** debe notificar los eventos de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] durante un informe RUNTIME. Solo se informa de los eventos considerados como condiciones de error. De forma predeterminada, **ssbdiagnose** solamente supervisa los eventos de error; no informa de ellos en la salida.  
  
 **-NEW**  
 Solicita la supervisión en tiempo de ejecución de la primera conversación que comienza después de que **ssbdiagnose** inicie su ejecución.  
  
 **-ID**  
 Solicita la supervisión en tiempo de ejecución de los elementos de la conversación especificados. Puede especificar **-ID** varias veces.  
  
 Si especifica un identificador de conversación, solo se informa de los eventos asociados al extremo de conversación correspondiente. Si especifica un identificador de conversación, se informa de todos los eventos de la conversación, así como de los de sus extremos de destino e iniciador. Si se especifica un identificador de grupo de conversación, se informa de todos los eventos para todas las conversaciones y todos los extremos del grupo de conversación.  
  
 *conversation_handle*  
 Un identificador único que identifica un extremo de conversación en una aplicación. Los identificadores de conversación son únicos para cada extremo de la conversación; los extremos de destino e iniciador tienen identificadores de conversación independientes.  
  
 El parámetro *\@dialog_handle* de la instrucción **BEGIN DIALOG** y la columna **conversation_handle** del conjunto de resultados de una instrucción **RECEIVE** son los encargados de devolver los identificadores de conversación a las aplicaciones.  
  
 Los identificadores de conversación se presentan en la columna **conversation_handle** de las vistas de catálogo **sys.transmission_queue** y **sys.conversation_endpoints** .  
  
 *conversation_group_id*  
 El identificador único que identifica un grupo de conversación.  
  
 El parámetro *\@conversation_group_id* de la instrucción **GET CONVERSATION GROUP** y la columna **conversation_group_id** del conjunto de resultados de una instrucción **RECEIVE** son los encargados de devolver los identificadores de grupo de conversación a las aplicaciones.  
  
 Los identificadores de grupo de conversación se presentan en la columna **conversation_group_id** de las vistas de catálogo **sys.conversation_groups** y **sys.conversation_endpoints** .  
  
 *conversation_id*  
 El identificador único que identifica una conversación. Los identificadores de conversación son los mismos para los extremos de destino e iniciador de una conversación.  
  
 Los identificadores de conversación se presentan en la columna **conversation_id** de la vista de catálogo **sys.conversation_endpoints** .  
  
 **-TIMEOUT** _timeout_interval_  
 Especifica el número de segundos durante los que se debe ejecutar un informe **RUNTIME** . Si no se especifica **-TIMEOUT** , el informe RUNTIME se ejecuta de forma indefinida. **-TIMEOUT** solo se usa en informes **RUNTIME** , no en informes **CONFIGURATION** . Use CTRL+C para salir de **ssbdiagnose** si no se ha especificado **-TIMEOUT** o si quiere finalizar un informe RUNTIME antes de que expire el intervalo de tiempo de espera. **-** *timeout_interval* debe ser un número comprendido entre 1 y 2 147 483 647.  
  
 **\<runtimeconnectionoptions>**  
 Especifica la información de conexión para las bases de datos que contienen los servicios asociados a los elementos de conversación que se están supervisando. Si todos los servicios se encuentran en la misma base de datos, solo es necesario especificar una cláusula **CONNECT TO** . Por el contrario, si los servicios se encuentran en bases de datos independientes, será necesario especificar una cláusula **CONNECT TO** para cada base de datos. Si **runtimeconnectionoptions** no se especifica, **ssbdiagnose** usa la información de conexión de **baseconnectionoptions**.  
  
 **-E**  
 Abre una conexión con autenticación de Windows para una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y usa la cuenta de Windows actual como identificador de inicio de sesión. El usuario debe ser miembro del rol fijo de servidor **sysadmin** .  
  
 La opción -E omite la configuración de usuario y de contraseña de las variables de entorno SQLCMDUSER y SQLCMDPASSWORD.  
  
 Si no se especifican **-E** ni **-U** , **ssbdiagnose** usa el valor de la variable de entorno SQLCMDUSER. Si tampoco se establece SQLCMDUSER, **ssbdiagnose** usa la autenticación de Windows.  
  
 Si se usa la opción **-E** junto con la opción **-U** o **-P** , se genera un mensaje de error.  
  
 **-U** _login_id_  
 Abre una conexión con autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y usa el identificador de inicio de sesión especificado. El usuario debe ser miembro del rol fijo de servidor **sysadmin** .  
  
 Si no se especifican **-E** ni **-U** , **ssbdiagnose** usa el valor de la variable de entorno SQLCMDUSER. Si tampoco se establece SQLCMDUSER, **ssbdiagnose** intenta la conexión mediante el modo de autenticación de Windows basado en la cuenta de Windows del usuario que está ejecutando **ssbdiagnose**.  
  
 Si se usa la opción **-U** junto con la opción **-E** , se genera un mensaje de error. Si la opción **-U** va seguida de más de un argumento, se genera un mensaje de error y el programa se cierra.  
  
 **-P** _password_  
 Especifica la contraseña para el identificador de inicio de sesión **-U** . En las contraseñas se distingue entre mayúsculas y minúsculas. Si se usa la opción **-U** pero no la opción **-P** , **ssbdiagnose** usa el valor de la variable de entorno SQLCMDPASSWORD. Si tampoco se establece SQLCMDPASSWORD, **ssbdiagnose** solicita al usuario una contraseña.  
  
> [!IMPORTANT]  
>  Cuando escriba un comando SET SQLCMDPASSWORD, la contraseña será visible para cualquiera que pueda ver el monitor.  
  
 Si se especifica la opción **-P** sin una contraseña, **ssbdiagnose** usa la contraseña predeterminada (NULL).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)] Para obtener más información, consulte [Contraseñas seguras](../../relational-databases/security/strong-passwords.md).  
  
 El mensaje de contraseña se muestra en la consola de la siguiente manera: `Password:`  
  
 La entrada del usuario queda oculta. Esto significa que no se muestra nada y que el cursor permanece en su posición.  
  
 Si se usa la opción **-P** junto con la opción **-E** , se genera un mensaje de error.  
  
 Si la opción **-P** va seguida de más de un argumento, se genera un mensaje de error.  
  
 **-S** _server_name_[\\*instance_name*]  
 Especifica la instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] que contiene los servicios de [!INCLUDE[ssSB](../../includes/sssb-md.md)] que se van a analizar.  
  
 Especifique *server_name* para conectar con la instancia predeterminada de [!INCLUDE[ssDE](../../includes/ssde-md.md)] en ese servidor. Especifique _server\_name_ **\\** _instance\_name_ para conectar una instancia con nombre de [!INCLUDE[ssDE](../../includes/ssde-md.md)] en ese servidor. Si no se especifica **-S** , **ssbdiagnose** usa el valor de la variable de entorno SQLCMDSERVER. Si tampoco se establece SQLCMDSERVER, **ssbdiagnose** se conecta a la instancia predeterminada de [!INCLUDE[ssDE](../../includes/ssde-md.md)] en el equipo local.  
  
 **-d** _database_name_  
 Especifica la base de datos que contiene los servicios de [!INCLUDE[ssSB](../../includes/sssb-md.md)] que se van a analizar. Si la base de datos no existe, se genera un mensaje de error. Si no se especifica **-d** , el valor predeterminado es la base de datos especificada en la propiedad de base de datos predeterminada del inicio de sesión.  
  
 **-l** _login_timeout_  
 Especifica el número de segundos que deben transcurrir antes de que se agote el tiempo de espera de un intento de conexión con un servidor. Si no se especifica **-l** , **ssbdiagnose** usa el valor establecido en la variable de entorno SQLCMDLOGINTIMEOUT. Si tampoco se establece SQLCMDLOGINTIMEOUT, el valor predeterminado del tiempo de espera es de treinta segundos. El período de tiempo de espera de inicio de sesión debe ser un número comprendido entre 0 y 65534. Si el valor proporcionado no es numérico o no está dentro de este intervalo, **ssbdiagnose** genera un mensaje de error. El valor 0 especifica que el tiempo de espera es infinito.  
  
 **-?**  
 Muestra la ayuda de la línea de comandos.  
  
## <a name="remarks"></a>Observaciones  
 Use **ssbdiagnose** para lo siguiente:  
  
-   Confirmar que no hay errores de configuración en una aplicación de [!INCLUDE[ssSB](../../includes/sssb-md.md)] que se acaba de configurar.  
  
-   Confirmar que no hay errores de configuración después de cambiar la configuración de una aplicación de [!INCLUDE[ssSB](../../includes/sssb-md.md)] existente.  
  
-   Confirmar que no hay errores de configuración después de que una base de datos de [!INCLUDE[ssSB](../../includes/sssb-md.md)] se haya separado y vuelto a adjuntar a una nueva instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   Comprobar si hay errores de configuración cuando los mensajes no se transmiten correctamente entre los servicios.  
  
-   Obtener un informe de los errores que se producen en un conjunto de elementos de conversación de [!INCLUDE[ssSB](../../includes/sssb-md.md)] .  
  
## <a name="configuration-reporting"></a>Creación de informes de configuración  
 Para analizar correctamente la configuración que se usa en una conversación, ejecute un informe de configuración de **ssbdiagnose** que use las mismas opciones que la conversación. Si especifica para **ssbdiagnose** un nivel de opciones inferior al que usa la conversación, es posible que **ssbdiagnose** no informe de las condiciones requeridas por la conversación. Si especifica un nivel de opciones superior para **ssbdiagnose**, es posible que se informe de elementos no requeridos por la conversación. Por ejemplo, una conversación entre dos servicios de la misma base de datos se puede ejecutar con ENCPRYPTION OFF. Si ejecuta **ssbdiagnose** para validar la configuración entre ambos servicios, pero usa la opción ENCRYPTION ON predeterminada, **ssbdiagnose** informa de que falta una clave maestra en la base de datos. No se requiere una clave maestra en la conversación.  
  
 Cada vez que se ejecuta, el informe de configuración de **ssbdiagnose** solamente analiza un servicio de [!INCLUDE[ssSB](../../includes/sssb-md.md)] o un único par de servicios. Si desea crear informes sobre varios pares de servicios de [!INCLUDE[ssSB](../../includes/sssb-md.md)] , cree un archivo de comandos .cmd que llame varias veces a **ssbdiagnose** .  
  
## <a name="runtime-reporting"></a>Creación de informes en tiempo de ejecución  
 Cuando se especifica -RUNTIME, **ssbdiagnose** busca en todas las bases de datos indicadas en **runtimeconnectionoptions** y **baseconnectionoptions** para generar una lista de identificadores de [!INCLUDE[ssSB](../../includes/sssb-md.md)] . La lista completa de identificadores generada depende de lo que se especifique para -NEW e -ID:  
  
-   Si no se especifican **-NEW** ni **-ID** , la lista incluye todas las conversaciones para todas las bases de datos especificadas en las opciones de conexión.  
  
-   Si se especifica **-NEW** , **ssbdiagnose** incluye los elementos para la primera conversación que se inicia después de ejecutar **ssbdiagnose** . Esto incluye el identificador de conversación y los identificadores de conversación tanto para el extremo iniciador de la conversación como para el de destino.  
  
-   Si se especifica **-ID** con un identificador de conversación (de tipo handle), solo se incluirá este identificador en la lista.  
  
-   Si se especifica **-ID** con un identificador de conversación (de tipo ID), se agregarán a la lista este identificador y los identificadores de tipo handle para ambos puntos de conexión de la conversación.  
  
-   Si se especifica **-ID** con un identificador de grupo de conversación, se agregarán a la lista todos los identificadores de conversación de tipo ID y de tipo handle de dicho grupo.  
  
 La lista no incluye los elementos de las bases de datos que no están cubiertas por las opciones de conexión. Por ejemplo, supongamos que se usa **-ID** para especificar un identificador de conversación de tipo ID, pero que solo se proporciona una cláusula **runtimeconnectionoptions** para la base de datos del iniciador, no para la base de datos de destino. **ssbdiagnose** no incluirá el identificador de tipo handle de la conversación de destino en su lista de identificadores; solo incluirá el identificador de conversación de tipo ID y el identificador de tipo handle de conversación del iniciador.  
  
 **ssbdiagnose** supervisa los eventos de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] de las bases de datos cubiertas por **runtimeconnectionoptions** y **baseconnectionoptions**. La utilidad busca eventos de [!INCLUDE[ssSB](../../includes/sssb-md.md)] que indican un error encontrado por parte de uno o varios de los identificadores de [!INCLUDE[ssSB](../../includes/sssb-md.md)] en la lista de tiempo de ejecución. **ssbdiagnose** también busca eventos de error de [!INCLUDE[ssSB](../../includes/sssb-md.md)] en el nivel de sistema que no están asociados específicamente a ningún grupo de conversación.  
  
 Si **ssbdiagnose** encuentra errores de conversación, la utilidad intentará notificar la causa raíz de los eventos mediante la ejecución de un informe de configuración. **ssbdiagnose** usa los metadatos de las bases de datos para intentar determinar las instancias, los identificadores de [!INCLUDE[ssSB](../../includes/sssb-md.md)] , las bases de datos, los servicios y los contratos usados por la conversación. A continuación, ejecuta un informe de configuración mediante toda la información disponible.  
  
 De forma predeterminada, **ssbdiagnose** no informa de los eventos de error. Solo informa de los problemas subyacentes encontrados durante la comprobación de la configuración. Esto minimiza la cantidad de información proporcionada y ayuda a concentrarse en los problemas de configuración subyacentes. Puede especificar **-SHOWEVENTS** para ver los eventos de error que encuentra **ssbdiagnose**.  
  
## <a name="issues-reported-by-ssbdiagnose"></a>Problemas encontrados por ssbdiagnose  
 **ssbdiagnose** informa de tres clases de problemas. En el archivo de salida XML, cada clase de problema aparece como un tipo independiente de elemento Issue. Los tres tipos de problemas notificados por **ssbdiagnose** son los siguientes:  
  
 **Diagnóstico**  
 Informa de un problema de configuración. Esto incluye los problemas encontrados durante la ejecución de un informe **CONFIGURATION** o durante la fase de configuración de un informe **RUNTIME** . **ssbdiagnose** notifica una vez cada problema de configuración.  
  
 **Evento**  
 Notifica un evento de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] que indica que una conversación que se estaba supervisando ha encontrado un problema durante un informe **RUNTIME** . **ssbdiagnose** notifica los eventos cada vez que se generan. Es posible que se informe más de una vez de los eventos si varias conversaciones encuentran el problema.  
  
 **Problema**  
 Notifica un problema que impide que **ssbdiagnose** complete un análisis de configuración o supervise conversaciones.  
  
## <a name="sqlcmd-environment-variables"></a>Variables de entorno de sqlcmd  
 La utilidad **ssbdiagnose** admite las variables de entorno SQLCMDSERVER, SQLCMDUSER, SQLCMDPASSWORD y SQLCMDLOGINTIMOUT que también emplea la utilidad **sqlcmd** . Puede establecer las variables de entorno mediante el comando SET del símbolo del sistema o el comando **setvar** en los scripts de [!INCLUDE[tsql](../../includes/tsql-md.md)] que ejecute con **sqlcmd**. Para obtener más información sobre cómo usar **setvar** en **sqlcmd**, vea [Usar sqlcmd con variables de script](../../ssms/scripting/sqlcmd-use-with-scripting-variables.md).  
  
## <a name="permissions"></a>Permisos  
 En cada cláusula **connectionoptions** , el inicio de sesión especificado con los parámetros **-E** o **-U** debe ser miembro del rol fijo de servidor **sysadmin** en la instancia especificada en **-S**.  
  
## <a name="examples"></a>Ejemplos  
 Esta sección contiene ejemplos de uso de **ssbdiagnose** en el símbolo del sistema.  
  
### <a name="a-checking-the-configuration-of-two-services-in-the-same-database"></a>A. Comprobar la configuración de dos servicios en la misma base de datos  
 En el ejemplo siguiente se muestra cómo solicitar un informe de configuración cuando se cumplen las siguientes condiciones;  
  
-   El servicio iniciador y el servicio de destino están en la misma base de datos.  
  
-   La base de datos está en la instancia predeterminada de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   La instancia se encuentra en el mismo equipo en el que se ejecuta **ssbdiagnose** .  
  
 La utilidad **ssbdiagnose** notifica la configuración que usa el contrato DEFAULT porque no se ha especificado ON CONTRACT.  
  
```  
ssbdiagnose -E -d MyDatabase CONFIGURATION FROM SERVICE /test/initiator TO SERVICE /test/target  
```  
  
### <a name="b-checking-the-configuration-of-two-services-on-separate-computers-that-use-one-login"></a>B. Comprobar la configuración de dos servicios en equipos independientes que usan un mismo inicio de sesión  
 En el ejemplo siguiente se muestra cómo solicitar un informe de configuración cuando el servicio iniciador y el servicio de destino están en equipos distintos, pero se puede obtener acceso a ellos usando el mismo inicio de sesión con autenticación de Windows.  
  
```  
ssbdiagnose -E CONFIGURATION FROM SERVICE /text/initiator -S InitiatorComputer -d InitiatorDatabase TO SERVICE /test/target -S TargetComputer -d TargetDatabase ON CONTRACT TestContract  
```  
  
### <a name="c-checking-the-configuration-of-two-services-on-separate-computers-that-use-separate-logins"></a>C. Comprobar la configuración de dos servicios en equipos independientes que usan inicios de sesión distintos  
 En el ejemplo siguiente se muestra cómo solicitar un informe de configuración cuando el servicio iniciador y el servicio de destino están en equipos independientes, y se requieren inicios de sesión con autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] distintos para cada instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
```  
ssbdiagnose CONFIGURATION FROM SERVICE /text/initiator   
-S InitiatorComputer -U InitiatorLogin -p !wEx23Dvb   
-d InitiatorDatabase TO SERVICE /test/target -S TargetComputer   
-U TargetLogin -p ER!49jiy -d TargetDatabase ON CONTRACT TestContract  
```  
  
### <a name="d-checking-mirrored-service-configurations-on-separate-computers-with-anonymous-encryption"></a>D. Comprobar las configuraciones de servicio reflejado en equipos independientes con cifrado anónimo  
 En el ejemplo siguiente se muestra cómo solicitar un informe de configuración cuando el servicio iniciador y el servicio de destino están en equipos independientes, y el iniciador está reflejado en una instancia con nombre. El informe también comprueba que los servicios están configurados para usar el cifrado anónimo.  
  
```  
ssbdiagnose -E CONFIGURATION FROM SERVICE /text/initiator   
-S InitiatorComputer -d InitiatorDatabase MIRROR   
-S MirrorComputer/MirrorInstance TO SERVICE /test/target   
-S TargetComputer -d TargetDatabase ON CONTRACT TestContract ENCRYPTION ANONYMOUS  
```  
  
### <a name="e-checking-the-configuration-of-two-contracts"></a>E. Comprobar la configuración de dos contratos  
 En el ejemplo siguiente se muestra cómo crear un archivo de comandos que solicite informes de configuración cuando se cumplan las condiciones siguientes:  
  
-   El servicio iniciador y el servicio de destino están en la misma base de datos.  
  
-   La base de datos está en la instancia predeterminada de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   La instancia se encuentra en el mismo equipo en el que se ejecuta **ssbdiagnose** .  
  
 Cada vez que se ejecuta **ssbdiagnose** , notifica la configuración de un contrato distinto entre los mismos servicios.  
  
```  
ssbdiagnose -E -d MyDatabase CONFIGURATION FROM SERVICE   
/test/initiator TO SERVICE /test/target ON CONTRACT PayRaiseContract  
ssbdiagnose -E -d MyDatabase CONFIGURATION FROM SERVICE /test/initiator   
TO SERVICE /test/target ON CONTRACT PromotionContract  
```  
  
### <a name="f-monitor-the-status-of-a-specific-conversation-on-the-local-computer-with-a-time-out"></a>F. Supervisar el estado de una conversación concreta en el equipo local con un tiempo de espera  
 En el ejemplo siguiente se muestra cómo supervisar una conversación concreta en la que los servicios de iniciador y destino están en la misma base de datos de la instancia predeterminada del mismo equipo que está ejecutando **ssbdiagnose**. El intervalo de tiempo de espera se establece en 20 segundos.  
  
```  
ssbdiagnose -E -d TestDatabase RUNTIME -ID D68D77A9-B1CF-41BF-A5CE-279ABCAB140D -TIMEOUT 20  
```  
  
### <a name="g-monitor-the-status-of-a-conversation-that-spans-two-computers"></a>G. Supervisar el estado de una conversación que abarca dos equipos  
 En el ejemplo siguiente se muestra cómo supervisar una conversación concreta en la que los servicios iniciador y de destino están en equipos independientes.  
  
```  
ssbdiagnose RUNTIME -ID D68D77A9-B1CF-41BF-A5CE-279ABCAB140D   
-TIMEOUT 10 CONNECT TO -E -S InitiatorComputer/InitiatorInstance   
-d InitiatorDatabase CONNECT TO -E -S TargetComputer/TargetInstance   
-d TargetDatabase  
```  
  
### <a name="h-monitor-the-status-of-a-conversation-in-two-databases-in-the-same-instance"></a>H. Supervisar el estado de una conversación en dos bases de datos de la misma instancia  
 En el ejemplo siguiente se muestra cómo supervisar una conversación concreta en la que los servicios de iniciador y destino están en bases de datos independientes de la misma instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)]. En el ejemplo se usa **baseconnectionoptions** para especificar la instancia y la información de inicio de sesión, y dos cláusulas CONNECT TO para especificar las bases de datos. Se especifica -SHOWEVENTS para que todos los eventos en tiempo de ejecución estén incluidos en los resultados del informe.  
  
```  
ssbdiagnose -E -S TestComputer/DevTestInstance RUNTIME -SHOWEVENTS   
-ID 5094d4a7-e38c-4c37-da37-1d58b1cb8455 -TIMEOUT 10 CONNECT TO   
-d InitiatorDatabase CONNECT TO -d TargetDatabase  
```  
  
### <a name="i-monitor-the-status-of-two-conversations-between-two-databases"></a>I. Supervisar el estado de dos conversaciones entre dos bases de datos  
 En el ejemplo siguiente se muestra cómo supervisar dos conversaciones en las que los servicios de iniciador y destino están en bases de datos independientes de la misma instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)]. En el ejemplo se usa **baseconnectionoptions** para especificar la instancia y la información de inicio de sesión, y dos cláusulas CONNECT TO para especificar las bases de datos.  
  
```  
ssbdiagnose -E -S TestComputer/DevTestInstance RUNTIME   
-ID 5094d4a7-e38c-4c37-da37-1d58b1cb8455   
-ID 9b293be9-226b-4e22-e169-1d2c2c15be86 -TIMEOUT 10 CONNECT TO   
-d InitiatorDatabase CONNECT TO -d TargetDatabase  
```  
  
### <a name="j-monitor-the-status-of-all-conversations-between-two-databases"></a>J. Supervisar el estado de todas las conversaciones entre dos bases de datos  
 En el ejemplo siguiente se muestra cómo supervisar todas las conversaciones entre dos bases de datos de la misma instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)]. En el ejemplo se usa **baseconnectionoptions** para especificar la instancia y la información de inicio de sesión, y dos cláusulas CONNECT TO para especificar las bases de datos.  
  
```  
ssbdiagnose -E -S TestComputer/DevTestInstance RUNTIME   
-TIMEOUT 10 CONNECT TO -d InitiatorDatabase CONNECT TO   
-d TargetDatabase  
```  
  
### <a name="k-ignore-specific-errors"></a>K. Omitir errores concretos  
 En el ejemplo siguiente se muestra cómo omitir errores conocidos (303 y 304) y cómo está configurada la activación en un sistema de prueba.  
  
```  
ssbdiagnose -IGNORE 303 -IGNORE 304 -E -d TestDatabase   
CONFIGURATION FROM SERVICE /test/initiator TO SERVICE /test/target   
ON CONTRACT TextContract  
```  
  
### <a name="l-redirecting-ssbdiagnose-xml-output"></a>L. Redirigir la salida XML de ssbdiagnose  
 En el ejemplo siguiente se muestra cómo solicitar que **ssbdiagnose** genere la salida en forma de archivo XML que se redirige a un archivo. A continuación, el archivo TestDiag.xml puede abrirse en una aplicación diseñada para analizar o informar sobre los archivos XML de **ssbdiagnose** . O bien, se puede ver en un editor XML estándar, como XML Notepad.  
  
```  
ssbdiagnose -XML -E -d MyDatabase CONFIGURATION FROM SERVICE   
/test/initiator TO SERVICE /test/target > c:\MyDiagnostics\TestDiag.xml  
```  
  
### <a name="m-using-an-environment-variable"></a>M. Usar una variable de entorno  
 En el ejemplo siguiente se establece primero la variable de entorno SQLCMDSERVER para que contenga el nombre del servidor y, después, se ejecuta **ssbdiagnose** sin especificar **-S**.  
  
```  
SET SQLCMDSERVER=MyComputer  
ssbdiagnose -XML -E -d MyDatabase CONFIGURATION FROM SERVICE   
/test/initiator TO SERVICE /test/target  
```  
  
## <a name="see-also"></a>Consulte también  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)   
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [CREATE BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/create-broker-priority-transact-sql.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE CONTRACT &#40;Transact-SQL&#41;](../../t-sql/statements/create-contract-transact-sql.md)   
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md)   
 [CREATE MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-message-type-transact-sql.md)   
 [CREATE QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/create-queue-transact-sql.md)   
 [CREATE REMOTE SERVICE BINDING &#40;Transact-SQL&#41;](../../t-sql/statements/create-remote-service-binding-transact-sql.md)   
 [CREATE ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/create-route-transact-sql.md)   
 [CREATE SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/create-service-transact-sql.md)   
 [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md)   
 [sys.transmission_queue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-transmission-queue-transact-sql.md)   
 [sys.conversation_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)   
 [sys.conversation_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-groups-transact-sql.md)  
  
