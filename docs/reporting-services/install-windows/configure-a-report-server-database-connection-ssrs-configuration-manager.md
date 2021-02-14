---
title: Configuración de una conexión a la base de datos del servidor de informes (Administrador de configuración) | Microsoft Docs
description: Cada instancia del servidor de informes requiere una conexión a la base de datos del servidor de informes que almacena informes, orígenes de datos compartidos, recursos y metadatos administrados por el servidor.
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.custom: seo-lt-2019, seo-mmd-2019
ms.date: 01/04/2020
ms.openlocfilehash: fdbf12310fb46add3ce9e3240d7c249028eda606
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100057300"
---
# <a name="configure-a-report-server-database-connection-report-server-configuration-manager"></a>Configurar una conexión de base de datos del servidor de informes (Administrador de configuración del servidor de informes)

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Cada instancia del servidor de informes requiere una conexión a la base de datos del servidor de informes que almacena informes, orígenes de datos compartidos, recursos y metadatos administrados por el servidor. La conexión inicial se puede crear durante la instalación de un servidor de informes si va a instalar la configuración predeterminada. En la mayoría de los casos, también puede utilizar la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para configurar la conexión una vez completada la instalación. Puede modificar la conexión en cualquier momento para cambiar el tipo de cuenta o restablecer las credenciales. Para obtener instrucciones paso a paso sobre cómo crear la base de datos y configurar la conexión, vea [Crear una base de datos del servidor de informes en modo nativo &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).

Debe configurar una conexión a la base de datos del servidor de informes en los siguientes casos:  

- Configurar un servidor de informes para usar por primera vez.  

- Configurar un servidor de informes para que utilice una base de datos de servidor de informes diferente.  

- Cambiar la cuenta o contraseña de usuario que se utiliza para la conexión a la base de datos. Solo tiene que actualizar la conexión a la base de datos cuando la información de la cuenta esté almacenada en el archivo RSReportServer.config. Si utiliza la cuenta de servicio para la conexión (que utiliza la seguridad integrada de Windows como tipo de credenciales), la contraseña no se almacena, por lo que no es necesario actualizar la información de conexión. Para más información sobre cómo cambiar cuentas, vea [Configurar la cuenta de servicio del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).  

- Configurar una implementación escalada de un servidor de informes. Configurar una implementación de ampliación requiere crear varias conexiones a una base de datos del servidor de informes. Para más información sobre cómo llevar a cabo esta operación compuesta de varios pasos, vea [Configurar una implementación escalada horizontalmente del servidor de informes en modo nativo &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md).  

## <a name="how-reporting-services-connects-to-the-database-engine"></a>Cómo se conecta Reporting Services al motor de base de datos

El acceso del servidor de informes a la base de datos de un servidor de informes depende de las credenciales y de la información de conexión, así como de las claves de cifrado que son válidas para la instancia del servidor de informes que utiliza esa base de datos. Es necesario tener claves de cifrado válidas para almacenar y recuperar datos confidenciales. Las claves de cifrado se crean automáticamente al configurar la base de datos por primera vez. Una vez creadas las claves, debe actualizarlas si cambia la identidad del servicio Servidor de informes. Para más información sobre cómo trabajar con las claves de cifrado, vea [Configurar y administrar las claves de cifrado &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md).  

La base de datos del servidor de informes es un componente interno, al que solo tiene acceso el servidor de informes. El servidor de informes utiliza exclusivamente las credenciales y la información de conexión que se especifique para la base de datos del servidor de informes. Los usuarios que solicitan los informes no requieren permisos de bases de datos o un inicio de sesión de base de datos para la base de datos del servidor de informes.  

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] usa **System.Data.SqlClient** para conectarse al [!INCLUDE[ssDE](../../includes/ssde-md.md)] que hospeda la base de datos del servidor de informes. Si usa una instancia local de [!INCLUDE[ssDE](../../includes/ssde-md.md)], el servidor de informes establecerá la conexión utilizando la memoria compartida. Si usa un servidor de bases de datos remoto para la base de datos del servidor de informes, es posible que tenga que habilitar las conexiones remotas según la edición que utilice. Si está usando la edición Enterprise Edition, las conexiones remotas están habilitadas para TCP/IP de forma predeterminada.  

Para comprobar que la instancia acepta conexiones remotas, haga clic sucesivamente en **Inicio**, **Todos los programas**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Herramientas de configuración**, **Administrador de configuración de SQL Server** y, luego, compruebe que el protocolo TCP/IP está habilitado para cada servicio.  

Al habilitar las conexiones remotas, los protocolos de servidor y de cliente también se habilitarán. Para comprobar que los protocolos están habilitados, haga clic sucesivamente en **Inicio**, **Todos los programas**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Herramientas de configuración**, **Administrador de configuración de SQL Server**, **Configuración de red de SQL Server** y, por último, haga clic en **Protocolos de MSSQLSERVER**. Para más información, vea [Habilitar o deshabilitar un protocolo de red de servidor](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md) en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

## <a name="defining-a-report-server-database-connection"></a>Definir una conexión a la base de datos del servidor de informes

Para configurar la conexión, debe utilizar la herramienta Administrador de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o la utilidad de línea de comandos **rsconfig** . Un servidor de informes requiere la siguiente información de conexión:  

- Nombre de la instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] que hospeda la base de datos del servidor de informes.  

- Nombre de la base de datos del servidor de informes. Cuando se crea una conexión por primera vez, puede crear una base de datos del servidor de informes nueva o seleccionar una existente. Para más información, vea [Crear una base de datos del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md).  

- Tipo de credencial. Puede utilizar cuentas de servicio, una cuenta de dominio de Windows o un inicio de sesión de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

- Nombre de usuario y contraseña (solo son necesarios si utiliza una cuenta de dominio de Windows o un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ).

Las credenciales que proporcione deben disponer de acceso a la base de datos del servidor de informes. Si utiliza la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , este paso se realiza automáticamente. Para obtener información acerca de los permisos que necesita para tener acceso a la base de datos, vea la sección "Permisos para la base de datos" en este tema.  

### <a name="storing-database-connection-information"></a>Almacenar información de conexión a la base de datos

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] almacena y cifra la información de conexión en los siguientes valores del archivo RSreportserver.config. Debe utilizar la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o la utilidad rsconfig para crear valores cifrados para esta configuración.  

No todos los valores se establecen para todos los tipos de conexión. Si configura la conexión usando los valores predeterminados (es decir, usando las cuentas de servicio para realizar la conexión), \<**LogonUser**>, \<**LogonDomain**> y \<**LogonCred**> estarán en blanco, como se muestra aquí:  

```
<Dsn></Dsn>  
<ConnectionType></ConnectionType>  
<LogonUser></LogonUser>  
<LogonDomain></LogonDomain>  
<LogonCred></LogonCred>  
```

Si configura la conexión para utilizar una cuenta de Windows o un inicio de sesión de base de datos específicos, debe acordarse de actualizar los valores almacenados si posteriormente cambia la cuenta o el inicio de sesión.  

### <a name="choosing-a-credential-type"></a>Elegir un tipo de credenciales

Hay tres tipos de credenciales que se pueden utilizar en una conexión a la base de datos del servidor de informes:  

- La seguridad integrada de Windows con la cuenta de servicio Servidor de informes. Dado que el servidor de informes se implementa como un servicio único, solo la cuenta bajo la que el servicio se ejecuta requiere el acceso a bases de datos.  
  
- Cuenta de usuario de Windows. Si el servidor de informes y su base de datos están instalados en el mismo equipo, puede utilizar una cuenta local. En caso contrario, debe especificar una cuenta de dominio.  
  
- Inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
  
> [!NOTE]
> No se puede utilizar una extensión de autenticación personalizada para conectarse a la base de datos de un servidor de informes. Las extensiones de autenticación personalizadas se utilizan únicamente para autenticar una entidad de seguridad en un servidor de informes. No afectan a las conexiones con la base de datos del servidor de informes o con orígenes de datos externos que proporcionan contenido a los informes.
  
Si la instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] se configura para la autenticación de Windows y está en el mismo dominio o en un dominio de confianza con el equipo del servidor de informes, puede configurar la conexión para utilizar la cuenta de servicio o una cuenta de usuario de dominio que administre como una propiedad de conexión a través de la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Si el servidor de bases de datos está en un dominio diferente o si utiliza la seguridad del grupo de trabajo, debe configurar la conexión para utilizar un inicio de sesión de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . En este caso, asegúrese de cifrar la conexión.  

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> Al usar Instancia administrada de Azure SQL Database para hospedar las bases de datos del servidor de informes, la autenticación de SQL Server es el único tipo de credencial compatible. Además, tenga en cuenta que Instancia administrada no puede hospedar la instancia del servidor de informes.

::: moniker-end

#### <a name="using-service-accounts-and-integrated-security"></a>Usar cuentas de servicio y seguridad integrada

Puede utilizar la seguridad integrada de Windows para conectarse a través de la cuenta del servicio Servidor de informes. A la cuenta se le conceden derechos de inicio de sesión en la base de datos del servidor de informes. Éste es el tipo de credenciales predeterminado que elige el programa de instalación si instala [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en la configuración predeterminada.  

La cuenta de servicio es una cuenta de confianza que proporciona un modo de administrar la conexión a una base de datos del servidor de informes que requiere poco mantenimiento. Como la cuenta de servicio utiliza la seguridad integrada de Windows para establecer la conexión, no es necesario que se almacenen las credenciales. Sin embargo, si posteriormente cambia la contraseña de la cuenta de servicio o identidad (por ejemplo, pasando de una cuenta integrada a una cuenta de dominio), asegúrese de utilizar la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para realizar el cambio. La herramienta actualiza automáticamente los permisos de base de datos para utilizar la información de la cuenta revisada. Para más información, vea [Configurar la cuenta de servicio del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).  

Si configura la conexión de base de datos para usar la cuenta de servicio, la cuenta deberá contar con permisos de red si la base de datos del servidor de informes se encuentra en un equipo remoto. No utilice la cuenta de servicio si la base de datos del servidor de informes se encuentra en un dominio distinto, detrás de un firewall o si está utilizando seguridad de grupo de trabajo en lugar de seguridad de dominio. Utilice en su lugar una cuenta de usuario de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

#### <a name="using-a-domain-user-account"></a>Usar una cuenta de usuario de dominio

Puede especificar una cuenta de usuario de Windows para la conexión del servidor de informes a la base de datos del servidor de informes. Si utiliza una cuenta local o de dominio, puede actualizar la conexión a la base de datos del servidor de informes cada vez que cambie la contraseña o la cuenta. Utilice siempre la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para actualizar la conexión.  

#### <a name="using-a-sql-server-login"></a>Usar un inicio de sesión de SQL Server

Puede especificar un solo inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para conectarse a la base de datos del servidor de informes. Si usa la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y la base de datos del servidor de informes se encuentra en un equipo remoto, utilice IPSEC para contribuir a proteger la transmisión de datos entre los servidores. Si utiliza un inicio de sesión de base de datos, debe actualizar la conexión a la base de datos del servidor de informes cada vez que cambie la contraseña o la cuenta.

### <a name="database-permissions"></a>Permisos para la base de datos

A las cuentas utilizadas para conectarse a la base de datos del servidor de informes se les conceden los siguientes roles:  

- Roles **public** y **RSExecRole** para la base de datos **ReportServer** .  

- Rol **RSExecRole** para las bases de datos **master**, **msdb** y **ReportServerTempDB** .  

Cuando utiliza la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] con el fin de crear o modificar la conexión, estos permisos se conceden automáticamente. Si usa la utilidad rsconfig y especifica una cuenta diferente para la conexión, debe actualizar el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para esa nueva cuenta. Puede crear archivos de scripts con la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] que actualicen el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para el servidor de informes.  

### <a name="verifying-the-database-name"></a>Comprobar el nombre de la base de datos

Utilice la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para saber qué base de datos del servidor de informes utiliza una instancia concreta del servidor de informes. Para buscar el nombre, conéctese a la instancia del servidor de informes y abra la página Instalación de base de datos.  

## <a name="using-a-different-report-server-database-or-moving-a-report-server-database"></a>Usar una base de datos de servidor de informes diferente o mover una base de datos de servidor de informes

Puede configurar una instancia del servidor de informes para que utilice una base de datos de servidor de informes diferente cambiando la información de conexión. Una situación común para intercambiar bases de datos es cuando se implementa un servidor de informes de producción. Cambiar de una base de datos de servidor de informes de prueba a otra de producción es como se suelen implementar los servidores de producción. También puede mover una base de datos del servidor de informes a otro equipo. Para más información, vea [Actualizar y migrar Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md) en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

## <a name="configuring-multiple-reports-servers-to-use-the-same-report-server-database"></a>Configurar varios servidores de informes para que utilicen la misma base de datos del servidor de informes

Puede configurar varios servidores de informes para que utilicen la misma base de datos de servidor de informes. Esta configuración de implementación se denomina implementación escalada. Dicha configuración es necesaria si se desea ejecutar varios servidores de informes en un clúster de servidores. Sin embargo, también se puede utilizar esta configuración si desea segmentar las aplicaciones de servicio o probar la instalación y configuración de una instancia nueva del servidor de informes con el fin de compararla con un servidor de informes existente. Para más información, vea [Configurar una implementación escalada horizontalmente del servidor de informes en modo nativo &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md).  

## <a name="next-steps"></a>Pasos siguientes

- [Crear una base de datos del servidor de informes](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)  
- [Administración de un servidor de informes en modo nativo de Reporting Services](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)   
- [Configurar la cuenta del servicio del servidor de informes](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
