---
title: Problemas comunes al configurar la creación de reflejo de bases de datos
description: Proporciona información que ayudará a solucionar problemas de configuración de una sesión de creación de reflejo de una base de datos.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- endpoints [SQL Server], database mirroring
- database mirroring [SQL Server], troubleshooting
- troubleshooting [SQL Server], database mirroring
ms.assetid: 87d3801b-dc52-419e-9316-8b1f1490946c
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 4f8b18819666c7cec7c0f50b074a5a9a7ed5a804
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352227"
---
# <a name="troubleshoot-database-mirroring-configuration-sql-server"></a>Solucionar problemas de configuración de creación de reflejo de la base de datos (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se ofrece información que le ayudará a solucionar problemas de configuración de una sesión de creación de reflejo de una base de datos.  
  
> [!NOTE]  
>  Asegúrese de que cumple todos los [requisitos previos para la creación de reflejo de la base de datos](../../database-engine/database-mirroring/prerequisites-restrictions-and-recommendations-for-database-mirroring.md).  
  
|Problema|Resumen|  
|-----------|-------------|  
|Mensaje de error 1418|Este mensaje de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] indica que la dirección de red del servidor es inaccesible o no existe, y recomienda que se compruebe el nombre de dirección de red y se vuelva a emitir el comando. |  
|[Cuentas](#Accounts)|Analiza los requisitos para configurar correctamente las cuentas en que se ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|[Extremos](#Endpoints)|Explica los requisitos para configurar correctamente el extremo de la creación de reflejo de la base de datos de cada instancia de servidor.|  
|[SystemAddress](#SystemAddress)|Resume las alternativas para especificar el nombre del sistema de una instancia de servidor en una configuración de creación de reflejo de la base de datos.|  
|[Acceso de red](#NetworkAccess)|Explica el requisito por el que cada instancia de servidor debe poder tener acceso a los puertos de las otras instancias de servidor a través de TCP.|  
|[Preparación de la base de datos reflejada](#MirrorDbPrep)|Resume los requisitos de preparación de la base de datos reflejada para habilitar el inicio de la creación de reflejo.|  
|[Error en una operación de creación de archivo](#FailedCreateFileOp)|Describe cómo responder a un error en una operación de creación de archivo.|  
|[Iniciar la creación de reflejo mediante Transact-SQL](#StartDbm)|Describe el orden necesario de las instrucciones ALTER DATABASE *database_name* SET PARTNER **='** _partner_server_ **'** .|  
|[Transacciones entre bases de datos](#CrossDbTxns)|Una conmutación automática por error podría provocar la resolución automática y posiblemente incorrecta de transacciones dudosas. Por esta razón, la creación de reflejo de la base de datos no admite transacciones entre bases de datos.|  
  
##  <a name="accounts"></a><a name="Accounts"></a> Cuentas  
 Las cuentas en las que se ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deben estar configuradas correctamente.  
  
1.  ¿Tienen las cuentas los permisos adecuados?  
  
    1.  Si las cuentas se ejecutan en el mismo dominio, disminuye la probabilidad de una configuración incorrecta.  
  
    2.  Si las cuentas se ejecutan en distintos dominios o no son cuentas de dominio, cree en la base de datos **master** del otro equipo el inicio de sesión de una cuenta y concédale permisos CONNECT en el extremo. Para obtener más información, vea [Administrar los metadatos cuando una base de datos pasa a estar disponible en otra instancia del servidor &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md). Esto incluye la cuenta Servicio de red.  
  
2.  Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecuta como un servicio que usa la cuenta del sistema local, deben utilizarse certificados para la autenticación. Para obtener más información, vea [Usar certificados para un punto de conexión de creación de reflejo de la base de datos &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
##  <a name="endpoints"></a><a name="Endpoints"></a> Endpoints  
 Los extremos deben estar configurados correctamente.  
  
1.  Asegúrese de que cada instancia de servidor (principal, reflejado y testigo, si existe) tiene un extremo de creación de reflejo de la base de datos. Para obtener más información, vea [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md) y, según la forma de autenticación, ya sea [Crear un punto de conexión de creación de reflejo de la base de datos para la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md) o [Usar certificados para un punto de conexión de creación de reflejo de la base de datos &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md).  
  
2.  Compruebe que los números de puerto son correctos.  
  
     Para identificar el puerto asociado actualmente al punto de conexión de creación de reflejo de la base de datos de una instancia de servidor, use las vistas de catálogo [sys.database_mirroring_endpoints](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md) y [sys.tcp_endpoints](../../relational-databases/system-catalog-views/sys-tcp-endpoints-transact-sql.md) .  
  
3.  En caso de problemas de configuración de la creación de reflejo de la base de datos difíciles de explicar, se recomienda que compruebe cada una de las instancias de servidor para determinar si escuchan en los puertos correctos.   
  
4.  Asegúrese de que se han iniciado los extremos (STATE=STARTED). En cada una de las instancias de servidor, utilice la siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
    ```  
    SELECT state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
     Para obtener más información sobre la columna **state_desc**, vea [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md).  
  
     Para iniciar un extremo, utilice la siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
    ```  
    ALTER ENDPOINT Endpoint_Mirroring   
    STATE = STARTED   
    AS TCP (LISTENER_PORT = <port_number>)  
    FOR database_mirroring (ROLE = ALL);  
    GO  
    ```  
  
     Para obtener más información, vea [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
5.  Compruebe que el valor de ROLE sea correcto. En cada instancia de servidor, use la siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
    ```  
    SELECT role FROM sys.database_mirroring_endpoints;  
    GO  
    ```  
  
     Para obtener más información, vea [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md).  
  
6.  El inicio de sesión de la cuenta de servicio desde la otra instancia de servidor necesita el permiso CONNECT. Asegúrese de que el inicio de sesión del otro servidor dispone de permiso CONNECT. Para determinar quién tiene permiso CONNECT para un extremo, use la siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] en cada instancia de servidor.  
  
    ```  
    SELECT 'Metadata Check';  
    SELECT EP.name, SP.STATE,   
       CONVERT(nvarchar(38), suser_name(SP.grantor_principal_id))   
          AS GRANTOR,   
       SP.TYPE AS PERMISSION,  
       CONVERT(nvarchar(46),suser_name(SP.grantee_principal_id))   
          AS GRANTEE   
       FROM sys.server_permissions SP , sys.endpoints EP  
       WHERE SP.major_id = EP.endpoint_id  
       ORDER BY Permission,grantor, grantee;   
    GO  
  
    ```  
  
##  <a name="system-address"></a><a name="SystemAddress"></a> Dirección del sistema  
 Como nombre del sistema de una instancia de servidor en una configuración de creación de reflejo de la base de datos, se puede utilizar cualquier nombre que identifique el sistema de forma inequívoca. La dirección del sistema puede ser un nombre del sistema (si los sistemas se encuentran en el mismo dominio), un nombre de dominio completo o una dirección IP (de preferencia, una dirección IP estática). Se garantiza que el uso de un nombre de dominio completo funciona correctamente. Para obtener más información, vea [Especificar una dirección de red de servidor &#40;creación de reflejo de la base de datos&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
##  <a name="network-access"></a><a name="NetworkAccess"></a> Network Access  
 Cada instancia de servidor debe tener acceso a los puertos de las demás instancias de servidor a través de TCP. Esto es especialmente importante si las instancias de servidor están en distintos dominios que no confían unos en otros (dominios que no son de confianza). Así se restringe mucho la comunicación entre las instancias de servidor.  
  
##  <a name="mirror-database-preparation"></a><a name="MirrorDbPrep"></a> Mirror Database Preparation  
 Cuando se inicia la creación de reflejo por primera vez o se inicia de nuevo después de eliminar la creación de reflejo, compruebe que la base de datos reflejada esté preparada para esta operación.  
  
 Al crear la base de datos reflejada en el servidor reflejado, asegúrese de restaurar la copia de seguridad de la base de datos principal especificando la misma base de datos con la opción WITH NORECOVERY. Además, también deben aplicarse todas las copias de seguridad de registros creadas después de crear la copia de seguridad, de nuevo con WITH NORECOVERY.  
  
 También se recomienda que, si es posible, la ruta de acceso al archivo (incluida la letra de unidad) de la base de datos reflejada sea idéntica a la de la base de datos principal. Si las rutas de acceso de archivo deben ser diferentes (por ejemplo, si la base de datos principal se encuentra en la unidad 'F:' pero el sistema reflejado no tiene unidad F:), se debe incluir la opción MOVE en la instrucción RESTORE.  
  
> [!IMPORTANT]  
>  Si mueve los archivos de la base de datos al crear la base de datos reflejada, es posible que no pueda agregar archivos a la base de datos posteriormente sin suspender la creación de reflejo.  
  
 Si la creación de reflejo de la base de datos se ha detenido, todas las copias de seguridad de registros subsiguientes que se realicen en la base de datos principal deben aplicarse a la base de datos reflejada para poder reiniciar la creación de reflejo.  
  
 Para obtener más información, vea [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
##  <a name="failed-create-file-operation"></a><a name="FailedCreateFileOp"></a> Failed Create-File Operation  
 Para poder agregar un archivo sin afectar a una sesión de creación de reflejo, la ruta de acceso del archivo debe existir en ambos servidores. Por consiguiente, si mueve los archivos de base de datos al crear la base de datos reflejada, se podría producir un error en una operación posterior para agregar un archivo en la base de datos reflejada y provocar la suspensión de la creación del reflejo.  
  
 Para corregir el problema:  
  
1.  El propietario de la base de datos debe quitar la sesión de creación de reflejo y restaurar una copia de seguridad completa del grupo de archivos que contiene el archivo agregado.  
  
2.  A continuación, el propietario debe hacer una copia de seguridad del registro que contenga la operación para agregar el archivo en el servidor principal y restaurar manualmente la copia de seguridad del registro en la base de datos reflejada utilizando las opciones WITH NORECOVERY y MOVE. De este modo, se crea la ruta de acceso del archivo especificado en el servidor reflejado y se restaura el archivo nuevo en esa ubicación.  
  
3.  Para preparar la base de datos para una nueva sesión de creación de reflejo, el propietario también debe restaurar WITH NO RECOVERY cualquier otra copia de seguridad del registro pendiente desde el servidor principal.  
  
 Para obtener más información, vea [Quitar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md), [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md), [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md), [Usar certificados para un punto de conexión de creación de reflejo de la base de datos &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md) o [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md).  
  
##  <a name="starting-mirroring-by-using-transact-sql"></a><a name="StartDbm"></a> Iniciar la creación de reflejo mediante Transact-SQL  
 El orden en que se emiten las instrucciones ALTER DATABASE *database_name* SET PARTNER **='** _partner_server_ **'** es muy importante.  
  
1.  La primera instrucción se debe ejecutar en el servidor reflejado. Cuando se emite la instrucción, el servidor reflejado no intenta ponerse en contacto con ninguna otra instancia de servidor. En lugar de ello, el servidor reflejado indica a su base de datos que espere a que el servidor principal se haya puesto en contacto con el servidor reflejado.  
  
2.  La segunda instrucción ALTER DATABASE se debe ejecutar en el servidor principal. Esta instrucción provoca que el servidor principal intente conectarse al servidor reflejado. Una vez creada dicha conexión, el servidor reflejado intenta conectarse al servidor principal a través de otra conexión.  
  
 Para obtener más información, vea [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
> [!NOTE]  
>  Para obtener información sobre cómo usar [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para iniciar la creación de reflejo, vea [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md).  
  
##  <a name="cross-database-transactions"></a><a name="CrossDbTxns"></a> Transacciones entre bases de datos  
 Cuando se realiza la creación de reflejo de una base de datos en modo de alta seguridad con conmutación automática por error, dicha conmutación puede generar una solución automática y posiblemente incorrecta de transacciones dudosas. Si se produce una conmutación automática por error en una base de datos cuando se está confirmando una transacción entre bases de datos, se pueden generar incoherencias lógicas entre las bases de datos.  
  
 Los tipos de transacciones entre bases de datos que pueden verse afectadas por una conmutación automática por error son:  
  
-   Una transacción que actualice varias bases de datos en la misma instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Transacciones que utilizan el Coordinador de transacciones distribuidas de [!INCLUDE[msCoName](../../includes/msconame-md.md)] DTC (Coordinador de transacciones distribuidas).  
  
 Para obtener más información, vea [Transacciones entre bases de datos y transacciones distribuidas para la creación de reflejo de la base de datos o grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).  
  
## <a name="see-also"></a>Consulte también  
 [Configurar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Seguridad de transporte para la creación de reflejo de la base de datos y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)  
  
  


