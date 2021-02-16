---
title: Grupos de disponibilidad básicos para una sola base de datos
description: 'Se describen las diferencias entre un grupo de disponibilidad Always On normal y básico, así como la forma de configurar un grupo de disponibilidad básico. '
ms.custom: seodec18
ms.date: 02/01/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 285adbc7-ac9b-40f6-b4a9-3f1591d3b632
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1308a8b703a07c961a5230affd694eb40e13bb54
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343841"
---
# <a name="basic-always-on-availability-groups-for-a-single-database"></a>Grupos de disponibilidad Always On básicos para una sola base de datos
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

 Los grupos de disponibilidad Always On ofrecen una solución de alta disponibilidad para SQL Server desde la versión 2016 y superiores en Standard Edition. Un grupo de disponibilidad básica admite un entorno de conmutación por error para una base de datos única. Se crea y administra prácticamente igual que [Grupos de disponibilidad Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md) tradicional (avanzado) con Enterprise Edition. En este documento se resumen las diferencias y limitaciones de los grupos de disponibilidad básica.

## <a name="features"></a>Características  
 Grupos de disponibilidad básica AlwaysOn reemplaza la característica en desuso de creación de reflejo de la base de datos y proporciona un nivel similar de compatibilidad de características. Los grupos de disponibilidad básica permiten que una base de datos principal mantenga una réplica única. Esta réplica puede usar el modo de confirmación sincrónica o el modo de confirmación asincrónica. Para obtener más información sobre los modos de disponibilidad, vea [Modos de disponibilidad &#40;Grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md). La réplica secundaria permanece inactiva a menos que exista una necesidad de conmutación por error. Esta conmutación por error invierte las asignaciones principales y secundarias de roles, lo que provoca que la réplica secundaria se convierta en la principal base de datos activa. Para obtener más información sobre la conmutación por error, vea [Conmutación por error y modos de conmutación por error &#40;Grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md). Los grupos de disponibilidad básica pueden funcionar en un entorno híbrido que abarca el local y Microsoft Azure.  
  
## <a name="limitations"></a>Limitaciones  
 Los grupos de disponibilidad básica usan un subconjunto de características en comparación con los grupos de disponibilidad avanzada en SQL Server 2016 Enterprise Edition. Los grupos de disponibilidad básica incluyen las siguientes limitaciones:  
  
- Límite de dos réplicas (principal y secundaria). Los grupos de disponibilidad básica para SQL Server 2017 en Linux admiten una única réplica de configuración adicional.
  
- Sin acceso de lectura en la réplica secundaria.  
  
- Sin copias de seguridad en la réplica secundaria.  

- Sin comprobaciones de integridad en las réplicas secundarias. 

- No se admiten réplicas hospedadas en servidores que ejecutan una versión de SQL Server anterior a SQL Server 2016 Community Technology Preview 3 (CTP3).  

- Admite una base de datos de disponibilidad.  
  
- Los grupos de disponibilidad básica no pueden actualizarse a los grupos de disponibilidad avanzada. El grupo debe quitarse y agregarse de nuevo a un grupo que contenga servidores que solo ejecuten SQL Server 2016 Enterprise Edition.  
  
- Los grupos de disponibilidad básica solo son compatibles con los servidores Standard Edition. 

- Los grupos de disponibilidad básica no pueden formar parte de un grupo de disponibilidad distribuido. 

- Puede tener varios grupos de disponibilidad básica conectados a una única instancia de SQL Server.

  
## <a name="configuration"></a>Configuración  
 Un grupo de disponibilidad básica AlwaysOn se puede crear en dos servidores SQL Server 2016 Standard Edition. Cuando crea un grupo de disponibilidad básica, debe especificar ambas réplicas durante la creación.  
  
 Para crear un grupo de disponibilidad básica, use el comando Transact-SQL **CREATE AVAILABILITY GROUP** y especifique la opción **WITH BASIC** (el valor predeterminado es **ADVANCED**). También puede crear el grupo de disponibilidad básico con la interfaz de usuario de SQL Server Management Studio a partir de la versión 17.8. Para obtener más información, vea [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/create-availability-group-transact-sql.md). 

Vea el ejemplo siguiente para crear un grupo de disponibilidad básico mediante Transact-SQL (T-SQL): 

```sql
CREATE AVAILABILITY GROUP [BasicAG]
WITH (AUTOMATED_BACKUP_PREFERENCE = PRIMARY,
BASIC,
DB_FAILOVER = OFF,
DTC_SUPPORT = NONE,
REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT = 0)
FOR DATABASE [AdventureWorks]
REPLICA ON N'SQLVM1\MSSQLSERVER' WITH (ENDPOINT_URL = N'TCP://SQLVM1.Contoso.com:5022', FAILOVER_MODE = AUTOMATIC, AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, SEEDING_MODE = AUTOMATIC, SECONDARY_ROLE(ALLOW_CONNECTIONS = NO)),
    N'SQLVM2\MSSQLSERVER' WITH (ENDPOINT_URL = N'TCP://SQLVM2.Contoso.com:5022', FAILOVER_MODE = AUTOMATIC, AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, SEEDING_MODE = AUTOMATIC, SECONDARY_ROLE(ALLOW_CONNECTIONS = NO));

GO
```

  
> [!NOTE]  
>  Las limitaciones de los grupos de disponibilidad básica se aplican al comando **CREATE AVAILABILITY GROUP** cuando **WITH BASIC** está especificado. Por ejemplo, obtendrá un error si intenta crear un grupo de disponibilidad básica que permita el acceso de lectura. Otras limitaciones se aplican de la misma manera. Consulte la sección Limitaciones de este tema para obtener más información.  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
