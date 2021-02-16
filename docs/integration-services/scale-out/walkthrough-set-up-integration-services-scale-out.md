---
title: 'Tutorial: Configuración de la escalabilidad horizontal de SSIS | Microsoft Docs'
description: Obtenga información sobre la instalación y configuración del Servicio principal de Escalabilidad horizontal de SQL Server Integration Services (SSIS).
ms.custom: performance
ms.date: 12/13/2017
ms.prod: sql
ms.prod_service: integration-services
ms.technology: integration-services
ms.topic: conceptual
author: HaoQian-MS
ms.author: haoqian
ms.reviewer: maghan
ms.openlocfilehash: d25471ef16b88ee0280ecb7dd349762b72e35312
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337532"
---
# <a name="walkthrough-set-up-integration-services-ssis-scale-out"></a>Tutorial: Configuración de Escalabilidad horizontal de Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Para configurar la escalabilidad horizontal de [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)] (SSIS), lleve a cabo las siguientes tareas. 

> [!TIP]
> Si va a instalar la escalabilidad horizontal en un único equipo, instale las características de patrón y trabajador de escalabilidad horizontal al mismo tiempo. Al hacerlo, el punto de conexión se genera automáticamente para conectarse a patrón de escalado horizontal. 

* [Instalar el patrón de escalado horizontal](#InstallMaster)

* [Instalar el trabajador de escalado horizontal](#InstallWorker)

* [Instalar el certificado de cliente del trabajador de escalado horizontal](#InstallCert)

* [Abrir el puerto del firewall](#Firewall)

* [Iniciar el servicio de patrón y trabajador de escalado horizontal de SQL Server](#Start)

* [Habilitar el patrón de escalado horizontal](#EnableMaster)

* [Habilitación del modo de autenticación de SQL Server](#EnableAuth)

* [Habilitar el trabajador de escalado horizontal](#EnableWorker)

## <a name="install-scale-out-master"></a><a name="InstallMaster"></a> Instalar el patrón de escalado horizontal

Para configurar el patrón de escalabilidad horizontal, deberá instalar los servicios de motor de base de datos, [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)], y la característica de patrón de escalabilidad horizontal de SSIS al configurar [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]. 

Para obtener más información sobre cómo configurar el motor de base de datos y [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)], vea [Instalar el motor de base de datos de SQL Server](../../database-engine/install-windows/install-sql-server-database-engine.md) e [Instalar Integration Services](../install-windows/install-integration-services.md).

> [!NOTE]
> Para usar la cuenta de autenticación de SQL Server predeterminada para el registro de escalabilidad horizontal, seleccione Modo mixto para el modo de autenticación en la página **Configuración del motor de base de datos** durante su instalación. Consulte [Change the account for Scale Out logging](change-logdb-account.md) (Cambio de cuenta para el registro de la escalabilidad horizontal) para obtener más información.

Para instalar la característica de patrón de escalabilidad horizontal, use el Asistente para la instalación de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] o el símbolo del sistema.

### <a name="install-scale-out-master-with-the-sql-server-installation-wizard"></a>Instalar el patrón de escalabilidad horizontal con el Asistente para la instalación de SQL Server
1.  En la página **Selección de características**, seleccione **Patrón de escalabilidad horizontal**, que aparece en [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].   
  
    ![Selección de características: patrón](media/feature-select-master.PNG)
  
2.  En la página **Configuración del servidor** , seleccione la cuenta para ejecutar el servicio **Patrón de escalado horizontal de SQL Server Integration Services** y, después, seleccione el **tipo de inicio**.  
    ![Configuración del servidor](media/server-config.PNG)

3.  En la página **Configuración del patrón de escalado horizontal de Integration Services** , especifique el número de puerto que usa el patrón de escalado horizontal para comunicarse con el trabajador de escalado horizontal. El número de puerto predeterminado es 8391.  

    ![Configuración principal](media/master-config.PNG "Configuración principal")

4.  Especifique el certificado TLS/SSL usado para proteger la comunicación entre el patrón y el trabajo de escalabilidad horizontal; para ello, siga uno de los siguientes pasos.
    * Deje que el proceso de instalación cree un certificado TLS/SSL autofirmado predeterminado; para ello, haga clic en **Crear un nuevo certificado SSL**.  El certificado predeterminado se instala en Entidades de certificación raíz de confianza, Equipo local. Puede especificar los CN en este certificado. El nombre de host del punto de conexión principal debe incluirse en los CN. De forma predeterminada, se incluyen el nombre de la máquina y la dirección IP del nodo principal.
    * Para seleccionar un certificado TLS/SSL existente del equipo local, haga clic en **Usar un certificado SSL existente** y en **Examinar**. La huella digital del certificado aparece en el cuadro de texto. Haga clic en **Examinar** para que se muestren los certificados almacenados en Entidades de certificación raíz de confianza, Equipo local. El certificado que vaya a seleccionar debe almacenarse aquí.       

    ![Configuración principal 2](media/master-config-2.PNG "Configuración principal 2")
  
5.  Finalice el Asistente para la instalación de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] .

### <a name="install-scale-out-master-from-the-command-prompt"></a>Instalar el patrón de escalabilidad horizontal desde el símbolo del sistema

Siga las instrucciones descritas en [Instalar SQL Server desde el símbolo del sistema](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md). Configure los parámetros del patrón de escalabilidad horizontal siguiendo estos pasos:
 
1.  Agregar `IS_Master` al parámetro `/FEATURES`

2.  Configure el patrón de escalabilidad horizontal especificando los siguientes parámetros y sus valores:
    -   `/ISMASTERSVCACCOUNT`
    -   `/ISMASTERSVCPASSWORD`
    -   `/ISMASTERSVCSTARTUPTYPE`
    -   `/ISMASTERSVCPORT`
    -   `/ISMasterSVCSSLCertCN` (opcional)
    -   `/ISMASTERSVCTHUMBPRINT` (opcional)

    > [!NOTE]
    > Si el patrón de escalabilidad horizontal no está instalado junto con el motor de base de datos y este motor es una instancia con nombre, deberá configurar `SqlServerName` en el archivo de configuración de servicio del patrón de escalabilidad horizontal después de la instalación. Para obtener más información, vea [Patrón de escalabilidad horizontal](integration-services-ssis-scale-out-master.md).

## <a name="install-scale-out-worker"></a><a name="InstallWorker"></a> Instalar el trabajador de escalado horizontal
 
Para configurar el trabajador de escalabilidad horizontal, deberá instalar [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)] y la característica de trabajador de escalabilidad horizontal en la configuración de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].

Para instalar la característica de trabajador de escalado horizontal, use el Asistente para la instalación de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] o el símbolo del sistema.

### <a name="install-scale-out-worker-with-the-sql-server-installation-wizard"></a>Instalar el trabajador de escalabilidad horizontal con el Asistente para la instalación de SQL Server

1.  En la página **Selección de características**, seleccione **Trabajo de escalabilidad horizontal**, que aparece en [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].

    ![Selección de características: trabajador](media/feature-select-worker.PNG)

2.  En la página **Configuración del servidor** , seleccione la cuenta para ejecutar el servicio **Trabajador de escalado horizontal de SQL Server Integration Services** y, después, seleccione el **tipo de inicio**.

    ![Configuración del servidor 2](media/server-config-2.PNG "Configuración del servidor 2")

3.  En la página **Configuración del trabajador de escalado horizontal de Integration Services** , especifique el punto de conexión para conectarse al patrón de escalado horizontal. 

    - Para un entorno de **un único equipo**, el punto de conexión se genera automáticamente al instalar simultáneamente el patrón y el trabajador de escalabilidad horizontal. 

    - Para un entorno de **varios equipos**, el punto de conexión está formado por el nombre o la dirección IP del equipo que tiene instalado el patrón de escalabilidad horizontal y el número de puerto especificado durante la instalación del patrón de escalabilidad horizontal.
   
    ![Configuración del trabajador 1](media/worker-config.PNG "Configuración del trabajador 1")    

    > [!NOTE]
    > También puede omitir la configuración del trabajador en este momento y asociar el trabajador de escalabilidad horizontal al patrón de escalabilidad horizontal usando [Scale Out Manager](integration-services-ssis-scale-out-manager.md) tras la instalación.

4. Para un entorno de **varios equipos**, especifique el certificado TLS/SSL de cliente que se usará para validar el patrón de escalabilidad horizontal. Para un entorno de **un único equipo**, no tendrá que especificar un certificado de cliente TLS/SSL. 
  
    Haga clic en **Examinar** para buscar el archivo de certificado (*.cer). Para usar el certificado de TLS/SSL predeterminado, seleccione el archivo `SSISScaleOutMaster.cer`, que se encuentra en `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn` en el equipo que contiene el patrón de escalabilidad horizontal.   

    ![Configuración del trabajador 2](media/worker-config-2.PNG "Configuración del trabajador 2")

    > [!NOTE]
    > Si el certificado TLS/SSL que usa el patrón de escalabilidad horizontal está autofirmado, deberá haber un certificado TLS/SSL de cliente correspondiente instalado en el equipo que contiene el trabajo de escalabilidad horizontal. Si proporciona la ruta de acceso del archivo del certificado TLS/SSL de cliente en la página **Configuración del trabajo de escalabilidad horizontal de Integration Services**, se instalará automáticamente; en caso contrario, tendrá que instalar el certificado manualmente más adelante. 
     
5. Finalice el Asistente para la instalación de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] .

### <a name="install-scale-out-worker-from-the-command-prompt"></a>Instalar el trabajador de escalabilidad horizontal desde el símbolo del sistema

Siga las instrucciones descritas en [Instalar SQL Server desde el símbolo del sistema](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md). Configure los parámetros del trabajador de escalabilidad horizontal siguiendo estos pasos:

1.  Agregue IS_Worker al parámetro `/FEATURES`.

2. Configure el trabajador de escalabilidad horizontal especificando los siguientes parámetros y sus valores:
    -   `/ISWORKERSVCACCOUNT`
    -   `/ISWORKERSVCPASSWORD`
    -   `/ISWORKERSVCSTARTUPTYPE`
    -   `/ISWORKERSVCMASTER` (opcional)
    -   `/ISWORKERSVCCERT` (opcional)
 
## <a name="install-scale-out-worker-client-certificate"></a><a name="InstallCert"></a> Instalar el certificado de cliente del trabajador de escalado horizontal

Durante la instalación del trabajador de escalabilidad horizontal, se creará e instalará automáticamente un certificado de trabajador en el equipo. También se instalará un certificado de cliente correspondiente, SSISScaleOutWorker.cer, en `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn`. Para que el patrón de escalabilidad horizontal autentique el trabajador de escalabilidad horizontal, deberá agregar este certificado de cliente al almacén raíz del equipo local con el patrón de escalabilidad horizontal.
  
Para agregar el certificado de cliente al almacén raíz, haga doble clic en el archivo .cer y, luego, haga clic en **Instalar certificado** en el cuadro de diálogo del certificado. Se abrirá el **Asistente para importar certificados**.  

## <a name="open-firewall-port"></a><a name="Firewall"></a> Abrir el puerto del firewall

En el equipo que contiene el patrón de escalabilidad horizontal, abra el puerto especificado durante la instalación del patrón de escalabilidad horizontal y el puerto para SQL Server (1433 de manera predeterminada) en el Firewall de Windows.

> [!Note]
> Después de abrir el puerto del firewall, también deberá reiniciar el servicio de trabajador de escalabilidad horizontal.
    
## <a name="start-sql-server-scale-out-master-and-worker-services"></a><a name="Start"></a> Iniciar los servicios de patrón y trabajador de escalado horizontal de SQL Server

Si no ha configurado el tipo de inicio de los servicios como **Automático** durante la instalación, inicie los siguientes servicios:

-   SQL Server Integration Services Scale Out Master 14.0 (SSISScaleOutMaster140)

-   SQL Server Integration Services Scale Out Worker 14.0 (SSISScaleOutWorker140)

## <a name="enable-scale-out-master"></a><a name="EnableMaster"></a> Habilitar el patrón de escalado horizontal

Al crear el catálogo de SSISDB en [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio_md](../../includes/ssmanstudio-md.md)], seleccione **Habilitar este servidor como servicio principal de Escalabilidad horizontal de SSIS** en el cuadro de diálogo **Crear catálogo**.

Tras crear el catálogo, podrá habilitar el patrón de escalabilidad horizontal con [Scale Out Manager](integration-services-ssis-scale-out-manager.md).

## <a name="enable-sql-server-authentication-mode"></a><a name="EnableAuth"></a> Habilitar el modo de autenticación de SQL Server
Si no habilitó la autenticación de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] durante la instalación del motor de base de datos, habilite el modo de autenticación de SQL Server en la instancia de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] que contiene el catálogo de SSISDB. 

La ejecución de paquetes no se bloquea si la autenticación de SQL Server está deshabilitada, Pero el registro de ejecución no puede escribir en la base de datos de SSISDB.

## <a name="enable-scale-out-worker"></a><a name="EnableWorker"></a> Habilitar el trabajador de escalado horizontal

Puede habilitar el trabajador de escalabilidad horizontal con [Scale Out Manager](integration-services-ssis-scale-out-manager.md), que proporciona una interfaz de usuario gráfica, o mediante un procedimiento almacenado.

Para habilitar el trabajador de escalabilidad horizontal mediante un procedimiento almacenado, ejecute el procedimiento almacenado `[catalog].[enable_worker_agent]` con **WorkerAgentId** como parámetro. 

Obtendrá el valor **WorkerAgentId** de la vista `[catalog].[worker_agents]` en SSISDB cuando el trabajador de escalabilidad horizontal se haya registrado en el patrón de escalabilidad horizontal. El registro tardará varios minutos una vez iniciados los servicios de patrón y trabajador de escalabilidad horizontal.

#### <a name="example"></a>Ejemplo
En este ejemplo se habilita el trabajador de escalado horizontal en `computerA`.

```sql
SELECT WorkerAgentId, MachineName FROM [catalog].[worker_agents]
GO
-- Result: --
-- WorkerAgentId                           MachineName  --
-- 6583054A-E915-4C2A-80E4-C765E79EF61D    computerA    --

EXEC [catalog].[enable_worker_agent] '6583054A-E915-4C2A-80E4-C765E79EF61D'
GO 
```

## <a name="next-steps"></a>Pasos siguientes
-   [Ejecutar paquetes en la escalabilidad horizontal de Integration Services (SSIS)](run-packages-in-integration-services-ssis-scale-out.md).