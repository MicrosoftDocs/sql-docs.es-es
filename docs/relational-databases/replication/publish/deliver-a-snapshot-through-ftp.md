---
description: Entregar una instantánea mediante FTP
title: Entrega de una instantánea mediante FTP | Microsoft Docs
ms.custom: ''
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- snapshots [SQL Server replication], FTP snapshots
- FTP snapshots [SQL Server replication]
- snapshot replication [SQL Server], FTP
ms.assetid: 99872c4f-40ce-4405-8fd4-44052d3bd827
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e2cd6bb1841279be3f699c5d2f59b09f6b9c9c27
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076723"
---
# <a name="deliver-a-snapshot-through-ftp"></a>Entregar una instantánea mediante FTP
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo entregar una instantánea a través de FTP en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  

De manera predeterminada, las instantáneas se almacenan en carpetas definidas como recursos compartidos UNC. La replicación también permite especificar un recurso compartido FTP (Protocolo de transferencia de archivos) en lugar de UNC. Para utilizar FTP, debe configurar un servidor FTP y, a continuación, configurar una publicación y una o varias suscripciones para que utilicen FTP. Para obtener información sobre cómo configurar un servidor FTP, vea la documentación de Internet Information Services (IIS). Si especifica información FTP para una publicación, las suscripciones a la misma utilizarán FTP de forma predeterminada. FTP solamente se utiliza con la sincronización web cuando el equipo en que se ejecuta IIS se encuentra separado del distribuidor mediante un firewall. En este caso, FTP se puede utilizar para transferir la instantánea del distribuidor y el equipo que está ejecutando IIS. (La instantánea siempre se transfiere al suscriptor utilizando HTTPS).  
  
> [!IMPORTANT]  
>  Se recomienda usar la autenticación de Microsoft Windows y un recurso compartido UNC en lugar de un recurso compartido FTP porque las contraseñas de FTP se tienen que almacenar, y la contraseña se envía desde el suscriptor (o el equipo donde se ejecuta IIS cuando se usa la sincronización web) al servidor FTP en texto simple. Además, debido a que una sola cuenta controla el acceso al recurso compartido de instantáneas, no es posible garantizar que un suscriptor de una publicación de combinación filtrada tenga acceso únicamente a los archivos de instantáneas de su partición de datos.  
  

## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
  
-   El Agente de instantáneas debe tener permisos de escritura para el directorio especificado y el Agente de distribución o de mezcla debe tener permisos de lectura. Si usa suscripciones de extracción, debe especificar un directorio compartido como una ruta de acceso UNC (Convención de nomenclatura universal), por ejemplo, \\\servidorFTP\inicio\instantáneas. Para obtener más información, vea [Proteger la carpeta de instantáneas](../../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  
## <a name="prerequisites"></a>Requisitos previos  
  
-   Para transferir archivos de instantáneas con el Protocolo de transferencia de archivos (FTP), primero debe configurar un servidor de FTP. Para obtener más información, vea la documentación de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Internet Information Services (IIS).  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
 Para contribuir a mejorar la seguridad, se recomienda implementar una red privada virtual (VPN) al utilizar la entrega de instantáneas a través de FTP por Internet. Para más información, vea [Publicar datos a través de Internet mediante VPN](../../../relational-databases/replication/publish-data-over-the-internet-using-vpn.md).  
  
 Por seguridad, se recomienda no permitir el inicio de sesión anónimo en el servidor de FTP. El Agente de instantáneas debe tener permisos de escritura para el directorio especificado y el Agente de distribución o de mezcla debe tener permisos de lectura. Si usa suscripciones de extracción, debe especificar un directorio compartido como una ruta de acceso UNC (Convención de nomenclatura universal), por ejemplo, \\\servidorFTP\inicio\instantáneas. Para obtener más información, vea [Proteger la carpeta de instantáneas](../../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  
 Cuando sea posible, pida a los usuarios que proporcionen sus credenciales en tiempo de ejecución. Si almacena las credenciales en un archivo de script, debe proteger el archivo.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Después de configurar el servidor FTP, especifique la información de directorios y seguridad para este servidor en el cuadro de diálogo **Propiedades de la publicación \<Publication>** . Para obtener más información sobre el acceso a este cuadro de diálogo, vea [View and Modify Publication Properties](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-specify-ftp-information"></a>Para especificar la información de FTP  
  
1.  En el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , seleccione **Permitir a los suscriptores descargar archivos de instantánea usando FTP** en una de las páginas siguientes:  
  
    -   La página **Instantánea de FTP**, para las publicaciones transaccionales y de instantáneas, y las publicaciones de combinación para los publicadores que ejecuten versiones anteriores a [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  
  
    -   La página **Instantánea de FTP e Internet** , para las publicaciones de combinación de los publicadores que ejecuten [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] o posterior.  
  
2.  Especifique valores para **Nombre del servidor FTP**, **Número de puerto**, **Ruta de acceso de la carpeta raíz del servidor FTP**, **Inicio de sesión** y **Contraseña**.  
  
     Por ejemplo, si el directorio raíz del servidor de FTP es \\\servidorFTP\inicio y desea almacenar las instantáneas en \\\servidorFTP\inicio\instantáneas, especifique \instantáneas\ftp para la propiedad **Ruta de acceso de la carpeta raíz del servidor FTP** (la replicación adjunta 'ftp' a la ruta de acceso de la carpeta de instantáneas al crear los archivos de instantáneas).  
  
3.  Especifique que el Agente de instantáneas copie los archivos de instantáneas en el directorio especificado en el paso 2. Por ejemplo, para que el Agente de instantáneas copie los archivos de instantáneas en \\\servidorFTP\inicio\instantáneas\ftp, debe especificar la ruta de acceso \\\servidorFTP\inicio\instantáneas en uno de estos dos lugares:  
  
    -   La ubicación de instantáneas predeterminada en el distribuidor asociado con esta publicación.    
    -   Una carpeta de instantáneas alternativa para esta publicación. Se necesita una ubicación alternativa si la instantánea está comprimida.  

Para más información sobre cómo modificar las propiedades de ubicación de la carpeta de instantáneas, vea [Opciones de instantánea](../snapshot-options.md).
  

4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Se puede establecer la opción para que los archivos de instantáneas estén disponibles en un servidor FTP y se puede modificar esta configuración mediante programación empleando los procedimientos almacenados de replicación. El procedimiento usado depende del tipo de publicación. La entrega de instantáneas a través de FTP solamente se usa con suscripciones de extracción.  
  
#### <a name="to-enable-ftp-snapshot-delivery-for-a-snapshot-or-transactional-publication"></a>Para habilitar la entrega de instantáneas a través de FTP para una publicación transaccional o de instantáneas  
  
1.  En la base de datos de publicación del Publicador, ejecute [sp_addpublication](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md). Especifique `@publication`, un valor de **true** para `@enabled_for_internet` y los valores adecuados para los parámetros siguientes:  
  
    -   `@ftp_address`: la dirección del servidor FTP usada para entregar la instantánea  
  
    -   (Opcional) `@ftp_port`: el puerto que usa el servidor FTP.  
  
    -   (Opcional) `@ftp_subdirectory`: el subdirectorio del directorio FTP predeterminado asignado a un inicio de sesión de FTP. Por ejemplo, si el directorio raíz del servidor de FTP es \\\servidorFTP\inicio y quiere almacenar las instantáneas en \\\servidorFTP\inicio\instantáneas, especifique **\instantáneas\ftp** para `@ftp_subdirectory` (la replicación anexa "ftp" a la ruta de acceso de la carpeta de instantáneas al crear los archivos de instantáneas).  
  
    -   (Opcional) `@ftp_login`: una cuenta de inicio de sesión que se usa al conectar al servidor FTP.  
  
    -   (Opcional) `@ftp_password`: la contraseña para el inicio de sesión de FTP.  
  
     Esto crea una publicación que usa FTP. Para obtener más información, vea [Crear una suscripción](../../../relational-databases/replication/publish/create-a-publication.md).  
  
#### <a name="to-enable-ftp-snapshot-delivery-for-a-merge-publication"></a>Para habilitar la entrega de instantáneas a través de FTP para una publicación de combinación  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_addmergepublication](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md). Especifique `@publication`, un valor de **true** para `@enabled_for_internet` y los valores adecuados para los parámetros siguientes:  
  
    -   `@ftp_address`: la dirección del servidor FTP usada para entregar la instantánea  
  
    -   (Opcional) `@ftp_port`: el puerto que usa el servidor FTP.  
  
    -   (Opcional) `@ftp_subdirectory`: el subdirectorio del directorio FTP predeterminado asignado a un inicio de sesión de FTP. Por ejemplo, si el directorio raíz del servidor de FTP es \\\servidorFTP\inicio y quiere almacenar las instantáneas en \\\servidorFTP\inicio\instantáneas, especifique **\instantáneas\ftp** para `@ftp_subdirectory` (la replicación anexa "ftp" a la ruta de acceso de la carpeta de instantáneas al crear los archivos de instantáneas).  
  
    -   (Opcional) `@ftp_login`: una cuenta de inicio de sesión que se usa al conectar al servidor FTP.  
  
    -   (Opcional) `@ftp_password`: la contraseña para el inicio de sesión de FTP.  
  
     Esto crea una publicación que usa FTP. Para obtener más información, vea [Crear una suscripción](../../../relational-databases/replication/publish/create-a-publication.md).  
  
#### <a name="to-create-a-pull-subscription-to-a-snapshot-or-transactional-publication-that-uses-ftp-snapshot-delivery"></a>Para crear una suscripción de extracción a una publicación transaccional o de instantáneas que usa la entrega de instantáneas a través de FTP  
  
1.  En el publicador de la base de datos de suscripciones, ejecute [sp_addpullsubscription](../../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md). Especifique `@publisher` y `@publication`.  
  
    -   En el suscriptor de la base de datos de suscripciones, ejecute [sp_addpullsubscription_agent](../../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md). Especifique `@publisher`, `@publisher_db`, `@publication`, las credenciales de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows con las que se ejecuta el Agente de distribución en el suscriptor para `@job_login` y `@job_password`, y un valor de **true** para `@use_ftp`.  
  
2.  En la base de datos de publicación del publicador, ejecute [sp_addsubscription](../../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) para registrar la suscripción de extracción. Para obtener más información, consulte [Create a Pull Subscription](../../../relational-databases/replication/create-a-pull-subscription.md).  
  
#### <a name="to-create-a-pull-subscription-to-a-merge-publication-that-uses-ftp-snapshot-delivery"></a>Para crear una suscripción de extracción a una publicación de combinación que use la entrega de instantáneas a través de FTP  
  
1.  En la base de datos de suscripciones del suscriptor, ejecute [sp_addmergepullsubscription](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md). Especifique `@publisher` y `@publication`.  
  
2.  En el suscriptor de la base de datos de suscripciones, ejecute [sp_addmergepullsubscription_agent](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md). Especifique `@publisher`, `@publisher_db`, `@publication`, las credenciales de Windows con las que se ejecuta el Agente de distribución en el suscriptor para `@job_login` y `@job_password`, y un valor de `true` para `@use_ftp`.  
  
3.  En la base de datos de publicación del publicador, ejecute [sp_addmergesubscription](../../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md) para registrar la suscripción de extracción. Para obtener más información, consulte [Create a Pull Subscription](../../../relational-databases/replication/create-a-pull-subscription.md).  
  
#### <a name="to-change-one-or-more-ftp-snapshot-delivery-settings-for-a-snapshot-or-transactional-publication"></a>Para cambiar una o más configuraciones de entrega de instantáneas a través de FTP para una publicación transaccional o de instantáneas  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_changepublication](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md). Especifique uno de los valores siguientes para `@property` y un nuevo valor de esta configuración para `@value`:  
  
    -   `ftp_address `: la dirección del servidor FTP que se usa para entregar la instantánea.  
  
    -   `ftp_port`: el puerto usado por el servidor FTP  
  
    -   `ftp_subdirectory`: el subdirectorio del directorio FTP predeterminado usado para la instantánea de FTP  
  
    -   `ftp_login`: un inicio de sesión para conectar al servidor de FTP  
  
    -   `ftp_password`: la contraseña del inicio de sesión de FTP  
  
2.  (Opcional) Repita el paso 1 para cada configuración de FTP que se está cambiando.  
  
3.  (Opcional) Para deshabilitar la entrega de instantáneas a través de FTP, ejecute [sp_changepublication](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) en el Publicador de la base de datos de publicación. Especifique un valor de `enabled_for_internet` para `@property` y un valor de `false` para `@value`.  
  
#### <a name="to-change-ftp-snapshot-delivery-settings-for-a-merge-publication"></a>Para cambiar la configuración de entrega de instantáneas a través de FTP para una publicación de combinación  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md). Especifique uno de los valores siguientes para `@property` y un nuevo valor de esta configuración para `@value`:  
  
    -   `ftp_address`: la dirección del servidor FTP usada para entregar la instantánea  
  
    -   `ftp_port`: el puerto usado por el servidor FTP  
  
    -   `ftp_subdirectory`: el subdirectorio del directorio FTP predeterminado usado para la instantánea de FTP  
  
    -   `ftp_login`: un inicio de sesión para conectar al servidor de FTP  
  
    -   `ftp_password`: la contraseña del inicio de sesión de FTP  
  
2.  (Opcional) Repita el paso 1 para cada configuración de FTP que se está cambiando.  
  
3.  (Opcional) Para deshabilitar la entrega de instantáneas a través de FTP, ejecute [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md) en el Publicador de la base de datos de publicación. Especifique un valor de `enabled_for_internet` para `@property` y un valor de `false` para `@value`.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Ejemplos (Transact-SQL)  
 El ejemplo siguiente crea una publicación de combinación que permite a los Suscriptores tener acceso a los datos de instantánea usando FTP. El suscriptor debe usar una conexión VPN segura al obtener acceso al recurso compartido de FTP. Las variables de scripting de **sqlcmd** se usan para proporcionar valores de inicio de sesión y de contraseña. Para obtener más información, vea [Usar sqlcmd con variables de script](../../../ssms/scripting/sqlcmd-use-with-scripting-variables.md).  
  
 [!code-sql[HowTo#sp_createmergepub_ftp](../../../relational-databases/replication/codesnippet/tsql/deliver-a-snapshot-throu_1.sql)]  
  
 El ejemplo siguiente crea una suscripción a una publicación de combinación, donde el Suscriptor obtiene la instantánea mediante FTP. El suscriptor debe usar una conexión VPN segura al obtener acceso al recurso compartido de FTP. Las variables de scripting de **sqlcmd** se usan para proporcionar valores de inicio de sesión y de contraseña. Para obtener más información, vea [Usar sqlcmd con variables de script](../../../ssms/scripting/sqlcmd-use-with-scripting-variables.md).  
  
 [!code-sql[HowTo#sp_createmergepullsub_ftp](../../../relational-databases/replication/codesnippet/tsql/deliver-a-snapshot-throu_2.sql)]  
  
 [!code-sql[HowTo#sp_createmergepullsubagent_ftp](../../../relational-databases/replication/codesnippet/tsql/deliver-a-snapshot-throu_3.sql)]  
  
## <a name="see-also"></a>Consulte también  
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Cambiar las propiedades de la publicación y de los artículos](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Inicializar una suscripción con una instantánea](../../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)  
  
