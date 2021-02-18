---
title: Administración de inicios de sesión y trabajos después de la conmutación por error del reflejo
description: Aprenda a administrar inicios de sesión y trabajos después de la conmutación por error de la base de datos reflejada desde la base de datos principal a la secundaria.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: high-availability
ms.topic: how-to
helpviewer_keywords:
- role switching [SQL Server]
ms.assetid: fc2fc949-746f-40c7-b5d4-3fd51ccfbd7b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1e410b42c5e53b479f28febee1ccd716d68437d8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354009"
---
# <a name="management-of-logins-and-jobs-after-role-switching-sql-server"></a>Administración de inicios de sesión y trabajos tras la conmutación de roles (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
   Al implementar una solución de alta disponibilidad o de recuperación ante desastres para una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], es importante reproducir información relevante que se almacena para dicha base de datos en las bases de datos **master** o **msdb**. Generalmente, la información relevante incluye los trabajos de la base de datos principal y los inicios de sesión de los usuarios o los procesos que necesitan conectarse con la base de datos. Debe duplicar esta información en las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que hospeden una base de datos reflejada o secundaria. Si es posible, tras la conmutación de roles es mejor reproducir mediante programación la información en la nueva base de datos principal.  
  
## <a name="logins"></a>Inicios de sesión  
 En las instancias de servidor que hospeden una copia de la base de datos, debe reproducir los inicios de sesión que tengan permiso de acceso a la base de datos principal. Cuando cambie el rol principal, solo los usuarios cuyos inicios de sesión existan en la nueva instancia del servidor principal podrán tener acceso la nueva base de datos principal. Los usuarios cuyos inicios de sesión no estén definidos en la nueva instancia del servidor principal se quedarán huérfanos y no podrán tener acceso a la base de datos.  
  
 Si un usuario se queda huérfano, cree el inicio de sesión en la nueva instancia del servidor principal y ejecute [sp_change_users_login](../../relational-databases/system-stored-procedures/sp-change-users-login-transact-sql.md). Para obtener más información, vea [Solucionar problemas de usuarios huérfanos &#40;SQL Server&#41;](../../sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server.md).  
  
###  <a name="logins-of-applications-that-use-sql-server-authentication-or-a-local-windows-login"></a><a name="SSauthentication"></a> Inicios de sesión de aplicaciones que usan la autenticación de SQL Server o un inicio de sesión local de Windows  
 Si una aplicación usa la Autenticación de SQL Server o un inicio de sesión local de Windows, los SID que no coinciden pueden impedir que el inicio de sesión de la aplicación se resuelva en una instancia remota de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los SID que no coincidan harán que el inicio de sesión sea un usuario huérfano en la instancia del servidor remoto. Este problema puede producirse cuando una aplicación se conecta con una base de datos reflejada o de trasvase de registros después de una conmutación por error o con una base de datos Suscriptor de replicación que se inicializó desde una copia de seguridad.  
  
 Para evitar que se produzca este problema, se recomienda tomar medidas preventivas al configurar una aplicación de este tipo para que utilice una base de datos hospedada por una instancia remota de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La prevención implica la transferencia de los inicios de sesión y las contraseñas de la instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la instancia remota de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información sobre cómo evitar que se produzca este problema, vea el artículo de KB 918992: [Cómo transferir inicios de sesión y contraseñas entre instancias de SQL Server](https://support.microsoft.com/kb/918992/).  
  
> [!NOTE]  
>  Este problema afecta a las cuentas locales de Windows en distintos equipos. Sin embargo, este problema no ocurre en las cuentas de dominio debido a que el SID es el mismo en todos los equipos.  
  
 Para obtener más información, vea [Usuarios huérfanos con creación de reflejo de la base de datos y trasvase de registros](/archive/blogs/sqlserverfaq/orphaned-users-with-database-mirroring-and-log-shipping) (un blog del motor de base de datos).  
  
## <a name="jobs"></a>Trabajos  
 Los trabajos, como, por ejemplo, los trabajos de copia de seguridad, requieren una consideración especial. Normalmente, tras una conmutación de roles, el propietario de la base de datos o el administrador del sistema deben volver a crear los trabajos de la nueva base de datos principal.  
  
 Cuando esté disponible la que antes era la instancia del servidor principal, debería eliminar los trabajos originales en dicha instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Observe que los trabajos de la base de datos reflejada actual generan un error porque la base de datos se encuentra en el estado RESTORING y, por lo tanto, no está disponible.  
  
> [!NOTE]  
>  Las distintas instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pueden configurarse de forma diferente, con distintas letras de unidad, por ejemplo. Los trabajos para cada asociado deben permitir tales diferencias.  
  
## <a name="see-also"></a>Vea también  
 [Administrar los metadatos cuando una base de datos pasa a estar disponible en otra instancia del servidor &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [Solucionar problemas de usuarios huérfanos &#40;SQL Server&#41;](../../sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server.md)  
  
