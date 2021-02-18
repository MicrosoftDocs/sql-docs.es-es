---
title: Protocolos de red y bibliotecas de red | Microsoft Docs
description: Un servidor se puede configurar para que supervise varios protocolos de red. Puede cambiar la configuración mediante el Administrador de configuración de SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- protocols [SQL Server]
- configuration options [SQL Server], protocols
- network libraries [SQL Server]
- protocols [SQL Server], about network protocols
- pipes [SQL Server]
- network protocols [SQL Server]
- default SQL Server configurations
- library [SQL Server]
- network protocols [SQL Server], about network protocols
- configuration options [SQL Server], libraries
ms.assetid: 8cd437f6-9af1-44ce-9cb0-4d10c83da9ce
author: cawrites
ms.author: chadam
ms.openlocfilehash: 81bf039f0a63aaad7a2214c48bb5948618e7ce7e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336159"
---
# <a name="network-protocols-and-network-libraries"></a>Protocolos de red y bibliotecas de red
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Un servidor puede escuchar en, o supervisar, varios protocolos de red al mismo tiempo. Sin embargo, cada protocolo debe estar configurado. Si un protocolo concreto no está configurado, el servidor no podrá escuchar en dicho protocolo. Después de la instalación, podrá cambiar las configuraciones de protocolo mediante el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="default-sql-server-network-configuration"></a>Configuración de red de SQL Server predeterminada  
 Se configura una instancia predeterminada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para el puerto TCP/IP 1433 y la canalización con nombre \\\\.\pipe\sql\query. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se configuran para puertos dinámicos TCP, con un número de puerto asignado por el sistema operativo.  
  
 Si no puede utilizar direcciones de puerto dinámicas (por ejemplo, cuando las conexiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deben pasar por un servidor de firewall configurado pasar a través de direcciones de puerto específicas). Seleccione un número de puerto sin asignar. La Internet Assigned Numbers Authority administra las asignaciones del número de puerto, que se muestran en [https://www.iana.org](https://go.microsoft.com/fwlink/?LinkId=48844).  
  
 Para mejorar la seguridad, la conectividad de red no se habilita totalmente al instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para habilitar, deshabilitar y configurar protocolos de red después de completar la instalación, utilice el área Configuración de red de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="server-message-block-protocol"></a>Protocolo Bloque de mensajes del servidor  
 Los servidores de la red perimétrica deben tener todos los protocolos innecesarios deshabilitados, incluido el bloque de mensajes del servidor (SMB). Los servidores web y los servidores del Sistema de nombres de dominio (DNS) no necesitan SMB. Este protocolo debería deshabilitarse para contrarrestar la amenaza de enumeración de usuarios.  
  
> [!WARNING]
>  Si deshabilita el Bloque de mensajes de servidor, se bloqueará [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o el Servicio de clúster de Windows para tener acceso al recurso compartido de archivos remoto. No deshabilite SMB si hace algo de lo siguiente o prevé hacerlo:  
> 
>  -   Usar el modo de quórum de la mayoría del recurso compartido de archivos y el Nodo de clúster de Windows  
> -   Especificar un recurso compartido de archivos SMB como directorio de datos durante la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
> -   Crear un archivo de base de datos en un recurso compartido de archivos SMB  
  
#### <a name="to-disable-smb"></a>Para deshabilitar SMB  
  
1.  En el menú **Inicio** , seleccione **Configuración** y, después, haga clic en **Conexiones de red y de acceso telefónico**.  
  
     Haga clic con el botón derecho en la conexión con Internet y, después, haga clic en **Propiedades**.  
  
2.  Active la casilla **Cliente para redes Microsoft** y, a continuación, haga clic en **Desinstalar**.  
  
3.  Siga los pasos de desinstalación.  
  
4.  Seleccione **Compartir impresoras y archivos para redes Microsoft** y, a continuación, haga clic en **Desinstalar**.  
  
5.  Siga los pasos de desinstalación.  
  
#### <a name="to-disable-smb-on-servers-accessible-from-the-internet"></a>Para deshabilitar SMB en servidores accesibles desde Internet  
  
-   En las propiedades de Conexión de área local, utilice el cuadro de diálogo **Propiedades de Protocolo de control de transporte/Protocolo Internet (TCP/IP)** para quitar **Impresoras y archivos para redes Microsoft** y **Cliente para redes Microsoft**.  
  
## <a name="endpoints"></a>Puntos de conexión  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] presenta un nuevo concepto para conexiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; la conexión se representa en el extremo de servidor mediante un [!INCLUDE[tsql](../../includes/tsql-md.md)]*de*. Se pueden otorgar, revocar y denegar permisos para extremos de [!INCLUDE[tsql](../../includes/tsql-md.md)] . De manera predeterminada, todos los usuarios tienen permisos para obtener acceso a un extremo, a menos que los permisos sean denegados o revocados por un miembro del grupo sysadmin o por el propietario del extremo. La sintaxis GRANT, REVOKE y DENY ENDPOINT utiliza un Id. de extremo que el administrador debe obtener de la vista de catálogo del extremo.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea extremos de [!INCLUDE[tsql](../../includes/tsql-md.md)] para todos los protocolos de red admitidos, así como para la conexión de administrador dedicada.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] creados por el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] son los siguientes:  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] equipo local  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] canalizaciones con nombre  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] Puerto TCP predeterminado  
  
 Para obtener más información sobre los puntos de conexión, vea [Configurar el motor de base de datos para escuchar en varios puertos TCP](../../database-engine/configure-windows/configure-the-database-engine-to-listen-on-multiple-tcp-ports.md) y [Vistas de catálogo de puntos de conexión &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md).  
  
 Para más información sobre las configuraciones de red de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea estos artículos en los Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
-   [Configuración de red del servidor](../../database-engine/configure-windows/server-network-configuration.md)  
  
## <a name="see-also"></a>Consulte también  
 [Configuración de Área expuesta](../../relational-databases/security/surface-area-configuration.md)   
 [Consideraciones de seguridad para una instalación de SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)   
 [Planear una instalación de SQL Server](../../sql-server/install/planning-a-sql-server-installation.md)  
  
  
