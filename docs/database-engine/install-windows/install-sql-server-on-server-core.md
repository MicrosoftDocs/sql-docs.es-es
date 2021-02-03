---
title: Instalación de SQL Server en Server Core | Microsoft Docs
description: Puede instalar SQL Server en una instalación Server Core. La opción de instalación Server Core proporciona un entorno mínimo para ejecutar determinados roles de servidor.
ms.custom: ''
ms.date: 06/29/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 1dd294cc-5b69-4d0c-9005-3e307b75678b
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: f704712b5d2fe7adde99a6440db5a085163e5f31
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236407"
---
# <a name="install-sql-server-on-server-core"></a>Instalar SQL Server en Server Core

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Puede instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en una instalación Server Core.   
  
La opción de instalación Server Core proporciona un entorno mínimo para ejecutar determinados roles de servidor. Esto ayuda a reducir los requisitos de administración y mantenimiento y la superficie de ataque para esos roles de servidor. Para más información sobre Server Core, vea [Instalación de Server Core](/windows-server/get-started/getting-started-with-server-core). Para obtener más información sobre Server Core tal y como se implementa en [!INCLUDE[win8srv](../../includes/win8srv-md.md)], vea [Server Core para Windows Server 2012](https://msdn.microsoft.com/library/hh846323\(VS.85\).aspx) (https://msdn.microsoft.com/library/hh846323(VS.85).aspx).  
  
 Para obtener una lista de los sistemas operativos admitidos actualmente, vea [Requisitos de hardware y software para instalar SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).

## <a name="prerequisites"></a>Requisitos previos  
  
|Requisito|Cómo instalar|  
|-----------------|--------------------|  
|[!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 4.6.1 |En todas las ediciones de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] excepto [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)], el programa de instalación requiere el perfil [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 4.6.1 Server Core. El programa de instalación de SQL Server lo instalará automáticamente si no está ya instalado. La instalación requiere que reinicie el equipo. Puede instalar [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] antes de ejecutar el programa de instalación para no tener que reiniciar.|  
|Windows Installer 4.5|Incluido con la instalación Server Core.|  
|Windows PowerShell|Incluido con la instalación Server Core.|  
|Java Runtime |Para usar PolyBase, es necesario instalar el Java Runtime adecuado. Para más información, vea [Guía de PolyBase](../../relational-databases/polybase/polybase-installation.md).|
  
##  <a name="supported-features"></a><a name="BK_SupportedFeatures"></a> Características admitidas  
 Use la siguiente tabla para saber qué características se admiten en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] en una instalación Server Core.  
  
|Característica|Compatible|Información adicional|  
|-------------|---------------|----------------------------|  
|[!INCLUDE[ssDE](../../includes/ssde-md.md)] Servicios|Sí||  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Replicación|Sí||  
|Búsqueda de texto completo|Sí||  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|Sí||  
|[!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)]|Sí||  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|No||  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Data Tools (SSDT)|No||  
|Conectividad con las herramientas de cliente|Sí||  
|Servidor Integration Services|Sí||  
|Compatibilidad con las versiones anteriores de las herramientas de cliente|No||  
|SDK de las herramientas de cliente|No||  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Libros en pantalla|No||  
|Herramientas de administración: básicas|Solo remoto|No se admite la instalación de estas características en Server Core. Estos componentes se pueden instalar en otro servidor que no sea Server Core y conectarse a los servicios de [!INCLUDE[ssDE](../../includes/ssde-md.md)] instalados en Server Core.|  
|Herramientas de administración - Completa|Solo remoto|No se admite la instalación de estas características en Server Core. Estos componentes se pueden instalar en otro servidor que no sea Server Core y conectarse a los servicios de [!INCLUDE[ssDE](../../includes/ssde-md.md)] instalados en Server Core.|  
|Distributed Replay Controller|No||  
|Distributed Replay Client|Solo remoto|No se admite la instalación de estas características en Server Core. Estos componentes se pueden instalar en otro servidor que no sea Server Core y conectarse a los servicios de [!INCLUDE[ssDE](../../includes/ssde-md.md)] instalados en Server Core.|  
|SDK de conectividad de cliente SQL|Sí||  
|Microsoft Synchronization Framework|Sí|Microsoft Sync Framework no se incluye en el paquete de instalación de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] . Puede descargar la versión adecuada de Sync Framework desde esta página del [Centro de descarga de Microsoft](https://go.microsoft.com/fwlink/?LinkId=221788) (https://go.microsoft.com/fwlink/?LinkId=221788) e instalarla en un equipo que ejecute la instalación Server Core.|  
|[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]|No||  
|[!INCLUDE[ssDQSnoversion](../../includes/ssdqsnoversion-md.md)]|No||  
  
## <a name="supported-scenarios"></a>Escenarios admitidos  
 En la siguiente tabla se muestra la matriz de escenarios admitidos para instalar [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] en una instalación Server Core.  
  
| Instalación | Destino válido |  
|-|-|  
|Ediciones de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Todas las ediciones de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] de 64 bits |  
|Idioma de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Todos los idiomas|  
|Idioma de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el idioma o configuración regional (combinación) del sistema operativo|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en inglés en Windows en japonés<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en inglés en Windows en alemán<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en inglés en Windows en chino<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en inglés en Windows en árabe<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en inglés en Windows en tailandés<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en inglés en Windows en turco<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en inglés en Windows en portugués de Portugal<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en inglés en Windows en inglés|  
|Edición de Windows|Windows Server 2019 Datacenter <br/><br/> Windows Server 2019 Standard <br /><br />  [!INCLUDE[winserver2016_datacenter_md](../../includes/winserver2016-datacenter-md.md)]<br/><br/>[!INCLUDE[winserver2016_standard_md](../../includes/winserver2016-standard-md.md)]<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Datacenter<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Standard<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Essentials<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Foundation<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Datacenter<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Standard<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Essentials<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Foundation|  
  
## <a name="upgrade"></a>Actualizar 
 En las instalaciones básicas Server Core, se admite la actualización de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] .  
  
## <a name="install"></a>Instalar  
 [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] no admite la instalación con el asistente de instalación en el sistema operativo Server Core. Al realizar la instalación en Server Core, el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite el modo totalmente silencioso mediante el uso del parámetro /Q o el modo silencioso simple mediante el parámetro /QS. Para más información, consulte [Instalar SQL Server 2016 desde el símbolo del sistema](./install-sql-server-from-the-command-prompt.md).  
  
 Con independencia del método de instalación, es necesario confirmar la aceptación de los términos de la licencia de software como usuario individual o en nombre de una entidad, a menos que el uso del software en su caso se rija por un acuerdo independiente, como un acuerdo de licencia por volumen de [!INCLUDE[msCoName](../../includes/msconame-md.md)] o un acuerdo suscrito con un ISV u OEM.  
  
 Los términos de la licencia se muestran para revisarlos y aceptarlos en la interfaz de usuario del programa de instalación. Las instalaciones desatendidas (mediante los parámetros /Q o /QS) deben incluir el parámetro /IACCEPTSQLSERVERLICENSETERMS. Puede revisar separadamente los términos de licencia en [Términos de licencia de software de Microsoft](https://go.microsoft.com/fwlink/?LinkId=148209).  
  
> [!NOTE]  
>  En función de cómo haya recibido el software (por ejemplo, a través de un contrato de licencias por volumen de [!INCLUDE[msCoName](../../includes/msconame-md.md)] ), su uso del software puede estar sujeto a términos y condiciones adicionales.  
  
 Para instalar características específicas, use el parámetro /FEATURES y especifique la característica primaria o los valores de las características. Para obtener más información sobre los parámetros de las características y su uso, consulte las siguientes secciones.  
  
### <a name="feature-parameters"></a>Parámetros de características  
  
|Parámetro de característica|Descripción|  
|-----------------------|-----------------|  
|SQLENGINE|Instala solo el [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|REPLICACIÓN|Instala el componente Replicación junto con [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|FULLTEXT|Instala el componente Texto completo junto con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|AS|Instala todos los componentes de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .|  
|IS|Instala todos los componentes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|  
|CONN|Instala los componentes de conectividad.| 
|ADVANCEDANALYTICS |Instala R Services, requiere el motor de base de datos. Las instalaciones desatendidas requieren el parámetro /IACCEPTROPENLICENSETERMS.  |


 Vea los siguientes ejemplos de uso de los parámetros de la característica:  
  
|Parámetro y valores|Descripción|  
|--------------------------|-----------------|  
|/FEATURES=SQLEngine|Instala solo el [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|/FEATURES=SQLEngine,FullText|Instala [!INCLUDE[ssDE](../../includes/ssde-md.md)] y Texto completo.|  
|/FEATURES=SQLEngine,Conn|Instala los componentes de conectividad y [!INCLUDE[ssDE](../../includes/ssde-md.md)] .|  
|/FEATURES=SQLEngine,AS,IS,Conn|Instala el [!INCLUDE[ssDE](../../includes/ssde-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]y los componentes de conectividad.|  
|/FEATURES=SQLENGINE,ADVANCEDANALYTICS /IACCEPTROPENLICENSETERMS |Instala el  [!INCLUDE[ssDE](../../includes/ssde-md.md)] y [!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)].|  

  
### <a name="installation-options"></a>Opción de instalación  
 El programa de instalación admite las opciones de instalación siguientes al instalar [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] en un sistema operativo Server Core:  
  
1.  **Instalación desde la línea de comandos**  
  
     Para instalar características específicas mediante la opción de instalación del símbolo del sistema, use el parámetro /FEATURES y especifique la característica principal o los valores de la característica. El siguiente es un ejemplo del uso de los parámetros de la línea de comandos:  
  
    ```console
    Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,Replication /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="<StrongPassword>" /SQLSYSADMINACCOUNTS="<DomainName\UserName>" /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /TCPENABLED=1 /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
2.  **Instalación mediante el archivo de configuración**  
  
     El programa de instalación admite el uso del archivo de configuración solamente a través del símbolo del sistema. El archivo de configuración es un archivo de texto con la estructura básica de un parámetro (pares nombre-valor) y un comentario descriptivo. El archivo de configuración especificado en el símbolo del sistema debe tener una extensión de nombre de archivo .INI. Vea los ejemplos siguientes de ConfigurationFile.INI:  
  
    - Instalación de [!INCLUDE[ssDE](../../includes/ssde-md.md)]. 
    
    En el siguiente ejemplo se muestra cómo instalar una nueva instancia independiente que incluye el [!INCLUDE[ssDE](../../includes/ssde-md.md)] de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  

    ```console
    ; SQL Server Configuration File  
    [OPTIONS]  
    
    ; Specifies a Setup work flow, like INSTALL, UNINSTALL, or UPGRADE. This is a required parameter.   
    
    ACTION="Install"  
    
    ; Specifies features to install, uninstall, or upgrade. The lists of features include SQLEngine, FullText, Replication, AS, IS, and Conn.   
    
    FEATURES=SQLENGINE  
    
    ; Specify a default or named instance. MSSQLSERVER is the default instance for non-Express editions and SQLExpress for Express editions. This parameter is required when installing the ssNoVersion Database Engine, and Analysis Services (AS).  
    
    INSTANCENAME="MSSQLSERVER"  
    
    ; Specify the Instance ID for the ssNoVersion features you have specified. ssNoVersion directory structure, registry structure, and service names will incorporate the instance ID of the ssNoVersion instance.   
    
    INSTANCEID="MSSQLSERVER"  
    
    ; Account for ssNoVersion service: Domain\User or system account.   
    
    SQLSVCACCOUNT="NT Service\MSSQLSERVER"  
    
    ; Windows account(s) to provision as ssNoVersion system administrators.   
    
    SQLSYSADMINACCOUNTS="\<DomainName\UserName>"  
    
    ; Accept the License agreement to continue with Installation  
    
    IAcceptSQLServerLicenseTerms="True"  
    
    ```

    -   Instalación de los componentes de conectividad. En el siguiente ejemplo se muestra cómo instalar los componentes de conectividad:  
  
        ```console
        ; SQL Server Configuration File  
        [OPTIONS]  
  
        ; Specifies a Setup work flow, like INSTALL, UNINSTALL, or UPGRADE. This is a required parameter.   
  
        ACTION="Install"  
  
        ; Specifies features to install, uninstall, or upgrade. The lists of features include SQLEngine, FullText, Replication, AS, IS, and Conn.   
  
        FEATURES=Conn  
  
        ; Specifies acceptance of License Terms  
  
        IAcceptSQLServerLicenseTerms="True  
  
        ```  
  
    -   Instalar todas las características compatibles  
  
        En el siguiente ejemplo se muestra cómo instalar todas las características admitidas de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] en Server Core:  
  
        ```console
        ; SQL Server Configuration File  
        [OPTIONS]  
        ; Specifies a Setup work flow, like INSTALL, UNINSTALL, or UPGRADE. This is a required parameter.   
  
        ACTION="Install"  
  
        ; Specifies features to install, uninstall, or upgrade. The lists of features include SQLEngine, FullText, Replication, AS, IS, and Conn.   
  
        FEATURES=SQLENGINE,FullText,Replication,AS,IS,Conn  
  
        ; Specify a default or named instance. MSSQLSERVER is the default instance for non-Express editions and SQLExpress for Express editions. This parameter is required when installing the ssNoVersion Database Engine (SQL), or Analysis Services (AS).  
  
        INSTANCENAME="MSSQLSERVER"  
  
        ; Specify the Instance ID for the ssNoVersion features you have specified. ssNoVersion directory structure, registry structure, and service names will incorporate the instance ID of the ssNoVersion instance.   
  
        INSTANCEID="MSSQLSERVER"  
  
        ; Account for ssNoVersion service: Domain\User or system account.   
  
        SQLSVCACCOUNT="NT Service\MSSQLSERVER"  
  
        ; Windows account(s) to provision as ssNoVersion system administrators.   
  
        SQLSYSADMINACCOUNTS="\<DomainName\UserName>"  
  
        ; The name of the account that the Analysis Services service runs under.   
  
        ASSVCACCOUNT= "NT Service\MSSQLServerOLAPService"  
  
        ; Specifies the list of administrator accounts that need to be provisioned.   
  
        ASSYSADMINACCOUNTS="\<DomainName\UserName>"  
  
        ; Specifies the server mode of the Analysis Services instance. Valid values are MULTIDIMENSIONAL, POWERPIVOT or TABULAR. ASSERVERMODE is case-sensitive. All values must be expressed in upper case.   
  
        ASSERVERMODE="MULTIDIMENSIONAL"  
  
        ; Optional value, which specifies the state of the TCP protocol for the ssNoVersion service. Supported values are: 0 to disable the TCP protocol, and 1 to enable the TCP protocol.  
  
        TCPENABLED=1  
  
        ;Specifies acceptance of License Terms  
  
        IAcceptSQLServerLicenseTerms="True"  
        ```  
  
     A continuación se muestra cómo iniciar la instalación mediante un archivo de configuración personalizado o predeterminado:  
  
    -   Iniciar la instalación mediante un archivo de configuración personalizado:  
  
         Para especificar el archivo de configuración en el símbolo del sistema:  
  
        ```console
        Setup.exe /QS /ConfigurationFile=MyConfigurationFile.INI  
        ```  
  
         Para especificar las contraseñas en el símbolo del sistema en lugar de hacerlo en el archivo de configuración:  
  
        ```console
        Setup.exe /QS /SQLSVCPASSWORD="************" /ASSVCPASSWORD="************"  /ConfigurationFile=MyConfigurationFile.INI  
        ```  
  
    -   Iniciar la instalación mediante DefaultSetup.ini:  
  
         Si tiene el archivo DefaultSetup.ini en las carpetas \x86 y \x64 en el nivel raíz del medio de origen de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , abra el archivo DefaultSetup.ini y agregue el parámetro *Features* al archivo.  
  
         Si el archivo DefaultSetup.ini no existe, puede crearlo y copiarlo a las carpetas \x86 y \x64 en el nivel raíz de la ubicación de origen de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="configure-remote-access-of-ssnoversion-on-server-core"></a>Configurar el acceso remoto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en Server Core  
 Lleve a cabo las acciones descritas a continuación para configurar el acceso remoto de una instancia de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] que se está ejecutando en Server Core.  
  
### <a name="enable-remote-connections-on-the-instance-of-ssnoversion"></a>Habilitar las conexiones remotas en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  

Para habilitar las conexiones remotas, use SQLCMD.exe localmente y ejecute las instrucciones siguientes en la instancia de Server Core:  

```sql
EXEC sys.sp_configure N'remote access', N'1'  
GO
RECONFIGURE WITH OVERRIDE
GO
```  
  
### <a name="enable-and-start-the-ssnoversion-browser-service"></a>Habilitar e iniciar el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] browser service  
 De forma predeterminada, el servicio Explorer se deshabilita.  Si se deshabilita en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se ejecuta en Server Core, ejecute el siguiente comando desde el símbolo del sistema para habilitarlo:  
  
 `sc config SQLBROWSER start= auto`  
  
 Una vez habilitado, ejecute el siguiente comando desde el símbolo del sistema para iniciar el servicio:  
  
 `net start SQLBROWSER`  
  
### <a name="create-exceptions-in-windows-firewall"></a>Crear excepciones en Firewall de Windows  
 Para crear excepciones para el acceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en Firewall de Windows, siga los pasos especificados en [Configurar Firewall de Windows para permitir el acceso a SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
### <a name="enable-tcpip-on-the-instance-of-ssnoversion"></a>Habilitar TCP/IP en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 El protocolo TCP/IP puede habilitarse a través de Windows PowerShell para una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en Server Core. Siga estos pasos:  
  
1.  En PowerShell: Import-Module SQLPS.  
  
2.  En la ventana **Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Powershell**, ejecute el siguiente script para habilitar el protocolo TCP/IP:  
  
```powershell  
$smo = 'Microsoft.SqlServer.Management.Smo.'  
$wmi = new-object ($smo + 'Wmi.ManagedComputer')  
# Enable the TCP protocol on the default instance.  If the instance is named, replace MSSQLSERVER with the instance name in the following line.  
$uri = "ManagedComputer[@Name='" + (get-item env:\computername).Value + "']/ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Tcp']"  
$Tcp = $wmi.GetSmoObject($uri)  
$Tcp.IsEnabled = $true  
$Tcp.Alter()  
$Tcp  
```  
  
## <a name="uninstall"></a>Desinstalación

 Después de iniciar sesión en un equipo que ejecuta Server Core, tiene un entorno de escritorio limitado con un símbolo del sistema del administrador. Puede usar este símbolo del sistema para iniciar la desinstalación de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]. Para desinstalar una instancia de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)], inicie la desinstalación desde el símbolo del sistema el modo completamente silencioso utilizando el parámetro /Q o en modo silencioso simple utilizando el parámetro /QS. El parámetro /QS muestra el progreso a través de la interfaz de usuario, pero no acepta ninguna entrada. /Q se ejecuta en modo silencioso sin ninguna interfaz de usuario.  
  
 Para desinstalar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]existente.  
  
```console
Setup.exe /Q /Action=Uninstall /FEATURES=SQLEngine,AS,IS /INSTANCENAME=MSSQLSERVER  
```  
  
 Para quitar una instancia con nombre, especifique el nombre de la instancia en lugar de `MSSQLSERVER` en el ejemplo anterior.  
  
## <a name="start-a-new-command-prompt"></a>Iniciar un nuevo símbolo del sistema

Si cierra el símbolo del sistema accidentalmente, puede iniciar un nuevo símbolo del sistema siguiendo estos pasos:  
 
1.  Presione Ctrl+Mayús+Esc para mostrar el Administrador de tareas.  
2.  En la pestaña **Aplicaciones** , haga clic en **Nueva tarea**.  
3.  En el cuadro de diálogo **Crear nueva tarea** , escriba **cmd** en el campo **Abrir** y haga clic en [!INCLUDE[clickOK](../../includes/clickok-md.md)].  
  
## <a name="see-also"></a>Consulte también  
 [Instalar SQL Server mediante un archivo de configuración](./install-sql-server-using-a-configuration-file.md)   
 [Instalar SQL Server desde el símbolo del sistema](./install-sql-server-from-the-command-prompt.md)   
 [Ediciones y características admitidas de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md)   
 [Instalación de Server Core](/windows-server/get-started/getting-started-with-server-core)   
 [Configurar una instalación de Server Core de Windows Server 2016 con Sconfig.cmd](/windows-server/get-started/sconfig-on-ws2016)   
 [Cmdlets de clúster de conmutación por error en Windows PowerShell](/powershell/module/failoverclusters/)

  
