---
title: Cambio del nombre del equipo que hospeda la instancia
description: Al cambiar el nombre de un equipo que hospeda una instancia de SQL Server, actualice los metadatos del sistema almacenados en sys.servers.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- remote login errors [SQL Server]
- standalone computer names [SQL Server]
- names [SQL Server], standalone instances of SQL Server
- renaming standalone instances of SQL Server
- sysservers system table
- removing remote logins
- deleting remote logins
- dropping remote logins
ms.assetid: bbaf1445-b8a2-4ebf-babe-17d8cf20b037
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: b84f700ab19b940d8aa742be94dc2c3ab3726507
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353041"
---
# <a name="rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server"></a>Cambiar el nombre de un equipo que hospeda una instancia independiente de SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Cuando se cambia el nombre del equipo que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el nombre nuevo se reconoce durante el inicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. No es necesario que vuelva a ejecutar el programa de instalación para restablecer el nombre del equipo. En su lugar, realice los siguientes pasos para actualizar los metadatos del sistema que están almacenados en sys.servers y que son notificados por la función de sistema @@SERVERNAME. Actualice los metadatos del sistema para reflejar los cambios de nombre de equipo de las conexiones remotas y las aplicaciones que usan @@SERVERNAME, o que consultan el nombre del servidor desde sys.servers.  
  
Los siguientes pasos no se pueden utilizar para cambiar el nombre de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Estos pasos solo se pueden usar para cambiar la parte del nombre de la instancia que corresponde al nombre del equipo. Por ejemplo, puede cambiar el nombre de un equipo denominado MB1 que hospeda una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] denominada Instance1 por otro nombre, por ejemplo MB2. Sin embargo, la parte del nombre que corresponde a la instancia, Instance1, permanecerá intacta. En este ejemplo, \\\\*nombreDeEquipo*\\*nombreDeInstancia* cambiará de \\\MB1\Instance1 a \\\MB2\Instance1.  
  
 **Antes de empezar**  
  
 Antes de comenzar el proceso de cambio de nombre, lea la siguiente información:  
  
-   Cuando una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] forma parte de un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , el proceso para cambiar el nombre del equipo difiere del que se usa en el equipo que hospeda una instancia independiente.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no permite cambiar el nombre de los equipos implicados en un proceso de replicación, excepto cuando se utiliza el trasvase de registros con la replicación. Se puede cambiar el nombre del equipo secundario del trasvase de registros si el equipo primario se pierde de manera permanente. Para obtener más información, vea [Trasvase de registros y replicación &#40;SQL Server&#41;](../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md).  
  
-   Al cambiar el nombre de un equipo que está configurado para utilizar [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] podría no estar disponible después del cambio del nombre de equipo. Para obtener más información, vea [Cambiar el nombre de un equipo que ejecuta un servidor de informes](../../reporting-services/report-server/rename-a-report-server-computer.md).  
  
-   Cuando cambia el nombre de un equipo que está configurado para utilizar la creación de reflejo de la base de datos, ésta debe desactivarse antes de realizar la operación de cambio de nombre. A continuación, vuelva a establecer la creación de reflejo de la base de datos con el nuevo nombre de equipo. Los metadatos para la creación de reflejo de la base de datos no se actualizan automáticamente para reflejar el nuevo nombre del equipo. Utilice los pasos siguientes para actualizar los metadatos del sistema.  
  
-   Es posible que los usuarios que se conectan a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a través de un grupo de Windows que utilice una referencia codificada de forma rígida al nombre del equipo no se puedan conectar a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esto puede ocurrir después de cambiar el nombre si el grupo de Windows especifica el nombre de equipo anterior. Para asegurarse de que estos grupos de Windows tienen conexión con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] después de la operación de cambio de nombre, actualice el grupo de Windows para especificar el nuevo nombre del equipo.  
  
 Puede conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con el nuevo nombre del equipo después de haber reiniciado [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para asegurarse de que @@SERVERNAME devuelve el nombre actualizado de la instancia del servidor local, conviene ejecutar manualmente el procedimiento correspondiente a su situación de entre los siguientes. El procedimiento que use dependerá de si está actualizando un equipo que hospeda o una instancia predeterminada o con nombre de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="rename-a-computer-that-hosts-a-stand-alone-instance-of-ssnoversion"></a>Cambiar el nombre de un equipo que hospeda una instancia independiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   En un equipo con el nombre cambiado que hospeda una instancia predeterminada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ejecute los procedimientos siguientes:  
  
    ```sql
    EXEC sp_dropserver '<old_name>';  
    GO  
    EXEC sp_addserver '<new_name>', local;  
    GO  
    ```  
  
     Reinicie la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   En un equipo con el nombre cambiado que hospeda una instancia con nombre de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ejecute los procedimientos siguientes:  
  
    ```sql
    EXEC sp_dropserver '<old_name\instancename>';  
    GO  
    EXEC sp_addserver '<new_name\instancename>', local;  
    GO  
    ```  
  
     Reinicie la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="after-the-renaming-operation"></a>Después de la operación de cambio de nombre  
 Después de cambiar el nombre del equipo, las conexiones que utilizaban el nombre anterior deben realizarse con el nombre nuevo.  
  
## <a name="verify-renaming-operation"></a>Comprobar la operación de cambio de nombre  
  
-   Seleccione la información de @@SERVERNAME o de sys.servers. La función @@SERVERNAME devolverá el nombre nuevo y la tabla sys.servers lo mostrará. En el siguiente ejemplo se muestra el uso de @@SERVERNAME.  
  
    ```  
    SELECT @@SERVERNAME AS 'Server Name';  
    ```  
  
## <a name="additional-considerations"></a>Consideraciones adicionales  
 **Remote Logins:** si el equipo dispone de algún inicio de sesión remoto, al ejecutar sesión remoto, al ejecutar **sp_dropserver** podría generarse un error similar al siguiente:  
  
 `Server: Msg 15190, Level 16, State 1, Procedure sp_dropserver, Line 44 There are still remote logins for the server 'SERVER1'.`  
  
 Para solucionar el error, debe quitar los inicios de sesión remotos de este servidor.  
  
### <a name="drop-remote-logins"></a>Quitar inicios de sesión remotos  
  
-   Para una instancia predeterminada, ejecute el siguiente procedimiento:  
  
    ```sql
    EXEC sp_dropremotelogin old_name;  
    GO  
    ```  
  
-   Para una instancia con nombre, ejecute el siguiente procedimiento:  
  
    ```sql
    EXEC sp_dropremotelogin old_name\instancename;  
    GO  
    ```  
  
 **Configuraciones de servidores vinculados:** el equipo que realiza la operación de cambio de nombre afectará a las configuraciones de los servidores vinculados. Use **sp_addlinkedserver** o **sp_setnetname** para actualizar las referencias al nombre de equipo. Para obtener más información, vea [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) o [sp_setnetname &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-setnetname-transact-sql.md).  
  
 **Nombres de alias de cliente:** la operación de cambio de nombre de equipo afectará a los alias de cliente que usen canalizaciones con nombre. Por ejemplo, si se creó un alias "PROD_SRVR" para señalar a SRVR1 y usa el protocolo de canalizaciones con nombre, el nombre de la canalización será similar a `\\SRVR1\pipe\sql\query`. Una vez cambiado el nombre del equipo, la ruta de acceso de la canalización con nombre ya no será válida. Para obtener más información sobre las canalizaciones con nombre, vea el tema sobre cómo [crear una cadena de conexión válida con canalizaciones con nombre](/previous-versions/sql/sql-server-2008/ms189307(v=sql.100)).  
  
## <a name="see-also"></a>Consulte también  
 [Instalar SQL Server](../../database-engine/install-windows/install-sql-server.md)  
  
