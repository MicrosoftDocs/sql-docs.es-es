---
title: Agente de registro del LOG de replicación | Microsoft Docs
description: El Agente de registro del LOG de replicación supervisa las bases de datos de SQL Server configuradas para la replicación transaccional y copia las transacciones en la base de datos de distribución.
ms.custom: ''
ms.date: 10/29/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Log Reader Agent, executables
- Log Reader Agent, parameter reference
- agents [SQL Server replication], Log Reader Agent
- command prompt [SQL Server replication]
ms.assetid: 5487b645-d99b-454c-8bd2-aff470709a0e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 58e9ed82d9518d423001342fa288e2c7aba1f5bf
ms.sourcegitcommit: 98acedd435aecfda1b3c4c23d3f0c3c1a12682a4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102532347"
---
# <a name="replication-log-reader-agent"></a>Agente de registro del LOG de replicación
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  El Agente de registro del LOG de replicación es un archivo ejecutable que supervisa el registro de transacciones de cada base de datos configurada para la replicación transaccional y copia las transacciones marcadas para ser replicadas desde el registro de transacciones a la base de datos de distribución.  
  
> [!NOTE]  
>  Los parámetros se pueden especificar en cualquier orden. Cuando no se especifican parámetros opcionales, se utilizan valores predefinidos basados en el perfil de agente predeterminado.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
logread [-?]   
-Publisher server_name[\instance_name]   
-PublisherDB publisher_database   
[-Continuous]  
[-DefinitionFile def_path_and_file_name]  
[-Distributor server_name[\instance_name]]  
[-DistributorLogin distributor_login]  
[-DistributorPassword distributor_password]  
[-DistributorSecurityMode [0|1]]  
[-EncryptionLevel [0|1|2]]  
[-ExtendedEventConfigFile configuration_path_and_file_name]  
[-HistoryVerboseLevel [0|1|2]]  
[-KeepAliveMessageInterval keep_alive_message_interval_seconds]  
[-LoginTimeOut login_time_out_seconds]  
[-LogScanThreshold scan_threshold]  
[-MaxCmdsInTran number_of_commands]  
[-MessageInterval message_interval]
[-MultiSubnetFailover [0|1]]
[-Output output_path_and_file_name]  
[-OutputVerboseLevel [0|1|2|3|4]]  
[-PacketSize packet_size]  
[-PollingInterval polling_interval]  
[-ProfileName profile_name]   
[-PublisherFailoverPartner server_name[\instance_name] ]  
[-PublisherSecurityMode [0|1]]  
[-PublisherLogin publisher_login]  
[-PublisherPassword publisher_password]   
[-QueryTimeOut query_time_out_seconds]  
[-ReadBatchSize number_of_transactions]   
[-ReadBatchThreshold read_batch_threshold]  
[-RecoverFromDataErrors]  
```  
  
## <a name="arguments"></a>Argumentos  
 **-?**  
 Muestra información de uso.  
  
 **-Publisher** _server_name_[ **\\** _instance_name_]  
 Es el nombre del publicador. Especifique *server_name* para la instancia predeterminada de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en ese servidor. Especifique _server_name_ **\\** _instance_name_ para una instancia con nombre de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en ese servidor.  
  
 **-PublisherDB** _publisher_database_  
 Es el nombre de la base de datos del publicador.  
  
 **-Continuous**  
 Especifica si el agente intenta sondear las transacciones replicadas continuamente. Si se especifica, el agente sondea las transacciones replicadas del origen en intervalos de sondeo, aunque no haya ninguna transacción pendiente.  
  
 **-DefinitionFile** _def_path_and_file_name_  
 Es la ruta de acceso del archivo de definición de agente. Un archivo de definición de agente contiene los argumentos de línea de comandos para el agente. El contenido del archivo se analiza como un archivo ejecutable. Utilice las comillas tipográficas (") para especificar valores de argumento que contienen caracteres arbitrarios.  
  
 **-Distributor** _server_name_[ **\\** _instance_name_]  
 Es el nombre del distribuidor. Especifique *server_name* para conectarse a la instancia predeterminada del [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en ese servidor. Especifique _server_name_ **\\** _instance_name_ para una instancia con nombre de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en ese servidor.  
  
 **-DistributorLogin** _distributor_login_  
 Es el nombre de inicio de sesión del distribuidor.  
  
 **-DistributorPassword** _distributor_password_  
 Es la contraseña del distribuidor.  
  
 **-DistributorSecurityMode** [ **0**\| **1**]  
 Especifica el modo de seguridad del distribuidor. Un valor de **0** hace referencia a la autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (valor predeterminado) y un valor de **1** hace referencia al modo de autenticación de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows.  
  
 **-EncryptionLevel** [ **0** \| **1** \| **2** ]  
 Es el nivel de cifrado de la Seguridad de la capa de transporte (TLS), conocida anteriormente como Capa de sockets seguros (SSL), utilizado por el Agente de lectura del LOG cuando realiza conexiones.  
  
|Valor de EncryptionLevel|Descripción|  
|---------------------------|-----------------|  
|**0**|Especifica que no se utiliza TLS.|  
|**1**|Especifica que se utiliza TLS, pero el agente no comprueba que un emisor confiable haya firmado el certificado del servidor TLS/SSL.|  
|**2**|Especifica que se usa TLS y que se ha comprobado el certificado.|  

 > [!NOTE]  
 >  Un certificado TLS/SSL válido se define con un nombre de dominio completo de SQL Server. Para que el agente se conecte correctamente al establecer -EncryptionLevel en 2, cree un alias en la instancia local de SQL Server. El parámetro "Alias Name" debe ser el nombre del servidor, mientras que el parámetro "Server" debe establecerse en el nombre completo de la instancia de SQL Server.
 
 Para más información, consulte [Ver y modificar la configuración de seguridad de la replicación](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md).  
  
 **-ExtendedEventConfigFile** _configuration_path_and_file_name_  
 Especifica el nombre y la ruta del archivo para el archivo de configuración XML de eventos extendidos. El archivo de configuración de eventos extendidos le permite configurar sesiones y habilitar eventos para su seguimiento.  
  
 **-HistoryVerboseLevel** [ **0**\| **1**\| **2**]  
 Especifica la cantidad de historial registrado durante una operación del lector de registro. Puede minimizar el efecto sobre el rendimiento del registro del historial seleccionando **1**.  
  
|Valor HistoryVerboseLevel|Descripción|  
|-------------------------------|-----------------|  
|**0**||  
|**1**|Predeterminada. Siempre actualiza un mensaje del historial anterior del mismo estado (inicio, progreso, éxito, etc.). Si no existe ningún registro anterior con el mismo estado, inserta un nuevo registro.|  
|**2**|Inserta nuevos registros de historial a menos que el registro sea para mensajes de inactividad o mensajes de trabajos de ejecución prolongada, en cuyo caso actualiza los registros anteriores.|  
  
 **-KeepAliveMessageInterval** _keep_alive_message_interval_seconds_  
 Es el número de segundos antes de que el subproceso del historial compruebe si cualquiera de las conexiones existentes está esperando una respuesta del servidor. Este valor se puede reducir para evitar que la comprobación del agente marque al agente de registro del LOG como sospechoso al ejecutar un lote de ejecución prolongada. El valor predeterminado es 300 segundos.  
  
 **-LoginTimeOut** _login_time_out_seconds_  
 Es el número de segundos antes de que el inicio de sesión exceda el tiempo de espera. El valor predeterminado es 15 segundos.  
  
 **-LogScanThreshold** _scan_threshold_  
 Exclusivamente para uso interno.  
  
 **-MaxCmdsInTran** _number_of_commands_  
 Especifica el número máximo de instrucciones agrupadas en una transacción cuando el Agente de registro del LOG escribe comandos en la base de datos de distribución. El uso de este parámetro permite al Agente de registro del LOG y al Agente de distribución dividir las transacciones grandes (compuestas por muchos comandos) del publicador en varias transacciones más pequeñas cuando se aplican en el suscriptor. Especificando este parámetro se puede reducir la contención en el distribuidor y la latencia entre el publicador y el suscriptor. Puesto que la transacción original se aplica en unidades más pequeñas, el suscriptor puede obtener acceso a las filas de una transacción lógica del publicador de gran tamaño antes de que finalice la transacción original, lo que interrumpe la estricta atomicidad transaccional. El valor predeterminado es **0**, que conserva los límites de la transacción del publicador.  
  
> [!NOTE]
>  Este parámetro se omite para las publicaciones que no son de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Para obtener más información, vea la sección "Configurar el trabajo del conjunto de transacciones" en [Performance Tuning for Oracle Publishers](../../../relational-databases/replication/non-sql/performance-tuning-for-oracle-publishers.md).  
  
 **-MessageInterval** _message_interval_  
 Es el intervalo de tiempo utilizado para el registro del historial. Un evento del historial se registra cuando se alcanza el valor de **MessageInterval** una vez registrado el último evento de historial.  
  
 Si no hay ninguna transacción replicada disponible en el origen, el agente envía un mensaje de no transacción al distribuidor. Esta opción especifica cuánto tiempo espera el agente para enviar otro mensaje que indica que no hay ninguna transacción. Los agentes siempre envían un mensaje que indica que no hay ninguna transacción cuando detectan que no hay ninguna transacción disponible en el origen después de procesar previamente las transacciones replicadas. El valor predeterminado es 60 segundos.  
 
 **-MultiSubnetFailover** [**0**\|**1**] Especifica si la propiedad MultiSubnetFailover está habilitada o no. En un grupo de disponibilidad (AG) AlwaysOn en subredes diferentes, al establecer MultiSubnetFailover en 1 (true), se detecta más rápidamente el servidor (actualmente) activo y se acelera la conexión a este.   
   **Se aplica a**: [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)]).  
  
 **-Output** _output_path_and_file_name_  
 Es la ruta de acceso del archivo de salida del agente. Si no se proporciona un nombre de archivo, el resultado se envía a la consola. Si el nombre de archivo especificado existe, el resultado se anexa al archivo.  
  
 **-OutputVerboseLevel** [ **0**\| **1**\| **2** \| **3** \| **4** ]  
 Especifica si el resultado debería ser detallado.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0**|Solo se imprimen los mensajes de error.|  
|**1**|Se imprimen todos los mensajes de informe de progreso de agente.|  
|**2** (predeterminado)|Se imprimen todos los mensajes de error y mensajes de informe de progreso de agente.|  
|**3**|Se imprimen los primeros 100 bytes de cada comando replicado.|  
|**4**|Se imprimen todos los comandos replicados.|  
  
 Valora 2-4 son útiles al depurar.  
  
 **-PacketSize** _packet_size_  
 Es el tamaño del paquete, en bytes. El valor predeterminado es 4096 (bytes).  
  
 **-PollingInterval** _polling_interval_  
 Es la frecuencia, en segundos, con la que el registro recibe consultas de transacciones replicadas. El valor predeterminado es 5 segundos.  
  
 **-ProfileName** _profile_name_  
 Especifica un perfil de agente para utilizar para los parámetros del agente. Si **ProfileName** es NULL, el perfil de agente se deshabilita. Si no se especifica **ProfileName** , se utiliza el perfil predeterminado para el tipo de agente. Para obtener información, vea [Perfiles del Agente de replicación](../../../relational-databases/replication/agents/replication-agent-profiles.md).  
  
 **-PublisherFailoverPartner** _server_name_[ **\\** _instance_name_]  
 Especifica la instancia del asociado de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que participa en una sesión de creación de reflejo de la base de datos con la base de datos de publicación. Para obtener más información, vea [Replicación y creación de reflejo de la base de datos &#40;SQL Server&#41;](../../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md).  
  
 **-PublisherSecurityMode** [ **0**\| **1**]  
 Especifica el modo de seguridad del publicador. Un valor de **0** hace referencia a la autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (predeterminado) y un valor de **1** hace referencia al modo de autenticación de Windows.  
  
 **-PublisherLogin** _publisher_login_  
 Es el nombre de inicio de sesión del publicador.  
  
 **-PublisherPassword** _publisher_password_  
 Es la contraseña del publicador.  
  
 **-QueryTimeOut** _query_time_out_seconds_  
 Es el número de segundos antes de que la consulta exceda el tiempo de espera. El valor predeterminado es 1800 segundos.  
  
 **-ReadBatchSize** _number_of_transactions_  
 Es el número máximo de transacciones leídas del registro de transacciones de la base de datos de publicación por ciclo de procesamiento, con un valor predeterminado de 500 y un máximo de 10000. El agente continuará leyendo las transacciones en lotes hasta que se lean todas las transacciones del registro. Este parámetro no se admite en publicadores de Oracle.  
  
 **-ReadBatchThreshold** _number_of_commands_  
 Es el número de comandos de replicación que se deben leer del registro de transacciones antes de que el Agente de distribución las envíe al Suscriptor. El valor predeterminado es 0. Si no se especifica este parámetro, el Agente de registro del LOG leerá al final del registro o al número especificado en **-ReadBatchSize** (número de transacciones).  
  
 **-RecoverFromDataErrors**  
 Especifica que el Agente de registro del LOC continúe ejecutándose cuando encuentra errores en datos de columna publicados en un Publicador que no es de SQL Server. De forma predeterminada, tales errores hacen el Agente de registro del LOG devuelva un error. Al utilizar **-RecoverFromDataErrors**, los datos de columna erróneos se replican como NULL o un valor nonnull adecuado y los mensajes de advertencia se registran en la tabla [MSlogreader_history](../../../relational-databases/system-tables/mslogreader-history-transact-sql.md) . Este parámetro solamente se admite en publicadores de Oracle.  
  
## <a name="remarks"></a>Observaciones  
  
> [!IMPORTANT]  
>  Si ha instalado el agente de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para que se ejecute en una cuenta de sistema local en lugar de debajo de una cuenta de usuario de dominio (el valor predeterminado), el servicio puede tener acceso solamente al equipo local. Si el Agente de registro del LOG que se ejecuta en el agente de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se configura para utilizar el modo de autenticación de Windows cuando inicia sesión en una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], el Agente de registro del LOC devuelve un error. La configuración predeterminada es la autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Para obtener más información acerca de cómo cambiar cuentas de seguridad, vea [View and Modify Replication Security Settings](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md).  
  
 Para iniciar el Agente de registro del LOG, ejecute **logread.exe** en el símbolo del sistema. Para obtener información, vea [Conceptos de los ejecutables del Agente de replicación](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
## <a name="change-history"></a>Historial de cambios  
  
|Contenido actualizado|  
|---------------------|  
|Se ha agregado el parámetro **-ExtendedEventConfigFile** .|  
|Se ha agregado el parámetro **-MultiSubnetFailover**.|
  
## <a name="see-also"></a>Consulte también  
 [Administración del Agente de replicación](../../../relational-databases/replication/agents/replication-agent-administration.md)  
  
  
