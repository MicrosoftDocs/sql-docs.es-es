---
title: Configurar la creación de reflejo de la base de datos (SQL Server) | Microsoft Docs
description: Obtenga información sobre los requisitos previos, las recomendaciones y los pasos para configurar la creación de reflejo de la base de datos en SQL Server, incluida una introducción a una sesión de creación de reflejo de la base de datos.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
ms.assetid: da45efed-55eb-4c71-be34-ac2589dfce8d
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c085a7e1571e85e4fc3f3f9267e033e17302e09e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352281"
---
# <a name="setting-up-database-mirroring-sql-server"></a>Configurar la creación de reflejo de la base de datos (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En esta sección se describen los requisitos previos, las recomendaciones y los pasos para configurar la creación de reflejo de la base de datos. Para obtener una introducción a la creación de reflejo de la base de datos, vea [Creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md).  
  
> [!IMPORTANT]  
>  Se recomienda configurar la creación de reflejo de la base de datos durante los periodos de poca actividad, dado que este proceso puede afectar al rendimiento.  
  
  
##  <a name="preparing-a-server-instance-to-host-a-mirror-server"></a><a name="PrepareInstances"></a> Preparar una instancia de servidor para hospedar un servidor reflejado  
 Para cada sesión de creación de reflejo de la base de datos:  
  
1.  El servidor principal, el servidor reflejado y el testigo, si existen, deben estar hospedados en instancias de servidor independientes que, a su vez, deben estar en sistemas de host diferentes. Cada una de las instancias de servidor requiere un extremo de creación de reflejo de la base de datos. Si necesita crear un extremo de creación de reflejo de la base de datos, asegúrese de que sea accesible a las demás instancias de servidor.  
  
     El tipo de autenticación que utilice la instancia de servidor para la creación de reflejo de la base de datos es una propiedad del extremo de reflejo de la base de datos. Existen dos tipos de seguridad de transporte para la creación de reflejo de la base de datos: autenticación de Windows o autenticación basada en certificados. Para obtener más información, vea [Seguridad de transporte para la creación de reflejo de la base de datos y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md).  
  
     Los requisitos para el acceso de red son específicos del tipo de autenticación, del siguiente modo:  
  
    -   **Si se usa la autenticación de Windows.**  
  
         Si las instancias de servidor se ejecutan con distintas cuentas de usuario de dominio, cada una requiere un inicio de sesión en la base de datos **master** de las demás. Si el inicio de sesión no existe, deberá crearlo. Para obtener más información, vea [Permitir el acceso de red a un punto de conexión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
    -   **Si se usan certificados**  
  
         Para habilitar la autenticación de certificados para la creación de reflejos de la base de datos en una instancia determinada del servidor, el administrador del sistema debe configurar cada instancia del servidor para que utilice certificados en las conexiones de entrada y salida. Las conexiones de salida deben configurarse en primer lugar. Para obtener más información, vea [Usar certificados para un punto de conexión de creación de reflejo de la base de datos &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
2.  Asegúrese de que existen inicios de sesión en el servidor reflejado para todos los usuarios de la base de datos. Para obtener más información, vea [Configurar cuentas de inicio de sesión para la creación de reflejo de la base de datos o grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md).  
  
3.  En la instancia de servidor que hospedará la base de datos reflejada, configure el resto del entorno necesario para la base de datos reflejada. Para obtener más información, vea [Administrar los metadatos cuando una base de datos pasa a estar disponible en otra instancia del servidor &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
##  <a name="overview-establishing-a-database-mirroring-session"></a><a name="EstablishUsingWinAuthentication"></a> Información general: establecer una sesión de reflejo de la base de datos  
 Los pasos básicos para establecer una sesión de creación de reflejo son los siguientes:  
  
1.  Cree la base de datos reflejada restaurando las copias de seguridad siguientes, utilizando RESTORE WITH NORECOVERY en cada operación de restauración:  
  
    1.  Restaure una copia de seguridad de base de datos completa reciente de la base de datos principal, después de asegurarse de que la base de datos principal utilizaba ya el modelo de recuperación completa cuando se realizó la copia de seguridad. La base de datos reflejada debe tener el mismo nombre que la base de datos principal.  
  
    2.  Si ha realizado copias de seguridad diferenciales de la base de datos desde la copia de seguridad completa restaurada, restaure la copia de seguridad diferencial más reciente.  
  
    3.  Restaure todas las copias de seguridad de registros realizadas desde la copia de seguridad diferencial o completa de la base de datos.  
  
     Para obtener más información, vea [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
    > [!IMPORTANT]  
    >  Complete los pasos de configuración restantes lo antes posible después de realizar la copia de seguridad de la base de datos principal. Para poder iniciar la creación de reflejo en los asociados, debe crear una copia de seguridad de registros actual en la base de datos original y restaurarla en la base de datos reflejada que se va a crear.  
  
2.  Puede configurar la creación de reflejo mediante [!INCLUDE[tsql](../../includes/tsql-md.md)] o con el Asistente de creación de reflejo de la base de datos. Para obtener más información, vea uno de los siguientes temas:  
  
    -   [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
    -   [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
3.  De forma predeterminada, una sesión se configura con la seguridad de las transacciones completa (SAFETY se establece en FULL), lo que inicia la sesión en modo sincrónico de alta seguridad sin conmutación automática por error. Puede volver a configurar la sesión para ejecutarla en modo de alta seguridad con conmutación automática por error o en modo asincrónico de alto rendimiento, como se indica a continuación:  
  
    -   **Modo de seguridad alta con conmutación automática por error**  
  
         Si desea que una sesión en modo de alta seguridad admita la conmutación automática por error, agregue una instancia de servidor testigo.  
  
         **Para agregar un testigo**  
  
        -   [Agregar un testigo de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)  
  
        -   [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
        > [!NOTE]  
        >  El propietario de la base de datos puede desactivar el testigo de una base de datos en cualquier momento. El hecho de desactivar el testigo equivale a no tener testigo, por lo que no puede ocurrir una conmutación automática por error.  
  
    -   **Modo de alto rendimiento**  
  
         Otra opción, si no desea la conmutación automática por error y prefiere poner más énfasis en el rendimiento en lugar de la disponibilidad, es desactivar la seguridad de las transacciones. Para obtener más información, vea [Cambiar la seguridad de las transacciones en una sesión de creación de reflejo de la base de datos &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md).  
  
        > [!NOTE]  
        >  En el modo de alto rendimiento, WITNESS debe establecerse en OFF. Para más información, vea [Cuórum: cómo un testigo afecta a la disponibilidad de la base de datos &#40;creación de reflejo de la base de datos&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
> [!NOTE]  
>  Para obtener un ejemplo del uso de [!INCLUDE[tsql](../../includes/tsql-md.md)] para configurar la creación de reflejo de la base de datos mediante la autenticación de Microsoft Windows, vea [Ejemplo: Configurar la creación de reflejo de la base de datos mediante la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md).  
>   
>  Para obtener un ejemplo del uso de [!INCLUDE[tsql](../../includes/tsql-md.md)] para configurar la creación de reflejo de la base de datos mediante la seguridad basada en certificados, vea [Ejemplo: configurar la creación de reflejo de la base de datos mediante certificados &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md).  
  
  
##  <a name="in-this-section"></a><a name="InThisSection"></a> En esta sección  
 [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
 Resume los pasos para crear una base de datos reflejada o preparar una base de datos reflejada antes de reanudar una sesión suspendida. También se proporcionan vínculos a temas de procedimientos.  
  
 [Especificar una dirección de red de servidor &#40;creación de reflejo de la base de datos&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
 Describe la sintaxis de una dirección de red del servidor, cómo identifica la dirección el extremo de creación de reflejo de la base de datos de la instancia del servidor y cómo buscar el nombre de dominio completo de un sistema.  
  
 [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
 Describe cómo utilizar el Asistente para la configuración de seguridad de la creación de reflejo de bases de datos para iniciar la creación de reflejo en una base de datos.  
  
 [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
 Describe los pasos de [!INCLUDE[tsql](../../includes/tsql-md.md)] para configurar la creación de reflejo de la base de datos.  
  
 [Ejemplo: Configurar la creación de reflejo de la base de datos mediante la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md).  
 Contiene un ejemplo de todos los pasos necesarios para crear una sesión de creación de reflejo de la base de datos con un testigo usando la autenticación de Windows.  
  
 [Ejemplo: Configurar la creación de reflejo de la base de datos con certificados &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)  
 Contiene un ejemplo de todos los pasos necesarios para crear una sesión de creación de reflejo de la base de datos con un testigo usando la autenticación basada en certificados.  
  
 [Configurar cuentas de inicio de sesión para la creación de reflejo de la base de datos o grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md)  
 Describe cómo crear un inicio de sesión para una instancia del servidor remoto que usa una cuenta diferente a la de la instancia del servidor local.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
 **SQL Server Management Studio**  
  
-   [Iniciar el Asistente para la configuración de seguridad de la creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
-   [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
 **Transact-SQL**  
  
-   [Permitir el acceso de red a un punto de conexión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)  
  
-   [Permitir que un punto de conexión de creación de reflejo de la base de datos utilice certificados para las conexiones salientes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
-   [Permitir que un punto de conexión de creación de reflejo de la base de datos use certificados para las conexiones entrantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Crear un punto de conexión de creación de reflejo de la base de datos para la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
-   [Agregar un testigo de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)  
  
-   [Configurar una base de datos reflejada para usar la propiedad Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)  
  
 **Transact-SQL/SQL Server Management Studio**  
  
-   [Actualización de instancias reflejadas](../../database-engine/database-mirroring/upgrading-mirrored-instances.md)  
  
-   [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
-   [Solucionar problemas de configuración de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
  
## <a name="see-also"></a>Consulte también  
 [Creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Creación de reflejo de la base de datos: interoperabilidad y coexistencia &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-interoperability-and-coexistence-sql-server.md)   
 [Seguridad de transporte para la creación de reflejo de la base de datos y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Especificar una dirección de red de servidor &#40;creación de reflejo de la base de datos&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
  
  
