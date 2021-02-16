---
title: Creación de reflejo de la base de datos - Permitir el acceso de red - Autenticación de Windows | Microsoft Docs
description: Obtenga información sobre cómo usar la autenticación de Windows para conectar los puntos de conexión de creación de reflejo de la base de datos de dos instancias de SQL Server, que pueden requerir configuración manual.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- Windows authentication [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 28c8fec5-5feb-4c84-8d72-f2bd1ae3b40d
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: dfe10e2d812d133571d4df64d05ea7705d076464
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353191"
---
# <a name="database-mirroring---allow-network-access---windows-authentication"></a>Creación de reflejo de la base de datos - Permitir el acceso de red - Autenticación de Windows
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Si utiliza la autenticación de Windows para conectar los extremos de creación de reflejo de la base de datos de dos instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , se requiere la configuración manual de las cuentas de inicio de sesión en las siguientes condiciones:  
  
-   Si las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecutan como servicios bajo diferentes cuentas de dominio (en el mismo dominio o en dominios de confianza), el inicio de sesión de cada cuenta debe crearse en **master** en cada una de las instancias de los servidores remotos y se deben conceder permisos CONNECT a ese inicio de sesión en el punto de conexión.  
  
-   Si las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecutan con la cuenta de servicio de red, el inicio de sesión de cada cuenta de equipo host (*NombreDeDominio* **\\** _NombreDeEquipo$_ ) se debe crear en **master** en cada una de las instancias de los servidores remotos, y a ese inicio de sesión se le deben conceder permisos CONNECT en el punto de conexión. Esto se debe a que una instancia de servidor que se ejecuta en la cuenta Servicio de red se autentica mediante la cuenta de dominio del equipo host.  
  
> [!NOTE]  
>  Asegúrese de que exista un extremo para cada instancia del servidor. Para obtener más información, vea [Crear un punto de conexión de creación de reflejo de la base de datos para la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md).  
  
### <a name="to-configure-logins-for-windows-authentication"></a>Para configurar inicios de sesión para la autenticación de Windows  
  
1.  Para la cuenta de usuario de cada instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cree un inicio de sesión en las demás instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilice una instrucción [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md) con la cláusula FROM WINDOWS.  
  
     Para obtener más información, vea [Crear un inicio de sesión](../../relational-databases/security/authentication-access/create-a-login.md).  
  
2.  Asimismo, para asegurarse de que el usuario de inicio de sesión tiene acceso al extremo, utilice la instrucción [GRANT](../../t-sql/statements/grant-transact-sql.md) para conceder permisos de conexión en el extremo para el inicio de sesión. Tenga en cuenta que si el usuario es un administrador, no es necesario que conceda permisos de conexión al extremo.  
  
     Para más información, consulte [Grant a Permission to a Principal](../../relational-databases/security/authentication-access/grant-a-permission-to-a-principal.md).  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente de [!INCLUDE[tsql](../../includes/tsql-md.md)] se crea un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para una cuenta de usuario denominada `Otheruser` que pertenece a un dominio denominado `Adomain`. En el ejemplo se concede a este usuario permiso para conectarse a un extremo para la creación de reflejo de base de datos ya existente llamado `Mirroring_Endpoint`.  
  
```  
USE master;  
GO  
CREATE LOGIN [Adomain\Otheruser] FROM WINDOWS;  
GO  
GRANT CONNECT on ENDPOINT::Mirroring_Endpoint TO [Adomain\Otheruser];  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Seguridad de transporte para la creación de reflejo de la base de datos y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [El punto de conexión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
  
  
