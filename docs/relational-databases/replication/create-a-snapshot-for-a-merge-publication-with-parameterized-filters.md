---
title: Creación de una instantánea con filtros con parámetros (combinación)
description: Obtenga información sobre cómo crear una instantánea de una publicación de combinación mediante filtros con parámetros.
ms.custom: seo-lt-2019
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- parameterized filters [SQL Server replication], snapshots
- snapshots [SQL Server replication], parameterized filters and
- filters [SQL Server replication], parameterized
ms.assetid: 00dfb229-f1de-4d33-90b0-d7c99ab52dcb
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 89a470964db8b0475a6c5d4f20a5007337645cb1
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076157"
---
# <a name="create-a-snapshot-for-a-merge-publication-with-parameterized-filters"></a>Crear una instantánea para una publicación de mezcla con filtros con parámetros
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
En este tema se describe cómo se crea una instantánea de una publicación de combinación con filtros con parámetros en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o Replication Management Objects (RMO).  

Cuando se utilizan filtros de fila con parámetros en las publicaciones de combinación, la replicación inicializa cada suscripción con una instantánea en dos partes. Primero, se crea una instantánea de esquema que contiene todos los objetos necesarios para la replicación y el esquema de los objetos publicados, pero no los datos. Después, se inicializa cada suscripción con una instantánea que incluye los objetos y el esquema de la instantánea de esquema, y los datos que pertenecen a la partición de la suscripción. Si hay más de una suscripción que recibe una partición determinada (es decir, que reciben el mismo esquema y los mismos datos), la instantánea de esa partición se creará una sola vez; se inicializarán varias suscripciones con la misma instantánea. Para obtener más información acerca de los filtros de fila con parámetros, vea [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 Puede crear instantáneas para publicaciones con filtros con parámetros de las tres formas siguientes:  
  
-   **Genere previamente las instantáneas para cada partición.** Esta opción le permite controlar cuándo se generan las instantáneas.    
     También puede elegir que las instantáneas se actualicen de acuerdo con una programación. Los suscriptores nuevos que se suscriban a una partición para la que se ha creado una instantánea recibirán una instantánea actualizada.   
-   **Permita a los suscriptores que soliciten la generación y aplicación de instantáneas** la primera vez que se sincronicen. Esta opción permite a los nuevos suscriptores sincronizarse sin la intervención de un administrador (el Agente[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se debe estar ejecutando en el publicador para que se pueda generar la instantánea).  
  
    > [!NOTE]  
    >  Si el filtrado de uno o más artículos de la publicación produce particiones no superpuestas y únicas para cada suscripción, los metadatos se limpian cada vez que se ejecuta el Agente de mezcla. Esto significa que la instantánea con particiones expira antes. Cuando utilice esta opción, puede ser conveniente permitir a los suscriptores que inicien la generación y entrega de instantáneas. Para obtener más información acerca de las opciones de filtro, vea [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
-   **Genere manualmente una instantánea para cada suscriptor con el Agente de instantáneas**. A continuación, el suscriptor deberá proporcionar la ubicación de la instantánea al Agente de mezcla para que pueda recuperar y aplicar la instantánea correcta.  
  
    > [!NOTE]  
    >  Esta opción se admite para la compatibilidad con versiones anteriores y no permite los recursos compartidos de instantáneas en FTP.  
  
 El enfoque más flexible es utilizar una combinación de opciones de instantáneas generadas previamente y solicitadas por el suscriptor: las instantáneas se generan previamente y se actualizan según una programación (por lo general, durante los períodos de menor actividad), pero un suscriptor puede generar su propia instantánea si se crea una suscripción que necesita una partición nueva.  
  
 Considere [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)], que tiene un personal móvil que proporciona inventarios a las tiendas. Cada vendedor recibe una suscripción según su inicio de sesión, que recupera los datos de las tiendas a las que prestan servicio. El administrador genera previamente las instantáneas y las actualiza cada domingo. Ocasionalmente, se agrega al sistema un usuario nuevo que necesita datos para una partición que no tiene una instantánea disponible. El administrador también permite las instantáneas iniciadas por el suscriptor, con el fin de evitar situaciones en las que un suscriptor no puede suscribirse a la publicación porque la instantánea aún no está disponible. Cuando el nuevo suscriptor se conecta por primera vez, se genera la instantánea para la partición especificada y se aplica al suscriptor (debe ejecutarse el Agente[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el publicador para que se pueda generar la instantánea).  
  
 Para crear una instantánea para una publicación con filtros con parámetros, vea [Crear una instantánea para una publicación de mezcla con filtros con parámetros](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
## <a name="security-settings-for-the-snapshot-agent"></a>Configuración de seguridad para el Agente de instantáneas  
 El Agente de instantáneas crea instantáneas para cada partición. Para las instantáneas generadas previamente y las solicitadas por un suscriptor, el agente se ejecuta y establece conexiones con las credenciales especificadas cuando se creó el trabajo del Agente de instantáneas para la publicación (el trabajo lo crea el Asistente para nueva publicación o **sp_addpublication_snapshot**). Para cambiar las credenciales, utilice **sp_changedynamicsnapshot_job**. Para obtener más información, consulte [sp_changedynamicsnapshot_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedynamicsnapshot-job-transact-sql.md).  

  
##  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Si desea generar una instantánea de una publicación de combinación mediante filtros con parámetros, debe generar primero una instantánea estándar (o de esquema) que contenga todos los datos publicados y los metadatos del Suscriptor para la suscripción. Para más información, consulte [Crear y aplicar la instantánea inicial](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md). Después de haber creado la instantánea del esquema, puede generar la instantánea que contiene la partición específica del Suscriptor de los datos publicados.  
  
-   Si el filtrado de uno o más artículos de la publicación produce particiones no superpuestas y únicas para cada suscripción, los metadatos se limpian cada vez que se ejecuta el Agente de mezcla. Esto significa que la instantánea con particiones expira antes. Cuando utilice esta opción, puede ser conveniente permitir a los suscriptores que inicien la generación y entrega de instantáneas. 
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Genere instantáneas de particiones en la página **Particiones de datos** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** . Para obtener más información sobre el acceso a este cuadro de diálogo, vea [View and Modify Publication Properties](../../relational-databases/replication/publish/view-and-modify-publication-properties.md). Puede permitir a los suscriptores iniciar la generación y entrega de instantáneas y/o generar instantáneas.  
  
 Antes de generar instantáneas de una o varias particiones debe:  
  
1.  Crear una publicación de combinación con el Asistente para nueva publicación y especificar uno o varios filtros de fila con parámetros en la página **Agregar filtro** del asistente. Para más información, consulte [Definir y modificar un filtro de fila con parámetros para un artículo de mezcla](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
2.  Generar una instantánea del esquema para la publicación. De forma predeterminada, se genera una instantánea del esquema al completar el Asistente para nueva publicación, aunque también puede generar una instantánea del esquema desde [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  

#### <a name="to-generate-a-schema-snapshot"></a>Para generar una instantánea del esquema  
  
1.  Conéctese al publicador en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]y luego expanda el nodo del servidor.  
  
2.  Expanda la carpeta **Replicación** y, a continuación, la carpeta **Publicaciones** .  
  
3.  Haga clic con el botón secundario en la publicación para la que desee crear una instantánea y, a continuación, haga clic en **Ver estado del agente de instantáneas**.  
  
4.  En el cuadro de diálogo **Ver estado del agente de instantáneas: \<Publication>** , haga clic en **Iniciar**.  
  
     Cuando el Agente de instantáneas termina de generar la instantánea, aparece un mensaje del tipo "[100%] Se ha generado una instantánea de 17 artículos".  
  
#### <a name="to-allow-subscribers-to-initiate-snapshot-generation-and-delivery"></a>Para permitir a los suscriptores inicializar la generación y distribución de instantáneas  
  
1.  En la página **Particiones de datos** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** , seleccione **Definir automáticamente una partición y generar una instantánea si es necesario cuando un suscriptor nuevo intente sincronizarse**.  
  
2.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
#### <a name="to-generate-and-refresh-snapshots"></a>Para generar y actualizar instantáneas  
  
1.  En la página **Particiones de datos** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** , haga clic en **Agregar**.  
  
2.  Escriba un valor para **HOST_NAME()** y/o el valor **SUSER_SNAME()** asociado con la partición para la que desea crear una instantánea.  
  
3.  También puede especificar una programación para actualizar las instantáneas:  
  
    1.  Seleccione **Programar el Agente de instantáneas para que esta partición se ejecute a las horas siguientes**.  
  
    2.  Acepte la programación predeterminada para actualizar instantáneas o haga clic en **Cambiar** para especificar una programación diferente.  
  
4.  Haga clic en **Aceptar**, que le lleva de vuelta al cuadro de diálogo **Propiedades de la publicación: \<Publication>** .  
  
5.  Seleccione la partición en la cuadrícula de propiedades y, a continuación, haga clic en **Generar instantáneas seleccionadas ahora**.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Con los procedimientos almacenados y el Agente de instantáneas, podrá hacer lo siguiente:  
  
-   Permita a los suscriptores que soliciten la generación y aplicación de instantáneas la primera vez que se sincronicen.  
  
-   Genere previamente las instantáneas para cada partición.  
  
-   Generar manualmente una instantánea para cada Suscriptor.  
  
    > [!IMPORTANT]  
    >  Cuando sea posible, pida a los usuarios que proporcionen credenciales de seguridad en tiempo de ejecución. Si debe almacenar las credenciales en un archivo de script, proteja el archivo para evitar el acceso no autorizado.  
  
#### <a name="to-create-a-publication-that-allows-subscribers-to-initiate-snapshot-generation-and-delivery"></a>Para crear una publicación que permita a los suscriptores iniciar la generación y entrega de instantáneas  
  
1.  En el publicador de la base de datos de publicación, ejecute [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md). Especifique los parámetros siguientes:  
  
    -   El nombre de la base de datos de publicación para **\@publication**.  
  
    -   Un valor de **true** para **\@allow_subscriber_initiated_snapshot**, que permite a los suscriptores iniciar el proceso de instantánea.  
  
    -   (Opcional) El número de procesos de instantáneas dinámicas que se pueden ejecutar de manera simultánea para **\@max_concurrent_dynamic_snapshots**. Si se está ejecutando el número máximo de procesos y un Suscriptor intenta generar una instantánea, el proceso se coloca en una cola. De forma predeterminada, no hay límite al número de procesos simultáneos.  
  
2.  En el publicador, ejecute [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Especifique el nombre de publicación usado en el paso 1 para **\@publication** y las credenciales de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows con las que se ejecuta el [Agente de instantáneas de replicación](../../relational-databases/replication/agents/replication-snapshot-agent.md) para **\@job_login** y **\@password**. Si el agente va a usar autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al conectarse al publicador, también debe especificar un valor de **0** para **\@publisher_security_mode** y la información de inicio de sesión de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para **\@publisher_login** y **\@publisher_password**. Esto crea un trabajo de Agente de instantáneas para la publicación. Para obtener más información sobre cómo generar una instantánea inicial y definir una programación personalizada para el Agente de instantáneas, vea [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
    > [!IMPORTANT]  
    >  Al configurar un publicador con un distribuidor remoto, los valores suministrados para todos los parámetros, incluidos *job_login* y *job_password*, se envían al distribuidor como texto sin formato. Antes de ejecutar este procedimiento almacenado, se recomienda cifrar la conexión entre el publicador y su distribuidor remoto. Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
3.  Ejecute [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) para agregar artículos a la publicación. Este procedimiento almacenado se debe ejecutar una vez para cada artículo de la publicación. Al usar filtros con parámetros, debe especificar un filtro de fila con parámetros para uno o más artículos mediante el parámetro **\@subset_filterclause**. Para más información, consulte [Definir y modificar un filtro de fila con parámetros para un artículo de mezcla](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
4.  Si se van a filtrar otros artículos según el filtro de fila con parámetros, ejecute [sp_addmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md) para definir la combinación o las relaciones de registros lógicos entre los artículos. Este procedimiento almacenado se debe ejecutar una vez para cada relación que se está definiendo. Para obtener más información, consulte [Definir y modificar un filtro de combinación entre artículos de mezcla](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
5.  Cuando el Agente de mezcla solicita a la instantánea que inicialice el Suscriptor, se genera automáticamente la instantánea para la partición de la suscripción solicitante.  
  
#### <a name="to-create-a-publication-and-pre-generate-or-automatically-refresh-snapshots"></a>Para crear una publicación y pregenerar o actualizar automáticamente las instantáneas  
  
1.  Ejecute [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md) para crear la publicación. Para obtener más información, vea [Crear una suscripción](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  En el publicador, ejecute [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Especifique el nombre de publicación usado en el paso 1 para **\@publication** y las credenciales de Windows con las que se ejecuta el Agente de instantáneas para **\@job_login** y **\@password**. Si el agente va a usar autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al conectarse al publicador, también debe especificar un valor de **0** para **\@publisher_security_mode** y la información de inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para **\@publisher_login** y **\@publisher_password**. Esto crea un trabajo de Agente de instantáneas para la publicación. Para obtener más información sobre cómo generar una instantánea inicial y definir una programación personalizada para el Agente de instantáneas, vea [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
    > [!IMPORTANT]  
    >  Al configurar un publicador con un distribuidor remoto, los valores suministrados para todos los parámetros, incluidos *job_login* y *job_password*, se envían al distribuidor como texto sin formato. Antes de ejecutar este procedimiento almacenado, se recomienda cifrar la conexión entre el publicador y su distribuidor remoto. Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
3.  Ejecute [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) para agregar artículos a la publicación. Este procedimiento almacenado se debe ejecutar una vez para cada artículo de la publicación. Al usar filtros con parámetros, debe especificar un filtro de fila con parámetros para un artículo mediante el parámetro **\@subset_filterclause**. Para más información, consulte [Definir y modificar un filtro de fila con parámetros para un artículo de mezcla](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
4.  Si se van a filtrar otros artículos según el filtro de fila con parámetros, ejecute [sp_addmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md) para definir la combinación o las relaciones de registros lógicos entre los artículos. Este procedimiento almacenado se debe ejecutar una vez para cada relación que se está definiendo. Para obtener más información, consulte [Definir y modificar un filtro de combinación entre artículos de mezcla](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
5.  En el publicador de la base de datos de publicación, ejecute [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md) y especifique el valor de **\@publication** del paso 1. Tenga en cuenta el valor de **snapshot_jobid** en el conjunto de resultados.  
  
6.  Convierta el valor de **snapshot_jobid** obtenido en el paso 5 a **uniqueidentifier**.  
  
7.  En el publicador en la base de datos **msdb**, ejecute [sp_start_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md) y especifique el valor convertido obtenido en el paso 6 para **\@job_id**.  
  
8.  En el publicador de la base de datos de publicación, ejecute [sp_addmergepartition &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md). Especifique el nombre de la publicación del paso 1 para **\@publication** y el valor usado para definir la partición para **\@suser_sname** si se ha usado [SUSER_SNAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sname-transact-sql.md) en la cláusula de filtro o para **\@host_name** si se ha usado [HOST_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/host-name-transact-sql.md).  
  
9. En el publicador de la base de datos de publicación, ejecute [sp_adddynamicsnapshot_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddynamicsnapshot-job-transact-sql.md). Especifique el nombre de la publicación del paso 1 para **\@publication**, el valor de **\@suser_sname** o **\@host_name** del paso 8, y una programación para el trabajo. Esto crea el trabajo que genera la instantánea con parámetros para la partición especificada. Para obtener más información, consulte [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
    > [!NOTE]  
    >  Este trabajo se ejecuta usando la misma cuenta de Windows que el trabajo de la instantánea inicial definido en el paso 2. Para quitar el trabajo de la instantánea con parámetros y su partición de datos relacionados, ejecute [sp_dropdynamicsnapshot_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdynamicsnapshot-job-transact-sql.md).  
  
10. En el publicador de la base de datos de publicación, ejecute [sp_helpmergepartition &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepartition-transact-sql.md) y especifique el valor de **\@publication** del paso 1 y el valor de **\@suser_sname** o **\@host_name** del paso 8. Tenga en cuenta el valor de **dynamic_snapshot_jobid** en el conjunto de resultados.  
  
11. En el distribuidor de la base de datos **msdb**, ejecute [sp_start_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md) y especifique el valor obtenido en el paso 9 para **\@job_id**. Esto inicia el trabajo de instantánea con parámetros para la partición.  
  
12. Repita los pasos 8-11 para generar una instantánea con particiones para cada suscripción.  
  
#### <a name="to-create-a-publication-and-manually-create-snapshots-for-each-partition"></a>Para crear una publicación y crear manualmente instantáneas para cada partición  
  
1.  Ejecute [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md) para crear la publicación. Para obtener más información, vea [Crear una suscripción](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  En el publicador, ejecute [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Especifique el nombre de publicación usado en el paso 1 para **\@publication** y las credenciales de Windows con las que se ejecuta el Agente de instantáneas para **\@job_login** y **\@password**. Si el agente va a usar autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al conectarse al publicador, también debe especificar un valor de **0** para **\@publisher_security_mode** y la información de inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para **\@publisher_login** y **\@publisher_password**. Esto crea un trabajo de Agente de instantáneas para la publicación. Para obtener más información sobre cómo generar una instantánea inicial y definir una programación personalizada para el Agente de instantáneas, vea [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
    > [!IMPORTANT]  
    >  Al configurar un publicador con un distribuidor remoto, los valores suministrados para todos los parámetros, incluidos *job_login* y *job_password*, se envían al distribuidor como texto sin formato. Antes de ejecutar este procedimiento almacenado, se recomienda cifrar la conexión entre el publicador y su distribuidor remoto. Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
3.  Ejecute [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) para agregar artículos a la publicación. Este procedimiento almacenado se debe ejecutar una vez para cada artículo de la publicación. Al usar filtros con parámetros, debe especificar un filtro de fila con parámetros para al menos un artículo mediante el parámetro **\@subset_filterclause**. Para más información, consulte [Definir y modificar un filtro de fila con parámetros para un artículo de mezcla](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
4.  Si se van a filtrar otros artículos según el filtro de fila con parámetros, ejecute [sp_addmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md) para definir la combinación o las relaciones de registros lógicos entre los artículos. Este procedimiento almacenado se debe ejecutar una vez para cada relación que se está definiendo. Para obtener más información, consulte [Definir y modificar un filtro de combinación entre artículos de mezcla](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
5.  Inicie el trabajo de la instantánea o ejecute el Agente de instantáneas de replicación desde el símbolo del sistema para generar el esquema de instantáneas estándar y otros archivos. Para más información, consulte [Crear y aplicar la instantánea inicial](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
6.  Ejecute de nuevo el Agente de instantáneas de replicación desde el símbolo del sistema para generar los archivos de copia masiva (.bcp), especificando la ubicación de la instantánea con particiones para **-DynamicSnapshotLocation** y una o ambas de las siguientes propiedades que define la partición:  
  
    -   **-DynamicFilterHostName**: el valor si se utiliza [HOST_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/host-name-transact-sql.md).  
  
    -   **-DynamicFilterLogin**: el valor si se utiliza [SUSER_SNAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sname-transact-sql.md).  
  
7.  Repita el paso 6 para generar una instantánea con particiones para cada suscripción.  
  
8.  Ejecute el Agente de mezcla para que cada suscripción aplique la instantánea con particiones inicial en los Suscriptores, especificando las propiedades siguientes:  
  
    -   **-Hostname** - el valor usado para definir la partición si se invalida el valor real de HOST_NAME.  
  
    -   **-DynamicSnapshotLocation** - la ubicación de la instantánea dinámica para esta partición.  
  
> [!NOTE]  
>  Para obtener más información sobre cómo programar los agentes de replicación, consulte [Conceptos de los ejecutables del Agente de replicación](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Ejemplos (Transact-SQL)  
 Este ejemplo crea una publicación de combinación con filtros con parámetros donde los Suscriptores inician el proceso de generación de instantáneas. Los valores para **\@job_login** y **\@job_password** se pasan mediante variables de scripting.  
  
 [!code-sql[HowTo#sp_MergeDynamicPub1](../../relational-databases/replication/codesnippet/tsql/create-a-snapshot-for-a-_1.sql)]  
  
 Este ejemplo crea una publicación mediante un filtro con parámetros donde cada Suscriptor tiene su partición definida ejecutando [sp_addmergepartition](../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md) y el trabajo de instantánea filtrado creado ejecutando [sp_adddynamicsnapshot_job](../../relational-databases/system-stored-procedures/sp-adddynamicsnapshot-job-transact-sql.md) que pasa la información de la partición. Los valores para **\@job_login** y **\@job_password** se pasan mediante variables de scripting.  
  
 [!code-sql[HowTo#sp_MergeDynamicPubPlusPartition](../../relational-databases/replication/codesnippet/tsql/create-a-snapshot-for-a-_2.sql)]  
  
 Este ejemplo crea una publicación mediante un filtro con parámetros donde cada Suscriptor debe tener su partición de datos y el trabajo de instantánea filtrado creado proporcionando la información de la partición. Un Suscriptor proporciona información de la partición mediante los parámetros de línea de comandos al ejecutar manualmente los agentes de replicación. Este ejemplo supone que también se ha creado una suscripción a la publicación.  
  
 [!code-sql[HowTo#sp_MergeDynamicPubPartitionManual](../../relational-databases/replication/codesnippet/tsql/create-a-snapshot-for-a-_3.sql)]  
  
```  
  
REM Line breaks are added to improve readability.   
REM In a batch file, commands must be made in a single line.  
REM Run the Snapshot agent from the command line to generate the standard snapshot   
REM schema and other files.   
SET DistPub=%computername%  
SET PubDB=AdventureWorks2012   
SET PubName=AdvWorksSalesPersonMerge  
  
"C:\Program Files\Microsoft SQL Server\120\COM\SNAPSHOT.EXE" -Publication %PubName%    
-Publisher %DistPub% -Distributor  %DistPub%  -PublisherDB %PubDB%  -ReplicationType 2    
-OutputVerboseLevel 1  -DistributorSecurityMode 1  
  
PAUSE  
  
```  
  
```  
  
REM Run the Snapshot agent from the command line, this time to generate   
REM the bulk copy (.bcp) data for each Subscriber partition.    
SET DistPub=%computername%  
SET PubDB=AdventureWorks2012   
SET PubName=AdvWorksSalesPersonMerge  
SET SnapshotDir=\\%DistPub%\repldata\unc\fernando  
  
MD %SnapshotDir%  
  
"C:\Program Files\Microsoft SQL Server\120\COM\SNAPSHOT.EXE" -Publication %PubName%    
-Publisher %DistPub%  -Distributor  %DistPub%  -PublisherDB %PubDB%  -ReplicationType 2    
-OutputVerboseLevel 1  -DistributorSecurityMode 1  -DynamicFilterHostName "adventure-works\Fernando"    
-DynamicSnapshotLocation %SnapshotDir%  
  
PAUSE  
  
```  
  
```  
  
REM Run the Merge Agent for each subscription to apply the partitioned   
REM snapshot for each Subscriber.    
SET Publisher = %computername%  
SET Subscriber = %computername%  
SET PubDB = AdventureWorks2012   
SET SubDB = AdventureWorks2012Replica   
SET PubName = AdvWorksSalesPersonMerge   
SET SnapshotDir=\\%DistPub%\repldata\unc\fernando  
  
"C:\Program Files\Microsoft SQL Server\120\COM\REPLMERG.EXE" -Publisher  %Publisher%    
-Subscriber  %Subscriber%  -Distributor %Publisher%  -PublisherDB %PubDB%    
-SubscriberDB %SubDB% -Publication %PubName%  -PublisherSecurityMode 1  -OutputVerboseLevel 3    
-Output -SubscriberSecurityMode 1  -SubscriptionType 3 -DistributorSecurityMode 1    
-Hostname "adventure-works\Fernando"  -DynamicSnapshotLocation %SnapshotDir%  
  
PAUSE  
  
```  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Uso de Replication Management Objects (RMO)  
 Puede utilizar Replication Management Objects (RMO) para generar mediante programación instantáneas con particiones de las maneras siguientes:  
  
-   Permita a los suscriptores que soliciten la generación y aplicación de instantáneas la primera vez que se sincronicen.  
  
-   Genere previamente las instantáneas para cada partición.  
  
-   Genere manualmente una instantánea para cada suscriptor ejecutando el Agente de instantáneas.  
  
> [!NOTE]  
>  Si al filtrar un artículo se producen particiones no superpuestas que son únicas para cada suscripción (especificando el valor F:Microsoft.SqlServer.Replication.PartitionOptions.NonOverlappingSingleSubscription para P:Microsoft.SqlServer.Replication.MergeArticle.PartitionOption al crear un artículo de mezcla), cada vez que se ejecuta el Agente de mezcla se limpian los metadatos. Esto significa que la instantánea con particiones expira antes. Cuando utilice esta opción, puede ser conveniente permitir a los suscriptores que soliciten la generación de instantáneas. Para obtener más información, vea la sección Usar las opciones de filtrado apropiadas del tema [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
> [!IMPORTANT]  
>  Cuando sea posible, pida a los usuarios que proporcionen credenciales de seguridad en tiempo de ejecución. Si debe almacenar credenciales, use los [servicios de cifrado](/previous-versions/aa719848(v=vs.71)) (en inglés) proporcionados por [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows .NET Framework.  
  
#### <a name="to-create-a-publication-that-allows-subscribers-to-initiate-snapshot-generation-and-delivery"></a>Para crear una publicación que permita a los suscriptores iniciar la generación y entrega de instantáneas  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.ReplicationDatabase> para la base de datos de publicación, establezca la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en la instancia de <xref:Microsoft.SqlServer.Management.Common.ServerConnection> del paso 1 y llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . If <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> devuelve **false**, confirme que la base de datos existe.  
  
3.  If <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledMergePublishing%2A> está establecida en **false**, cámbiela a **true** y llame a <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A>.  
  
4.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergePublication> y establezca las propiedades siguientes para este objeto:  
  
    -   La <xref:Microsoft.SqlServer.Management.Common.ServerConnection> del paso 1 para <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
    -   El nombre de la base de datos publicada para <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>.  
  
    -   Un nombre de publicación para <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>.  
  
    -   El número máximo de trabajos de instantáneas dinámicas que se van a ejecutar para <xref:Microsoft.SqlServer.Replication.MergePublication.MaxConcurrentDynamicSnapshots%2A>. Dado que las solicitudes de instantáneas iniciadas por el suscriptor pueden producirse en cualquier momento, esta propiedad limita el número de trabajos del Agente de instantáneas que se pueden ejecutar simultáneamente cuando varios suscriptores solicitan al mismo tiempo su instantánea con particiones. Cuando se está ejecutando el número máximo de trabajos, las demás solicitudes de instantáneas con particiones se ponen en la cola hasta que se completa uno de los trabajos en ejecución.  
  
    -   Utilice el operador lógico OR bit a bit ( **|** en Visual C# y **Or** en Visual Basic) para agregar el valor <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowSubscriberInitiatedSnapshot> a <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A>.  
  
    -   Los campos <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> y <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> para proporcionar las credenciales de la cuenta de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows con la que se ejecuta el trabajo del Agente de mezcla.  
  
        > [!NOTE]  
        >  Se recomienda establecer <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> cuando un miembro del rol fijo de servidor **sysadmin** crea la publicación. Para más información, consulte [Modelo de seguridad del agente de replicación](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
5.  Llame al método <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> para crear la publicación.  
  
    > [!IMPORTANT]  
    >  Cuando se configura un Publicador con un Distribuidor remoto, los valores suministrados para todas las propiedades, incluidos <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A>, se envían al Distribuidor como texto simple. Debe cifrar la conexión entre el publicador y su distribuidor remoto antes de llamar al método <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> . Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
6.  Utilice la propiedad <xref:Microsoft.SqlServer.Replication.MergeArticle> para agregar artículos a la publicación. Especifique la propiedad <xref:Microsoft.SqlServer.Replication.MergeArticle.FilterClause%2A> al menos para un artículo que defina el filtro con parámetros. (Opcional) Cree objetos <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> que definan filtros de combinación entre artículos. Para más información, consulte [Define an Article](../../relational-databases/replication/publish/define-an-article.md).  
  
7.  Si el valor de <xref:Microsoft.SqlServer.Replication.Publication.SnapshotAgentExists%2A> es **false**, llame a <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> para crear el trabajo inicial de Agente de instantáneas para esta publicación.  
  
8.  Llame al método <xref:Microsoft.SqlServer.Replication.Publication.StartSnapshotGenerationAgentJob%2A> del objeto <xref:Microsoft.SqlServer.Replication.MergePublication> creado en el paso 4. Esto inicia el trabajo de agente que genera la instantánea inicial. Para obtener más información sobre cómo generar una instantánea inicial y definir una programación personalizada para el Agente de instantáneas, vea [Create and Apply the Initial Snapshot](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
9. (Opcional) Compruebe que hay establecido un valor **true** en la propiedad <xref:Microsoft.SqlServer.Replication.MergePublication.SnapshotAvailable%2A> para que determine cuándo está lista para usarse la instantánea inicial.  
  
10. Cuando el Agente de mezcla de un suscriptor se conecta por primera vez, se genera automáticamente una instantánea con particiones.  
  
#### <a name="to-create-a-publication-and-pregenerate-or-automatically-refresh-snapshots"></a>Para crear una publicación y pregenerar o actualizar automáticamente las instantáneas  
  
1.  Utilice una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergePublication> para definir una publicación de combinación. Para obtener más información, vea [Crear una suscripción](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Utilice la propiedad <xref:Microsoft.SqlServer.Replication.MergeArticle> para agregar artículos a la publicación. Especifique la propiedad <xref:Microsoft.SqlServer.Replication.MergeArticle.FilterClause%2A> al menos para un artículo que defina el filtro con parámetros y cree objetos <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> que definan filtros de unión entre artículos. Para más información, consulte [Define an Article](../../relational-databases/replication/publish/define-an-article.md).  
  
3.  Si el valor de <xref:Microsoft.SqlServer.Replication.Publication.SnapshotAgentExists%2A> es **false**, llame a <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> para crear el trabajo de Agente de instantáneas para esta publicación.  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.Publication.StartSnapshotGenerationAgentJob%2A> del objeto <xref:Microsoft.SqlServer.Replication.MergePublication> creado en el paso 1. Este método inicia el trabajo de agente que genera la instantánea inicial. Para obtener más información sobre cómo generar una instantánea inicial y definir una programación personalizada para el Agente de instantáneas, vea [Crear y aplicar la instantánea inicial](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
5.  Compruebe que hay establecido un valor **true** en la propiedad <xref:Microsoft.SqlServer.Replication.MergePublication.SnapshotAvailable%2A> para que determine cuándo está lista para usarse la instantánea inicial.  
  
6.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergePartition> y establezca los criterios de filtro con parámetros para el suscriptor utilizando una o ambas de las propiedades siguientes:  
  
    -   Si el resultado de [SUSER_SNAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sname-transact-sql.md) define la partición del suscriptor, use <xref:Microsoft.SqlServer.Replication.MergePartition.DynamicFilterLogin%2A>.  
  
    -   Si el resultado de [HOST_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/host-name-transact-sql.md) o una sobrecarga de esta función definen la partición del suscriptor, use <xref:Microsoft.SqlServer.Replication.MergePartition.DynamicFilterHostName%2A>.  
  
7.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergeDynamicSnapshotJob> y establezca la misma propiedad como en el paso 6.  
  
8.  Utilice la clase <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule> para definir una programación para generar la instantánea filtrada para la partición del suscriptor.  
  
9. Utilizando la instancia de <xref:Microsoft.SqlServer.Replication.MergePublication> del paso 1, llame a <xref:Microsoft.SqlServer.Replication.MergePublication.AddMergePartition%2A>. Pase el objeto <xref:Microsoft.SqlServer.Replication.MergePartition> del paso 6.  
  
10. Utilizando la instancia de <xref:Microsoft.SqlServer.Replication.MergePublication> del paso 1, llame al método <xref:Microsoft.SqlServer.Replication.MergePublication.AddMergeDynamicSnapshotJob%2A> . Pase el objeto <xref:Microsoft.SqlServer.Replication.MergeDynamicSnapshotJob> del paso 7 y el objeto <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule> del paso 8.  
  
11. Llame a <xref:Microsoft.SqlServer.Replication.MergePublication.EnumMergeDynamicSnapshotJobs%2A>y busque el objeto <xref:Microsoft.SqlServer.Replication.MergeDynamicSnapshotJob> para el trabajo de instantánea con particiones recientemente agregado en la matriz devuelta.  
  
12. Obtenga la propiedad <xref:Microsoft.SqlServer.Replication.MergeDynamicSnapshotJob.Name%2A> de la tarea.  
  
13. Cree una conexión al distribuidor mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
14. Cree una instancia de la clase de Objetos de administración de SQL Server (SMO) <xref:Microsoft.SqlServer.Management.Smo.Server> , pasando el objeto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> del paso 13.  
  
15. Cree una instancia de la clase <xref:Microsoft.SqlServer.Management.Smo.Agent.Job> , pasando la propiedad <xref:Microsoft.SqlServer.Management.Smo.Server.JobServer%2A> del objeto <xref:Microsoft.SqlServer.Management.Smo.Server> del paso 14 y el nombre de trabajo del paso 12.  
  
16. Llame al método <xref:Microsoft.SqlServer.Management.Smo.Agent.Job.Start%2A> para iniciar el trabajo de instantánea con particiones.  
  
17. Repita los pasos 6-16 para cada suscriptor.  
  
#### <a name="to-create-a-publication-and-manually-create-snapshots-for-each-partition"></a>Para crear una publicación y crear manualmente instantáneas para cada partición  
  
1.  Utilice una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergePublication> para definir una publicación de combinación. Para obtener más información, vea [Crear una suscripción](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Utilice la propiedad <xref:Microsoft.SqlServer.Replication.MergeArticle> para agregar artículos a la publicación. Especifique la propiedad <xref:Microsoft.SqlServer.Replication.MergeArticle.FilterClause%2A> al menos para un artículo que defina el filtro con parámetros y cree objetos <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> que definan filtros de combinanción entre artículos. Para más información, consulte [Define an Article](../../relational-databases/replication/publish/define-an-article.md).  
  
3.  Genere la instantánea inicial. Para más información, consulte [Crear y aplicar la instantánea inicial](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
4.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent> y establezca las propiedades necesarias siguientes:  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publisher%2A> - nombre del publicador  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherDatabase%2A> - nombre de la base de datos de publicación  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publication%2A> - nombre de la publicación  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Distributor%2A> - nombre del distribuidor  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherSecurityMode%2A> - el valor <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> para utilizar autenticación integrada de Windows o un valor <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> para usar autenticación de SQL Server.  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorSecurityMode%2A> - el valor <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> para utilizar autenticación integrada de Windows o un valor <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> para usar autenticación de SQL Server.  
  
5.  Establezca un valor <xref:Microsoft.SqlServer.Replication.ReplicationType.Merge> para <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.ReplicationType%2A>.  
  
6.  Establezca una o más de las propiedades siguientes para definir los parámetros del particionamiento:  
  
    -   Si el resultado de [SUSER_SNAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sname-transact-sql.md) define la partición del suscriptor, use <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DynamicFilterLogin%2A>.  
  
    -   Si el resultado de [HOST_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/host-name-transact-sql.md) o una sobrecarga de esta función definen la partición del suscriptor, use <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DynamicFilterHostName%2A>.  
  
7.  Llame al método <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.GenerateSnapshot%2A> .  
  
8.  Repita los pasos 4-7 para cada suscriptor.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Ejemplos (RMO)  
 En este ejemplo se crea una publicación de combinación que permite a los suscriptores solicitar la generación de instantáneas.  
  
 [!code-cs[HowTo#rmo_CreateMergePub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepub)]  
  
 En este ejemplo se crea manualmente la partición del suscriptor y la instantánea filtrada para una publicación de combinación con filtros de filas con parámetros.  
  
 [!code-cs[HowTo#rmo_CreateMergePartition](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepartition)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePartition](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepartition)]  
  
 En este ejemplo se inicia manualmente el Agente de instantáneas para generar la instantánea de datos filtrados de un suscriptor para una publicación de combinación con filtros de filas con parámetros.  
  
 [!code-cs[HowTo#rmo_GenerateFilteredSnapshot](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_generatefilteredsnapshot)]  
  
 [!code-vb[HowTo#rmo_vb_GenerateFilteredSnapshot](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_generatefilteredsnapshot)]  
  
## <a name="see-also"></a>Consulte también  
 [Filtros de fila con parámetros](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [Replication System Stored Procedures Concepts](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Procedimientos recomendados de seguridad de replicación](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
