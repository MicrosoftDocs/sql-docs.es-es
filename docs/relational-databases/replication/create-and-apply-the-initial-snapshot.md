---
title: Creación y aplicación de la instantánea inicial | Microsoft Docs
description: Obtenga información sobre cómo crear y aplicar la instantánea inicial en SQL Server mediante SQL Server Management Studio, Transact-SQL o Replication Management Objects.
ms.custom: ''
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- snapshots [SQL Server replication], creating
- snapshot replication [SQL Server], initial snapshots
ms.assetid: 742727a1-5189-44ec-b3ae-6fd7aa1f5347
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: e1ed0fd0126f63032fe7d041bfc4cd949561c14f
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076347"
---
# <a name="create-and-apply-the-initial-snapshot"></a>Crear y aplicar la instantánea inicial
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
En este tema se describe cómo crear y aplicar la instantánea inicial en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o Replication Management Objects (RMO). Las publicaciones de mezcla que usan filtros con parámetros necesitan una instantánea de dos partes. Para más información, consulte [Crear una instantánea para una publicación de mezcla con filtros con parámetros](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  El Agente de instantáneas genera instantáneas una vez creada la publicación. Se pueden generar de la siguiente manera:  
  
-   Inmediatamente. De manera predeterminada, se genera una instantánea para una publicación de combinación inmediatamente después de haberse creado la publicación en el Asistente para nueva publicación.    
-   A la hora programada. Especifique una programación en la página **Agente de instantáneas** del Asistente para nueva publicación o al utilizar procedimientos almacenados o Replication Management Objects (RMO).    
-   Manualmente. Ejecute el Agente de instantáneas desde el símbolo del sistema o desde [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Para obtener más información, vea [Conceptos de los ejecutables del Agente de replicación](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md) e [Iniciar y detener un Agente de replicación &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md).  
  
Para la replicación de mezcla se genera una instantánea cada vez que se ejecuta el Agente de instantáneas. Para la replicación transaccional, la generación de instantáneas depende de la configuración de la propiedad de publicación **immediate_sync**. Si la propiedad se define como TRUE (la opción predeterminada cuando se utiliza el Asistente para nueva publicación), se genera una instantánea cada vez que se ejecuta el Agente de instantáneas, y puede aplicarse a un suscriptor en cualquier momento. Si la propiedad se define como FALSE (la opción predeterminada cuando se utiliza **sp_addpublication**), la instantánea se genera solo si se ha agregado una nueva suscripción desde la última ejecución del Agente de instantáneas; los suscriptores deberán esperar a que el Agente de instantáneas finalice para poder sincronizarse.  
  
De manera predeterminada, cuando se generan instantáneas, éstas se guardan en la carpeta predeterminada de instantáneas situada en el distribuidor. También puede guardar archivos de instantáneas en medios extraíbles, como discos extraíbles o CD-ROM, o en otras ubicaciones distintas de la carpeta de instantáneas predeterminada. Además, puede comprimir los archivos para que sean más fáciles de almacenar y transferir, así como ejecutar scripts antes o después de aplicar la instantánea al suscriptor. Para obtener más información acerca de estas opciones, consulte [Snapshot Options](../../relational-databases/replication/snapshot-options.md).  
  
Si la instantánea es para una publicación de combinación que utiliza filtros con parámetros, se crea mediante un proceso de dos partes. En primer lugar se crea una instantánea del esquema que contiene los scripts y el esquema de replicación de los objetos publicados, pero no los datos. A continuación, cada suscripción se inicializa con una instantánea que incluye los scripts y el esquema copiados de la instantánea de esquema y los datos que pertenecen a la partición de la suscripción. Para más información, consulte [Instantáneas para publicaciones de combinación con filtros con parámetros](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
Una vez creada la instantánea en el publicador y almacenada en una ubicación de instantáneas predeterminada o alternativa, la misma puede transferirse al suscriptor y aplicarse. El Agente de distribución (para replicación transaccional o de instantáneas) o el Agente de mezcla (para la replicación de mezcla) transfiere la instantánea y aplica el esquema y los archivos de datos a la base de datos de suscripciones del suscriptor durante la sincronización inicial. De manera predeterminada, la sincronización inicial se produce inmediatamente después de creada la suscripción si se utiliza el Asistente para nueva publicación. Este comportamiento se controla mediante la opción **Inicializar cuando** de la página **Inicializar suscripciones** del asistente. Cuando se generan instantáneas después de inicializada una suscripción, éstas no se aplican al suscriptor a menos que se marque una suscripción para reinicialización. Para obtener más información, vea [Reinicializar suscripciones](../../relational-databases/replication/reinitialize-subscriptions.md).  
  
Una vez que el Agente de distribución o el Agente de mezcla aplican la instantánea inicial, el agente propaga las actualizaciones posteriores y otras modificaciones de datos. Cuando se distribuyen y se aplican instantáneas a los suscriptores, solo se ven afectados los suscriptores que estén esperando instantáneas iniciales o nuevas. Los demás suscriptores de esa publicación (aquellos que ya están recibiendo inserciones, actualizaciones, eliminaciones u otras modificaciones de los datos publicados) no se ven afectados.  

Para ver o modificar la ubicación de la carpeta de instantáneas predeterminada, vea  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]: [Modificación de las opciones de la instantánea](../../relational-databases/replication/snapshot-options.md)  
  
-   Programación de la replicación y programación con RMO: [Configurar la publicación y la distribución](../../relational-databases/replication/configure-publishing-and-distribution.md)  

## <a name="default-snapshot-location"></a>Ubicación predeterminada de instantáneas

 Especifique la ubicación predeterminada de instantáneas en la página **Carpeta de instantáneas** del Asistente para configurar la distribución. Para obtener más información sobre cómo usar este asistente, vea [Configure Publishing and Distribution](../../relational-databases/replication/configure-publishing-and-distribution.md) (Configurar la publicación y la distribución). Si crea una publicación en un servidor que no está configurado como un distribuidor, especifique una ubicación predeterminada de instantáneas en la página **Carpeta de instantáneas** del Asistente para nueva publicación. Para obtener más información sobre cómo usar este asistente, vea [Crear una publicación](../../relational-databases/replication/publish/create-a-publication.md).  
  
 Modifique la ubicación de instantáneas predeterminada en la página **Publicadores** del cuadro de diálogo **Propiedades del distribuidor \<Distributor>** . Para obtener más información, vea [View and Modify Distributor and Publisher Properties](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md) (Ver y modificar las propiedades del distribuidor y del publicador). Establezca la carpeta de instantáneas para cada publicación en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** . Para más información, consulte [View and Modify Publication Properties](../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
### <a name="modify-the-default-snapshot-location"></a>Modificación de la ubicación predeterminada de instantáneas  
  
1.  En la página **Publicadores** del cuadro de diálogo **Propiedades del distribuidor: \<Distributor>** , haga clic en el botón de propiedades ( **...** ) correspondiente al publicador para el que quiera cambiar la ubicación de instantáneas predeterminada.  
  
2.  En el cuadro de diálogo **Propiedades del publicador: \<Publisher>** , escriba un valor para la propiedad **Carpeta de instantáneas predeterminada**.  
  
    > [!NOTE]  
    >  El Agente de instantáneas debe tener permisos de escritura para el directorio especificado y el Agente de distribución o de mezcla debe tener permisos de lectura. Si usa suscripciones de extracción, debe especificar un directorio compartido como ruta de acceso, según la convención de nomenclatura universal (UNC), por ejemplo \\\nombreDeEquipo\instantánea. Para obtener más información, vea [Proteger la carpeta de instantáneas](../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  

## <a name="create-snapshot"></a>Creación de instantáneas
De forma predeterminada, si se está ejecutando el Agente SQL Server, el Agente de instantáneas genera una instantánea inmediatamente después de que se cree una publicación con el Asistente para nueva publicación. A continuación, el Agente de distribución (para la replicación de instantáneas y transaccional) o el Agente de mezcla (para las suscripciones de mezcla) la aplican a todas las suscripciones. Las instantáneas también se pueden generar mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y el Monitor de replicación. Para información sobre cómo iniciar el Monitor de replicación, vea [Iniciar el Monitor de replicación](../../relational-databases/replication/monitor/start-the-replication-monitor.md).  

### <a name="using-sql-server-management-studio"></a>Uso de SQL Server Management Studio

1.  Conéctese al publicador en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]y luego expanda el nodo del servidor.    
2.  Expanda la carpeta **Replicación** y, a continuación, expanda la carpeta **Publicaciones locales** .    
3.  Haga clic con el botón secundario en la publicación para la que desee crear una instantánea y, a continuación, haga clic en **Ver estado del agente de instantáneas**.    
4.  En el cuadro de diálogo **Ver estado del Agente de instantáneas: \<Publication>** , haga clic en **Iniciar**.    
 Cuando el Agente de instantáneas termina de generar la instantánea, aparece un mensaje del tipo "[100%] Se ha generado una instantánea de 17 artículos".  
  
### <a name="in-replication-monitor"></a>En el Monitor de replicación  
  
1.  En el Monitor de replicación, expanda un grupo de publicador en el panel izquierdo y, a continuación, expanda un publicador.    
2.  Haga clic con el botón secundario en la publicación para la que desee crear una instantánea y, a continuación, haga clic en **Generar instantánea**.    
3.  Para ver el estado del Agente de instantáneas, haga clic en la pestaña **Agentes** . Para obtener información más detallada, haga clic con el botón secundario en el Agente de instantáneas en la cuadrícula y, a continuación, haga clic en **Ver detalles**.  

## <a name="using-transact-sql"></a>Usar Transact-SQL
Las instantáneas iniciales se pueden crear mediante programación o creando y ejecutando un trabajo del Agente de instantáneas o ejecutando el archivo ejecutable Agente de instantáneas desde un archivo por lotes. Una vez generada una instantánea inicial, se transfiere y se aplica en el suscriptor cuando se sincroniza la suscripción por primera vez. Si ejecuta el Agente de instantáneas desde un símbolo del sistema o un archivo por lotes, necesitará volver a ejecutar el agente cada vez que la instantánea existente deje de ser válida.  
  
> [!IMPORTANT]  
>  Cuando sea posible, pida a los usuarios que proporcionen credenciales de seguridad en tiempo de ejecución. Si debe almacenar las credenciales en un archivo de script, proteja el archivo para evitar el acceso no autorizado.  

1.  Cree una publicación de instantáneas, transaccional o de combinación. Para obtener más información, vea [Crear una suscripción](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Ejecute [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Especifique **\@publication** y los parámetros siguientes:  
  
    -   **El parámetro \@job_login, que especifica** las credenciales de autenticación de Windows con las que se ejecuta el Agente de instantáneas en el distribuidor.  
  
    -   **El parámetro \@job_password**, que es la contraseña para las credenciales de Windows proporcionadas.  
  
    -   (Opcional) Un valor de **0** para **\@publisher_security_mode** si el agente va a usar la autenticación de SQL Server para conectarse al publicador. En este caso, también debe especificar la información de inicio de sesión de autenticación de SQL Server para **\@publisher_login** y **\@publisher_password**.  
  
    -   (Opcional) Un programa de sincronización para el trabajo del Agente de instantáneas. Para obtener más información, consulte [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
    > [!IMPORTANT]  
    >  Al configurar un publicador con un distribuidor remoto, los valores suministrados para todos los parámetros, incluidos *job_login* y *job_password*, se envían al distribuidor como texto sin formato. Antes de ejecutar este procedimiento almacenado, se recomienda cifrar la conexión entre el publicador y su distribuidor remoto. Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
3.  Agregue artículos a la publicación. Para más información, consulte [Define an Article](../../relational-databases/replication/publish/define-an-article.md).  
  
4.  En el publicador de la base de datos de publicación, ejecute [sp_startpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-startpublication-snapshot-transact-sql.md) y especifique el valor de **\@publication** del paso 1.  
  
## <a name="apply-a-snapshot"></a>Aplicación de una instantánea  

### <a name="using-sql-server-management-studio"></a>Uso de SQL Server Management Studio
  
1.  Una vez generada la instantánea, se aplica mediante la sincronización de la suscripción con el Agente de distribución o el Agente de mezcla:   
    -   Si el agente se ejecuta de forma continua (valor predeterminado para la replicación transaccional), la instantánea se aplica inmediatamente después de haberse generado.   
    -   Si el agente se ejecuta según una programación, la instantánea se aplica la siguiente vez que está programada la ejecución del agente.    
    -   Si el agente se ejecuta a petición, se aplica la siguiente vez que se ejecuta el agente.  
  
     Para obtener más información acerca de cómo sincronizar suscripciones, vea [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md) y [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
###   <a name="use-transact-sql"></a>Uso de Transact-SQL  
 
1.  Cree una publicación de instantáneas, transaccional o de combinación. Para obtener más información, vea [Crear una suscripción](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Agregue artículos a la publicación. Para más información, consulte [Define an Article](../../relational-databases/replication/publish/define-an-article.md).  
  
3.  Desde el símbolo del sistema o en un archivo por lotes, inicie el [Replication Snapshot Agent](../../relational-databases/replication/agents/replication-snapshot-agent.md) ejecutando **snapshot.exe** y especifique los argumentos de la línea de comandos siguientes:  
  
    -   **-Publication**  
    -   **-Publisher**  
    -   **-Distributor**   
    -   **-PublisherDB**   
    -   **-ReplicationType**  
  
     Si está usando la autenticación de SQL Server, también debe especificar los argumentos siguientes:  
  
    -   **-DistributorLogin**    
    -   **-DistributorPassword**   
    -   **-DistributorSecurityMode** = **0**    
    -   **-PublisherLogin**    
    -   **-PublisherPassword**    
    -   **-PublisherSecurityMode** = **0**  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Ejemplos (Transact-SQL)  
 Este ejemplo muestra cómo crear una publicación transaccional y agregar un trabajo del Agente de instantáneas para la nueva publicación (utilizando variables de scripting de **sqlcmd** ). En el ejemplo también se inicia el trabajo.  
  
 [!code-sql[HowTo#sp_trangenerate_snapshot](../../relational-databases/replication/codesnippet/tsql/create-and-apply-the-ini_1.sql)]  
  
 En este ejemplo se crea una publicación de combinación y se agrega un trabajo del Agente de instantáneas (utilizando variables de **sqlcmd** ) para la publicación. En este ejemplo también se inicia el trabajo.  
  
 [!code-sql[HowTo#sp_mergegenerate_snapshot](../../relational-databases/replication/codesnippet/tsql/create-and-apply-the-ini_2.sql)]  
  
 Los siguientes argumentos de la línea de comandos inician el Agente de instantáneas para que genere la instantánea para una publicación de combinación.  
  
> [!NOTE]  
>  Los saltos de línea se incluyeron para mejorar la legibilidad. En un archivo por lotes, los comandos se deben realizar en una única línea.  
  
```  
  
REM -- Declare variables  
SET Publisher=%InstanceName%  
SET PublicationDB=AdventureWorks2012   
SET Publication=AdvWorksSalesOrdersMerge   
  
REM --Start the Snapshot Agent to generate the snapshot for AdvWorksSalesOrdersMerge.  
"C:\Program Files\Microsoft SQL Server\120\COM\SNAPSHOT.EXE" -Publication %Publication%   
-Publisher %Publisher% -Distributor %Publisher% -PublisherDB %PublicationDB%   
-ReplicationType 2 -OutputVerboseLevel 1 -DistributorSecurityMode 1  
  
```  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Uso de Replication Management Objects (RMO)  
 El Agente de instantáneas genera instantáneas una vez creada la publicación. Puede generar estas instantáneas mediante programación utilizando Replication Management Objects (RMO) y el acceso de código administrado directo a las funcionalidades del agente de replicación. Los objetos que se usan dependen del tipo de replicación. El Agente de instantáneas se puede iniciar sincrónicamente con el objeto <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent> o de forma asincrónica con el trabajo de agente. Una vez generada la instantánea inicial, se transfiere y se aplica al suscriptor cuando se sincroniza la suscripción por primera vez. Deberá volver a ejecutar el agente cada vez que la instantánea existente no contenga datos válidos y actualizados. Para obtener más información, vea [Mantener publicaciones](../../relational-databases/replication/publish/maintain-publications.md).  
  
> [!IMPORTANT]  
>  Cuando sea posible, pida a los usuarios que proporcionen credenciales de seguridad en tiempo de ejecución. Si debe almacenar credenciales, use los [servicios de cifrado](/previous-versions/aa719848(v=vs.71)) (en inglés) proporcionados por [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows .NET Framework.  
  
#### <a name="to-generate-the-initial-snapshot-for-a-snapshot-or-transactional-publication-by-starting-the-snapshot-agent-job-asynchronous"></a>Para generar la instantánea inicial de una publicación transaccional o de instantáneas iniciando el trabajo del Agente de instantáneas (asincrónico)  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransPublication>. Establezca las propiedades <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> y <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> para la publicación y la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en la conexión creada en el paso 1.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para cargar las propiedades restantes del objeto. Si este método devuelve **false**, significa que las propiedades de publicación del paso 2 se definieron incorrectamente, o bien que la publicación no existe.  
  
4.  Si el valor de <xref:Microsoft.SqlServer.Replication.Publication.SnapshotAgentExists%2A> es **false**, llame a <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> para crear el trabajo de Agente de instantáneas para esta publicación.  
  
5.  Llame al método <xref:Microsoft.SqlServer.Replication.Publication.StartSnapshotGenerationAgentJob%2A> para iniciar el trabajo del agente que genera la instantánea inicial de esta publicación.  
  
6.  (Opcional) Cuando el valor de <xref:Microsoft.SqlServer.Replication.TransPublication.SnapshotAvailable%2A> es **true**, la instantánea está disponible para los suscriptores.  
  
#### <a name="to-generate-the-initial-snapshot-for-a-snapshot-or-transactional-publication-by-running-the-snapshot-agent-synchronous"></a>Para generar la instantánea inicial de una publicación transaccional o de instantáneas ejecutando el trabajo del Agente de instantáneas (sincrónico)  
  
1.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent> y establezca las propiedades necesarias siguientes:  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publisher%2A> - nombre del publicador  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherDatabase%2A> - nombre de la base de datos de publicación  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publication%2A> - nombre de la publicación  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Distributor%2A> - nombre del distribuidor  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherSecurityMode%2A> - un valor de <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> para usar la autenticación de Windows al conectarse al publicador o un valor de <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> y valores para <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherLogin%2A> y <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherPassword%2A> para usar la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al conectarse al publicador. Se recomienda la autenticación de Windows.  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorSecurityMode%2A> - un valor de <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> para usar la autenticación de Windows al conectarse al distribuidor o un valor de <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> y valores para <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorLogin%2A> y <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorPassword%2A> para usar la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al conectarse al distribuidor. Se recomienda la autenticación de Windows.  
  
2.  Establezca un valor de <xref:Microsoft.SqlServer.Replication.ReplicationType.Transactional> o <xref:Microsoft.SqlServer.Replication.ReplicationType.Snapshot> para <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.ReplicationType%2A>.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.GenerateSnapshot%2A> .  
  
#### <a name="to-generate-the-initial-snapshot-for-a-merge-publication-by-starting-the-snapshot-agent-job-asynchronous"></a>Para generar la instantánea inicial de una publicación de combinación iniciando el trabajo del Agente de instantáneas (asincrónico)  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergePublication>. Establezca las propiedades <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> y <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> para la publicación y la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en la conexión creada en el paso 1.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para cargar las propiedades restantes del objeto. Si este método devuelve **false**, significa que las propiedades de publicación del paso 2 se definieron incorrectamente, o bien que la publicación no existe.  
  
4.  Si el valor de <xref:Microsoft.SqlServer.Replication.Publication.SnapshotAgentExists%2A> es **false**, llame a <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> para crear el trabajo de Agente de instantáneas para esta publicación.  
  
5.  Llame al método <xref:Microsoft.SqlServer.Replication.Publication.StartSnapshotGenerationAgentJob%2A> para iniciar el trabajo del agente que genera la instantánea inicial de esta publicación.  
  
6.  (Opcional) Cuando el valor de <xref:Microsoft.SqlServer.Replication.MergePublication.SnapshotAvailable%2A> es **true**, la instantánea está disponible para los suscriptores.  
  
#### <a name="to-generate-the-initial-snapshot-for-a-merge-publication-by-running-the-snapshot-agent-synchronous"></a>Para generar la instantánea inicial de una publicación de combinación ejecutando el Agente de instantáneas (sincrónico)  
  
1.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent> y establezca las propiedades necesarias siguientes:  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publisher%2A> - nombre del publicador  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherDatabase%2A> - nombre de la base de datos de publicación  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publication%2A> - nombre de la publicación  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Distributor%2A> - nombre del distribuidor  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherSecurityMode%2A> - un valor de <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> para usar la autenticación de Windows al conectarse al publicador o un valor de <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> y valores para <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherLogin%2A> y <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherPassword%2A> para usar la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al conectarse al publicador. Se recomienda la autenticación de Windows.  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorSecurityMode%2A> - un valor de <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> para usar la autenticación de Windows al conectarse al distribuidor o un valor de <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> y valores para <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorLogin%2A> y <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorPassword%2A> para usar la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al conectarse al distribuidor. Se recomienda la autenticación de Windows.  
  
2.  Establezca un valor <xref:Microsoft.SqlServer.Replication.ReplicationType.Merge> para <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.ReplicationType%2A>.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.GenerateSnapshot%2A> .  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Ejemplos (RMO)  
 Este ejemplo ejecuta sincrónicamente el Agente de instantáneas para generar la instantánea inicial de una publicación transaccional.  
  
 [!code-cs[HowTo#rmo_GenerateSnapshot](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_generatesnapshot)]  
  
 [!code-vb[HowTo#rmo_vb_GenerateSnapshot](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_generatesnapshot)]  
  
 Este ejemplo inicia asincrónicamente el Agente de instantáneas para generar la instantánea inicial de una publicación transaccional.  
  
 [!code-cs[HowTo#rmo_GenerateSnapshot_WithJob](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_generatesnapshot_withjob)]  
  
 [!code-vb[HowTo#rmo_vb_GenerateSnapshot_WithJob](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_generatesnapshot_withjob)]  
  
## <a name="see-also"></a>Consulte también  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md)   
 [Inicializar una suscripción con una instantánea](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)   
 [Replication Management Objects Concepts](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Replication System Stored Procedures Concepts](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Usar sqlcmd con variables de script](../../ssms/scripting/sqlcmd-use-with-scripting-variables.md)  
  
