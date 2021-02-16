---
title: Administración de una base de datos del publicador replicada como parte de un grupo de disponibilidad
description: Una descripción sobre cómo administrar y mantener una base de datos que actúa como publicador en una replicación de SQL y también participa en un grupo de disponibilidad Always On.
ms.custom: seodec18
ms.date: 05/18/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: 55b345fe-2eb9-4b04-a900-63d858eec360
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3068eaae089c4a9e7cd6ab2599eb04695d56aa6f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348435"
---
# <a name="manage-a-replicated-publisher-database-as-part-of-an-always-on-availability-group"></a>Administración de una base de datos del publicador replicada como parte de un grupo de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  En este tema se tratan consideraciones especiales para mantener una base de datos de publicación cuando se usan grupos de disponibilidad AlwaysOn.  
  
##  <a name="maintaining-a-published-database-in-an-availability-group"></a><a name="MaintainPublDb"></a> Mantener una base de datos publicada en un grupo de disponibilidad  
 El mantenimiento de una base de datos de publicación AlwaysOn se realiza básicamente de la misma forma que para una base de datos de publicación estándar, con las siguientes salvedades:  
  
-   La administración debe producirse en el host de réplica principal. En [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], las publicaciones aparecen bajo la carpeta de **Publicaciones locales** para el host de réplica principal y también para las réplicas secundarias legibles. Después de la conmutación por error, puede que tenga que actualizar manualmente [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] para que el cambio quede reflejado si el elemento secundario promovido a principal no era legible.  
  
-   El Monitor de replicación muestra siempre la información de publicación en el publicador original. Sin embargo, esta información se puede ver en el Monitor de replicación de cualquier réplica al agregar el publicador original como servidor.  
  
-   Si se utilizan procedimientos almacenados o Replication Management Objects (RMO) para administrar la replicación en la réplica principal actual, en los casos en que se especifica el nombre del publicador, se debe especificar el nombre de la instancia en la que la base de datos se habilitó para la replicación (el publicador original). Para determinar el nombre correcto, use la función **PUBLISHINGSERVERNAME** . Cuando una base de datos de publicación se une a un grupo de disponibilidad, los metadatos de replicación almacenados en las réplicas de la base de datos secundaria son idénticos a los de la principal. En consecuencia, para las bases de datos de publicación habilitadas para replicación en la entidad principal, el nombre de la instancia del publicador que está almacenado en las tablas del sistema en la entidad secundaria es el nombre de la entidad principal en lugar del nombre de la entidad secundaria. Esto afecta a la configuración y al mantenimiento de la replicación si se produce la conmutación por error de la base de datos de publicación a la entidad secundaria. Por ejemplo, si va a configurar la replicación con procedimientos almacenados en una entidad secundaria después de la conmutación por error y quiere una suscripción de extracción a una base de datos de publicación que se ha habilitado en otra réplica, debe especificar el nombre del publicador original en lugar del publicador actual como parámetro *\@publicador* de **sp_addpullsubscription** o **sp_addmergepulllsubscription**. Sin embargo, si habilita una base de datos de publicación después de la conmutación por error, el nombre de la instancia del publicador almacenado en las tablas del sistema es el nombre del host principal actual. En este caso, usaría el nombre de host de la réplica principal actual para el parámetro *\@publisher*.  
  
    > [!NOTE]  
    >  En algunos procedimientos, como **sp_addpublication**, el parámetro *\@publisher* solo se admite para publicadores que no sean instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]; en estos casos, no es relevante para [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] AlwaysOn.  
  
-   Para sincronizar una suscripción en [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] tras una conmutación por error, sincronice las suscripciones de extracción del suscriptor y sincronice las suscripciones de inserción del publicador activo.  
  
##  <a name="removing-a-published-database-from-an-availability-group"></a><a name="RemovePublDb"></a> Quitar una base de datos publicada de un grupo de disponibilidad  
 Tenga en cuenta los siguientes problemas si quita una base de datos publicada de un grupo de disponibilidad, o si quita un grupo de disponibilidad que tiene una base de datos de miembros publicada.  
  
-   Si la base de datos de publicación del publicador original se quita de la réplica principal de un grupo de disponibilidad, debe ejecutar **sp_redirect_publisher** sin especificar un valor para el parámetro *\@redirected_publisher* a fin de quitar el redireccionamiento del par de publicador y base de datos.  
  
    ```  
    EXEC sys.sp_redirect_publisher   
        @original_publisher = 'MyPublisher',  
        @published_database = 'MyPublishedDB';  
    ```  
  
     La base de datos se quedará en el estado de recuperación en el servidor principal y debe restaurarse. Una vez que haya realizado esto, la replicación debe funcionar sin cambios en el publicador original.  
  
-   Si la base de datos de publicación conmuta por error desde el publicador original a una réplica y la base de datos se quita de la réplica principal del grupo de disponibilidad, use el procedimiento almacenado **sp_redirect_publisher** para redirigir explícitamente el publicador original al nuevo publicador. La base de datos se quedará en el estado de recuperación y debe restaurarse. Una vez que haya realizado esto, la replicación debe continuar funcionando como lo hizo con el grupo de disponibilidad.  
  
    ```  
    EXEC sys.sp_redirect_publisher   
        @original_publisher = 'MyPublisher',  
        @published_database = 'MyPublishedDB',  
        @redirected_publisher = 'MyNewPublisher';  
    ```  
  
     No quite el servidor remoto para el publicador original del distribuidor, incluso si ya no se puede tener acceso al servidor. Los metadatos de servidor del publicador original son necesarios en el distribuidor para satisfacer las consultas de los metadatos de la publicación.  
  
-   Si se quita un grupo de disponibilidad completo, el comportamiento con respecto a una base de datos replicada de miembros es el mismo que cuando se quita una base de datos publicada de un grupo de disponibilidad. La replicación se puede reanudar desde el último servidor principal tan pronto como se haya restaurado la base de datos y se haya modificado la redirección. Si la base de datos se restaura en su publicador original, debe quitase la redirección. Si la base de datos se restaura en un host diferente, la redirección debe ejecutarse explícitamente el nuevo host.  
  
    > [!NOTE]  
    >  Cuando se quita un grupo de disponibilidad que ha publicado bases de datos de miembros o una base de datos publicada de un grupo de disponibilidad, todas las copias de las bases de datos publicadas se dejarán en estado de recuperación. Si se restaura, cada una aparecerá como base de datos publicada. Solo se debe conservar una copia con los metadatos de la publicación. Para deshabilitar la replicación para una copia de la base de datos publicada, primero debe quitar todas las suscripciones y publicaciones de la base de datos.  
  
     Ejecute **sp_dropsubscription** para quitar las suscripciones de publicaciones. Asegúrese de establecer el parámetro *\@ignore_distributor* en 1 para mantener los metadatos de la base de datos de publicación activa en el distribuidor.  
  
    ```  
    USE MyDBName;  
    GO  
  
    EXEC sys.sp_dropsubscription   
        @subscriber = 'MySubscriber',  
        @publication = 'MyPublication',  
        @article = 'all',  
        @ignore_distributor = 1;  
    ```  
  
     Ejecute **sp_droppublication** para quitar todas las publicaciones. De nuevo, establezca el parámetro *\@ignore_distributor* en 1 para mantener los metadatos de la base de datos de publicación activa en el distribuidor.  
  
    ```  
    EXEC sys.sp_droppublication   
        @publication = 'MyPublication',  
        @ignore_distributor = 1;  
    ```  
  
     Ejecute **sp_replicationdboption** para deshabilitar la replicación de la base de datos.  
  
    ```  
    EXEC sys.sp_replicationdboption  
        @dbname = 'MyDBName',  
        @optname = 'publish',  
        @value = 'false';  
    ```  
  
     En este momento, la copia de la base de datos publicada se puede conservar o quitar.  

## <a name="remove-original-publisher"></a>Eliminación del publicador original

Puede haber instancias (reemplazo del servidor anterior, actualización del sistema operativo, etc.) donde desee quitar un publicador original de un grupo de disponibilidad Always On. Siga los pasos de esta sección para quitar el publicador del grupo de disponibilidad. 

Supongamos que tiene los servidores N1, N2 y D1, donde N1 y N2 son la réplica principal y secundaria del grupo de disponibilidad AG1, N1 es el publicador original de una publicación transaccional y D1 es el distribuidor. Quiere reemplazar el publicador original N1 por el nuevo publicador N3. 

Para quitar el publicador, siga estos pasos: 

1. Instale y configure SQL Server en el nodo N3. La versión de SQL Server debe ser la misma que la del publicador original. 
1. En el servidor de distribuidor D1, agregue N3 como publicador mediante [sp_adddistpublisher](../../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md). 
1. Configure N3 como publicador con D1 como su distribuidor. 
1. Agregue N3 como una réplica al grupo de disponibilidad AG1. 
1. En la réplica N3, compruebe que los suscriptores de inserción de la publicación aparecen como servidores vinculados. Use [sp_addlinkedserver](../../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) o SQL Server Management Studio. 
1. Una vez que N3 esté sincronizado, conmute por error el grupo de disponibilidad a N3 como principal. 
1. Quite N1 del grupo de disponibilidad AG1. 

Tenga en cuenta lo siguiente:
- No quite el servidor remoto del publicador original (en este caso, N1) ni los metadatos asociados a él del distribuidor, aunque ya no se pueda acceder al servidor. Los metadatos de servidor del publicador original son necesarios en el distribuidor para satisfacer las consultas de los metadatos de la publicación y, sin ellos, la replicación fallará. 
- Para SQL Server 2014, una vez que se haya quitado el publicador original, no podrá usar el nombre del publicador original para administrar la replicación en el monitor de replicación. Si intenta registrar nuevas réplicas como publicador en el monitor de replicación, la información no se mostrará ya que no hay metadatos asociados a él. Para administrar la replicación en este escenario, tendrá que hacer clic con el botón derecho en las publicaciones y suscripciones individuales en SQL Server Management Studio (SSMS).
- Para SQL Server 2016 SP2-CU3, SQL Server 2017 CU6 y versiones posteriores, registre el agente de escucha del publicador del grupo de disponibilidad en el monitor de replicación para administrar la replicación mediante la versión de 17.7 de SQL Server Management Studio y versiones posteriores. 
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Configurar la replicación para grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server.md)  
  
-   [Replicación, seguimiento de cambios, captura de datos modificados y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)  
  
-   [Preguntas más frecuentes para administradores de replicación](../../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)  
  
-   [Suscriptores de replicación y Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md)  
  
## <a name="see-also"></a>Consulte también  
 [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Grupos de disponibilidad Always On: interoperabilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
 [Replicación de SQL Server](../../../relational-databases/replication/sql-server-replication.md)  
  
  
