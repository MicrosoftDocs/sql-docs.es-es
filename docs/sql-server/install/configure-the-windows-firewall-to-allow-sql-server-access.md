---
title: Configurar el Firewall de Windows
description: Aprenda a configurar Firewall de Windows para permitir el acceso a una instancia de SQL Server a través del firewall.
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- Windows Firewall ports
- WMI firewall ports
- Windows Firewall [Database Engine]
- firewall systems, configuring
- advfirewall
- firewall systems
- rules firewall
- firewall systems, overview and port list
- 1433 TCP port
- portopening using netsh
- ports [SQL Server], TCP
- netsh to open firewall ports
ms.assetid: f55c6a0e-b6bd-4803-b51a-f3a419803024
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7a00c8196a3a20d17f07c215b8d0d0f1c3ffd84e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352448"
---
# <a name="configure-the-windows-firewall-to-allow-sql-server-access"></a>Configure the Windows Firewall to Allow SQL Server Access
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Los sistemas de firewall ayudan a evitar el acceso no autorizado a los recursos de los equipos. Si un firewall está activado pero no está configurado correctamente, es posible que se bloqueen los intentos de conexión a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Para obtener acceso a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a través de un firewall, debe configurar el firewall en el equipo en el que se ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El firewall es un componente de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. También puede instalar un firewall de otra compañía. En este artículo se explica cómo configurar el firewall de Windows, pero los principios básicos se aplican a otros programas de firewall.  
  
> [!NOTE]  
>  En este artículo se proporciona información general para configurar el firewall y se resume información de interés para administradores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información acerca del firewall e información autorizada, vea la documentación sobre el firewall como [Guía de implementación de Firewall de Windows Defender con seguridad avanzada](/windows/security/threat-protection/windows-firewall/windows-firewall-with-advanced-security-deployment-guide).  
  
 Los usuarios familiarizados con la administración de **Firewall de Windows** y que saben qué opciones del firewall quieren configurar pueden pasar directamente a los artículos más avanzados:  
  
-   [Configuración de Firewall de Windows para el acceso al motor de base de datos](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)    
-   [Configurar Firewall de Windows para permitir el acceso a Analysis Services](/analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access)    
-   [Configurar un firewall para el acceso del Servidor de informes](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md)  
  
##  <a name="basic-firewall-information"></a><a name="BKMK_basic"></a> Información básica del firewall  
 Los firewalls funcionan inspeccionando los paquetes entrantes y comparándolos con un conjunto de reglas. Si el paquete cumple los estándares impuestos por las reglas, el firewall pasa el paquete al protocolo TCP/IP para su procesamiento adicional. Si el paquete no cumple los estándares especificados por las reglas, el firewall descarta el paquete y, si está habilitado el registro, crea una entrada en el archivo de registro del firewall.  
  
 La lista de tráfico permitido se rellena de una de las maneras siguientes:  

- **Automáticamente**: cuando un equipo que tiene el firewall habilitado inicia la comunicación, el firewall crea una entrada en la lista para que se permita la respuesta. Esta respuesta se considera tráfico solicitado y no hay nada que deba configurarse. 
  
-   **Manualmente**: Un administrador configura las excepciones para el firewall. Esto permite el acceso a programas o puertos específicos del equipo. En este caso, el equipo acepta el tráfico de entrada no solicitado cuando actúa como un servidor, un agente de escucha o un equipo del mismo nivel. Éste es el tipo de configuración que se debe completar para conectar a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Elegir una estrategia del firewall es más complejo que decidir simplemente si un puerto determinado debe estar abierto o cerrado. Al diseñar una estrategia de firewall para la empresa, asegúrese considerar todas las reglas y opciones de configuración disponibles. En este artículo no se revisan todas las posibles opciones de firewall. Se recomienda que revise los siguientes documentos:  
  
 [Guía de implementación de Firewall de Windows Defender con seguridad avanzada](/windows/security/threat-protection/windows-firewall/windows-firewall-with-advanced-security-deployment-guide)    
 [Firewall de Windows Defender con seguridad avanzada](/windows/security/threat-protection/windows-firewall/windows-firewall-with-advanced-security-design-guide)    
 [Introducción al aislamiento de servidor y dominio](/windows/security/threat-protection/windows-firewall/domain-isolation-policy-design)  
  
##  <a name="default-firewall-settings"></a><a name="BKMK_default"></a> Configuración predeterminada del firewall  
 El primer paso para planear la configuración del firewall es determinar el estado actual del firewall para el sistema operativo. Si el sistema operativo se actualizó desde una versión anterior, se puede haber conservado la configuración de firewall anterior. Además, otro administrador o una Directiva de grupo podría haber cambiado la configuración del firewall en el dominio.  
  
> [!NOTE]  
>  La activación del firewall afectará a otros programas que tengan acceso a este equipo, tales como el uso compartido de impresoras y archivos, y las conexiones remotas al escritorio. Los administradores deben considerar todas las aplicaciones que se están ejecutando en el equipo antes de ajustar la configuración de firewall.  
  
##  <a name="programs-to-configure-the-firewall"></a><a name="BKMK_programs"></a> Programas para configurar el firewall  
Configure el Firewall de Windows con **Microsoft Management Console** o **netsh**.  

-  **Microsoft Management Console (MMC)**  
  
     El complemento MMC del Firewall de Windows con seguridad avanzada permite establecer opciones de configuración de firewall más avanzadas. Este complemento presenta la mayoría de las opciones de firewall de una manera fácil de usar y presenta todos los perfiles de firewall. Para más información, vea [Utilizar el complemento Firewall de Windows con seguridad avanzada](#BKMK_WF_msc), más adelante en este artículo.  
  
-   **netsh**  
  
     Un administrador puede usar la herramienta **netsh.exe** para configurar y supervisar equipos basados en Windows en un símbolo del sistema o un archivo por lotes **.** Con la herramienta **netsh** , puede dirigir los comandos de contexto que escriba en el asistente adecuado y el asistente ejecutará el comando. Un asistente es un archivo de biblioteca de vínculos dinámicos (.dll) que extiende la funcionalidad de la herramienta **netsh** proporcionando funciones de configuración, supervisión y soporte técnico de uno o más servicios, utilidades o protocolos. Todos los sistemas operativos que admiten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tienen un asistente de firewall. [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] también tiene un asistente avanzado denominado **advfirewall**. Los detalles del uso de **netsh** no se explican en este artículo. Sin embargo, muchas de las opciones de configuración descritas se pueden configurar con **netsh**. Por ejemplo, ejecute el script siguiente en un símbolo del sistema para abrir el puerto TCP 1433:  
  
    ```  
    netsh firewall set portopening protocol = TCP port = 1433 name = SQLPort mode = ENABLE scope = SUBNET profile = CURRENT  
    ```  
  
     Un ejemplo similar que usa Firewall de Windows para el asistente Seguridad avanzada:  
  
    ```  
    netsh advfirewall firewall add rule name = SQLPort dir = in protocol = tcp action = allow localport = 1433 remoteip = localsubnet profile = DOMAIN  
    ```  
  
     Para obtener más información acerca de **netsh**, consulte los siguientes vínculos:  
  
    -   [Sintaxis, contextos y formatos de comandos Netsh](/windows-server/networking/technologies/netsh/netsh-contexts)    
    -   [Cómo usar el contexto "netsh advfirewall firewall" en lugar del contexto "netsh firewall" para controlar el comportamiento del Firewall de Windows en Windows Server 2008 y en Windows Vista](https://support.microsoft.com/kb/947709)    

    
- **Para Linux**: en Linux, también debe abrir los puertos asociados con los servicios a los que necesita acceso. Las distintas distribuciones de Linux y los distintos firewalls tienen sus propios procedimientos. Para ver dos ejemplos, consulte [SQL Server en Red Hat](../../linux/quickstart-install-connect-red-hat.md) y [SQL Server en SUSE](../../linux/quickstart-install-connect-suse.md). 
  
## <a name="ports-used-by-ssnoversion"></a>Puertos utilizados por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Las tablas siguientes pueden ayudarle a identificar los puertos que utiliza [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
###  <a name="ports-used-by-the-database-engine"></a><a name="BKMK_ssde"></a> Ports Used By the Database Engine  
 

De forma predeterminada, los puertos que SQL Server suele utilizar y los servicios de motor de la base de datos asociados son los siguientes: TCP **1433**, **4022**, **135**, **1434**, UDP **1434**. En la tabla siguiente se detallan dichos puertos con mayor profundidad. Una instancia con nombre utiliza [puertos dinámicos](#BKMK_dynamic_ports).
 
 La tabla siguiente muestra los puertos de uso frecuente por parte de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
|Escenario|Port|Comentarios|  
|--------------|----------|--------------|  
|Instancia predeterminada en ejecución a través de TCP|Puerto TCP 1433|Éste es el puerto más común que permite el firewall. Se aplica a las conexiones rutinarias a la instalación predeterminada de [!INCLUDE[ssDE](../../includes/ssde-md.md)]o a una instancia con nombre que sea la única instancia que se ejecuta en el equipo. (Las instancias con nombre tienen consideraciones especiales. Vea [Puertos dinámicos](#BKMK_dynamic_ports) más adelante en este artículo).|  
|Instancias con nombre con puerto predeterminado|El puerto TCP es un puerto dinámico determinado en el momento en el que se inicia [!INCLUDE[ssDE](../../includes/ssde-md.md)] .|Vea la explicación siguiente en la sección [Puertos dinámicos](#BKMK_dynamic_ports). El puerto UDP 1434 puede ser necesario para el servicio Explorador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuando se utilizan instancias con nombre.|  
| Instancias con nombre con puerto fijo |El número de puerto configurado por el administrador.|Vea la explicación siguiente en la sección [Puertos dinámicos](#BKMK_dynamic_ports).|  
|Conexión de administrador dedicada|Puerto TCP 1434 para la instancia predeterminada. Otros puertos se utilizan para las instancias con nombre. Compruebe en el registro de errores el número de puerto.|De forma predeterminada, las conexiones remotas a DAC (Conexión de administrador dedicada) no están habilitadas. Para habilitar la DAC remota, utilice la faceta Configuración de área expuesta. Para obtener más información, vea [Surface Area Configuration](../../relational-databases/security/surface-area-configuration.md).|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Servicio Browser|Puerto UDP 1434|El servicio Explorador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] escucha las conexiones entrantes a una instancia con nombre y proporciona al cliente el número de puerto TCP que corresponde a esa instancia con nombre. Normalmente, el servicio Explorador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se inicia siempre que se utilizan instancias con nombre de [!INCLUDE[ssDE](../../includes/ssde-md.md)] . El servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser no tiene que iniciarse si el cliente está configurado para conectarse al puerto específico de la instancia con nombre.|  
|Instancia con punto de conexión HTTP.|Se puede especificar cuando se crea un extremo HTTP. El valor predeterminado es el puerto TCP 80 para el tráfico de CLEAR_PORT y el puerto 443 para el tráfico de SSL_PORT.|Se utiliza para una conexión HTTP a través de una dirección URL.|  
|Instancia predeterminada con punto de conexión HTTP |Puerto TCP 443|Se utiliza para una conexión HTTPS a través de una dirección URL. HTTPS es una conexión HTTP que usa la Seguridad de la capa de transporte (TLS), antes conocida como Capa de sockets seguros (SSL).|  
|[!INCLUDE[ssSB](../../includes/sssb-md.md)]|Puerto 4022 TCP. Para comprobar el puerto que se usa, ejecute la siguiente consulta:<br /><br /> `SELECT name, protocol_desc, port, state_desc`<br /><br /> `FROM sys.tcp_endpoints`<br /><br /> `WHERE type_desc = 'SERVICE_BROKER'`|No hay ningún puerto predeterminado para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssSB](../../includes/sssb-md.md)], pero esta es la configuración convencional que se usa en los ejemplos de Libros en pantalla.|  
|Creación de reflejo de la base de datos|Puerto elegido por el administrador. Para determinar el puerto, ejecute la siguiente consulta:<br /><br /> `SELECT name, protocol_desc, port, state_desc FROM sys.tcp_endpoints`<br /><br /> `WHERE type_desc = 'DATABASE_MIRRORING'`|No hay ningún puerto predeterminado para la creación de reflejo de la base de datos, pero en los ejemplos de los Libros en pantalla se usa el puerto TCP 5022 o 7022. Es importante evitar interrumpir un punto de conexión de creación de reflejo que se esté usando, sobre todo en el modo de alta seguridad con conmutación automática por error. La configuración del firewall debe evitar el romper el quórum. Para obtener más información, vea [Especificar una dirección de red de servidor &#40;creación de reflejo de la base de datos&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).|  
|Replicación|Las conexiones de replicación a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizan los puertos de [!INCLUDE[ssDE](../../includes/ssde-md.md)] normales (puerto TCP 1433, para la instancia predeterminada, etc.) habituales.<br /><br /> La sincronización web y el acceso de tipo FTP/UNC para la instantánea de replicación requieren que se abran puertos adicionales en el firewall. Para transferir los datos iniciales y los esquemas de una ubicación a otra, la replicación puede utilizar FTP (puerto TCP 21) o sincronizar sobre HTTP (puerto TCP 80) o Uso compartido de archivos. El uso compartido de archivos utiliza los puertos UDP 137 y 138, y el puerto TCP 139 si usa NetBIOS. El uso compartido de archivos usa el puerto TCP 445.|Para la sincronización sobre HTTP, la replicación utiliza el extremo IIS (para el que se pueden configurar los puertos, pero cuyo puerto predeterminado es el 80), pero el proceso IIS se conecta al servidor [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a través de los puertos estándar (1433 para la instancia predeterminada).<br /><br /> Durante la sincronización web mediante FTP, la transferencia FTP tiene lugar entre IIS y el publicador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , no entre el suscriptor e IIS.|  
|[!INCLUDE[tsql](../../includes/tsql-md.md)] depurador|Puerto TCP 135<br /><br /> Vea [Consideraciones especiales para el puerto 135](#BKMK_port_135)<br /><br /> Quizá sea necesaria también la excepción [IPsec](#BKMK_IPsec) .|Si utiliza [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], en el equipo host [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] debe agregar también **Devenv.exe** a la lista Excepciones y abrir el puerto TCP 135.<br /><br /> Si utiliza [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], en el equipo host [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] debe agregar también **ssms.exe** a la lista Excepciones y abrir el puerto TCP 135. Para obtener más información, vea [Configurar reglas de firewall antes de ejecutar al depurador de TSQL](../../ssms/scripting/configure-firewall-rules-before-running-the-tsql-debugger.md).|  
  
 Para obtener instrucciones detalladas sobre cómo configurar Firewall de Windows para el [!INCLUDE[ssDE](../../includes/ssde-md.md)], vea [Configurar Firewall de Windows para el acceso al motor de base de datos](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md).  
  
####  <a name="dynamic-ports"></a><a name="BKMK_dynamic_ports"></a> Puertos dinámicos  
 De forma predeterminada, las instancias con nombre (incluido [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]) utilizan puertos dinámicos. Eso significa que cada vez que se inicia [!INCLUDE[ssDE](../../includes/ssde-md.md)] , identifica un puerto disponible y utiliza ese número de puerto. Si la instancia con nombre es la única instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] instalada, probablemente utilizará el puerto TCP 1433. Si se instalan otras instancias de [!INCLUDE[ssDE](../../includes/ssde-md.md)] , probablemente utilizará un puerto TCP diferente. Dado que el puerto seleccionado puede cambiar cada vez que se inicia [!INCLUDE[ssDE](../../includes/ssde-md.md)] , es difícil configurar el firewall para permitir el acceso al número de puerto correcto. Por consiguiente, si se usa un firewall, recomendamos reconfigurar el [!INCLUDE[ssDE](../../includes/ssde-md.md)] para que utilice siempre el mismo número de puerto. Esto se denomina un puerto fijo o un puerto estático. Para obtener más información, vea [Configurar un servidor para que escuche en un puerto TCP específico &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md).  
  
 Una alternativa a configurar una instancia con nombre para escuchar en un puerto fijo es crear una excepción en el firewall para un programa de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] como **sqlservr.exe** (para el [!INCLUDE[ssDE](../../includes/ssde-md.md)]). Esto puede ser cómodo, pero el número de puerto no aparecerá en la columna **Puerto local** de la página **Reglas de entrada** cuando esté usando el complemento MMC de Firewall de Windows con seguridad avanzada. Esto puede hacer más difícil la tarea de auditar qué puertos están abiertos. Otra consideración es que un Service Pack o una actualización acumulativa puede cambiar la ruta de acceso a la aplicación ejecutable [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , lo que invalidará la regla de firewall.  
  
##### <a name="to-add-a-program-exception-to-the-firewall-using-windows-defender-firewall-with-advanced-security"></a>Para agregar una excepción de programa al firewall mediante el Firewall de Windows Defender con seguridad avanzada
  
1. En el menú Inicio, escriba *wf.msc*. Presione Entrar o seleccione el resultado de la búsqueda wf.msc para abrir el **Firewall de Windows Defender con seguridad avanzada**.
1. En el panel izquierdo, seleccione **Reglas de entrada**.
1. En el panel derecho, en **Acciones**, seleccione **Nueva regla…** . Se abre el **Asistente para nueva regla de entrada**.
1. En **Tipo de regla**, seleccione **Programa**. Seleccione **Next** (Siguiente).
1. En **Programa**, seleccione **Esta ruta de acceso del programa**. Seleccione **Examinar** para buscar la instancia de SQL Server. El programa se denomina sqlservr.exe. Suele encontrarse en:

   `C:\Program Files\Microsoft SQL Server\MSSQL15.<InstanceName>\MSSQL\Binn\sqlservr.exe`

   Seleccione **Next** (Siguiente).

1. En **Acción**, seleccione **Permitir la conexión**. Seleccione **Next** (Siguiente).
1. En **Perfil**, incluya los tres perfiles. Seleccione **Next** (Siguiente).
1. En **Nombre**, escriba un nombre para la regla. Seleccione **Finalizar**.

Para obtener más información sobre los puntos de conexión, vea [Configurar el motor de base de datos para escuchar en varios puertos TCP](../../database-engine/configure-windows/configure-the-database-engine-to-listen-on-multiple-tcp-ports.md) y [Vistas de catálogo de puntos de conexión &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md). 
  
###  <a name="ports-used-by-analysis-services"></a><a name="BKMK_ssas"></a> Puertos utilizados por Analysis Services  
 
De forma predeterminada, los puertos que SQL Server Analysis Services suele utilizar y los servicios asociados son los siguientes: TCP **2382**, **2383**, **80**, **443**. En la tabla siguiente se detallan dichos puertos con mayor profundidad.  
 
 La tabla siguiente muestra los puertos de uso frecuente por parte de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
|Característica|Port|Comentarios|  
|-------------|----------|--------------|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|Puerto TCP 2383 para la instancia predeterminada|El puerto estándar para la instancia predeterminada de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Servicio Browser|El puerto TCP 2382 solamente es necesario para una instancia con nombre de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|Las solicitudes de conexión de cliente para una instancia con nombre de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] que no especifican un número de puerto se dirigen al puerto 2382, el puerto en el que escucha el Explorador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a continuación redirige la solicitud al puerto que utiliza la instancia con nombre.|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] configurado para el uso a través de IIS/HTTP<br /><br /> (El Servicio PivotTable® utiliza HTTP o HTTPS)|Puerto TCP 80|Se utiliza para una conexión HTTP a través de una dirección URL.|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] configurado para el uso a través de IIS/HTTPS<br /><br /> (El Servicio PivotTable® utiliza HTTP o HTTPS)|Puerto TCP 443|Se utiliza para una conexión HTTPS a través de una dirección URL. HTTPS es una conexión HTTP que utiliza TLS.|  
  
 Si los usuarios tienen acceso a [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] a través de IIS e Internet, debe abrir el puerto en el que está escuchando IIS y especificar ese puerto en la cadena de conexión del cliente. En este caso, no se tiene que abrir ningún puerto para acceso directo a [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. El puerto predeterminado, 2389 y el puerto 2382 deben restringirse junto con los demás puertos que no sean necesarios.  
  
 Para obtener instrucciones detalladas sobre cómo configurar Firewall de Windows para [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], vea [Configurar Firewall de Windows para permitir el acceso a Analysis Services](/analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access).  
  
###  <a name="ports-used-by-reporting-services"></a><a name="BKMK_ssrs"></a> Puertos utilizados por Reporting Services  

De forma predeterminada, los puertos que SQL Server Reporting Services suele utilizar y los servicios asociados son los siguientes: TCP **80**, **443**. En la tabla siguiente se detallan dichos puertos con mayor profundidad. 


La tabla siguiente muestra los puertos de uso frecuente por parte de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
|Característica|Port|Comentarios|  
|-------------|----------|--------------|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Servicios Web|Puerto TCP 80|Se utiliza para una conexión HTTP a [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] a través de una dirección URL. Recomendamos no usar la regla preconfigurada **World Wide Web Services (HTTP)** . Para obtener más información, vea la sección [Interacción con otras reglas de firewall](#BKMK_other_rules) más adelante|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] configurado para el uso a través de HTTPS|Puerto TCP 443|Se utiliza para una conexión HTTPS a través de una dirección URL. HTTPS es una conexión HTTP que utiliza TLS. Recomendamos no usar la regla preconfigurada **Secure World Wide Web Services (HTTPS)** . Para obtener más información, vea la sección [Interacción con otras reglas de firewall](#BKMK_other_rules) más adelante|  
  
Cuando [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] se conecta a una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] o [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], también debe abrir los puertos adecuados para esos servicios. Para obtener instrucciones detalladas sobre cómo configurar Firewall de Windows para [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vea [Configurar un firewall para el acceso al servidor de informes](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md).  
  
###  <a name="ports-used-by-integration-services"></a><a name="BKMK_ssis"></a> Puertos utilizados por Integration Services  
 La tabla siguiente muestra los puertos de uso frecuente por parte del servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
|Característica|Port|Comentarios|  
|-------------|----------|--------------|  
|[!INCLUDE[msCoName](../../includes/msconame-md.md)] llamadas a procedimiento remoto (MS RPC)<br /><br /> Utilizado por el motor de tiempo de ejecución [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|Puerto TCP 135<br /><br /> Vea [Consideraciones especiales para el puerto 135](#BKMK_port_135)|El servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utiliza DCOM en el puerto 135. El Administrador de control de servicios usa el puerto 135 para realizar tareas tales como iniciar y detener el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , y transmitir solicitudes de control al servicio en funcionamiento. No se puede cambiar el número de puerto.<br /><br /> Solamente es necesario que este puerto esté abierto si se está conectando a una instancia remota del servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] desde [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] o desde una aplicación personalizada.|  
  
Para ver instrucciones paso a paso para configurar el Firewall de Windows para [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vea [Integration Services Service &#40;SSIS Service&#41;](/previous-versions/sql/sql-server-2012/ms137861(v=sql.110)) (Servicio de Integration Services &#40;Servicio SSIS&#41;).  
  
###  <a name="additional-ports-and-services"></a><a name="BKMK_additional_ports"></a> Puertos y servicios adicionales  
La tabla siguiente muestra los puertos y servicios de los que puede depender [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Escenario|Port|Comentarios|  
|--------------|----------|--------------|  
|Instrumental de administración de Windows<br /><br /> Para obtener más información acerca de WMI, vea [WMI Provider for Configuration Management Concepts](../../relational-databases/wmi-provider-configuration/wmi-provider-for-configuration-management.md).|WMI se ejecuta como parte de un host de servicio compartido con puertos asignados a través de DCOM. WMI podría estar utilizando el puerto TCP 135.<br /><br /> Vea [Consideraciones especiales para el puerto 135](#BKMK_port_135)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza WMI para enumerar y administrar servicios. Recomendamos usar el grupo de reglas preconfigurado **Instrumental de administración de Windows (WMI)** . Para obtener más información, vea la sección [Interacción con otras reglas de firewall](#BKMK_other_rules) más adelante|  
|[!INCLUDE[msCoName](../../includes/msconame-md.md)] Coordinador de transacciones distribuidas de (MS DTC)|Puerto TCP 135<br /><br /> Vea [Consideraciones especiales para el puerto 135](#BKMK_port_135)|Si la aplicación utiliza transacciones distribuidas, quizá deba configurar el firewall para permitir que el tráfico del Coordinador de transacciones distribuidas de [!INCLUDE[msCoName](../../includes/msconame-md.md)] (MS DTC) fluya entre instancias independientes de MS DTC y entre MS DTC y administradores de recursos como [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se recomienda usar el grupo de reglas preconfigurado **Coordinador de transacciones distribuidas** .<br /><br /> Cuando se configura un único MS DTC compartido para todo el clúster en un grupo de recursos independiente, se debería agregar sqlservr.exe como excepción al firewall.|  
|El botón Examinar en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] utiliza UDP para establecer conexión con el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser. Para obtener más información, vea [Servicio SQL Server Browser &#40;motor de base de datos y SSAS&#41;](../../database-engine/configure-windows/sql-server-browser-service-database-engine-and-ssas.md).|Puerto UDP 1434|UDP es un protocolo sin conexión.<br /><br /> El firewall tiene una configuración ([Propiedad UnicastResponsesToMulticastBroadcastDisabled de la interfaz INetFwProfile](/windows/win32/api/netfw/nf-netfw-inetfwprofile-get_unicastresponsestomulticastbroadcastdisabled)) que controla el comportamiento del firewall respecto a las respuestas de unidifusión a una solicitud UDP de difusión (o multidifusión).  Tiene dos comportamientos.<br /><br /> Si el valor es TRUE, no se permite en absoluto ninguna respuesta de unidifusión a una difusión. La enumeración de servicios producirá un error.<br /><br /> Si la configuración es FALSE (valor predeterminado), las respuestas de unidifusión se permiten durante 3 segundos. La longitud de tiempo no es configurable. En una red congestionada o de latencia alta, o en servidores muy cargados, los intentos de enumerar instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pueden devolver una lista parcial, que puede desorientar a los usuarios.|  
|<a name="BKMK_IPsec"></a> Tráfico IPSec|Puerto UDP 500 y puerto UDP 4500|Si la directiva de dominio requiere que las comunicaciones se realicen a través de IPSec, también debe agregar los puertos UDP 4500 y 500 a la lista de excepciones. IPSec es una opción que usa el **Asistente para nueva regla de entrada** en el complemento de Firewall de Windows. Para obtener más información, vea [Utilizar el complemento Firewall de Windows con seguridad avanzada](#BKMK_WF_msc) más adelante.|  
|Utilizar la autenticación de Windows con dominios de confianza|Los firewalls se deben configurar para permitir solicitudes de autenticación.|Para obtener más información, vea [Cómo configurar un firewall para dominios y confianza](https://support.microsoft.com/kb/179442/).|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y Agrupación en clústeres de Windows|La agrupación en clústeres requiere puertos adicionales que no se relacionan directamente con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|Para obtener más información, vea [Habilitar una red para el uso de clústeres](/previous-versions/windows/it-pro/windows-server-2003/cc728293(v=ws.10)).|  
|Espacios de nombres URL reservados en la API del Servidor HTTP (HTTP.SYS)|Probablemente el puerto TCP 80, pero se puede configurar en otros puertos. Para obtener información general, vea [Configurar HTTP y HTTPS](/dotnet/framework/wcf/feature-details/configuring-http-and-https).|Para obtener información específica de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sobre cómo reservar un punto de conexión HTTP.SYS con HttpCfg.exe, vea [Acerca de las reservas y el registro de reservas de URL &#40;Administrador de configuración de SSRS&#41;](../../reporting-services/install-windows/about-url-reservations-and-registration-ssrs-configuration-manager.md).|  
  
##  <a name="special-considerations-for-port-135"></a><a name="BKMK_port_135"></a> Consideraciones especiales para el puerto 135  
 Cuando utilice RPC con TCP/IP o con UDP/IP como transporte, los puertos entrantes suelen asignarse dinámicamente a los servicios del sistema cuando es necesario; se utilizan los puertos TCP/IP y UDP/IP mayores que el puerto 1024. Se suelen conocer informalmente como "puertos RPC aleatorios". En estos casos, los clientes RPC se apoyan en el mapeador de extremos RPC para indicar qué puertos dinámicos se asignaron al servidor. Para algunos servicios basados en RPC, puede configurar un puerto concreto en lugar de permitir que RPC asigne dinámicamente uno. También puede restringir el intervalo de puertos que RPC asigna dinámicamente a un intervalo pequeño, con independencia del servicio. Dado que el puerto 135 se utiliza para muchos servicios, los usuarios malintencionados lo atacan con frecuencia. Al abrir el puerto 135, considere restringir el ámbito de la regla de firewall.  
  
 Para obtener más información sobre el puerto 135, vea las siguientes referencias:  
  
-   [Introducción al servicio y requisitos del puerto de red para el sistema Windows Server](https://support.microsoft.com/kb/832017)   
-   [Cómo solucionar errores del mapeador de extremos de RPC](https://mskb.pkisolutions.com/kb/839880)  
-   [Llamada a procedimiento remoto (RPC)](/previous-versions/ms950395(v=msdn.10))    
-   [Configurar la asignación dinámica de puertos RPC para trabajar con firewalls](https://support.microsoft.com/kb/154596/)  
  
##  <a name="interaction-with-other-firewall-rules"></a><a name="BKMK_other_rules"></a> Interacción con otras reglas de firewall  
 El Firewall de Windows utiliza reglas y grupos de reglas para establecer su configuración. Cada regla o grupo de reglas suele estar asociado con un programa o servicio determinado, y ese programa o servicio podría modificar o eliminar esa regla sin su conocimiento. Por ejemplo, los grupos de reglas **World Wide Web Services (HTTP)** y **World Wide Web Services (HTTPS)** están asociados a IIS. Al habilitar esas reglas, se abrirán los puertos 80 y 443, y las características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que dependen de los puertos 80 y 443 funcionarán si esas reglas están habilitadas. Sin embargo, los administradores que configuran IIS podrían modificar o deshabilitar esas reglas. Por consiguiente, si está utilizando el puerto 80 o el puerto 443 para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], debe crear su propia regla o su propio grupo de reglas que mantenga la configuración de puerto que desee independientemente de las demás reglas IIS.  
  
 El complemento MMC del Firewall de Windows con seguridad avanzada permite cualquier tráfico que coincida con cualquier regla de permiso aplicable. Por lo tanto, si hay dos reglas que se apliquen al puerto 80 (con parámetros diferentes), se permitirá el tráfico que coincida con cualquiera de ellas. Así si una regla permite el tráfico sobre el puerto 80 de la subred local y otra permite el tráfico procedente de cualquier dirección, el efecto de la red será que se permita todo el tráfico dirigido al puerto 80, sin tener en cuenta su origen. Para administrar eficazmente el acceso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], los administradores deben revisar periódicamente las reglas de firewall habilitadas en el servidor.  
  
##  <a name="overview-of-firewall-profiles"></a><a name="BKMK_profiles"></a> Introducción a los perfiles de firewall  
 Los sistemas operativos utilizan los perfiles de firewall para identificar y recordar cada una de las redes a las que se conectan a efectos de conectividad, conexiones y categoría.  
  
 Hay tres tipos de ubicación de red en Firewall de Windows con seguridad avanzada:  
  
-   **Dominio**: Windows puede autenticar el acceso al controlador de dominio para el dominio al que está unido el equipo.
-   **Pública**: Excepto las redes de dominio, todas las redes se categorizan inicialmente como públicas. Las redes que representan conexiones directas a Internet o que están en ubicaciones públicas, tales como aeropuertos o cafeterías, deben dejarse como públicas.
-   **Privada**: Una red identificada por un usuario o una aplicación como privada. Solo las redes confiables se deben identificar como redes privadas. Es probable que los usuarios deseen identificar las redes domésticas o de pequeña empresa como privadas.  
  
 El administrador puede crear un perfil para cada tipo de ubicación de red, cada perfil conteniendo diferentes directivas de firewall. En cada momento se aplica solamente un perfil. El orden de perfile se aplica de la manera siguiente:  
  
1.  Si todas las interfaces se autentican para el controlador de dominio para el dominio del que es miembro el equipo, se aplica el perfil del dominio.    
2.  Si todas las interfaces se autentican para el controlador de dominio o están conectadas a redes clasificadas como ubicaciones de la red privada, se aplica el perfil privado.    
3.  De lo contrario, se aplica el perfil público.  
  
 Utilice el complemento MMD de Firewall de Windows con seguridad avanzada para ver y configurar todos los perfiles del firewall. El elemento **Firewall de Windows** del Panel de control solo configura el perfil actual.  
  
##  <a name="additional-firewall-settings-using-the-windows-firewall-item-in-control-panel"></a><a name="BKMK_additional_settings"></a> Configuración adicional de Firewall de Windows con el elemento Firewall de Windows del Panel de control  
 Las excepciones que agregue al firewall pueden restringir la apertura del puerto a las conexiones entrantes de equipos concretos o de la subred local. Esta restricción del ámbito de la apertura del puerto puede reducir la exposición del equipo a usuarios malintencionados, y está recomendada.  
  
> [!NOTE]  
>  El uso del elemento **Firewall de Windows** del Panel de control solamente configura el perfil del firewall actual.  
  
### <a name="to-change-the-scope-of-a-firewall-exception-using-the-windows-firewall-item-in-control-panel"></a>Para cambiar el ámbito de una excepción de firewall utilizando el elemento Firewall de Windows del Panel de control  
  
1.  En el elemento **Firewall de Windows** de Panel de control, seleccione un programa o puerto en la pestaña **Excepciones** y, a continuación, haga clic en **Propiedades** o **Editar**.  
  
2.  En el cuadro de diálogo **Modificar un programa** o **Modificar un puerto** , haga clic en **Cambiar ámbito**.  
  
3.  Elija una de las siguientes opciones:  
  
    -   **Cualquier equipo (incluyendo los que están en Internet)** : No se recomienda. Esto permitirá que cualquier equipo que pueda direccionar su equipo se conecte al programa o al puerto especificados. Esta configuración puede ser necesaria para permitir que se presente información a usuarios anónimos de Internet, pero aumenta la exposición a los usuarios malintencionados. La exposición puede aumentarse aún más si habilita esta configuración y también permite la exploración transversal de la Traducción de direcciones de red (NAT), como la opción Permitir cruce seguro del perímetro.  
  
    -   **Solo mi red (subred)** : Esta configuración de seguridad es más segura que **Cualquier equipo**. Solo los equipos de la subred local de la red pueden conectarse al programa o al puerto.  
  
    -   **Lista personalizada**: Solo los equipos cuyas direcciones IP estén en la lista se pueden conectar. Esta configuración puede ser más segura que **Sólo mi red (subred)** , pero los equipos cliente que usen DHCP pueden cambiar ocasionalmente su dirección IP. Entonces, el equipo deseado no podrá conectarse. Otro equipo, que no deseara autorizar, podría aceptar la dirección IP de la lista y, en consecuencia, podría conectarse La opción **Lista personalizada** puede ser adecuada para hacer una lista de otros servidores configurados para utilizar una dirección IP fija; no obstante, un intruso podría suplantar las direcciones IP. Las reglas restrictivas de firewall son tan fuertes como sea la infraestructura de red.  
  
##  <a name="using-the-windows-firewall-with-advanced-security-snap-in"></a><a name="BKMK_WF_msc"></a> Utilizar el complemento Firewall de Windows con seguridad avanzada  
 Se pueden configurar opciones de firewall avanzadas adicionales utilizando el complemento MMC del Firewall de Windows con seguridad avanzada. El complemento incluye un asistente de reglas y expone más valores de configuración que no están disponibles en el elemento **Firewall de Windows** en el Panel de control. Entre los ajustes se incluyen los siguientes:  
  
-   Configuración de cifrado  
-   Restricciones de servicios   
-   Restringir conexiones para equipos por nombre    
-   Restringir conexiones para usuarios o perfiles específicos  
-   Cruce de perímetro que permite que el tráfico evite los enrutadores de Traducción de direcciones de red (NAT)    
-   Configurar reglas salientes    
-   Configurar reglas de seguridad    
-   Requerir IPSec para conexiones entrantes  
  
### <a name="to-create-a-new-firewall-rule-using-the-new-rule-wizard"></a>Para crear una nueva regla de firewall mediante el asistente de Nueva regla  
  
1.  En el menú Inicio, elija **Ejecutar**, escriba **WF.msc** y seleccione **Aceptar**.    
2.  En **Firewall de Windows con seguridad avanzada**, en el panel izquierdo, haga clic con el botón derecho en **Reglas de entrada** y seleccione **Nueva regla**.   
3.  Complete el **Asistente para nueva regla de entrada** usando la configuración que desee.  
  
##  <a name="troubleshooting-firewall-settings"></a><a name="BKMK_troubleshooting"></a> Solucionar problemas de configuración del firewall  
 Las herramientas y técnicas siguientes pueden ser útiles para solucionar problemas del firewall:  
  
-   El estado efectivo del puerto es la unión de todas las reglas relacionadas con el puerto. Cuando intente bloquear el acceso a través de un puerto, puede que resulte útil revisar todas las reglas que citan el número de puerto. Para ello, utilice el complemento MMC del Firewall de Windows con seguridad avanzada y ordene las reglas entrantes y salientes por número de puerto.  
  
-   Revise los puertos activos en el equipo en el que se esté ejecutando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Este proceso de revisión incluye la comprobación de qué puertos TCP/IP están escuchando y también la comprobación del estado de los puertos.  
  
     Para comprobar qué puertos están escuchando, use la utilidad de línea de comandos **netstat** . Además de mostrar las conexiones TCP activas, la utilidad **netstat** también muestra diversa información y estadísticas de IP.  
  
    #### <a name="to-list-which-tcpip-ports-are-listening"></a>Para mostrar qué puertos TCP/IP están escuchando  
  
    1.  Abra la ventana de símbolo del sistema.  
  
    2.  En el símbolo del sistema, escriba **netstat -n -a**.  
  
         El modificador **-n** indica a **netstat** que muestre numéricamente la dirección y el número de puerto de las conexiones TCP activas. El modificador **-a** indica a **netstat** que muestre los puertos TCP y UDP en los que está escuchando el equipo.  
  
-   La utilidad **PortQry** se puede usar para notificar el estado de los puertos TCP/IP para indicar que están escuchando, no escuchando o filtrados. (Con un estado de filtrado, el puerto puede o no estar escuchando; este estado indica que la utilidad no ha recibido una respuesta del puerto.) La utilidad **PortQry** se puede descargar desde el [Centro de descargas de Microsoft](https://www.microsoft.com/download/details.aspx?id=17148).  
  
## <a name="see-also"></a>Consulte también  
 [Introducción al servicio y requisitos del puerto de red para el sistema Windows Server](https://support.microsoft.com/kb/832017)   
 [Cómo: Configurar los valores del firewall (Azure SQL Database)](/azure/azure-sql/database/firewall-configure)  
  
