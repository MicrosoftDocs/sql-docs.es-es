---
title: Medición de la latencia y validación de las conexiones (transaccional)
description: Aprenda a medir la latencia y a validar las conexiones de una publicación de transacción en SQL Server mediante el Monitor de replicación de SQL Server Management Studio (SSMS), Transact-SQL (T-SQL) o Replication Management Objects (RMO).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Replication Monitor, performance
- tracer tokens [SQL Server replication]
- latency [SQL Server replication]
- transactional replication, tracer tokens
- monitoring performance [SQL Server replication], tracer tokens
ms.assetid: 4addd426-7523-4067-8d7d-ca6bae4c9e34
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 99e2551cef1208a2fe2f0f4c14280fdf2bfe83df
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077163"
---
# <a name="measure-latency-and-validate-connections-for-transactional-replication"></a>Medir la latencia y validar las conexiones de la replicación transaccional
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  En este tema se describe cómo medir la latencia y validar conexiones para la replicación transaccional en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante el Monitor de replicación, [!INCLUDE[tsql](../../../includes/tsql-md.md)]o Replication Management Objects (RMO). La replicación transaccional proporciona la característica de testigo de seguimiento, que ofrece una forma cómoda de medir la latencia en topologías de replicación transaccional y validar las conexiones entre el publicador, el distribuidor y los suscriptores. Se escribe un token (una pequeña cantidad de datos) en el registro de transacción de la base de datos de publicaciones, marcado como si fuese una transacción replicada, y se envía a través del sistema, de forma que permite calcular:  
  
-   Cuánto tiempo transcurre desde que se confirma una transacción en el publicador hasta que se inserta el comando correspondiente en la base de datos de distribución del distribuidor.  
  
-   Cuánto tiempo transcurre desde que se inserta un comando en la base de datos de distribución hasta que se confirma la transacción correspondiente en el suscriptor.  
  
 A partir de estos cálculos, podrá responder a diversas preguntas, como por ejemplo:  
  
-   ¿Qué suscriptores tardan más en recibir un cambio del publicador?  
  
-   De los suscriptores que esperan recibir el testigo de seguimiento, ¿cuáles, si los hay, no lo han recibido?  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
-   **Para medir la latencia y validar conexiones con:**  
  
     [Monitor de replicación de SQL Server](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Replication Management Objects](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
 Los testigos de seguimiento también pueden ser útiles al detener el sistema, lo que implica detener todas las actividades y comprobar que todos los nodos han recibido todos los cambios pendientes. Para más información, vea [Poner en modo inactivo una topología de replicación &#40;programación de la replicación con Transact-SQL&#41;](../../../relational-databases/replication/administration/quiesce-a-replication-topology-replication-transact-sql-programming.md).  
  
 Para usar testigos de seguimiento, debe utilizar determinadas versiones de [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]:  
  
-   El distribuidor debe ser [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] o posterior.  
  
-   El publicador debe ser de [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] o posterior, o un publicador de Oracle.  
  
-   En el caso de las suscripciones de inserción, las estadísticas del token de seguimiento se obtienen del publicador, del distribuidor y de los suscriptores, si el suscriptor es [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 7.0 o posterior.  
  
-   Para las suscripciones de extracción, las estadísticas del token de seguimiento se obtienen solo de los suscriptores, si el suscriptor es de [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] o posterior. Si el suscriptor es [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 7.0 o [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)], las estadísticas se obtienen solo del publicador y del distribuidor.  
  
 También hay que tener en cuenta otros problemas y restricciones:  
  
-   Para recibir un token de seguimiento, las suscripciones deben estar activas. Una suscripción está activa si se ha inicializado.  
  
-   La reinicialización quita los testigos de seguimiento pendientes en las suscripciones correspondientes.  
  
-   Los suscriptores solo reciben los testigos de seguimiento que se han creado después de la sincronización inicial.  
  
-   Los suscriptores que vuelven a publicar no reenvían los testigos de seguimiento.  
  
-   Después de la conmutación por error a un elemento secundario, el Monitor de replicación no puede ajustar el nombre de la instancia de publicación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y seguirá mostrando información de replicación bajo el nombre de la instancia principal original de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Después de la conmutación por error, el Monitor de replicación no puede especificar un token de seguimiento, aunque muestra un token de seguimiento especificado en el nuevo publicador mediante [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
##  <a name="using-sql-server-replication-monitor"></a><a name="SSMSProcedure"></a> Usar el Monitor de replicación de SQL Server  
 Para información sobre cómo iniciar el Monitor de replicación, vea [Iniciar el Monitor de replicación](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
#### <a name="to-insert-a-tracer-token-and-view-information-on-the-token"></a>Para insertar un testigo de seguimiento y ver la información del token  
  
1.  Expanda un grupo de publicador en el panel izquierdo, expanda un publicador y, a continuación, haga clic en una publicación.  
  
2.  Haga clic en la pestaña **Testigos de seguimiento** .  
  
3.  Haga clic en **Insertar seguimiento**.  
  
4.  Vea el tiempo transcurrido para el testigo de seguimiento en las siguientes columnas: **Publicador a distribuidor**, **Distribuidor a suscriptor** y **Latencia total**. El valor **Pendiente** indica que el testigo no ha alcanzado un punto específico.  
  
#### <a name="to-view-information-on-a-tracer-token-inserted-previously"></a>Para ver información en el testigo de seguimiento insertado previamente  
  
1.  Expanda un grupo de publicador en el panel izquierdo, expanda un publicador y, a continuación, haga clic en una publicación.  
  
2.  Haga clic en la pestaña **Testigos de seguimiento** .  
  
3.  Seleccione una hora en la lista desplegable **Hora de inserción** .  
  
4.  Vea el tiempo transcurrido para el testigo de seguimiento en las siguientes columnas: **Publicador a distribuidor**, **Distribuidor a suscriptor** y **Latencia total**. El valor **Pendiente** indica que el testigo no ha alcanzado un punto específico.  
  
    > [!NOTE]  
    >  La información del testigo de seguimiento se guarda durante el mismo período que otros datos del historial, que depende del período de retención de historial de la base de datos de distribución. Para obtener información sobre cómo cambiar las propiedades de la base de datos de distribución, vea [Ver y modificar las propiedades del distribuidor y del publicador](../../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-post-a-tracer-token-to-a-transactional-publication"></a>Para exponer un token de seguimiento en una publicación transaccional  
  
1.  (Opcional) en la base de datos de publicación del publicador, ejecute [sp_helppublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md). Compruebe que la publicación existe y que el estado está activo.  
  
2.  (Opcional) en la base de datos de publicación del publicador, ejecute [sp_helpsubscription &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md). Compruebe que la suscripción existe y que el estado está activo.  
  
3.  En la base de datos de publicación del publicador, ejecute [sp_posttracertoken &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-posttracertoken-transact-sql.md) y especifique **\@publication**. Anote el valor del parámetro de salida **\@tracer_token_id**.  
  
#### <a name="to-determine-latency-and-validate-connections-for-a-transactional-publication"></a>Para medir la latencia y validar las conexiones de una replicación transaccional  
  
1.  Exponga un token de seguimiento en la publicación utilizando el procedimiento anterior.  
  
2.  En la base de datos de publicación del publicador, ejecute [sp_helptracertokens &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helptracertokens-transact-sql.md) y especifique **\@publication**. Esto devuelve una lista de todos los testigos de seguimiento expuestos en la publicación. Anote el **tracer_id** que desee del conjunto de resultados.  
  
3.  En la base de datos de publicación del publicador, ejecute [sp_helptracertokenhistory &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helptracertokenhistory-transact-sql.md) y especifique **\@publication** y el identificador del testigo de seguimiento del paso 2 para **\@tracer_id**. Esto devuelve información de latencia del token de seguimiento seleccionado.  
  
#### <a name="to-remove-tracer-tokens"></a>Para quitar los testigos de seguimiento  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_helptracertokens &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helptracertokens-transact-sql.md) y especifique **\@publication**. Esto devuelve una lista de todos los testigos de seguimiento expuestos en la publicación. Anote el **tracer_id** del token de seguimiento que se va a eliminar del conjunto de resultados.  
  
2.  En la base de datos de publicación del publicador, ejecute [sp_deletetracertokenhistory &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-deletetracertokenhistory-transact-sql.md) y especifique **\@publication** y el id. del seguimiento que se va a eliminar del paso 2 para `@tracer_id`.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Ejemplo (Transact-SQL)  
 Este ejemplo expone un registro de token de seguimiento y utiliza el Id. devuelto del token de seguimiento expuesto para ver información de la latencia.  
  
 [!code-sql[HowTo#sp_tracertokens](../../../relational-databases/replication/codesnippet/tsql/measure-latency-and-vali_1.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Uso de Replication Management Objects (RMO)  
  
#### <a name="to-post-a-tracer-token-to-a-transactional-publication"></a>Para exponer un token de seguimiento en una publicación transaccional  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransPublication>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> y <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> para la publicación y la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en la conexión creada en el paso 1.  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto. Si este método devuelve **false**, significa que las propiedades de publicación del paso 3 se definieron incorrectamente, o bien que la publicación no existe.  
  
5.  Llame al método <xref:Microsoft.SqlServer.Replication.TransPublication.PostTracerToken%2A> . Este método inserta un token de seguimiento en el registro de transacciones de la publicación.  
  
#### <a name="to-determine-latency-and-validate-connections-for-a-transactional-publication"></a>Para medir la latencia y validar las conexiones de una replicación transaccional  
  
1.  Cree una conexión al distribuidor mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.PublicationMonitor>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.PublicationMonitor.Name%2A>, <xref:Microsoft.SqlServer.Replication.PublicationMonitor.DistributionDBName%2A>, <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublisherName%2A>y <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublicationDBName%2A> , y la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en la conexión creada en el paso 1.  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto. Si este método devuelve **false**, significa que las propiedades del monitor de la publicación del paso 3 se definieron incorrectamente, o bien que la publicación no existe.  
  
5.  Llame al método <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumTracerTokens%2A> . Convierta el objeto <xref:System.Collections.ArrayList> devuelto a una matriz de objetos <xref:Microsoft.SqlServer.Replication.TracerToken> .  
  
6.  Llame al método <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumTracerTokenHistory%2A> . Pase un valor de <xref:Microsoft.SqlServer.Replication.TracerToken.TracerTokenId%2A> para un token de seguimiento del paso 5. Esto devuelve información de latencia del token de seguimiento seleccionado como objeto <xref:System.Data.DataSet> . Si se devuelve toda la información del token de seguimiento, la conexión entre el publicador y el distribuidor y la conexión entre el distribuidor y el suscriptor existen, y la topología de replicación está funcionando.  
  
#### <a name="to-remove-tracer-tokens"></a>Para quitar los testigos de seguimiento  
  
1.  Cree una conexión al distribuidor mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.PublicationMonitor>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.PublicationMonitor.Name%2A>, <xref:Microsoft.SqlServer.Replication.PublicationMonitor.DistributionDBName%2A>, <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublisherName%2A>y <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublicationDBName%2A> , y la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en la conexión creada en el paso 1.  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto. Si este método devuelve **false**, significa que las propiedades del monitor de la publicación del paso 3 se definieron incorrectamente, o bien que la publicación no existe.  
  
5.  Llame al método <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumTracerTokens%2A> . Convierta el objeto <xref:System.Collections.ArrayList> devuelto a una matriz de objetos <xref:Microsoft.SqlServer.Replication.TracerToken> .  
  
6.  Llame al método <xref:Microsoft.SqlServer.Replication.PublicationMonitor.CleanUpTracerTokenHistory%2A> . Pase uno de los siguientes valores:  
  
    -   El <xref:Microsoft.SqlServer.Replication.TracerToken.TracerTokenId%2A> para un token de seguimiento del paso 5. Esto elimina información de un token seleccionado.  
  
    -   Objeto <xref:System.DateTime> . Esto elimina información de todos los tokens anteriores a la fecha y hora especificada.  
  
  
