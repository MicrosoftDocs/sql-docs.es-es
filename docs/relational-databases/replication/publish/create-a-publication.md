---
title: Creación de una publicación | Microsoft Docs
description: Obtenga información sobre cómo crear una publicación en SQL Server mediante SQL Server Management Studio, Transact-SQL o Replication Management Objects.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- publications [SQL Server replication], creating
- articles [SQL Server replication], defining
- adding articles
- articles [SQL Server replication], adding
ms.assetid: 52ee6de9-1d58-4cb9-8711-372bddbe7154
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: b3c1655be2909c46368c42ea7781617674dc3025
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076823"
---
# <a name="create-a-publication"></a>Create a Publication
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  En este tema se describe cómo crear una publicación en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o Replication Management Objects (RMO).  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para crear una publicación y definir artículos con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Replication Management Objects (RMO)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Los nombres de publicaciones y artículos no pueden contener ninguno de los siguientes caracteres: % , \* , [ , ] , | , : , " , ? , ' , \ , / , < , >. Si los objetos de la base de datos contienen cualquiera de esos caracteres y quiere replicarlos, debe especificar un nombre de artículo distinto del nombre del objeto en el cuadro de diálogo **Propiedades del artículo: \<Article>** , que se encuentra en la página **Artículos** del asistente.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
 Cuando sea posible, pida a los usuarios que proporcionen credenciales de seguridad en tiempo de ejecución. Si debe almacenar credenciales, use los [servicios de cifrado](/previous-versions/aa719848(v=vs.71)) (en inglés) proporcionados por [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows .NET Framework.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Cree publicaciones y defina artículos con el Asistente para nueva publicación. Una vez que se ha creado la publicación, se pueden ver y modificar sus propiedades en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** . Para obtener información sobre cómo crear una publicación de una base de datos de Oracle, vea [Crear una publicación a partir de una base de datos de Oracle](../../../relational-databases/replication/publish/create-a-publication-from-an-oracle-database.md).  
  
#### <a name="to-create-a-publication-and-define-articles"></a>Para crear publicaciones y definir artículos  
  
1.  Conéctese al publicador de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] y expanda el nodo de servidor.  
  
2.  Expanda la carpeta **Replicación** y, a continuación, haga clic con el botón secundario en la carpeta **Publicaciones locales** .  
  
3.  Haga clic en **Nueva publicación**.  
  
4.  Siga las indicaciones de las páginas del Asistente para nueva publicación para:  
  
    -   Especificar un distribuidor si la distribución no se ha configurado en el servidor. Para obtener más información sobre cómo configurar la distribución, vea [Configurar la publicación y la distribución](../../../relational-databases/replication/configure-publishing-and-distribution.md).  
  
         Si en la página **Distribuidor** especifica que el servidor del publicador actúe como su propio distribuidor (un distribuidor local) y el servidor no está configurado como tal, el Asistente para nueva publicación configurará el servidor. Especifique una carpeta de instantáneas predeterminada para el distribuidor en la página **Carpeta de instantáneas** . La carpeta de instantáneas es simplemente un directorio designado como recurso compartido; los agentes que leen y escriben en esta carpeta deben tener permisos de acceso suficientes a ella. Para obtener más información sobre cómo proteger la carpeta adecuadamente, vea [Proteger la carpeta de instantáneas](../../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  
         Si especifica que otro servidor actúe como distribuidor, deberá escribir una contraseña en la página **Contraseña administrativa** para las conexiones que se realicen desde el publicador al distribuidor. Esta contraseña debe coincidir con la especificada cuando se habilitó el publicador en el distribuidor remoto.  
  
         Para más información, consulte [Configure Distribution](../../../relational-databases/replication/configure-distribution.md).  
  
    -   Elegir una base de datos de publicación.  
  
    -   Seleccionar un tipo de publicación. Para obtener más información, vea [Tipos de replicación](../../../relational-databases/replication/types-of-replication.md).  
  
    -   Especificar los datos y los objetos de base de datos que se van a publicar; de forma opcional, filtrar columnas de artículos de tablas y establecer las propiedades de los artículos.  
  
    -   De forma opcional, filtrar filas de artículos de tablas. Para obtener más información, vea [Filtrar datos publicados](../../../relational-databases/replication/publish/filter-published-data.md).  
  
    -   Establecer la programación del Agente de instantáneas.  
  
    -   Especificar las credenciales con las que los siguientes agentes de replicación se ejecutan y efectúan conexiones:  
  
         \- Agente de instantáneas (para todas las publicaciones)  
  
         \- Agente de registro del LOG (para todas las publicaciones transaccionales)  
  
         \- Agente de lectura de cola para publicaciones transaccionales que permiten suscripciones de actualización.  
  
         Para obtener más información, consulte [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md) y [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md).  
  
    -   De forma opcional, incluir la publicación. Para más información, consulte [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
    -   Especificar un nombre para la publicación.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Las publicaciones pueden crearse mediante programación con procedimientos almacenados de replicación. Los procedimientos almacenados que se usen dependerán del tipo de publicación creado.  
  
#### <a name="to-create-a-snapshot-or-transactional-publication"></a>Para crear una publicación transaccional o de instantáneas  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) para habilitar la publicación de la base de datos actual con la replicación transaccional o de instantáneas.  
  
2.  En el caso de una publicación transaccional, determine si existe un trabajo de Agente de registro del LOG para la base de datos de publicación. Este paso no es necesario para las publicaciones de instantáneas.  
  
    -   Si existe un trabajo de Agente de registro del LOG para la base de datos de publicación, continúe en el paso 3.  
  
    -   Si no está seguro de que exista un Agente de registro del LOG para una base de datos publicada, ejecute [sp_helplogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md) en la base de datos de publicación del publicador.  
  
    -   Si el conjunto de resultados está vacío, cree un trabajo de Agente de registro del LOG. En el publicador, ejecute [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md). Especifique las credenciales de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows con las que se ejecuta el agente para **\@job_name** y **\@password**. Si el agente va a usar la autenticación de SQL Server al conectarse al publicador, también debe especificar un valor de **0** para **\@publisher_security_mode** y la [!INCLUDE[msCoName](../../../includes/msconame-md.md)] información de inicio de sesión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para **\@publisher_login** y **\@publisher_password**. Continúe en el paso 3.  
  
3.  En el publicador, ejecute [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md). Especifique un nombre de publicación para **\@publication** y, para el parámetro **\@repl_freq**, especifique un valor de **snapshot** para una publicación de instantáneas o un valor de **continuous** para una publicación transaccional. Especifique cualquier otra opción de publicación. Esto define la publicación.  
  
    > [!NOTE]  
    >  Los nombres de publicación no pueden incluir los caracteres siguientes:  
    >   
    >  % * [ ] | : " ? \ / < >  
  
4.  En el publicador, ejecute [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Especifique el nombre de publicación usado en el paso 3 para **\@publication** y las credenciales de Windows con las que se ejecuta el Agente de instantáneas para **\@snapshot_job_name** y **\@password**. Si el agente va a usar la autenticación de SQL Server al conectarse al publicador, también debe especificar un valor de **0** para **\@publisher_security_mode** y la información de inicio de sesión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para **\@publisher_login** y **\@publisher_password**. Esto crea un trabajo de Agente de instantáneas para la publicación.  
  
    > [!IMPORTANT]  
    >  Al configurar un publicador con un distribuidor remoto, los valores suministrados para todos los parámetros, incluidos *job_login* y *job_password*, se envían al distribuidor como texto sin formato. Antes de ejecutar este procedimiento almacenado, se recomienda cifrar la conexión entre el publicador y su distribuidor remoto. Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
5.  Agregue artículos a la publicación. Para más información, consulte [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
6.  Inicie el trabajo del Agente de instantáneas para generar la instantánea inicial de esta publicación. Para más información, consulte [Crear y aplicar la instantánea inicial](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
#### <a name="to-create-a-merge-publication"></a>Para crear una publicación de combinación  
  
1.  En el publicador, ejecute [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) para habilitar la publicación de la base de datos actual con la replicación de mezcla.  
  
2.  En el publicador de la base de datos de publicación, ejecute [sp_addmergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md). Especifique un nombre de publicación para **\@publication** y cualquier otra opción de publicación. Esto define la publicación.  
  
    > [!NOTE]  
    >  Los nombres de publicación no pueden incluir los caracteres siguientes:  
    >   
    >  % * [ ] | : " ? \ / < >  
  
3.  En el publicador, ejecute [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Especifique el nombre de publicación usado en el paso 2 para **\@publication** y las credenciales de Windows con las que se ejecuta el Agente de instantáneas para **\@snapshot_job_name** y **\@password**. Si el agente va a usar la autenticación de SQL Server al conectarse al publicador, también debe especificar un valor de **0** para **\@publisher_security_mode** y la información de inicio de sesión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para **\@publisher_login** y **\@publisher_password**. Esto crea un trabajo de Agente de instantáneas para la publicación.  
  
    > [!IMPORTANT]  
    >  Al configurar un publicador con un distribuidor remoto, los valores suministrados para todos los parámetros, incluidos *job_login* y *job_password*, se envían al distribuidor como texto sin formato. Antes de ejecutar este procedimiento almacenado, se recomienda cifrar la conexión entre el publicador y su distribuidor remoto. Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
4.  Agregue artículos a la publicación. Para más información, consulte [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
5.  Inicie el trabajo del Agente de instantáneas para generar la instantánea inicial de esta publicación. Para más información, consulte [Crear y aplicar la instantánea inicial](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Ejemplo (Transact-SQL)  
 Este ejemplo crea una publicación transaccional. Las variables de scripting se usan para pasar las credenciales de Windows necesarias para crear los trabajos del Agente de instantáneas y del Agente de registro del LOG.  
  
 [!code-sql[HowTo#sp_AddTranPub](../../../relational-databases/replication/codesnippet/tsql/create-a-publication_1.sql)]  
  
 Este ejemplo crea una publicación de combinación. Las variables de scripting se usan para pasar las credenciales de Windows necesarias para crear el trabajo del Agente de instantáneas.  
  
 [!code-sql[HowTo#sp_AddMergePub](../../../relational-databases/replication/codesnippet/tsql/create-a-publication_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Uso de Replication Management Objects (RMO)  
 Puede crear publicaciones mediante programación utilizando Replication Management Objects (RMO). Las clases RMO que usa para crear una publicación dependen del tipo de publicación que crea.  
  
#### <a name="to-create-a-snapshot-or-transactional-publication"></a>Para crear una publicación transaccional o de instantáneas  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.ReplicationDatabase> para la base de datos de publicación, establezca la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en la instancia de <xref:Microsoft.SqlServer.Management.Common.ServerConnection> del paso 1 y llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Si <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> devuelve **false**, compruebe que la base de datos existe.  
  
3.  Si la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledTransPublishing%2A> es **false**, establézcala en **true**.  
  
4.  Para una publicación transaccional, compruebe el valor de la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.LogReaderAgentExists%2A> . Si esta propiedad es **true**, un trabajo del Agente de registro del LOG ya existe para esta base de datos. Si esta propiedad es **false**, haga lo siguiente:  
  
    -   Establezca los campos <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> y <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.LogReaderAgentProcessSecurity%2A> para proporcionar las credenciales para la cuenta de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows con la que se ejecuta el Agente de registro del LOG.  
  
        > [!NOTE]  
        >  No es necesario configurar <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.LogReaderAgentProcessSecurity%2A> cuando un miembro del rol fijo de servidor **sysadmin** crea la publicación. En este caso, el agente suplantará a la cuenta del Agente SQL Server. Para más información, consulte [Modelo de seguridad del agente de replicación](../../../relational-databases/replication/security/replication-agent-security-model.md).  
  
    -   (Opcional) configure los campos <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> y <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> o <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> de <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.LogReaderAgentPublisherSecurity%2A> si utiliza la autenticación de SQL Server para conectarse al publicador.  
  
    -   Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.CreateLogReaderAgent%2A> para crear el trabajo del Agente de registro del LOG para la base de datos.  
  
5.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransPublication> y establezca las propiedades siguientes para este objeto:  
  
    -   La <xref:Microsoft.SqlServer.Management.Common.ServerConnection> del paso 1 para <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
    -   El nombre de la base de datos publicada para <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>.  
  
    -   Un nombre de publicación para <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>.  
  
    -   Un <xref:Microsoft.SqlServer.Replication.PublicationType> de <xref:Microsoft.SqlServer.Replication.PublicationType.Transactional> o <xref:Microsoft.SqlServer.Replication.PublicationType.Snapshot>.  
  
    -   Los campos <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> y <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> para proporcionar las credenciales de la cuenta de Windows con la que se ejecuta el trabajo del Agente de mezcla. Se usa esta cuenta también cuando el Agente de instantáneas realiza las conexiones al Distribuidor local y para cualquier conexión remota cuando se usa Autenticación de Windows.  
  
        > [!NOTE]  
        >  No es necesario configurar <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> cuando un miembro del rol fijo de servidor **sysadmin** crea la publicación. En este caso, el agente suplantará a la cuenta del Agente SQL Server. Para más información, consulte [Modelo de seguridad del agente de replicación](../../../relational-databases/replication/security/replication-agent-security-model.md).  
  
    -   (Opcional) Los campos <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> y <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> o <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> de <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentPublisherSecurity%2A> si usa la autenticación de SQL Server para conectarse al Publicador.  
  
    -   (Opcional) Use el operador lógico OR inclusivo ( **|** en Visual C# y **Or** en Visual Basic) y el operador lógico OR exclusivo ( **^** en Visual C# y **Xor** en Visual Basic) para establecer los valores <xref:Microsoft.SqlServer.Replication.PublicationAttributes> para la propiedad <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> .  
  
    -   (Opcional) El nombre del Publicador para <xref:Microsoft.SqlServer.Replication.TransPublication.PublisherName%2A> cuando el Publicador no es de SQL Server.  
  
6.  Llame al método <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> para crear la publicación.  
  
    > [!IMPORTANT]  
    >  Cuando se configura un Publicador con un Distribuidor remoto, los valores suministrados para todas las propiedades, incluidos <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A>, se envían al Distribuidor como texto simple. Debe cifrar la conexión entre el publicador y su distribuidor remoto antes de llamar al método <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> . Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
7.  Llame al método <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> para crear el trabajo del Agente de instantáneas para la publicación.  
  
#### <a name="to-create-a-merge-publication"></a>Para crear una publicación de combinación  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.ReplicationDatabase> para la base de datos de publicación, establezca la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en la instancia de <xref:Microsoft.SqlServer.Management.Common.ServerConnection> del paso 1 y llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Si <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> devuelve **false**, compruebe que la base de datos existe.  
  
3.  Si <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledMergePublishing%2A> Property is **false**, establézcala en **true** y llame a <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A>.  
  
4.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergePublication> y establezca las propiedades siguientes para este objeto:  
  
    -   La <xref:Microsoft.SqlServer.Management.Common.ServerConnection> del paso 1 para <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
    -   El nombre de la base de datos publicada para <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>.  
  
    -   Un nombre de publicación para <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>.  
  
    -   Los campos <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> y <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> para proporcionar las credenciales de la cuenta de Windows con la que se ejecuta el trabajo del Agente de mezcla. Se usa esta cuenta también cuando el Agente de instantáneas realiza las conexiones al Distribuidor local y para cualquier conexión remota cuando se usa Autenticación de Windows.  
  
        > [!NOTE]  
        >  No es necesario configurar <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> cuando un miembro del rol fijo de servidor **sysadmin** crea la publicación. Para más información, consulte [Modelo de seguridad del agente de replicación](../../../relational-databases/replication/security/replication-agent-security-model.md).  
  
    -   (Opcional) Use el operador lógico OR inclusivo ( **|** en Visual C# y **Or** en Visual Basic) y el operador lógico OR exclusivo ( **^** en Visual C# y **Xor** en Visual Basic) para establecer los valores <xref:Microsoft.SqlServer.Replication.PublicationAttributes> para la propiedad <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> .  
  
5.  Llame al método <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> para crear la publicación.  
  
    > [!IMPORTANT]  
    >  Cuando se configura un Publicador con un Distribuidor remoto, los valores suministrados para todas las propiedades, incluidos <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A>, se envían al Distribuidor como texto simple. Debe cifrar la conexión entre el publicador y su distribuidor remoto antes de llamar al método <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> . Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
6.  Llame al método <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> para crear el trabajo del Agente de instantáneas para la publicación.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Ejemplos (RMO)  
 Este ejemplo habilita la base de datos de AdventureWorks para la publicación transaccional, define un trabajo del Agente de registro del LOG y crea la publicación de AdvWorksProductTran. Se debe definir un artículo para esta publicación. Las credenciales de cuenta de Windows que se necesitan para crear el trabajo del Agente de registro del LOG y el trabajo del Agente de instantáneas se pasan en tiempo de ejecución. Para obtener información sobre cómo usar RMO para definir artículos de instantáneas y de transacciones, vea [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
 [!code-cs[HowTo#rmo_CreateTranPub](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createtranpub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateTranPub](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createtranpub)]  
  
 Este ejemplo habilita la base de datos de AdventureWorks para la publicación de combinación y crea la publicación de AdvWorksSalesOrdersMerge. Todavía se deben definir artículos para esta publicación. Las credenciales de cuenta de Windows que se necesitan para crear el trabajo del Agente de registro del LOG se pasan en tiempo de ejecución. Para obtener información sobre cómo usar RMO para definir artículos de mezcla, vea [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
 [!code-cs[HowTo#rmo_CreateMergePub](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePub](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepub)]  
  
## <a name="see-also"></a>Consulte también  
 [Usar sqlcmd con variables de script](../../../ssms/scripting/sqlcmd-use-with-scripting-variables.md)   
 [Publicar datos y objetos de base de datos](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Replication Management Objects Concepts](../../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Define an Article](../../../relational-databases/replication/publish/define-an-article.md)   
 [Ver y modificar propiedades de publicación](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Configurar distribución](../../../relational-databases/replication/configure-distribution.md)   
 [Proteger el distribuidor](../../../relational-databases/replication/security/secure-the-distributor.md)   
 [Proteger al publicador](../../../relational-databases/replication/security/secure-the-publisher.md)  
  
