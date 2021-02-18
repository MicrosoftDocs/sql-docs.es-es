---
title: Consideraciones sobre la seguridad
description: En este artículo se describen algunos procedimientos recomendados de seguridad que debe tener en cuenta antes y después de instalar SQL Server.
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- firewall systems [SQL Server]
- server message blocks [SQL Server]
- disabling protocols
- security [SQL Server], installations
- protocols [SQL Server], disabling
- NetBIOS [SQL Server]
- services [SQL Server], security
- SMB [SQL Server]
- isolating services [SQL Server]
- Setup [SQL Server], security
- low SQL Server installation privileges
- NTFS [SQL Server]
- physical security [SQL Server]
- file system security [SQL Server]
- installing SQL Server, security
ms.assetid: cf96155f-30a8-48b7-8d6b-24ce90dafdc7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5f8a540cf1f740ad2f9cb712db4bdd51f6c1a623
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336120"
---
# <a name="security-considerations-for-a-sql-server-installation"></a>Consideraciones de seguridad para una instalación de SQL Server
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

 La seguridad es importante para todos los productos y negocios. Si aplica las siguientes prácticas recomendadas de seguridad, puede evitar muchas vulnerabilidades de seguridad. En este artículo se tratan algunas prácticas recomendadas de seguridad que deben tenerse en cuenta antes de instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y después de instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En los artículos de referencia para estas características se incluyen directrices de seguridad para características específicas.  
  
## <a name="before-installing-ssnoversion"></a>Antes de instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Siga estas prácticas recomendadas cuando configure el entorno del servidor.  
  
-   [Mejorar la seguridad física](#physical_security)  
  
-   [Usar firewalls](#firewalls)  
  
-   [Aislar servicios](#isolated_services)  
  
-   [Configurar un sistema de archivos seguro](#sa_with_least_privileges)  
  
-   [Deshabilitar NetBIOS y Bloque de mensajes de servidor](#disabled_protocols)  
  
-   [Instalar SQL Server en un controlador de dominio](../../sql-server/install/security-considerations-for-a-sql-server-installation.md#Install_DC)  
  
###  <a name="enhance-physical-security"></a><a name="physical_security"></a> Enhance Physical Security  
 El aislamiento físico y lógico constituye la base de la seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para mejorar la seguridad física de la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , realice las siguientes tareas:  
  
-   Coloque el servidor en una sala que solo sea accesible a personas autorizadas.  
  
-   Coloque los equipos que hospedan bases de datos en una ubicación protegida físicamente, como una sala de equipos cerrada con sistemas supervisados de detección de inundaciones y de extinción o detección de incendios.  
  
-   Instale las bases de datos en una zona segura de la intranet corporativa y no conecte sus servidores SQL Server directamente a Internet.  
  
-   Realice periódicamente copias de seguridad de todos los datos y manténgalas protegidas en una ubicación fuera de las instalaciones.  
  
###  <a name="use-firewalls"></a><a name="firewalls"></a> Use Firewalls  
 Los firewalls son importantes para ayudar a proteger la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Los firewalls serán más efectivos si sigue estas instrucciones:  
  
-   Instale un firewall entre el servidor e Internet. Habilite el firewall. Si el firewall está desactivado, actívelo. Si el firewall está activado, no lo desactive.  
  
-   Divida la red en zonas de seguridad separadas por firewalls. Bloquee todo el tráfico y, a continuación, admita solo el necesario.  
  
-   En un entorno de varios niveles, utilice varios firewalls para crear subredes filtradas.  
  
-   Si instala el servidor en un dominio de Windows, configure firewalls internos para permitir la autenticación de Windows.  
  
-   Si la aplicación utiliza transacciones distribuidas, puede que tenga que configurar el firewall para permitir que el tráfico del Coordinador de transacciones distribuidas de [!INCLUDE[msCoName](../../includes/msconame-md.md)] (MS DTC, Microsoft Distributed Transaction Coordinator) fluya entre instancias independientes de MS DTC. También tendrá que configurar el firewall para permitir que el tráfico fluya entre los administradores de recursos y MS DTC como [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Para obtener más información sobre la configuración predeterminada de Firewall de Windows y una descripción de los puertos TCP que afectan al [!INCLUDE[ssDE](../../includes/ssde-md.md)], a [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], a [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]y a [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vea [Configurar Firewall de Windows para permitir el acceso a SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
###  <a name="isolate-services"></a><a name="isolated_services"></a> Isolate Services  
 El aislamiento de servicios reduce el riesgo de que se utilice un servicio cuya seguridad se haya vulnerado para vulnerar la seguridad de otros servicios. Para aislar los servicios, tenga en cuenta estas instrucciones:  
  
-   Ejecute los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] por separado en distintas cuentas de Windows. Siempre que sea posible, utilice derechos de Windows independientes y bajos, o cuentas de usuario local para cada servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener más información, consulte [Configurar los permisos y las cuentas de servicio de Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
###  <a name="configure-a-secure-file-system"></a><a name="sa_with_least_privileges"></a> Configure a Secure File System  
 Si se usa el sistema de archivos correcto, se aumenta la seguridad. En las instalaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , debería hacer lo siguiente:  
  
-   Usar el sistema de archivos NTFS. NTFS es el sistema de archivos preferido para las instalaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] porque es más estable y recuperable que los sistemas de archivos FAT. NTFS también habilita opciones de seguridad como las listas de control de acceso de directorios (ACL, Access Control List) y archivos, y el cifrado de archivos del Sistema de archivos de cifrado (EFS, Encrypting File System). Durante la instalación, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] establecerá las ACL adecuadas en las claves del Registro y archivos si detecta NTFS. No se deberían cambiar estos permisos. Puede que las versiones futuras de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no admitan la instalación en equipos con sistemas de archivos FAT.  
  
    > [!NOTE]  
    >  Si utiliza EFS, los archivos de base de datos se cifrarán con la identidad de la cuenta que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Solo esta cuenta podrá descifrar los archivos. Si debe cambiar la cuenta que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], debería descifrar primero los archivos en la cuenta anterior y, a continuación, volver a cifrarlos en la cuenta nueva.  
  
-   Utilice una matriz redundante de discos independientes (RAID) para los archivos de datos esenciales.  
  
###  <a name="disable-netbios-and-server-message-block"></a><a name="disabled_protocols"></a> Disable NetBIOS and Server Message Block  
 Los servidores de la red perimetral deben tener deshabilitados todos los protocolos innecesarios, incluidos NetBIOS y Bloque de mensajes de servidor (SMB).  
  
 NetBIOS utiliza los siguientes puertos:  
  
-   UDP/137 (servicio de nombre NetBIOS)  
  
-   UDP/138 (servicio de datagrama NetBIOS)  
  
-   TCP/139 (servicio de sesión NetBIOS)  
  
 SMB utiliza los siguientes puertos:  
  
-   TCP/139  
  
-   TCP/445  
  
 Los servidores web y los servidores del Sistema de nombres de dominio (DNS) no requieren NetBIOS o SMB. En estos servidores, deshabilite los dos protocolos para reducir la amenaza de enumeración de usuarios.  
  
###  <a name="installing-ssnoversion-on-a-domain-controller"></a><a name="Install_DC"></a> Instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un controlador de dominio  
 Por razones de seguridad, se recomienda que no se instale [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un controlador de dominio. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no bloqueará la instalación en un equipo que sea un controlador de dominio, pero se aplican las limitaciones siguientes:  
  
-   No puede ejecutar los servicios [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un controlador de dominio bajo una cuenta del servicio local.  
  
-   Una vez instalado en un equipo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , no puede cambiar el equipo de un miembro de dominio a un controlador de dominio. Debe desinstalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antes de cambiar el equipo host a un controlador de dominio.  
  
-   Una vez instalado en un equipo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , no puede cambiar el equipo de un controlador de dominio a un miembro de dominio. Debe desinstalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antes de cambiar el equipo host a un miembro de dominio.  
  
-   Las instancias del clúster de conmutación por error de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se admiten si los nodos de clúster son controladores de dominio.  
  
-   El programa de instalación de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede crear grupos de seguridad ni suministrar cuentas de servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un controlador de dominio de solo lectura. En este escenario, se producirá un error de instalación.  
  
## <a name="during-or-after-installation-of-ssnoversion"></a>Durante o después de la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Tras la instalación, puede mejorar la seguridad de la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si sigue estas prácticas recomendadas relativas a las cuentas y los modos de autenticación:  
  
 **Cuentas de servicio**  
  
-   Ejecute los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con los mínimos permisos posibles.  
  
-   Asocie los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a las cuentas de usuario local de Windows con menos privilegios, o a las cuentas de usuario de dominio.  
  
-   Para obtener más información, consulte [Configurar los permisos y las cuentas de servicio de Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
 **Modo de autenticación**  
  
-   Requiera la autenticación de Windows para las conexiones a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Utilice la autenticación Kerberos. Para obtener más información, vea [Registrar un nombre de entidad de seguridad de servicio para las conexiones con Kerberos](../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md).  
  
 **Contraseñas seguras**  
  
-   Asigne una contraseña segura a la cuenta **sa** .  
  
-   Habilite siempre la directiva de contraseñas comprobando el nivel y la fecha de expiración de la contraseña.  
  
-   Use siempre contraseñas seguras en todos los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  Durante la instalación de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] se agrega un inicio de sesión para el grupo BUILTIN\Users. Esto permite que todos los usuarios autenticados del equipo tengan acceso a la instancia de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] como un miembro del rol public. El inicio de sesión BUILTIN\Users se pueden quitar de manera segura para restringir el acceso al [!INCLUDE[ssDE](../../includes/ssde-md.md)] a los usuarios de equipos que tienen inicios de sesión individuales o son miembros de otros grupos de Windows con inicios de sesión.  
  
## <a name="see-also"></a>Consulte también  
 [Requisitos de hardware y software para instalar SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)   
 [Protocolos de red y bibliotecas de red](../../sql-server/install/network-protocols-and-network-libraries.md)   
 [Registrar un nombre principal de servicio para las conexiones con Kerberos](../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md)  
  
  
