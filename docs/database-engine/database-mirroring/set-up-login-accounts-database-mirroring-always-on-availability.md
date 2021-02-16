---
title: Configuración de las cuentas de inicio de sesión (creación de reflejos y grupos de disponibilidad)
description: Configure las cuentas de inicio de sesión para que tengan acceso al punto de conexión de creación de reflejos de bases de datos de un reflejo de base de datos o un grupo de disponibilidad AlwaysOn.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- logins [SQL Server], database mirroring
ms.assetid: e9f5287b-1325-4cda-88a6-19eaaa52a652
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: bc823c6cd70688a3ad0738a6cf44a92bb86b6434
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352262"
---
# <a name="set-up-login-accounts---database-mirroring-always-on-availability"></a>Configurar cuentas de inicio de sesión - Creación de reflejo de la base de datos y Grupos de disponibilidad AlwaysOn
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Para que dos instancias del servidor se conecten a [extremos de creación de reflejo de la base de datos](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md) entre sí, la cuenta de inicio de sesión de cada instancia debe tener acceso a la otra instancia. Además, cada cuenta de inicio de sesión requiere permiso de conexión al extremo de creación de reflejo de la base de datos de la otra instancia.  
  
 El impacto de este requisito depende de si las instancias de servidor se ejecutan como la misma cuenta de usuario de dominio:  
  
-   Si las instancias del servidor se ejecutan como la misma cuenta de usuario de dominio, automáticamente existen los inicios de sesión de usuario correctos en ambas bases de datos **maestras** . Esto simplifica la configuración de seguridad de la creación de reflejo de la base de datos y de los grupos de disponibilidad AlwaysOn.  
  
-   Si las instancias de servidor se ejecutan como cuentas de usuario diferentes, los inicios de sesión de usuario en la instancia de servidor que hospeda el servidor principal o la réplica principal deben reproducirse manualmente en la instancia de servidor que hospeda el servidor reflejado o en cada instancia de servidor que hospeda una réplica secundaria. Para obtener más información, vea [Crear un inicio de sesión para una cuenta diferente](#CreateLogin) y [Conceder el permiso de conexión](#GrantConnect), más adelante en este tema.  
  
    > [!IMPORTANT]  
    >  Para crear un entorno más seguro, considere la posibilidad de usar cuentas de dominio diferentes para cada instancia de servidor.  
  
##  <a name="create-a-login-for-a-different-account"></a><a name="CreateLogin"></a> Crear un inicio de sesión para una cuenta diferente  
 Si dos instancias del servidor se ejecutan como cuentas diferentes, el administrador del sistema debe utilizar la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE LOGIN para crear un inicio de sesión para la cuenta de servicio de inicio de la instancia remota para cada instancia del servidor. Para obtener más información, vea [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md).  
  
> [!IMPORTANT]  
>  Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecuta con una cuenta que no sea de dominio, es necesario utilizar certificados. Para obtener más información, vea [Usar certificados para un punto de conexión de creación de reflejo de la base de datos &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
 Por ejemplo, para que la instancia del servidor sqlA, que se ejecuta con loginA, se conecte a la instancia del servidor sqlB, que se ejecuta con loginB, loginA debe estar en sqlB y loginB debe estar en sqlA. Además, en una sesión de creación de reflejo de la base de datos que incluya una instancia del servidor testigo (sqlC) y en la que las tres instancias del servidor se ejecuten con diferentes cuentas de dominio, deben crearse los siguientes inicios de sesión:  
  
|En la instancia...|Cree inicios de sesión y conceda permiso de conexión para ...|  
|--------------------|--------------------------------------------------------------|  
|sqlA|sqlB y sqlC|  
|sqlB|sqlA y sqlC|  
|sqlC|sqlA y sqlB|  
  
> [!NOTE]  
>  Es posible conectarse a la cuenta de servicio de red mediante la cuenta del equipo en lugar de un usuario de dominio. Si se utiliza la cuenta de equipo, debe agregarse como un usuario en la otra instancia del servidor.  
  
##  <a name="grant-connect-permission"></a><a name="GrantConnect"></a> Conceder el permiso de conexión  
 Una vez creado un inicio de sesión en una instancia del servidor, debe concederse al inicio de sesión permiso para conectarse al extremo de creación de reflejo de la base de datos de la instancia del servidor. El administrador del sistema concede el permiso de conexión mediante una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] GRANT. Para obtener más información, vea [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Crear un inicio de sesión](../../relational-databases/security/authentication-access/create-a-login.md)  
  
-   [Permitir el acceso de red a un punto de conexión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
-   [Usar certificados para un punto de conexión de creación de reflejo de la base de datos &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
## <a name="see-also"></a>Consulte también  
 [El punto de conexión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Solucionar problemas de configuración de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)   
 [Solucionar problemas de configuración de grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
  
