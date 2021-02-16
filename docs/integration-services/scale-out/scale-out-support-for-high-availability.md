---
title: Compatibilidad con la escalabilidad horizontal para una alta disponibilidad | Microsoft Docs
description: Aprenda a configurar el Servicio principal de Escalabilidad horizontal de SQL Server Integration Services (SSIS) para una alta disponibilidad.
ms.custom: performance
ms.date: 05/23/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: dd53bf6f045687f7d79f6c0e836490adb60cda44
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348867"
---
# <a name="scale-out-support-for-high-availability"></a>Compatibilidad con la escalabilidad horizontal para una alta disponibilidad

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



En la escalabilidad horizontal de SSIS, la alta disponibilidad del trabajo se proporciona a través de la ejecución de paquetes con varios trabajos de escalabilidad horizontal.

La alta disponibilidad en el lado del servicio principal de escalabilidad horizontal se logra con [Always On para el catálogo de SSIS](../catalog/ssis-catalog.md#always-on-for-ssis-catalog-ssisdb) y los clústeres de conmutación por error de Windows. En esta solución se hospedan varias instancias del servicio principal de escalabilidad horizontal en un clúster de conmutación por error de Windows. Si el Servicio principal de escalabilidad horizontal o SSISDB están desactivados en el nodo principal, el servicio o SSISDB en el nodo secundario continuarán aceptando solicitudes de usuario y comunicándose con los trabajos de escalabilidad horizontal.

Como alternativa, la alta disponibilidad en el lado del servicio principal de escalabilidad horizontal se puede lograr con Instancia de clúster de conmutación por error de SQL Server. Vea [Scale Out support for high availability via SQL Server failover cluster instance](scale-out-failover-cluster-instance.md) (Compatibilidad con la escalabilidad horizontal para una alta disponibilidad mediante Instancia de clústeres de conmutación por error de SQL Server).

Para configurar la alta disponibilidad en el lado del servicio principal de escalabilidad horizontal con AlwaysOn para el catálogo de SSIS, haga lo siguiente:

## <a name="1-prerequisites"></a>1. Prerrequisitos
Configure un clúster de conmutación por error de Windows. Vea la entrada de blog [Installing the Failover Cluster Feature and Tools for Windows Server 2012](https://techcommunity.microsoft.com/t5/failover-clustering/installing-the-failover-cluster-feature-and-tools-in-windows/ba-p/371733) (Instalación de las herramientas y la característica de clúster de conmutación por error para Windows Server 2012) para obtener instrucciones. Instale la característica y las herramientas en todos los nodos del clúster.

## <a name="2-install-scale-out-master-on-the-primary-node"></a>2. Instalación del Servicio principal de escalabilidad horizontal en el nodo principal
Instale los servicios de motor de base de datos de SQL Server, Integration Services y el Servicio principal de escalabilidad horizontal en el nodo principal del servicio. 

Durante la instalación, haga lo siguiente:

### <a name="21-set-the-account-running-scale-out-master-service-to-a-domain-account"></a>2.1. Establecer la cuenta que ejecuta el servicio del Servicio principal de escalabilidad horizontal en una cuenta de dominio
Esta cuenta debe ser capaz de acceder a SSISDB en el nodo secundario del clúster de conmutación por error de Windows en el futuro. Dado que la conmutación por error del Servicio principal de escalabilidad horizontal y SSISDB se puede realizar por separado, es posible que no estén en el mismo nodo tras la conmutación por error.

![Configuración del servidor HA](media/ha-server-config.PNG)

### <a name="22-include-the-dns-host-name-for-the-scale-out-master-service-in-the-cns-of-the-scale-out-master-certificate"></a>2.2. Incluir el nombre de host DNS del servicio del Servicio principal de escalabilidad horizontal en los nombres comunes del certificado del servicio

Este nombre de host es el punto de conexión principal de Escalabilidad horizontal, que se crea como un servicio genérico agrupado en el clúster de conmutación por error (consulte el paso 7).   (Asegúrese de proporcionar un nombre de host DNS y no un nombre de servidor).

![Configuración del patrón HA](media/ha-master-config.PNG)

## <a name="3-install-scale-out-master-on-the-secondary-node"></a>3. Instalación del Servicio principal de escalabilidad horizontal en el nodo secundario
Instale los servicios de motor de base de datos de SQL Server, Integration Services y el Servicio principal de escalabilidad horizontal en el nodo secundario del servicio. 

Use el mismo certificado del Servicio principal de escalabilidad horizontal que usó con el nodo principal. Exporte el certificado TLS/SSL del patrón de escalabilidad horizontal en el nodo principal con una clave privada e instálelo en el almacén de certificados raíz del equipo local en el nodo secundario. Seleccione este certificado cuando instale el Servicio principal de escalabilidad horizontal en el nodo secundario.

![Configuración del patrón HA 2](media/ha-master-config2.PNG)

> [!NOTE]
> Puede configurar varios patrones del Servicio principal de escalabilidad horizontal de copia de seguridad mediante la repetición de las operaciones del servicio en los nodos secundarios.

## <a name="4-set-up-and-configure-ssisdb-support-for-always-on"></a>4. Configuración de la compatibilidad con SSISDB para AlwaysOn

Siga las instrucciones para configurar la compatibilidad con SSISDB para AlwaysOn que encontrará en [Always On para el catálogo de SSIS (SSISDB)](../catalog/ssis-catalog.md#always-on-for-ssis-catalog-ssisdb).

Además, tendrá que crear un agente de escucha de grupo de disponibilidad para el grupo de disponibilidad en el que agregue SSISDB. Consulte [Create or Configure an Availability Group Listener](../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md) (Crear o configurar un agente de escucha de grupo de disponibilidad).

## <a name="5-update-the-scale-out-master-service-configuration-file"></a>5. Actualizar el archivo de configuración del servicio de patrón de escalabilidad horizontal
Actualice el archivo de configuración del Servicio principal de escalabilidad horizontal, `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config`, tanto en el nodo principal como en los secundarios. Actualice **SqlServerName** a *[Nombre DNS del agente de escucha de grupo de disponibilidad],[Puerto]* .

## <a name="6-enable-package-execution-logging"></a>6. Habilitar el registro de la ejecución de paquetes

El registro en SSISDB se realiza mediante el inicio de sesión **##MS_SSISLogDBWorkerAgentLogin##** , cuya contraseña se genera automáticamente. Para que el registro funcione para todas las réplicas de SSISDB, haga lo siguiente:

### <a name="61-change-the-password-of-ms_ssislogdbworkeragentlogin-on-the-primary-sql-server"></a>6.1. Cambio de la contraseña de **##MS_SSISLogDBWorkerAgentLogin##** en la instancia de SQL Server principal

### <a name="62-add-the-login-to-the-secondary-sql-server"></a>6.2. Agregar el inicio de sesión a la instancia secundaria de SQL Server

### <a name="63-update-the-connection-string-used-for-logging"></a>6.3. Actualización de la cadena de conexión que se usa para el registro
Llame al procedimiento almacenado `[catalog].[update_logdb_info]` con los siguientes valores de parámetro:

-   `@server_name = '[Availability Group Listener DNS name],[Port]'`

-   `@connection_string = 'Data Source=[Availability Group Listener DNS name],[Port];Initial Catalog=SSISDB;User Id=##MS_SSISLogDBWorkerAgentLogin##;Password=[Password]];'`

## <a name="7-configure-the-scale-out-master-service-role-of-the-windows-server-failover-cluster"></a>7. Configuración del rol del Servicio principal de escalabilidad horizontal del clúster de conmutación por error de Windows Server

1.  En el Administrador de clústeres de conmutación por error, conéctese al clúster de escalabilidad horizontal. Seleccione el clúster. Seleccione **Acción** en el menú y, a continuación, **Configurar rol**.

2.  En el cuadro de diálogo **Asistente para alta disponibilidad**, seleccione **Servicio genérico** en la página **Seleccionar rol**. En la página **Seleccionar servicio**, seleccione Servicio principal de escalabilidad horizontal de SQL Server Integration Services 14.0.

3.  En la página **Punto de acceso de cliente**, escriba el nombre de host DNS del Servicio principal de escalabilidad horizontal.

    ![Asistente HA 1](media/ha-wizard1.PNG)

4.  Finalice al asistente.

En máquinas virtuales de Azure, este paso de configuración requiere pasos adicionales. Una explicación completa de estos conceptos y pasos queda fuera del ámbito de este artículo.

1.  Tendrá que configurar un dominio de Azure. Los clústeres de conmutación por error de Windows Server requieren que todos los equipos del clúster sean miembros del mismo dominio. Para obtener más información, vea [Habilitación de Azure Active Directory Domain Services mediante Azure Portal](/azure/active-directory-domain-services/create-instance).

2. Tendrá que configurar un equilibrador de carga de Azure. Es un requisito para la escucha de grupo de disponibilidad. Para más información, vea [Tutorial: equilibrar la carga de tráfico interno de las máquinas virtuales con Load Balancer Básico mediante Azure Portal](/azure/load-balancer/tutorial-load-balancer-basic-internal-portal).

## <a name="8-update-the-scale-out-master-address-in-ssisdb"></a>8. Actualización de la dirección del Servicio principal de escalabilidad horizontal en SSISDB

En el servidor SQL Server principal, ejecute el procedimiento almacenado `[catalog].[update_master_address]` con el valor de parámetro `@MasterAddress = N'https://[Scale Out Master service DNS host name]:[Master Port]'`. 

## <a name="9-add-the-scale-out-workers"></a>9. Agregar trabajos de escalabilidad horizontal

Ahora puede agregar trabajos de escalabilidad horizontal con la ayuda del [Administrador de escalabilidad horizontal de Integration Services](integration-services-ssis-scale-out-manager.md). Escriba `[SQL Server Availability Group Listener DNS name],[Port]` en la página de conexión.

## <a name="upgrade-scale-out-in-high-availability-environment"></a>Actualización de la escalabilidad horizontal en entornos de alta disponibilidad
Para actualizar la escalabilidad horizontal en entornos de alta disponibilidad, siga los [pasos de actualización de Always On para el catálogo de SSIS](../catalog/ssis-catalog.md#Upgrade), actualice el Servicio principal de escalabilidad horizontal y el Trabajo de escalabilidad horizontal en todas las máquinas y vuelva a crear el rol de clúster de conmutación por error de Windows Server en el paso 7 anterior con la nueva versión del Servicio principal de escalabilidad horizontal.

## <a name="next-steps"></a>Pasos siguientes
Para más información, consulte los siguientes artículos:
-   [Servicio principal de escalabilidad horizontal de Integration Services (SSIS)](integration-services-ssis-scale-out-master.md)
-   [Trabajo de escalabilidad horizontal de Integration Services (SSIS)](integration-services-ssis-scale-out-worker.md)