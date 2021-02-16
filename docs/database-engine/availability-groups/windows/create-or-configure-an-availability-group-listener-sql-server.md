---
title: Configuración del agente de escucha del grupo de disponibilidad
description: 'Se describen los pasos que debe seguir al configurar un agente de escucha para un grupo de disponibilidad Always On con PowerShell o SQL Server Management Studio. '
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
f1_keywords:
- sql13.swb.availabilitygroup.newaglistener.general.f1
helpviewer_keywords:
- Availability Groups [SQL Server], listeners
- Availability Groups [SQL Server], client connectivity
ms.assetid: 2bc294f6-2312-4b6b-9478-2fb8a656e645
author: cawrites
ms.author: chadam
manager: erikre
ms.openlocfilehash: fa497fc004393388e34b3afbbe913a5915bbb120
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350804"
---
# <a name="configure-a-listener-for-an-always-on-availability-group"></a>Configuración de un agente de escucha para un grupo de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema se explica cómo crear o configurar un único *agente de escucha de grupo de disponibilidad* para un grupo de disponibilidad AlwaysOn mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]  
>  Para crear el primer agente de escucha del grupo de disponibilidad de un grupo de disponibilidad, se recomienda encarecidamente usar [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)] o [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Evite crear un agente de escucha directamente en el clúster de WSFC salvo cuando sea necesario, por ejemplo para crear un agente de escucha adicional.  
  
 
##  <a name="does-a-listener-exist-for-this-availability-group-already"></a><a name="DoesListenerExist"></a> ¿Existe ya un agente de escucha para este grupo de disponibilidad?  

 **Para determinar si ya existe un agente de escucha para el grupo de disponibilidad**  
  
-   [Ver las propiedades del agente de escucha de grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-listener-properties-sql-server.md)  
  
> [!NOTE]  
>  Si ya existe un agente de escucha y quiere crear otro, vea [Crear un agente de escucha adicional para un grupo de disponibilidad (opcional)](#CreateAdditionalListener)más adelante en este tema.  
  
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Solo puede crear un agente de escucha por cada grupo de disponibilidad mediante [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Normalmente, cada grupo de disponibilidad necesita un único agente de escucha. Sin embargo, algunos escenarios de clientes necesitan varios agentes de escucha para un grupo de disponibilidad.   Después de crear un agente de escucha mediante SQL Server, puede usar Windows PowerShell para los clústeres de conmutación por error o el Administrador de clústeres de conmutación por error de WSFC para crear agentes de escucha adicionales. Para obtener más información, vea [Crear un agente de escucha adicional para un grupo de disponibilidad (opcional)](#CreateAdditionalListener)más adelante en este tema.  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
 Aunque no es obligatorio, se recomienda usar una dirección IP estática si hay varias configuraciones de subred.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   Debe estar conectado a la instancia del servidor que hospeda la réplica principal.  
  
-   Si está configurando un agente de escucha del grupo de disponibilidad a través de varias subredes y planea usar direcciones IP estáticas, debe obtener la dirección IP estática de cada subred que hospede una réplica de disponibilidad del grupo de disponibilidad para el que está creando el agente de escucha. Normalmente, necesitará solicitar a sus administradores de red las direcciones IP estáticas.  
  
> [!IMPORTANT]  
>  Antes de crear el primer agente de escucha, se recomienda encarecidamente leer [Conectividad de cliente de AlwaysOn&#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md).  
  
###  <a name="requirements-for-the-dns-name-of-an-availability-group-listener"></a><a name="DNSnameReqs"></a> Requisitos del nombre DNS de un agente de escucha de grupo de disponibilidad  
 Cada agente de escucha de grupo de disponibilidad necesita un nombre de host DNS que sea único en el dominio y en NetBIOS. El nombre DNS es un valor de cadena. Este nombre solo puede contener caracteres alfanuméricos, guiones (-) y caracteres de subrayado (_), en cualquier orden. Los nombres de host DNS no distinguen entre mayúsculas y minúsculas. La longitud máxima es de 63 caracteres; sin embargo, en [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], la longitud máxima que puede especificar es de 15 caracteres.  
  
 Es recomendable que especifique una cadena que tenga sentido. Por ejemplo, para un grupo de disponibilidad denominado `AG1`, un nombre de host DNS significativo sería `ag1-listener`.  
  
> [!IMPORTANT]  
>  NetBIOS reconoce solo los 15 primeros caracteres en dns_name. Si tiene dos clústeres de WSFC controlados por la misma instancia de Active Directory e intenta crear agentes de escucha del grupo de disponibilidad en los dos clústeres utilizando nombres con más de 15 caracteres y un prefijo idéntico de 15 caracteres, obtendrá un error notificando que el recurso de nombre de red virtual no se pudo poner en línea. Para obtener información acerca de las reglas de nomenclatura de prefijos para los nombres DNS, vea [Asignación de nombres de dominio](https://technet.microsoft.com/library/cc731265\(WS.10\).aspx).  
  
##  <a name="windows-permissions"></a><a name="WinPermissions"></a> Permisos de Windows  
  
|Permisos|Vínculo|  
|-----------------|----------|  
|El nombre de objeto de clúster (CNO) del clúster de WSFC que hospeda el grupo de disponibilidad debe tener permiso para **crear objetos de equipo** .<br /><br /> En Active Directory, un CNO de forma predeterminada no tiene permiso para **crear objetos de equipo** explícitamente y puede crear diez objetos de equipo virtual (VCO). Una vez creados los 10 VCO, no podrán crearse otros adicionales. Puede evitar esto si concede el permiso de forma explícita al CNO del clúster de WSFC. Observe que los VCO para los grupos de disponibilidad que ha eliminado no son eliminados de forma automática en Active Directory y se contabilizan para el límite predeterminado de 10 VCO a menos que sean eliminados de forma manual.<br /><br /> Nota: En algunas organizaciones, la directiva de seguridad prohíbe conceder permiso para **crear objetos de equipo** a cuentas de usuario individuales.|*Pasos para configurar la cuenta para el usuario que instala el clúster* en la [Guía paso a paso de clústeres de conmutación por error: configurar cuentas en Active Directory](https://technet.microsoft.com/library/cc731002\(WS.10\).aspx#BKMK_steps_installer)<br /><br /> *Pasos para preconfigurar la cuenta de nombre de clúster* en la [Guía paso a paso de clústeres de conmutación por error: configurar cuentas en Active Directory](https://technet.microsoft.com/library/cc731002\(WS.10\).aspx#BKMK_steps_precreating)|  
|Si su organización necesita que preconfigure la cuenta de equipo para un nombre de red virtual de agente de escucha, deberá pertenecer al grupo **Operador de cuentas** o pedir ayuda a su administrador de dominio.|*Pasos para preconfigurar una cuenta para un servicio o una aplicación en clúster* en la [Guía paso a paso de clústeres de conmutación por error: configurar cuentas en Active Directory](https://technet.microsoft.com/library/cc731002\(WS.10\).aspx#BKMK_steps_precreating2).|  
  
> [!TIP]  
>  Generalmente, es más sencillo no ensayar previamente la cuenta del equipo para un nombre de red virtual de agente de escucha. Si puede, deje que la cuenta se cree y configure automáticamente al ejecutar el asistente para alta disponibilidad de WSFC.  
  
##  <a name="sql-server-permissions"></a><a name="SqlPermissions"></a> Permisos de SQL Server  
  
|Tarea|Permisos|  
|----------|-----------------|  
|Para crear un agente de escucha del grupo de disponibilidad.|Se requiere la pertenencia al rol fijo de servidor **sysadmin** y el permiso de servidor CREATE AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER.|  
|Para modificar un agente de escucha del grupo de disponibilidad existente|Se requiere el permiso ALTER AVAILABILITY GROUP en el grupo de disponibilidad, el permiso CONTROL AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER.|  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
> [!TIP]  
>  El [Asistente para nuevo grupo de disponibilidad](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md) admite la creación del agente de escucha de un nuevo grupo de disponibilidad.  
  
 **Para crear o configurar un agente de escucha del grupo de disponibilidad**  
  
1.  En el Explorador de objetos, conéctese a la instancia del servidor que hospeda la réplica principal del grupo de disponibilidad y haga clic en el nombre del servidor para expandir el árbol.  
  
2.  Expanda los nodos **Alta disponibilidad de AlwaysOn** y **Grupos de disponibilidad** .  
  
3.  Haga clic en el grupo de disponibilidad cuyo agente de escucha desea configurar y elija una de las alternativas siguientes:  
  
    -   Para crear un agente de escucha, haga clic con el botón derecho en el nodo **Agentes de escucha de grupo de disponibilidad** y seleccione el comando **Nuevo agente de escucha** . Se abrirá el cuadro de diálogo de **Nuevo agente de escucha del grupo de disponibilidad** . Para obtener más información, vea [Agregar agente de escucha de grupo de disponibilidad (cuadro de diálogo)](#AddAgListenerDialog)más adelante en este tema.  
  
    -   Para cambiar el número de puerto de un agente de escucha existente, expanda el nodo **Agentes de escucha de grupo de disponibilidad** , haga clic con el botón derecho en el agente de escucha y seleccione el comando **Propiedades** . Escriba el nuevo número de puerto en el campo **Puerto** y haga clic en **Aceptar**.  
  
###  <a name="new-availability-group-listener-dialog-box"></a><a name="AddAgListenerDialog"></a> Nuevo agente de escucha del grupo de disponibilidad (cuadro de diálogo)  
 **Nombre DNS del agente de escucha**  
 Especifica el nombre de host DNS del agente de escucha del grupo de disponibilidad. El nombre DNS es una cadena que debe ser única en el dominio y en NetBIOS. Este nombre solo puede contener caracteres alfanuméricos, guiones (-) y caracteres de subrayado (_), en cualquier orden. Los nombres de host DNS no distinguen entre mayúsculas y minúsculas. La longitud máxima es de 15 caracteres.  
  
 Para obtener más información, vea [Requisitos del nombre DNS de un agente de escucha de grupo de disponibilidad](#DNSnameReqs), anteriormente en este tema.  
  
 **Puerto**  
 El puerto TCP usado por este agente de escucha.  
  
 **Modo de red**  
 Indica el protocolo TCP utilizado por el agente de escucha; puede ser:  
  
 **DHCP**  
 El agente de escucha nos pedirá una dirección IP dinámica asignada por un servidor que ejecute el protocolo DHCP (Protocolo de configuración dinámica de host). DHCP está limitado a una sola subred.  
  
> [!IMPORTANT]  
>  No se recomienda DHCP en el entorno de producción. Si hay un tiempo de inactividad y expira la concesión de IP para DHCP, se necesitará más tiempo para registrar la nueva dirección IP de red DHCP que está asociada con el nombre DNS de escucha, lo que puede afectar a la conectividad de cliente. Sin embargo, DHCP es adecuado para configurar el entorno de desarrollo y pruebas para comprobar características básicas de los grupos de disponibilidad y para la integración con las aplicaciones.  
  
 **Dirección IP estática**  
 El agente de escucha utilizará una o más direcciones IP estáticas. Las direcciones IP adicionales son opcionales. Para crear un agente de escucha del grupo de disponibilidad a través de varias subredes, para cada subred se debe especificar una dirección IP estática en la configuración del agente de escucha. Póngase en contacto con su administrador de red para obtener estas direcciones IP estáticas.  
  
 Si selecciona **Dirección IP estática** , aparecerá una cuadrícula de subred debajo del campo **Modo de red** . Esta cuadrícula muestra información sobre cada subred a la que puede tener acceso este agente de escucha del grupo de disponibilidad. Esta cuadrícula estará vacía hasta que agregue una dirección IP estática haciendo clic en **Agregar**.  
  
 Estas son sus columnas:  
  
 **Subred**  
 Muestra el identificador de cada subred que se agrega al agente de escucha del grupo de disponibilidad.  
  
 **Dirección IP**  
 Muestra la dirección IP de una subred determinada.  Para una subred determinada, la dirección IP es una dirección IPv4 o una dirección IPv6.  
  
 **Add (Agregar)**  
 Haga clic en Agregar para agregar una dirección IP estática a una subred seleccionada o a otra subred para este agente de escucha. Se abrirá el cuadro de diálogo de **Agregar dirección IP** . Para obtener más información, vea el tema de ayuda [Agregar dirección IP &#40;cuadro de diálogo - SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/add-ip-address-dialog-box-sql-server-management-studio.md).  
  
 **Remove**  
 Haga clic para quitar la subred seleccionada de este agente de escucha.  
  
 **OK (CORRECTO)**  
 Haga clic para crear el agente de escucha del grupo de disponibilidad especificado.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 **Para crear o configurar un agente de escucha del grupo de disponibilidad**  
  
1.  Conéctese a la instancia del servidor que hospeda la réplica principal.  
  
2.  Use la opción LISTENER de la instrucción [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md) o la opción ADD LISTENER de la instrucción [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) .  
  
     En el siguiente ejemplo se agrega un agente de escucha de grupo de disponibilidad a un grupo de disponibilidad existente denominado `MyAg2`. Un nombre DNS único, `MyAg2ListenerIvP6`, se especifica para este agente de escucha. Las dos réplicas se encuentran en distintas subredes, por lo que, según lo recomendado, el agente de escucha usa direcciones IP estáticas. En cada una de las dos réplicas de disponibilidad, la cláusula WITH IP especifica una dirección IP estática, `2001:4898:f0:f00f::cf3c and 2001:4898:e0:f213::4ce2`, que usa el formato IPv6. Este ejemplo especifica también que se use el argumento opcional PORT para definir el puerto `60173` como puerto de escucha.  
  
    ```  
    ALTER AVAILABILITY GROUP MyAg2   
          ADD LISTENER 'MyAg2ListenerIvP6' ( WITH IP ( ('2001:db88:f0:f00f::cf3c'),('2001:4898:e0:f213::4ce2') ) , PORT = 60173 );   
    GO  
  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Usar PowerShell  
 **Para crear o configurar un agente de escucha del grupo de disponibilidad**  
  
1.  Cambie el directorio (**cd**) a la instancia del servidor que hospeda la réplica principal.  
  
2.  Para crear o modificar un agente de escucha del grupo de disponibilidad utilice uno de los cmdlets siguientes:  
  
     **New-SqlAvailabilityGroupListener**  
     Crea un nuevo agente de escucha del grupo de disponibilidad y lo adjunta a un grupo de disponibilidad existente.  
  
     Por ejemplo, el siguiente comando **New-SqlAvailabilityGroupListener** crea un agente de escucha de grupo de disponibilidad denominado `MyListener` para el grupo de disponibilidad `MyAg`. Este agente de escucha usará la dirección IPv4 transmitida al parámetro **-StaticIp** como dirección IP virtual.  
  
    ```  
    New-SqlAvailabilityGroupListener -Name MyListener `   
    -StaticIp '192.168.3.1/255.255.252.0' `   
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg  
  
    ```  
  
     **Set-SqlAvailabilityGroupListener**  
     Modifica la configuración del puerto en un agente de escucha del grupo de disponibilidad existente.  
  
     Por ejemplo, el siguiente comando **Set-SqlAvailabilityGroupListener** establece el número de puerto del agente de escucha de grupo de disponibilidad denominado `MyListener` en `1535`. Este puerto se usa para escuchar las conexiones del agente de escucha.  
  
    ```  
    Set-SqlAvailabilityGroupListener -Port 1535 `   
    -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\AGListeners\MyListener  
  
    ```  
  
     **Add-SqlAGListenerstaticIp**  
     Agrega una dirección IP estática a una configuración de agente de escucha del grupo de disponibilidad existente. La dirección IP estática puede ser una dirección IPv4 con subred o una dirección IPv6.  
  
     Por ejemplo, el siguiente comando **Add-SqlAGListenerstaticIp** agrega una dirección IPv4 estática al agente de escucha de grupo de disponibilidad `MyListener` en el grupo de disponibilidad `MyAg`. Esta dirección IPv6 actúa como dirección IP virtual del agente de escucha de la subred `255.255.252.0`. Si el grupo de disponibilidad abarca varias subredes, debe agregar una dirección IP estática para cada subred al agente de escucha.  
  
    ```  
    $path = "SQLSERVER:\SQL\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\AGListeners\ MyListener" `   
    Add-SqlAGListenerstaticIp -Path $path `   
    -StaticIp "2001:0db8:85a3:0000:0000:8a2e:0370:7334"  
    ```  
  
    > [!NOTE]  
    >  Para ver la sintaxis de un cmdlet, use el cmdlet **Get-Help**  en el entorno de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Para más información, consulte [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Para configurar y usar el proveedor de SQL Server PowerShell**  
  
-   [Proveedor de SQL Server PowerShell Provider](../../../powershell/sql-server-powershell-provider.md)  
  
## <a name="troubleshooting"></a>Solución de problemas  
  
###  <a name="failure-to-create-an-availability-group-listener-because-of-active-directory-quotas"></a><a name="ADQuotas"></a> No se ha podido crear un agente de escucha del grupo de disponibilidad por las cuotas de Active Directory  
 La creación de un nuevo agente de escucha del grupo de disponibilidad puede producir un error en la creación porque se ha alcanzado una cuota de Active Directory para la cuenta de equipo del nodo de clúster que participa.  Para más información, consulte los siguientes artículos.  
  
-   [Cómo solucionar problemas de la cuenta del Servicio de Cluster Server cuando modifica objetos de equipo](https://support.microsoft.com/kb/307532)  
  
-   [Cuotas de Active Directory](https://technet.microsoft.com/library/cc904295\(WS.10\).aspx)  
  
##  <a name="follow-up-after-creating-an-availability-group-listener"></a><a name="FollowUp"></a> Seguimiento: después de crear un agente de escucha de grupo de disponibilidad  
  
###  <a name="multisubnetfailover-keyword-and-associated-features"></a><a name="MultiSubnetFailover"></a> Palabra clave MultiSubnetFailover y características asociadas  
 **MultiSubnetFailover** es una nueva palabra clave de la cadena de conexión que se usa para habilitar la conmutación por error más rápida con los grupos de disponibilidad AlwaysOn y las instancias de clúster de conmutación por error AlwaysOn en SQL Server 2012. Las tres subcaracterísticas siguientes se habilitan cuando se establece `MultiSubnetFailover=True` en la cadena de conexión:  
  
-   Conmutación por error de varias subredes más rápida a un agente de escucha de varias subredes para instancias del clúster de conmutación por error o un grupo de disponibilidad AlwaysOn.  
  
-   Conmutación por error de una sola subred más rápida a un agente de escucha de una sola subred para instancias del clúster de conmutación por error o un grupo de disponibilidad AlwaysOn.  
  
    -   Esta característica se usa al conectarse a un agente de escucha que solo tiene una dirección IP en una sola subred. Así, se realizan más reintentos de conexión TCP para acelerar las conmutaciones por error en una sola subred.  
  
-   Resolución de una instancia con nombre en una instancia de clúster de conmutación por error AlwaysOn de varias subredes.  
  
    -   Esto sirve para agregar compatibilidad con la resolución de una instancia con nombre para instancias del clúster de conmutación por error AlwaysOn con varios puntos de conexión de subred.  
  
 **MultiSubnetFailover=True no se admite en .NET Framework 3.5 u OLEDB**  
  
 **Problema:** si la instancia del clúster de conmutación por error o el grupo de disponibilidad tiene un nombre de cliente de escucha (denominado nombre de red o punto de acceso cliente en el Administrador de clústeres de WSFC) que depende de varias direcciones IP de subredes diferentes, y usa ADO.NET con .NET Framework 3.5 SP1 o SQL Native Client 11.0 OLEDB, puede que se agote el tiempo de espera de conexión en el 50 % de las solicitudes de conexión de cliente al agente de escucha de grupo de disponibilidad.  
  
 **Soluciones alternativas:** le recomendamos que realice una de las tareas siguientes.  
  
-   Si no tiene permiso para manipular recursos de clúster, cambie el tiempo de espera de la conexión a 30 segundos (este valor produce un tiempo de espera de TCP de 20 segundos además de un búfer de 10 segundos).  
  
     **Ventajas**: Si se produce una conmutación por error entre subredes, el tiempo de recuperación del cliente es breve.  
  
     **Inconvenientes**: La mitad de las conexiones cliente tardarán más de 20 segundos  
  
-   Si tiene permiso para manipular recursos de clúster, el enfoque más recomendado consiste en establecer el nombre de red del agente de escucha del grupo de disponibilidad en `RegisterAllProvidersIP=0`. Para obtener más información, vea "Configuración de RegisterAllProvidersIP" más adelante en esta sección.  
  
     **Ventajas:** No es necesario aumentar el valor del tiempo de espera de la conexión de cliente.  
  
     **Inconvenientes:** si se produce una conmutación por error entre subredes, el tiempo de recuperación del cliente podría ser de 15 minutos o más, según la configuración de **HostRecordTTL** y la de la programación de replicación DNS/AD entre sitios.  
  
###  <a name="registerallprovidersip-setting"></a><a name="RegisterAllProvidersIP"></a> Valor de RegisterAllProvidersIP  
 Cuando se usa [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell para crear un agente de escucha de grupo de disponibilidad, el punto de acceso cliente se crea en WSFC con la propiedad **RegisterAllProvidersIP** establecida en 1 (true). El efecto de este valor de propiedad depende de la cadena de conexión de cliente, de la manera siguiente:  
  
-   Cadenas de conexión que establecen **MultiSubnetFailover** en true  
  
     [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] establece la propiedad  **RegisterAllProvidersIP** en 1 para reducir el tiempo de reconexión después de una conmutación por error para los clientes cuyas cadenas de conexión de cliente especifiquen `MultiSubnetFailover = True`, como se recomienda. Tenga en cuenta que para aprovechar la característica de múltiples subredes del agente de escucha, puede que los clientes necesiten un proveedor de datos que admita la palabra clave **MultiSubnetFailover** . Para más información sobre la compatibilidad del controlador con la conmutación por error de varias subredes, vea [Conectividad de cliente de AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md).  
  
     Para obtener información sobre la agrupación en clústeres de varias subredes, vea [Agrupación en clústeres de varias subredes de SQL Server&#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md).  
  
    > [!TIP]  
    >  Cuando `RegisterAllProvidersIP = 1`, si ejecuta el Asistente para validar una configuración de WSFC en el clúster de WSFC, el asistente genera el mensaje de advertencia siguiente:  
    >   
    >  "La propiedad RegisterAllProviderIP para el nombre de red 'Name:<nombre_red>' está establecida en 1. Para la configuración de clúster actual, este valor debería estar establecido en 0".  
    >   
    >  Omita este mensaje.  
  
-   Cadenas de conexión que no establecen **MultiSubnetFailover** en true  
  
     Cuando `RegisterAllProvidersIP = 1`, los clientes cuyas cadenas de conexión no usan =MultiSubnetFailover = True `MultiSubnetFailover = True`, experimentarán conexiones con latencia elevada. Esto se debe a que estos clientes intentan conexiones a todas las direcciones IP de forma secuencial. En cambio, si **RegisterAllProvidersIP** se cambia a 0, la dirección IP activa se registra en el punto de acceso cliente del clúster de WSFC, lo que reduce la latencia de los clientes heredados. Por tanto, si tiene clientes heredados que necesitan conectarse a un agente de escucha del grupo de disponibilidad y no puede usar la propiedad **MultiSubnetFailover** , se recomienda cambiar **RegisterAllProvidersIP** a 0.  
  
    > [!IMPORTANT]  
    >  Cuando se crea un agente de escucha de grupo de disponibilidad en el clúster de WSFC (GUI del Administrador de clústeres de conmutación por error), **RegisterAllProvidersIP** será 0 (False) de forma predeterminada.  
  
###  <a name="hostrecordttl-setting"></a><a name="HostRecordTTL"></a> Configuración de HostRecordTTL  
 De forma predeterminada, los clientes almacenan en memoria caché los registros DNS de clúster durante 20 minutos.  Al reducir **HostRecordTTL**(el período de vida (TTL)) del registro almacenado en memoria caché, los clientes heredados pueden volver a conectarse más rápido.  Pero la reducción del valor de **HostRecordTTL** también puede producir un aumento de tráfico en los servidores DNS.  
  
###  <a name="sample-powershell-script-to-disable-registerallprovidersip-and-reduce-ttl"></a><a name="SampleScript"></a> Script de PowerShell de ejemplo para deshabilitar RegisterAllProvidersIP y reducir TTL  
 En el ejemplo siguiente de PowerShell se muestra cómo configurar los parámetros de clúster **RegisterAllProvidersIP** y **HostRecordTTL** para el recurso de escucha.  El registro DNS se almacenará en memoria caché durante 5 minutos en lugar del valor predeterminado de 20 minutos.  La modificación de los dos parámetros de clúster puede reducir el tiempo necesario para conectarse a la dirección IP correcta después de una conmutación por error para los clientes heredados que no pueden utilizar el parámetro **MultiSubnetFailover** .  Reemplace `yourListenerName` por el nombre del agente de escucha que va a cambiar.  
  
```  
Import-Module FailoverClusters  
Get-ClusterResource yourListenerName | Set-ClusterParameter RegisterAllProvidersIP 0   
Get-ClusterResource yourListenerName | Set-ClusterParameter HostRecordTTL 300  
Stop-ClusterResource yourListenerName  
Start-ClusterResource yourListenerName  
Start-Clustergroup yourListenerGroupName
```  
  
 Para obtener más información sobre los tiempos de recuperación durante la conmutación por error, consulte [Client Recovery Latency During Failover](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md#DNS).  
  
###  <a name="follow-up-recommendations"></a><a name="FollowUpRecommendations"></a> Recomendaciones de seguimiento  
 Después de crear un agente de escucha del grupo de disponibilidad:  
  
-   Pida al administrador de red que reserve la dirección IP del agente de escucha para su uso exclusivo.  
  
-   Proporcione el nombre del host DNS del agente de escucha a los desarrolladores de aplicaciones para que lo usen en las cadenas de conexión cuando soliciten conexiones cliente a este grupo de disponibilidad.  
  
-   Anime a los desarrolladores a actualizar las cadenas de conexión de cliente para especificar `MultiSubnetFailover = True`, si es posible. Para más información sobre la compatibilidad del controlador con la conmutación por error de varias subredes, vea [Conectividad de cliente de AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md).  
  
###  <a name="create-an-additional-listener-for-an-availability-group-optional"></a><a name="CreateAdditionalListener"></a> Crear un agente de escucha adicional para un grupo de disponibilidad (opcional)  
 Después de crear un agente de escucha con SQL Server, puede agregar un agente de escucha adicional de la manera siguiente:  
  
1.  Cree el agente de escucha mediante una de las herramientas siguientes:  
  
    -   **Con el Administrador de clústeres de conmutación por error de WSFC:**  
  
        1.  Agregue un punto de acceso de cliente y configure la dirección IP.  
  
        2.  Ponga el agente de escucha en línea.  
  
        3.  Agregue una dependencia al recurso de grupo de disponibilidad de WSFC.  
  
         Para obtener información sobre los cuadros de diálogo y las pestañas del Administrador de clústeres de conmutación por error, vea [Interfaz de usuario: complemento Administrador de clúster de conmutación por error](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772502(v=ws.11)).  
  
    -   **Con Windows PowerShell para los clústeres de conmutación por error:**  
  
        1.  Use [Add-ClusterResource](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee460983(v=technet.10)) para crear un nombre de red y los recursos de dirección IP.  
  
        2.  Use [Start-ClusterResource](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461056(v=technet.10)) para iniciar el recurso de nombre de red.  
  
        3.  Use [Add-ClusterResourceDependency](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461014(v=technet.10)) para establecer la dependencia entre el nombre de red y el recurso de grupo de disponibilidad de SQL Server existente.  
  
         Para obtener información sobre cómo usar Windows PowerShell en los clústeres de conmutación por error, vea [Introducción a los comandos del Administrador del servidor](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732757(v=ws.11)#BKMK_wps).  
  
2.  Inicie la escucha de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en el nuevo agente de escucha. Después de crear el agente de escucha adicional, conéctese a la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que hospeda la réplica principal del grupo de disponibilidad y use [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell para modificar el puerto de escucha.  
  
 Para obtener más información, vea [How to create multiple listeners for same availability group (Cómo crear varios agentes de escucha para el mismo grupo de disponibilidad)](/archive/blogs/sqlalwayson/how-to-create-multiple-listeners-for-same-availability-group-goden-yao) (un blog del equipo de AlwaysOn de SQL Server).  
  
 
  
## <a name="next-steps"></a>Pasos siguientes

Ahora que ha creado el cliente de escucha, configure la aplicación para que se [conecte al cliente de escucha](listeners-client-connectivity-application-failover.md). También puede revisar varias [estrategias de supervisión de grupos de disponibilidad](monitoring-of-availability-groups-sql-server.md) para garantizar el estado del grupo de disponibilidad.

También puede [ver las propiedades de un cliente de escucha](view-availability-group-listener-properties-sql-server.md) u obtener información sobre cómo [quitar el cliente de escucha](remove-an-availability-group-listener-sql-server.md), si es necesario.