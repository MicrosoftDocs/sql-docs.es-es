---
title: Antes de instalar los clústeres de conmutación por error | Microsoft Docs
description: En este artículo se describen las consideraciones de planeación para instalar un clúster de conmutación por error de SQL Server, incluido el hardware, el sistema operativo y la configuración.
ms.custom: ''
ms.date: 08/24/2016
ms.reviewer: ''
ms.prod: sql
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- clusters [SQL Server], preinstallation checklist
- installing failover clusters
- failover clustering [SQL Server], preinstallation checklist
ms.assetid: a655225d-8c54-4b30-95fd-31f588167899
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2c40647cad7568789a2909adf79d8cb12508532d
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100341152"
---
# <a name="before-installing-failover-clustering"></a>Antes de instalar los clústeres de conmutación por error
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Antes de instalar un clúster de conmutación por error de SQL Server, debe seleccionar el hardware y el sistema operativo en el que se ejecutará SQL Server. También deberá configurar el servicio de clústeres de conmutación por error de Windows Server (WSFC), así como revisar la red, la seguridad y las consideraciones relativas al resto del software que se ejecutará en los clústeres de conmutación por error.  
  
 Si un clúster de Windows tiene una unidad de disco local y esa misma letra de unidad se usa también en uno o varios nodos del clúster como una unidad compartida, no puede instalar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en esa unidad. Esta restricción se aplica tanto a instancias de clúster de conmutación por error de SQL Server como a instancias independientes en un servidor que forma parte de una instancia de clúster de conmutación por error de Windows.
  
 También puede consultar los temas siguientes para obtener más información sobre los conceptos, las características y las tareas de clústeres de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
|Descripción del tema|Tema|  
|-----------------------|-----------|  
|Describe los conceptos de clústeres de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y proporciona vínculos a las tareas y contenido asociados.|[Instancias de clúster de conmutación por error de AlwaysOn &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)|  
|Describe los conceptos de la directiva de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y proporciona vínculos para configurarla de modo que satisfaga los requisitos de su organización.|[Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)|  
|Describe cómo mantener el clúster de conmutación por error existente de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|[Administración y mantenimiento de la instancia de clúster de conmutación por error](../../../sql-server/failover-clusters/windows/failover-cluster-instance-administration-and-maintenance.md)|  
|Explica cómo instalar [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en un Clúster de conmutación por error de Windows Server (WSFC).|[Organizar en clúster SQL Server Analysis Services](/previous-versions/sql/sql-server-2012/dn736073(v=msdn.10))|  
  
 
  
##  <a name="best-practices"></a><a name="BestPractices"></a> Procedimientos recomendados  
  
-   Revise las [notas de la versión de [!INCLUDE[SQL Server 2019](../../../includes/sssql19-md.md)]](../../sql-server-version-15-release-notes.md).
  
-   Instale el software previo requerido. Antes de ejecutar el programa para realizar la instalación o actualización, instale los siguientes requisitos previos para reducir el tiempo de instalación. Puede instalar el software previo en cada nodo de clúster de conmutación por error y, a continuación, reiniciar los nodos una vez antes de ejecutar el programa de instalación.  
  
    -   El programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ya no instala Windows PowerShell. Windows PowerShell es un requisito previo para instalar los componentes de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)][!INCLUDE[ssDE](../../../includes/ssde-md.md)] y [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Si Windows PowerShell no está presente en su equipo, puede habilitarlo siguiendo las instrucciones de la página [Marco de administración de Windows](/powershell/scripting/windows-powershell/wmf/overview) .  
  
    -   El programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ya no instala .NET Framework 3.5 SP1, pero quizá se necesite al instalar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en los sistemas operativos Windows anteriores. Para obtener más información, consulte [SQL Server 2019: Requisitos de hardware y de software](../../install/hardware-and-software-requirements-for-installing-sql-server-ver15.md).  
  
    -   **[!INCLUDE[msCoName](../../../includes/msconame-md.md)] Paquete de actualización:** Para evitar el reinicio del equipo debido a la instalación de .NET Framework 4, el programa de instalación de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] requiere la instalación de una actualización de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] en el equipo.  Si va a instalar [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] en Windows 7 SP1 o [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] SP2, esta actualización se incluye. Si instala en un sistema operativo Windows anterior, descárguelo desde [Microsoft Update para .NET Framework 4.0 en Windows Vista y Windows Server 2008](https://go.microsoft.com/fwlink/?LinkId=198093).  
  
    -   .NET Framework 4: el programa de instalación instala .NET Framework 4 en un sistema operativo en clúster. Para reducir el tiempo total de instalación, considere la instalación de .NET Framework 4 antes de ejecutar el programa de instalación principal.  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Puede instalar estos archivos ejecutando el archivo SqlSupport.msi ubicado en el soporte físico de instalación.  
  
-   Compruebe que no tiene instalado software antivirus en el clúster WSFC. Para obtener más información, vea el artículo de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Knowledge Base [Software antivirus que no es compatible con clúster puede causar problemas con servicios de Cluster Server](/troubleshoot/windows-server/high-availability/not-cluster-aware-antivirus-software-cause-issue).  
  
-   Cuando asigne nombre a un grupo de clústeres de la instalación de clústeres de conmutación por error, no debe utilizar ninguno de los caracteres siguientes:  
  
    -   Operador menor que (\<)  
  
    -   Operador mayor que (>)  
  
    -   Comillas dobles (")  
  
    -   Comillas simples (')  
  
    -   Símbolo de y comercial (&)  
  
     También compruebe que los nombres de los grupos de clústeres existentes no contienen los caracteres no compatibles.  
  
-   Asegúrese de que todos los nodos del clúster están configurados de forma idéntica, lo que incluye COM+, letras de unidad de disco y usuarios del grupo de administradores.  
  
-   Compruebe que ha borrado los registros del sistema en todos los nodos y ha consultado de nuevo los registros del sistema. Antes de continuar, asegúrese de que los registros no contienen mensajes de error.  
  
-   Antes de instalar o actualizar un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , deshabilite todas las aplicaciones y servicios que puedan usar componentes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] durante la instalación, pero conserve los recursos de disco en línea.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] establece automáticamente las dependencias entre el grupo de clúster de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y los discos que estarán en el clúster de conmutación por error. No establezca las dependencias para los discos antes de la instalación.  
  
    -   Durante la instalación de los clústeres de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , se crea el objeto de equipo (cuentas de equipo de Active Directory) para el nombre de recurso de red de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . En un clúster de [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] , la cuenta del nombre del clúster (cuenta de equipo del propio clúster) necesita tener permisos para crear objetos de equipo. Para obtener más información, vea [Configurar cuentas en Active Directory](https://technet.microsoft.com/library/cc731002\(WS.10\).aspx).  
  
    -   Si está utilizando el recurso compartido de archivos SMB como opción de almacenamiento, la cuenta del programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] debe tener SeSecurityPrivilege en el servidor de archivos. Para ello, use la consola de directivas de seguridad local del servidor de archivos para agregar la cuenta de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a los derechos **Administrar registro de seguridad y auditoría** .  
  
##  <a name="verify-your-hardware-solution"></a><a name="Hardware"></a> Comprobar la solución de hardware  
  
-   Si la solución de clúster incluye nodos de clúster geográficamente dispersos, deben comprobarse elementos adicionales como la latencia de red y la compatibilidad con discos compartidos.  
  
    -   Para obtener más información sobre [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] y [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)], vea [Validar el hardware para un clúster de conmutación por error](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732035(v=ws.10)) y [Directiva de compatibilidad para clústeres de conmutación por error de Windows](https://go.microsoft.com/fwlink/?LinkId=196818).  
  
-   Compruebe que el disco en el que se instalará [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no esté comprimido o cifrado. Si intenta instalar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en una unidad comprimida o en una unidad cifrada, se producirá un error en la instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Las configuraciones SAN también se admiten en las ediciones Advanced Server y Datacenter Server de [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] y [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] . En la categoría "Cluster/Multi-cluster Device" de Windows Catalog and Hardware Compatibility List se enumera el conjunto de dispositivos de almacenamiento habilitados para SAN que se han comprobado y se admiten como unidades de almacenamiento SAN con varios clústeres WSFC conectados. Ejecute la validación del clúster después de encontrar los componentes certificados.  
  
-   El recurso compartido de archivos de SMB también se admite para instalar los archivos de datos. Para obtener más información, vea [Storage Types for Data Files](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#StorageTypes).  
  
    > [!WARNING]  
    >  Si va a utilizar Servidor de archivos de Windows como almacenamiento de recursos compartidos de archivos de SMB, la cuenta del programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] debe tener SeSecurityPrivilege en el servidor de archivos. Para ello, use la consola de directivas de seguridad local del servidor de archivos para agregar la cuenta de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a los derechos **Administrar registro de seguridad y auditoría** .  
    >   
    >  Si utiliza un almacenamiento de recursos compartidos de archivos de SMB que no sea Servidor de archivos de Windows, solicite al proveedor de almacenamiento que le proporcione una configuración equivalente en el servidor de archivos.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] admite puntos de montaje.  
  
     Un volumen montado, o punto de montaje, le permite utilizar una sola letra de unidad para hacer referencia a muchos discos o volúmenes. Si tiene una letra de unidad D: para un disco o volumen normal, puede conectar o "montar" discos o volúmenes adicionales como directorios de la letra de unidad D: sin que dichos discos o volúmenes adicionales requieran letras de unidad propias.  
  
     Consideraciones adicionales sobre puntos de montaje para los clústeres de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] requiere que la unidad base de una unidad montada tenga una letra de unidad asociada. En las instalaciones de clústeres de conmutación por error, esta unidad base debe ser una unidad de clúster. En esta versión no se admiten los GUID de volumen.  
  
    -   La unidad base, la que tiene la letra de unidad, no se puede compartir con otras instancias de clústeres de conmutación por error. Se trata de una restricción normal para los clústeres de conmutación por error, pero no es una restricción en los servidores independientes con varias instancias.  
  
    -   Las instalaciones en clúster de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] están limitadas al número de letras de unidad disponibles. En el supuesto de que solo utilice una letra de unidad para el sistema operativo y las demás letras estén disponibles como unidades de clúster normales o unidades de clúster que hospedan puntos de montaje, existe un límite máximo de 25 instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] por clústeres de conmutación por error.  
  
        > [!TIP]  
        >  El límite de 25 instancias se puede superar utilizando la opción de recurso compartido de archivos de SMB. Si usa el recurso compartido de archivos de SMB como opción de almacenamiento, puede instalar hasta 50 instancias de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
    -   No se admite dar formato a una unidad después de montar unidades adicionales.  
  
-   La instalación de clústeres de conmutación por error de[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] admite el disco local solo para instalar los archivos tempdb. Asegúrese de que la ruta de acceso especificada para los archivos de datos y registro de tempdb es válida en todos los nodos del clúster. Durante la conmutación por error, si los directorios de tempdb no están disponibles en el nodo de destino de la conmutación por error, el recurso de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no podrá ponerse en línea. Para obtener más información, vea [Tipos de almacenamiento para los archivos de datos](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#StorageTypes) y [Configuración del motor de base de datos - Directorios de datos](../../../database-engine/install-windows/install-sql-server.md).  
  
-   Si implementa un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en componentes de tecnología de interfaz para pequeños equipos de Internet (iSCSI), se recomienda hacerlo con precaución. Para obtener más información, vea [Compatibilidad con SQL Server en componentes de tecnología iSCSI](/troubleshoot/sql/admin/support-iscsi-technology-components).  
  
-   Para obtener más información, vea [Directiva de compatibilidad de SQL Server para Clústeres de Microsoft](/troubleshoot/sql/failover-clusters/support-policy-clustered-configurations).  
  
-   Para obtener más información sobre la configuración correcta de unidades de quórum, vea el artículo de [Información de configuración de la unidad de cuórum](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770620(v=ws.10)).  
  
-   Para instalar un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cuando los archivos de instalación de origen de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y el clúster están en dominios diferentes, copie los archivos de instalación en el dominio actual disponible para los clústeres de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
##  <a name="review-security-considerations"></a><a name="Security"></a> Revisar las consideraciones relativas a la seguridad  
  
-   Para usar el cifrado, instale el certificado del servidor con el nombre DNS completo del clúster WSFC en todos los nodos de los clústeres de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Por ejemplo, si tiene un clúster con dos nodos cuyos nombres son "Test1.DomainName.com" y "Test2.DomainName.com" y una instancia de clústeres de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] denominada "Virtsql", debe obtener un certificado para "Virtsql.DomainName.com" e instalarlo en los nodos test1 y test2. A continuación, puede activar la casilla **Forzar cifrado de protocolo** en el Administrador de configuración de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para configurar el cifrado en los clústeres de conmutación por error.  
  
    > [!IMPORTANT]  
    >  No active la casilla **Forzar cifrado de protocolo** hasta que haya instalado certificados en todos los nodos participantes de la instancia de clústeres de conmutación por error.  
  
-   Para las instalaciones de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en configuraciones en paralelo con versiones anteriores, los servicios de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] deben usar cuentas que solo se encuentran en el grupo de dominios global. Además, las cuentas usadas por los servicios de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no deben aparecer en el grupo local de administradores. Si no se sigue esta directriz, se producirán comportamientos inesperados con respecto a la seguridad.  
  
-   Para crear un clústeres de conmutación por error, debe ser un administrador local con permisos para iniciar sesión como servicio y para actuar como parte del sistema operativo en todos los nodos de la instancia de clústeres de conmutación por error.  
  
-   En [!INCLUDE[nextref_longhorn](../../../includes/nextref-longhorn-md.md)], se generan automáticamente identificadores de seguridad (SID) de servicios para su utilización con los servicios de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] . Para las instancias de clústeres de conmutación por error de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] actualizadas desde versiones anteriores de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], se conservarán los grupos de dominios y las configuraciones de las listas de control de acceso (ACL) existentes.  
  
-   Los grupos de dominio deben estar dentro del mismo dominio que las cuentas de equipo. Por ejemplo, si el equipo donde se va a instalar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se encuentra en el dominio SQLSVR que es un elemento secundario de MYDOMAIN, debe especificar un grupo del dominio SQLSVR. El dominio SQLSVR puede contener cuentas de usuario de MYDOMAIN.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no se pueden instalar cuando los nodos de clúster son controladores de dominio.  
  
-   Revise el contenido de [Security Considerations for a SQL Server Installation](../../../sql-server/install/security-considerations-for-a-sql-server-installation.md).  
  
-   Para habilitar la autenticación Kerberos con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vea el artículo [Cómo utilizar la autenticación Kerberos en SQL Server](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/319723) de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Knowledge Base.  

-   Instancia de clúster de conmutación por error de SQL Server (FCI) requiere que los nodos de clúster estén unidos a un dominio. **No se admiten** las configuraciones siguientes: 
    *   FCI de SQL en clústeres de grupo de trabajo. 
    *   FCI de SQL en el clúster con múltiples dominios.   
    *   FCI de SQL en clústeres de dominio + grupo de trabajo. 

  
##  <a name="review-network-port-and-firewall-considerations"></a><a name="Network"></a> Revisar las consideraciones sobre la red, los puertos y el firewall  
  
-   Antes de iniciar el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , compruebe que ha deshabilitado NetBIOS para todas las tarjetas de red privada.  
  
-   El nombre de red y la dirección IP del servidor [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no deben utilizarse para ningún otro fin, por ejemplo el uso compartido de archivos. Si desea crear un recurso compartido de archivos, utilice un nombre de red y una dirección IP diferentes y únicos para el recurso.  
  
    > [!IMPORTANT]  
    >  Se recomienda no usar recursos compartidos de archivos en unidades de datos, porque pueden afectar al comportamiento y el rendimiento de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Aunque [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] admite canalizaciones con nombre y Sockets TCP/IP sobre TCP/IP en un clúster, se recomienda utilizar Sockets TCP/IP en una configuración en clúster.  
  
-   Observe que el servidor ISA no se admite en la agrupación en clústeres de Windows y, por consiguiente, tampoco se admite en los clústeres de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   El Servicio de Registro remoto debe estar activado y en ejecución.  
  
-   La administración remota debe estar habilitada.  
  
- Para las instancias de SQL Server con un puerto no predeterminado, use la configuración de red del Administrador de configuración de SQL Server para determinar el puerto que usa la instancia de SQL Server que quiere desbloquear. Habilite el puerto TCP para IPALL en el firewall si se quiere conectarse a la instancia de SQL Server mediante el [servicio SQL Server Browser](../../../tools/configuration-manager/sql-server-browser-service.md), que usa una dirección IP diferente de la instancia en clúster y el puerto UDP 1434. 
  
-   Las operaciones de instalación de los clústeres de conmutación por error incluyen una regla que comprueba el orden de los enlaces de red. Aunque el orden de los enlaces pueda parecer correcto, existe la posibilidad de haber deshabilitado configuraciones de NIC o haber dejado configuraciones de NIC "fantasma" en el sistema. Las configuraciones de NIC "fantasma" pueden afectar al orden de los enlaces y hacer que la regla de orden de enlaces emita una advertencia. Para evitar esta situación, realice los pasos siguientes con el fin de identificar y quitar los adaptadores de red deshabilitados:  
  
    1.  En un símbolo del sistema, escriba: set devmgr_Show_Nonpersistent_Devices=1.  
  
    2.  Escriba y ejecute: start Devmgmt.msc.  
  
    3.  Expanda la lista de adaptadores de red. Solo los adaptadores físicos deben estar en la lista. Si tiene un adaptador de red deshabilitado, el programa de instalación notificará un error para la regla de orden de enlaces de red. La utilidad Panel de control/Conexiones de red también mostrará que el adaptador estaba deshabilitado. Confirme que Configuración de red, en el Panel de control, muestra la misma lista de adaptadores físicos habilitados que devmgmt.msc.  
  
    4.  Quite los adaptadores de red deshabilitados antes de ejecutar el programa de instalación de SQL Server.  
  
    5.  Una vez que el programa de instalación finalice, vuelva a Conexiones de red en el Panel de control y deshabilite los adaptadores de red que no se usen actualmente.  
  
##  <a name="verify-your-operating-system"></a><a name="OS_Support"></a> Comprobar el sistema operativo  
 Asegúrese de que el sistema operativo esté correctamente instalado y diseñado para admitir los clústeres de conmutación por error. En la tabla siguiente se muestra una lista de las ediciones de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y de los sistemas operativos que las admiten.  
  
|Edición de[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]|Windows Server 2019 Datacenter|Windows Server 2019 Standard|Windows Server 2016 Datacenter|Windows Server 2016 Standard |  
|---------------------------------------|------------------------------------------------|-------------------------------------------------------|----------------------------------------------|-----------------------------------------------------|  
|[!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] Enterprise (64 bits) x64*|Sí|Sí|Sí|Sí|  
|[!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] Enterprise (32 bits)|Sí|Sí|||  
|[!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] Enterprise |Sí|Sí|Sí|Sí|
|[!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] Standard |Sí|Sí|Sí|Sí|
|[!INCLUDE[sssql17-md](../../../includes/sssql17-md.md)] Enterprise |Sí|Sí|Sí|Sí|
|[!INCLUDE[sssql17-md](../../../includes/sssql17-md.md)] Standard |Sí|Sí|Sí|Sí|
|[!INCLUDE[sssql19-md](../../../includes/sssql19-md.md)] Enterprise |Sí|Sí|Sí|Sí|
|[!INCLUDE[sssql19-md](../../../includes/sssql19-md.md)] Standard |Sí|Sí|Sí|Sí|

 *[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no admite clústeres en el modo WOW. Eso incluye las actualizaciones desde versiones anteriores de clústeres de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que estaban instaladas originalmente en WOW. La única opción de actualización en estos casos es instalar la nueva versión en paralelo y migrar.  

  
##  <a name="additional-considerations-for-multi-subnet-configurations"></a><a name="MultiSubnet"></a> Consideraciones adicionales para configuraciones de varias subredes  
 Las secciones siguientes describen los requisitos que se han de tener en cuenta al instalar un clúster de conmutación por error de varias subredes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . La configuración de varias subredes implica la agrupación en clústeres a través de varias subredes; por lo tanto, implica el uso de múltiples direcciones IP y cambios en las dependencias de recursos de dirección IP.  
  
### <a name="ssnoversion-edition-and-operating-system-considerations"></a>[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Consideraciones sobre el sistema operativo y las ediciones  
  
-   Para obtener más información sobre las ediciones de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que admiten un clúster de conmutación por error de múltiples subredes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
-   Para crear un clúster de conmutación por error de varias subredes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , primero debe crear el clúster de conmutación por error de varias subredes de [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] en varias subredes.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] depende del clúster de conmutación por error de Windows Server para garantizar que las condiciones de dependencia de IP sean válidas si se produce una conmutación por error.  
  
-   [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] requiere que todos los servidores del clúster estén en el mismo dominio de Active Directory. En consecuencia, el clúster de conmutación por error de varias subredes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] requiere que todos los nodos de clúster estén en el mismo dominio de Active Directory aunque estén en subredes distintas.  
  
#### <a name="ip-address-and-ip-address-resource-dependencies"></a>Dependencias de dirección IP y de recursos de dirección IP  
  
1.  La dependencia de recursos de dirección IP se establece en OR en una configuración de varias subredes. Para obtener más información, vea [Crear un nuevo clúster de conmutación por error de SQL Server &#40;programa de instalación&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md).  
  
2.  No se admiten las dependencias mixtas AND-OR de direcciones IP. Por ejemplo, no se admiten \<IP1> AND \<IP2> OR \<IP3>.  
  
3.  No se admite más de una dirección IP por cada subred.  
  
     Si decide usar más de una dirección IP configurada para la misma subred, puede experimentar errores de conexión de cliente durante el inicio de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
#### <a name="related-content"></a>Contenido relacionado  
 Para obtener más información sobre la conmutación por error de varios sitios de [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] , vea [Failover Clusters in Windows Server 2008 R2](https://technet.microsoft.com/library/ff182338\(v=WS.10\).aspx) (Clústeres de conmutación por error de Windows Server 2008 R2) y [Design for a Clustered Service or Application in a Multi-Site Failover Cluster](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197430(v=ws.10))(Diseño de una aplicación o un servicio de clúster en un clúster de conmutación por error de varios sitios).  
  
##  <a name="configure-windows-server-failover-cluster"></a><a name="WSFC"></a> Configurar los clústeres de conmutación por error de Windows Server  
  
-   [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Cluster Service (WSFC) debe configurarse al menos en un nodo del clúster de servidores. También debe ejecutar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Enterprise, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Business Intelligence o [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Standard en combinación con WSFC. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Enterprise admite clústeres de conmutación por error con un máximo de 16 nodos. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Business Intelligence y [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Standard admiten clústeres de conmutación por error de dos nodos.  
  
-   La DLL de recursos para el servicio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] exporta dos funciones usadas por el Administrador de clústeres de WSFC para comprobar la disponibilidad del recurso de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Para obtener más información, vea [Directiva de conmutación por error para instancias de clústeres de conmutación por error](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md).  
  
-   WSFC debe poder comprobar que la instancia en clústeres de conmutación por error está en ejecución mediante la comprobación IsAlive. Esto requiere conectarse al servidor mediante una conexión de confianza. De forma predeterminada, la cuenta que ejecuta el servicio de clúster no está configurada como administrador en los nodos del clúster y el grupo BUILTIN\Administradores no tiene permiso para iniciar sesión en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Esta configuración solo cambia si se cambian los permisos para los nodos del clúster.  
  
-   Configure el Servicio de nombres de dominio (DNS) o el Servicio de nombres Internet de Windows (WINS). En el entorno donde se va a instalar los clústeres de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] debe estar ejecutándose un servidor DNS o WINS. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] requiere el registro del servicio de nombres de dominio dinámicos de la referencia virtual de la interfaz IP de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . La configuración del servidor DNS debe permitir que los nodos de clúster registren dinámicamente un mapa de direcciones IP en línea en el nombre de red. Si no se puede completar el registro dinámico, se produce un error en el programa de instalación y esta se revierte. Para obtener más información, vea [este artículo de Knowledge Base](https://mskb.pkisolutions.com/kb/947048).  
  
##  <a name="install-msconame-distributed-transaction-coordinator"></a><a name="MSDTC"></a> Instalar el Coordinador de transacciones distribuidas de [!INCLUDE[msCoName](../../../includes/msconame-md.md)]  
 Antes de instalar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en un clúster de conmutación por error, determine si debe crearse el recurso de clúster de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] DTC (Coordinador de transacciones distribuidas). Si solo instala el [!INCLUDE[ssDE](../../../includes/ssde-md.md)], no será necesario el recurso de clúster de MSDTC. Si está instalando el [!INCLUDE[ssDE](../../../includes/ssde-md.md)] y SSIS o Componentes de la estación de trabajo o si va a usar transacciones distribuidas, debe instalar MSDTC. Observe que MSDTC no se requiere para las instancias que son solo para [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
 En [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] y [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)], puede instalar varias instancias de MSDTC en un único clúster de conmutación por error. La primera instancia de MSDTC instalada será la instancia predeterminada del clúster de MSDTC. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] aprovechará una instancia de MSDTC instalada en el grupo de recursos del clúster local de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilizando automáticamente la instancia de MSDTC. Sin embargo, las aplicaciones individuales pueden estar asignadas a cualquier instancia de MSDTC en el clúster.  
  
 A la hora de que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]elija una instancia de MSDTC, se aplican las siguientes reglas:  
  
-   Usar la instancia de MSDTC instalada en el grupo local; si no es posible,  
  
-   usar la instancia asignada de MSDTC; si no es posible,  
  
-   usar la instancia predeterminada del clúster de MSDTC; si no es posible,  
  
-   Usar la instancia de MSDTC instalada del equipo local  
  
> [!IMPORTANT]  
>  Si la instancia de MSDTC que se instala en el grupo de clústeres local de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] produce un error, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no intenta automáticamente utilizar la instancia del clúster predeterminado o la instancia del equipo local de MSDTC. Para poder utilizar otra instancia de MSDTC, se debería quitar completamente del grupo de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] la instancia de MSDTC que produjo el error . Igualmente, si crea una asignación para [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y la instancia asignada de MSDTC produce un error, las transacciones distribuidas también producirán un error. Si desea que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilice una instancia diferente de MSDTC, deberá agregar una instancia de MSDTC al grupo de clústeres local de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o eliminar la asignación.  
  
### <a name="configure-msconame-distributed-transaction-coordinator"></a>Configurar el Coordinador de transacciones distribuidas de [!INCLUDE[msCoName](../../../includes/msconame-md.md)]  
 Tras instalar el sistema operativo y configurar el clúster, debe configurar MSDTC para que funcione en un clúster mediante el Administrador de clústeres. Si no logra crear el clúster de MSDTC, no se bloqueará el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , pero la funcionalidad de la aplicación [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] puede verse afectada si MSDTC no se configura correctamente.  
  
## <a name="see-also"></a>Consulte también  
 [Requisitos de hardware y software para instalar SQL Server 2016](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)   
 [Comprobar los parámetros del Comprobador de configuración del sistema](../../../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md)   
 [Administración y mantenimiento de la instancia de clúster de conmutación por error](../../../sql-server/failover-clusters/windows/failover-cluster-instance-administration-and-maintenance.md)  
  
