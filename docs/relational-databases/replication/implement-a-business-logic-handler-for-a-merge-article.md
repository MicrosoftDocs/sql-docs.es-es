---
title: Configuración de un controlador de lógica de negocios para un artículo de combinación
description: Use la programación de la replicación o Replication Management Objects para configurar un controlador de lógica de negocios para la sincronización de la replicación de combinación.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- merge replication conflict resolution [SQL Server replication], business logic handlers
- merge replication business logic handlers [SQL Server replication]
- conflict resolution [SQL Server replication], merge replication
- business logic handlers [SQL Server replication]
- BusinessLogicModule class
ms.assetid: ed477595-6d46-4fa2-b0d3-a5358903ec05
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 45ed99072fb5abad47b9f63a22f879c0f4e438eb
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076833"
---
# <a name="implement-a-business-logic-handler-for-a-merge-article"></a>Implementar un controlador de lógica de negocios para un artículo de mezcla
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo implementar un controlador de lógica de negocios para un artículo de mezcla en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante programación de la replicación o Replication Management Objects (RMO).  
  
 El espacio de nombres <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport> implementa una interfaz que le permite escribir una lógica de negocios compleja para administrar eventos que se producen durante el proceso de sincronización de la replicación de mezcla. El proceso de la replicación puede invocar los métodos en el controlador de lógica de negocios para cada fila cambiada que se replica durante la sincronización.  
  
 El proceso general para implementar un controlador de lógica de negocios es:  
  
1.  Cree el ensamblado del controlador de lógica de negocios.  
  
2.  Registre el ensamblado en el distribuidor.  
  
3.  Implemente el ensamblado en el servidor en el que se ejecuta el Agente de mezcla. Para una suscripción de extracción, el agente se ejecuta en el suscriptor y, para una suscripción de inserción, se ejecuta en el distribuidor. Al usar la sincronización web, el agente se ejecuta en el servidor web.  
  
4.  Cree un artículo que use el controlador de lógica de negocios, o bien modifique un artículo existente para usar dicho controlador.  
  
 El controlador de lógica de negocios especificado se ejecuta para cada fila que se sincroniza. La lógica compleja y las llamadas a otras aplicaciones o servicios de red pueden afectar al rendimiento. Para más información sobre los controladores de lógica de negocios, vea [Ejecutar la lógica de negocios durante la sincronización de mezcla](../../relational-databases/replication/merge/execute-business-logic-during-merge-synchronization.md).  
  
 **En este tema**  
  
-   **Para implementar un controlador de lógica de negocios para un artículo de mezcla con:**  
  
     [Programación de la replicación](#ReplProg)  
  
     [Replication Management Objects (RMO)](#RMOProcedure)  
  
##  <a name="using-replication-programming"></a><a name="ReplProg"></a> Usar programación de la replicación  
  
#### <a name="to-create-and-deploy-a-business-logic-handler"></a>Para crear e implementar un controlador de lógica de negocios  
  
1.  En Visual Studio [!INCLUDE[msCoName](../../includes/msconame-md.md)] , cree un nuevo proyecto para el ensamblado .NET que contenga el código que implementa el controlador de lógica de negocios.  
  
2.  Agregue referencias al proyecto para los siguientes espacios de nombres.  
  
    |Referencia de ensamblado|Location|  
    |------------------------|--------------|  
    |<xref:Microsoft.SqlServer.Replication.BusinessLogicSupport>|[!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]COM (instalación predeterminada)|  
    |<xref:System.Data>|GAC (componente de .NET Framework)|  
    |<xref:System.Data.Common>|GAC (componente de .NET Framework)|  
  
3.  Agregue una clase que invalide la clase <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> .  
  
4.  Implemente la propiedad <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.HandledChangeStates%2A> para indicar los tipos de cambios que se administran.  
  
5.  Invalide uno o varios de los métodos siguientes de la clase <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> :  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.CommitHandler%2A> - se invoca cuando se confirma un cambio de datos durante la sincronización.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.DeleteErrorHandler%2A> - se invoca si se produce un error cuando una instrucción DELETE se carga o se descarga.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.DeleteHandler%2A> - se invoca cuando las instrucciones DELETE se cargan o se descargan.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.InsertErrorHandler%2A> - se invoca si se produce un error cuando una instrucción INSERT se carga o se descarga.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.InsertHandler%2A> - se invoca cuando las instrucciones INSERT se cargan o se descargan.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateConflictsHandler%2A> - se invoca cuando se producen instrucciones UPDATE conflictivas en el publicador y en el suscriptor.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateDeleteConflictHandler%2A> - se invoca cuando las instrucciones UPDATE entran en conflicto con las instrucciones DELETE en el publicador y en el suscriptor.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateErrorHandler%2A> - se invoca si se produce un error cuando una instrucción UPDATE se carga o se descarga.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateHandler%2A> - se invoca cuando las instrucciones UPDATE se cargan o se descargan.  
  
6.  Genere el proyecto para crear el ensamblado del controlador de lógica de negocios.  
  
7.  Implemente el ensamblado en el directorio que contiene el archivo ejecutable del Agente de mezcla (replmerg.exe), que para una instalación predeterminada es [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]COM, o bien instálelo en la caché de ensamblados global (GAC) de .NET. Solo debe instalar el ensamblado en la GAC si las aplicaciones distintas del Agente de mezcla requieren acceso al ensamblado. El ensamblado se puede instalar en la GAC con la herramienta Caché de ensamblados global (**Gacutil.exe)** proporcionada en .NET Framework SDK.  
  
    > [!NOTE]  
    >  Un controlador de lógica de negocios se debe implementar en cada servidor en el que se ejecute el Agente de mezcla, que incluye el servidor IIS que hospeda replisapi.dll al usar la sincronización web.  
  
#### <a name="to-register-a-business-logic-handler"></a>Para registrar un controlador de lógica de negocios  
  
1.  En el publicador, ejecute [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) para comprobar que el ensamblado aún no se ha registrado como un controlador de lógica de negocios.  
  
2.  En el distribuidor, ejecute [sp_registercustomresolver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-registercustomresolver-transact-sql.md), y especifique un nombre descriptivo del controlador de lógica de negocios para **\@article_resolver**, un valor **true** para **\@is_dotnet_assembly**, el nombre del ensamblado para **\@dotnet_assembly_name** y el nombre completo de la clase que invalida <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> para **\@dotnet_class_name**.  
  
    > [!NOTE]  
    >  Si el ensamblado no se implementa en el mismo directorio que el ejecutable del Agente de mezcla, en el mismo directorio que la aplicación que inicia de forma sincrónica ese agente o en la caché de ensamblados global (GAC), tendrá que especificar la ruta de acceso completa con el nombre del ensamblado para **\@dotnet_assembly_name**. Al usar la sincronización web, debe especificar la ubicación de ensamblado en el servidor web.  
  
#### <a name="to-use-a-business-logic-handler-with-a-new-table-article"></a>Para usar un controlador de lógica de negocios con un nuevo artículo de tabla  
  
1.  Ejecute [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) para definir un artículo, y especifique el nombre descriptivo del controlador de lógica de negocios para **\@article_resolver**. Para más información, consulte [Define an Article](../../relational-databases/replication/publish/define-an-article.md).  
  
#### <a name="to-use-a-business-logic-handler-with-an-existing-table-article"></a>Para usar un controlador de lógica de negocios con un artículo de tabla existente  
  
1.  Ejecute [sp_changemergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) y especifique **\@publication**, **\@article**, un valor de **article_resolver** para **\@property** y el nombre descriptivo del controlador de lógica de negocios para **\@value**.  
  
###  <a name="examples-replication-programming"></a><a name="TsqlExample"></a> Ejemplos (programación de la replicación)  
 Este ejemplo muestra un controlador de lógica de negocios que crea un registro de auditoría.  
  
 [!code-cs[HowTo#rmo_BusinessLogicCode](../../relational-databases/replication/codesnippet/csharp/rmohowto/businesslogic.cs#rmo_businesslogiccode)]  
  
 [!code-vb[HowTo#rmo_vb_BusinessLogicCode](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/businesslogic.vb#rmo_vb_businesslogiccode)]  
  
 El ejemplo siguiente registra un ensamblado de controlador de lógica de negocios en el distribuidor y cambia un artículo de mezcla existente para usar esta lógica empresarial personalizada.  
  
 [!code-sql[HowTo#sp_RegisterBLH_10](../../relational-databases/replication/codesnippet/tsql/implement-a-business-log_3.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Uso de Replication Management Objects (RMO)  
  
#### <a name="to-create-a-business-logic-handler"></a>Para crear un controlador de lógica de negocios  
  
1.  En Visual Studio [!INCLUDE[msCoName](../../includes/msconame-md.md)] , cree un nuevo proyecto para el ensamblado .NET que contenga el código que implementa el controlador de lógica de negocios.  
  
2.  Agregue referencias al proyecto para los siguientes espacios de nombres.  
  
    |Referencia de ensamblado|Location|  
    |------------------------|--------------|  
    |<xref:Microsoft.SqlServer.Replication.BusinessLogicSupport>|[!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]COM (instalación predeterminada)|  
    |<xref:System.Data>|GAC (componente de .NET Framework)|  
    |<xref:System.Data.Common>|GAC (componente de .NET Framework)|  
  
3.  Agregue una clase que invalide la clase <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> .  
  
4.  Implemente la propiedad <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.HandledChangeStates%2A> para indicar los tipos de cambios que se administran.  
  
5.  Invalide uno o varios de los métodos siguientes de la clase <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> :  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.CommitHandler%2A> - se invoca cuando se confirma un cambio de datos durante la sincronización.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.DeleteErrorHandler%2A> - se invoca si se produce un error mientras una instrucción DELETE se carga o se descarga.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.DeleteHandler%2A> - se invoca cuando las instrucciones DELETE se cargan o se descargan.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.InsertErrorHandler%2A> - se invoca si se produce un error cuando una instrucción INSERT se carga o se descarga.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.InsertHandler%2A> - se invoca cuando las instrucciones INSERT se cargan o se descargan.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateConflictsHandler%2A> - se invoca cuando se producen instrucciones UPDATE conflictivas en el publicador y en el suscriptor.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateDeleteConflictHandler%2A> - se invoca cuando las instrucciones UPDATE entran en conflicto con las instrucciones DELETE en el publicador y en el suscriptor.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateErrorHandler%2A> - se invoca si se produce un error cuando una instrucción UPDATE se carga o se descarga.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateHandler%2A> - se invoca cuando las instrucciones UPDATE se cargan o se descargan.  
  
    > [!NOTE]  
    >  El solucionador predeterminado del artículo administra los conflictos de artículo que no controle explícitamente la lógica de negocios personalizada.  
  
6.  Genere el proyecto para crear el ensamblado del controlador de lógica de negocios.  
  
#### <a name="to-register-a-business-logic-handler"></a>Para registrar un controlador de lógica de negocios  
  
1.  Cree una conexión al distribuidor mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.ReplicationServer>. Pase el objeto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> del paso 1.  
  
3.  Llame a <xref:Microsoft.SqlServer.Replication.ReplicationServer.EnumBusinessLogicHandlers%2A> y compruebe el objeto <xref:System.Collections.ArrayList> devuelto para asegurarse de que el ensamblado aún no se ha registrado como un controlador de lógica de negocios.  
  
4.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler>. Especifique las propiedades siguientes:  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.DotNetAssemblyName%2A> - nombre del ensamblado .NET. Si el ensamblado no está implementado en el mismo directorio que el ejecutable del Agente de mezcla, en el mismo directorio que la aplicación que inicia de forma sincrónica dicho agente o en la GAC, debe incluir la ruta de acceso completa con el nombre del ensamblado. Debe incluir la ruta de acceso completa con el nombre del ensamblado al usar un controlador de lógica de negocios con la sincronización web.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.DotNetClassName%2A> - el nombre completo de la clase que invalida <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> e implementa el controlador de lógica de negocios.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.FriendlyName%2A> - nombre descriptivo que se usa al obtener acceso al controlador de lógica de negocios.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.IsDotNetAssembly%2A> - un valor de **true**.  
  
#### <a name="to-deploy-a-business-logic-handler"></a>Para implementar un controlador de lógica de negocios  
  
1.  Implemente el ensamblado en el servidor donde se ejecuta el Agente de mezcla, en la ubicación del archivo especificada cuando el controlador de lógica de negocios se registró en el distribuidor. Para una suscripción de extracción, el agente se ejecuta en el suscriptor y, para una suscripción de inserción, se ejecuta en el distribuidor. Al usar la sincronización web, el agente se ejecuta en el servidor web. Si la ruta de acceso completa no estuviera incluida con el nombre del ensamblado cuando se registró el controlador de lógica de negocios, implemente el ensamblado en el mismo directorio como el ejecutable del Agente de mezcla, en el mismo directorio que la aplicación que inicia sincrónicamente dicho agente. Puede instalar el ensamblado en la GAC si hay varias aplicaciones que usen el mismo ensamblado.  
  
#### <a name="to-use-a-business-logic-handler-with-a-new-table-article"></a>Para usar un controlador de lógica de negocios con un nuevo artículo de tabla  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergeArticle>. Establezca las siguientes propiedades:  
  
    -   El nombre del artículo para <xref:Microsoft.SqlServer.Replication.Article.Name%2A>.  
  
    -   El nombre de la publicación para <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>.  
  
    -   El nombre de la base de datos de publicación para <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A>.  
  
    -   El nombre descriptivo del controlador de lógica de negocios (<xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.FriendlyName%2A>) para <xref:Microsoft.SqlServer.Replication.MergeArticle.ArticleResolver%2A>.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.Article.Create%2A> . Para más información, consulte [Define an Article](../../relational-databases/replication/publish/define-an-article.md).  
  
#### <a name="to-use-a-business-logic-handler-with-an-existing-table-article"></a>Para usar un controlador de lógica de negocios con un artículo de tabla existente  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergeArticle>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.Article.Name%2A>, <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>y <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A> .  
  
4.  Establezca la conexión del paso 1 para la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto. Si este método devuelve **false**, se definieron incorrectamente las propiedades de artículo en el paso 3, o bien el artículo no existe. Para más información, consulte [View and Modify Article Properties](../../relational-databases/replication/publish/view-and-modify-article-properties.md).  
  
6.  Establezca el nombre descriptivo del controlador de negocios para <xref:Microsoft.SqlServer.Replication.MergeArticle.ArticleResolver%2A>. Este es el valor de la propiedad <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.FriendlyName%2A> especificado al registrar el controlador de lógica de negocios.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Ejemplos (RMO)  
 En este ejemplo se muestra un controlador de lógica de negocios que registra información acerca de inserciones, actualizaciones y eliminaciones en el suscriptor.  
  
 [!code-cs[HowTo#rmo_BusinessLogicCode](../../relational-databases/replication/codesnippet/csharp/rmohowto/businesslogic.cs#rmo_businesslogiccode)]  
  
 [!code-vb[HowTo#rmo_vb_BusinessLogicCode](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/businesslogic.vb#rmo_vb_businesslogiccode)]  
  
 Este ejemplo registra el controlador de lógica de negocios en el distribuidor.  
  
 [!code-cs[HowTo#rmo_RegisterBLH_10](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_registerblh_10)]  
  
 [!code-vb[HowTo#rmo_vb_RegisterBLH_10](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_registerblh_10)]  
  
 Este ejemplo cambia un artículo existente para usar el controlador de lógica de negocios.  
  
 [!code-cs[HowTo#rmo_ChangeMergeArticle_BLH](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_changemergearticle_blh)]  
  
 [!code-vb[HowTo#rmo_vb_ChangeMergeArticle_BLH](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_changemergearticle_blh)]  
  
## <a name="see-also"></a>Consulte también  
 [Implementar un solucionador de conflictos personalizado para un artículo de mezcla](../../relational-databases/replication/implement-a-custom-conflict-resolver-for-a-merge-article.md)   
 [Depurar un controlador de lógica de negocios &#40;programación de la replicación&#41;](../../relational-databases/replication/debug-a-business-logic-handler-replication-programming.md)   
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Replication Management Objects Concepts (Conceptos de Replication Management Objects)](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)  
  
  
