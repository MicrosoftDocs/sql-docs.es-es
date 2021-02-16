---
title: Página General (Propiedades de las réplicas de disponibilidad)
description: Una descripción de las distintas propiedades de la página "General" de la página "Propiedades de réplica de disponibilidad" en SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.availabilityreplicaproperties.general.f1
ms.assetid: 8318fefb-e045-4fab-8507-e1951fc7cec6
author: cawrites
ms.author: chadam
ms.openlocfilehash: e81251f9d5cb1f87f3b823b9fc23bcb69c26e79e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343829"
---
# <a name="availability-replica-properties-general-page-for-always-on-availability-groups"></a>Propiedades de las réplicas de disponibilidad (página General) para grupos de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Use este cuadro de diálogo para ver las propiedades de una réplica de disponibilidad.  
  
## <a name="task-list"></a>Lista de tareas  
 **Para ver las propiedades de una réplica de disponibilidad**  
  
-   [Ver las propiedades de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
-   [Usar el Panel de AlwaysOn &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
## <a name="ui-element-list"></a>Lista de elementos de la interfaz de usuario  
 **Nombre del grupo de disponibilidad**  
 Nombre del grupo de disponibilidad. Es un nombre definido por el usuario que debe ser único dentro del clúster de conmutación por error de Windows Server (WSFC).  
  
 **Instancia del servidor**  
 Nombre de servidor de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que hospeda esta réplica y, para una instancia no predeterminada, su nombre de instancia.  
  
 **Rol**  
 **Principal**  
 Actualmente la réplica principal.  
  
 **Secundario**  
 Actualmente una réplica secundaria.  
  
 **Resolver**  
 Actualmente el rol de la réplica están en proceso de resolverse al rol principal o al rol secundario.  
  
 **Modo de disponibilidad**  
 Modo de disponibilidad de la réplica, que puede ser alguno de los siguientes:  
  
 **Confirmación asincrónica**  
 La réplica principal puede confirmar transacciones sin esperar a que la réplica secundaria escriba el registro en disco.  
  
 **Confirmación sincrónica**  
 La réplica principal espera para confirmar una determinada transacción hasta que la réplica secundaria escribe la transacción en el disco.  
  
 Para obtener más información, vea [Modos de disponibilidad &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
 **Failover mode**  
 Modo de conmutación por error de la réplica, que puede ser uno de los siguientes:  
  
 **Automático**  
 Conmutación por error automática. La réplica es un destino de las conmutaciones por error automáticas. Esto solo se admite si el modo de disponibilidad se establece en confirmación sincrónica.  
  
 **Manual**  
 Conmutación por error manual. La réplica solo puede ser objeto de conmutación por error manual por parte del administrador de base de datos.  
  
 **Conexiones el en rol principal**  
 El tipo de conexiones de cliente admitidas cuando la réplica posee el rol principal.  
  
 **Permitir todas las conexiones**  
 Se permiten todas las conexiones con las bases de datos de la réplica principal. Esta es la configuración predeterminada.  
  
 **Permitir conexiones de lectura o escritura**  
 No se permiten las conexiones en las que la propiedad de conexión Application Intent esté establecida en **ReadOnly** . Cuando la propiedad Application Intent está establecida en **ReadWrite** o la propiedad Application Intent Connection no tiene ningún valor, se permite la conexión.  
  
 **Secundario legible**  
 Si una réplica de disponibilidad que está realizando el rol secundario (es decir, una réplica secundaria) puede aceptar conexiones de clientes; puede tener uno de los valores siguientes:  
  
 **No**  
 No se permiten conexiones directas a las bases de datos secundarias de esta réplica. No están disponibles para acceso de lectura. Esta es la configuración predeterminada.  
  
 **Solo intento de lectura**  
 Únicamente se permiten conexiones directas de solo lectura a las bases de datos secundarias de esta réplica. Todas las bases de datos secundarias están disponibles para acceso de lectura.  
  
 **Sí**  
 Se permiten todas las conexiones a las bases de datos secundarias de esta réplica, pero solo para acceso de lectura. Todas las bases de datos secundarias están disponibles para acceso de lectura.  
  
 Para más información, consulte [Secundarias activas: réplicas secundarias legibles &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
 **Tiempo de espera de sesión (segundos)**  
 Período de tiempo de espera, en segundos. El período de tiempo de espera es el tiempo máximo que la réplica espera hasta recibir un mensaje de otra réplica antes de considerar que se ha producido un error en la conexión entre la réplica principal y la secundaria. El tiempo de espera de la sesión detecta si las réplicas secundarias están conectadas a la réplica principal. Al detectar un error en la conexión con una réplica secundaria, la réplica principal considera que la réplica secundaria no se ha sincronizado (NOT_SYNCHRONIZED). Al detectar un error en la conexión con la réplica principal, la réplica secundaria intenta volver a conectarse.  
  
> [!NOTE]  
>  Cuando se agota el tiempo de espera de la sesión, no se realiza una conmutación por error automática.  
  
 **Dirección URL del extremo**  
 Representación en forma de cadena de la base de datos definida por el usuario que crea un reflejo del extremo usado por las conexiones entre las réplicas principal y secundaria para la sincronización de datos. Para obtener más información sobre la sintaxis de las direcciones URL del punto de conexión, vea [Especificar la dirección URL del punto de conexión al agregar o modificar una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md).  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
