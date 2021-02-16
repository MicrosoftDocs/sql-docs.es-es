---
title: ¿Qué es una escucha de grupo de disponibilidad?
description: 'Información general sobre la escucha de grupo de disponibilidad Always On y cómo funciona para dirigir el tráfico automáticamente al servidor previsto. '
ms.custom: seodec18
ms.date: 02/27/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], listeners
- read-only routing
- read-intent connections [AlwaysOn Availability Groups]
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], read-only routing
- Availability Groups [SQL Server], client connectivity
ms.assetid: 76fb3eca-6b08-4610-8d79-64019dd56c44
author: cawrites
ms.author: chadam
ms.openlocfilehash: e96b57c13cccf07424ce43aab74fde8b4fa43d08
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340853"
---
# <a name="what-is-an-availability-group-listener"></a>¿Qué es una escucha de grupo de disponibilidad?  
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Una escucha de grupo de disponibilidad es un nombre de red virtual (VNN) al que los clientes se pueden conectar para acceder a una base de datos situada en una réplica principal o secundaria de un grupo de disponibilidad Always On. Un cliente de escucha permite a un cliente conectarse a una réplica sin tener que conocer el nombre de la instancia física de SQL Server. Como el cliente de escucha enruta el tráfico, no es necesario modificar la cadena de conexión del cliente después de que se produzca una conmutación por error. 

Un agente de escucha de grupo de disponibilidad se compone de un nombre de agente de escucha del Sistema de nombres de dominio (DNS), de una designación de puerto de agente de escucha y de una o varias direcciones IP. Solo el protocolo TCP es compatible con el agente de escucha del grupo de disponibilidad.  El nombre DNS del agente de escucha debe ser único en el dominio y en NetBIOS.  Cuando se crea un cliente de escucha, se convierte en un recurso de un clúster con un nombre de red virtual (VNN), una dirección IP virtual (VIP) y una dependencia de grupo de disponibilidad asociados. Un cliente utiliza DNS para resolver VNN en varias direcciones IP y después intenta conectarse a cada dirección hasta que una solicitud de conexión tiene éxito o hasta que se agota el tiempo de espera de las solicitudes de conexión.  
  
Si el enrutamiento de solo lectura se configura para una o varias [réplicas secundarias legibles](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md), las conexiones de cliente con intención de lectura dirigidas al cliente de escucha se redirigen de forma automática a una réplica secundaria legible. 
  
En este artículo se ofrece información general sobre una escucha de grupo de disponibilidad. También puede [configurar el cliente de escucha](create-or-configure-an-availability-group-listener-sql-server.md) y, después, obtener información sobre cómo [conectarse a él](listeners-client-connectivity-application-failover.md).
  
  
##  <a name="listener-parameters"></a><a name="AGlConfig"></a> Parámetros de cliente de escucha  

 Una escucha de grupo de disponibilidad se define por lo siguiente:
  
 **Un nombre de DNS único**  
 Esto también se conoce como Nombre de red virtual (VNN). Se aplican las reglas de nomenclatura de Active Directory para los nombres de host DNS. Para obtener más información, vea el artículo de KB [Convenciones de nomenclatura en Active Directory para equipos, dominios, sitios y unidades organizativas](https://support.microsoft.com/kb/909264) .  
  
**Una o varias direcciones IP virtuales (VIP)**  
 Las direcciones VIP se configuran para una o varias subredes en las que el grupo de disponibilidad puede realizar la conmutación por error.  
  
**Configuración de dirección IP**  
 Para una escucha de grupo de disponibilidad determinada, la dirección IP usa el Protocolo de configuración dinámica de host (DHCP), o bien una o varias direcciones IP estáticas. El uso de DHCP puede producir retrasos de conectividad durante la conmutación por error, por lo que no se recomienda en entornos de producción. Los grupos de disponibilidad que se extienden a través de varias subredes, o que usan configuraciones de red híbridas, deben usar una dirección IP estática. 
 
  
##  <a name="listener-port"></a><a name="SelectListenerPort"></a> Puerto de cliente de escucha 
 Al configurar un agente de escucha del grupo de disponibilidad, debe designar un puerto.  Puede configurar el puerto predeterminado en 1433 para permitir la sencillez de las cadenas de conexión de cliente. Si utiliza 1433, no necesita indicar un número de puerto en una cadena de conexión. Además, puesto que cada agente de escucha del grupo de disponibilidad tendrá un nombre de red virtual independiente, cada agente de escucha del grupo de disponibilidad configurado en un único WSFC se puede configurar para hacer referencia al mismo puerto predeterminado 1433.  
  
 También puede diseñar un puerto de agente de escucha no estándar, aunque esto significa que también necesitará especificar explícitamente un puerto de destino en la cadena de conexión cada vez que se conecte al agente de escucha del grupo de disponibilidad.  También necesitará abrir permiso en el firewall para el puerto no estándar.  
  
 Si utiliza el puerto predeterminado 1433 para los VNN del agente de escucha del grupo de disponibilidad, seguirá siendo necesario asegurarse de que ningún otro servicio en el nodo de clúster esté utilizando este puerto; de lo contrario, se produciría un conflicto de puerto.  
  
 Si una de las instancias de SQL Server ya está escuchando en el puerto TCP 1433 a través del agente de escucha de la instancia y no hay otros servicios (incluidas las instancias adicionales de SQL Server) en el equipo que escuchen en el puerto 1433, no se producirá un conflicto de puerto con el agente de escucha del grupo de disponibilidad.  Esto se debe a que el agente de escucha del grupo de disponibilidad puede compartir el mismo puerto TCP en el mismo proceso del servicio.  Sin embargo, no se deben configurar varias instancias de SQL Server (en paralelo) para escuchar en el mismo puerto.  
  
  
##  <a name="behavior-of-client-connections-on-failover"></a><a name="CCBehaviorOnFailover"></a> Comportamiento de conexiones de cliente en la conmutación por error  

 Cuando se produce una conmutación por error de grupo de disponibilidad, las conexiones persistentes existentes al grupo de disponibilidad se terminan y el cliente debe establecer una nueva conexión para continuar trabajando con la misma base de datos principal o una base de datos secundaria de solo lectura.  Mientras una conmutación por error se produce en el servidor, la conectividad al grupo de disponibilidad puede sufrir un error, forzando a la aplicación cliente a volver a intentar la conexión hasta que el servidor principal vuelve a ponerse totalmente en línea.  
  
 Si el grupo de disponibilidad vuelve a ponerse en línea durante el intento de conexión de una aplicación cliente pero antes del tiempo de espera de conexión, el controlador cliente puede conectarse correctamente durante uno de los intentos internos de reintento y no se producirá ningún error en la aplicación en este caso.  


## <a name="next-steps"></a>Pasos siguientes

Ahora que está familiarizado con el funcionamiento de una escucha de grupo de disponibilidad, [cree el cliente de escucha](create-or-configure-an-availability-group-listener-sql-server.md) y, después, configure la aplicación para que [se conecte al cliente de escucha](listeners-client-connectivity-application-failover.md). También puede revisar varias [estrategias de supervisión de grupos de disponibilidad](monitoring-of-availability-groups-sql-server.md) para garantizar el estado del grupo de disponibilidad. 

Para obtener más información sobre los grupos de disponibilidad, vea [Información general de los grupos de disponibilidad Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md). 
  

  
  
