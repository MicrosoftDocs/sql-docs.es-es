---
title: Ver y modificar la configuración de seguridad de la replicación | Microsoft Docs
description: Obtenga información sobre cómo ver y modificar la configuración de seguridad de la replicación en SQL Server mediante SQL Server Management Studio, Transact-SQL o Replication Management Objects.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- modifying replication security settings
- replication [SQL Server], security
- security [SQL Server replication], viewing settings
- viewing replication security settings
- security [SQL Server replication], modifying settings
ms.assetid: 67d79532-1482-4de1-ac9f-4a23d162c85e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 1d7779f3e400345efcd183db16bf0d5780365f69
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250648"
---
# <a name="view-and-modify-replication-security-settings"></a>Ver y modificar la configuración de seguridad de la replicación
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  En este tema se describe cómo ver y modificar la configuración de seguridad de la replicación en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o Replication Management Objects (RMO). Por ejemplo, puede que desee cambiar la conexión del Agente de registro del LOG al publicador de la autenticación de SQL Server a la autenticación de Windows integrada, o puede que necesite cambiar las credenciales utilizadas para ejecutar un trabajo de agente cuando la contraseña de la cuenta de Windows ha cambiado. Para obtener información acerca de los permisos que necesita cada agente, vea [Modelo de seguridad del agente de replicación](../../../relational-databases/replication/security/replication-agent-security-model.md).  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para ver y modificar la configuración de seguridad de la replicación con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Replication Management Objects (RMO)](#RMOProcedure)  
  
-   **Seguimiento:**  [después de modificar la configuración de seguridad de la replicación](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Los procedimientos almacenados que se usen dependerán del tipo de agente y del tipo de conexión al servidor.  
  
-   Las propiedades y clases RMO que utilice dependerán dependen del tipo de agente y el tipo de conexión al servidor.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
 Por razones de seguridad, los valores reales de las contraseñas se enmascaran en los conjuntos de resultados devueltos por los procedimientos almacenados de replicación.  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Vea y modifique la configuración de seguridad en los siguientes cuadros de diálogo:  
  
1.  El cuadro de diálogo **Actualizar contraseñas de replicación** , que está disponible en la carpeta **Replicación** de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Si cambia la contraseña de una cuenta de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o de Windows en un servidor de una topología de replicación, utilice este cuadro de diálogo después de actualizar la contraseña para cada agente que utilice la cuenta. Si agentes de varios servidores utilizan la misma cuenta, debe conectarse a cada servidor y cambiar la contraseña. La contraseña se actualiza en todos los lugares en los que la replicación utiliza la contraseña. La contraseña no se actualiza en otros lugares, como los servidores vinculados.  
  
2.  La página **Seguridad del agente** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** . Para obtener más información sobre el acceso a este cuadro de diálogo, vea [View and Modify Publication Properties](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
3.  El cuadro de diálogo **Propiedades de suscripción: \<Subscription>** . Para obtener más información acerca de cómo obtener acceso a este cuadro de diálogo, vea [View and Modify Push Subscription Properties](../../../relational-databases/replication/view-and-modify-push-subscription-properties.md) y [View and Modify Pull Subscription Properties](../../../relational-databases/replication/view-and-modify-pull-subscription-properties.md).  
  
4.  Los cuadros de diálogo **Propiedades del distribuidor: \<Distributor>** y **Propiedades de la base de datos de distribución: \<Database>** . Para obtener más información acerca de cómo obtener acceso a estos cuadros de diálogo, vea [View and Modify Distributor and Publisher Properties](../../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md).  
  
5.  El cuadro de diálogo **Propiedades del publicador: \<Publisher>** . Para obtener más información acerca de cómo obtener acceso a este cuadro de diálogo, [View and Modify Distributor and Publisher Properties](../../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md).  

#### <a name="to-change-the-password-for-an-account-used-by-one-or-more-agents"></a>Para cambiar la contraseña de una cuenta utilizada por uno o varios agentes  
  
1.  Si se trata de una cuenta de SQL Server, este cuadro de diálogo también cambia la contraseña de dicha cuenta. Si es una cuenta de Windows, cambie primero la contraseña en Windows. Para obtener más información, consulte la documentación de Windows.  
  
    > [!NOTE]  
    >  Después de cambiar una contraseña de replicación, es necesario detener y reiniciar cada agente que utilice la contraseña para que el cambio surta efecto en dicho agente.  
  
2.  Conéctese al servidor en [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]y, a continuación, expanda el nodo de servidor.  
  
3.  Haga clic con el botón secundario en la carpeta **replicación** y, a continuación, haga clic en **Actualizar contraseñas de replicación**.  
  
4.  En el cuadro de diálogo **Actualizar contraseñas de replicación** , especifique la cuenta y la contraseña nueva.  
  
5.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-change-security-settings-for-the-snapshot-agent"></a>Para cambiar la configuración de seguridad del Agente de instantáneas  
  
1.  En la página **Seguridad del agente** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** , haga clic en el botón **Configuración de seguridad** situado junto al cuadro de texto **Agente de instantáneas**.  
  
2.  En el cuadro de diálogo **Seguridad del Agente de instantáneas** , especifique la cuenta con la que debe ejecutarse el agente:  
  
    -   Indique una nueva cuenta de Windows en el cuadro de texto **Cuenta del agente** .  
  
    -   Escriba una nueva contraseña en los cuadros de texto **Contraseña** y **Confirmar contraseña** .  
  
3.  Especifique el contexto en el que el agente deberá conectarse del distribuidor al publicador. Si selecciona **Mediante el siguiente inicio de sesión de SQL Server**, deberá especificar también el inicio de sesión:  
  
    -   Indique un inicio de sesión en el cuadro de texto **Inicio de sesión** .  
  
    -   Escriba una nueva contraseña en los cuadros de texto **Contraseña** y **Confirmar contraseña** .  
  
    > [!NOTE]  
    >  Si el publicador es un Publicador de Oracle, el contexto de conexión se especifica en el cuadro de diálogo **Propiedades del distribuidor: \<Distributor>** . Vea el procedimiento para cambiar el contexto más adelante en este tema.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-change-security-settings-for-the-log-reader-agent"></a>Para cambiar la configuración de seguridad del Agente de registro del LOG  
  
1.  En la página **Seguridad del agente** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** , haga clic en el botón **Configuración de seguridad** situado junto al cuadro de texto **Agente de registro del LOG**.  
  
2.  En el cuadro diálogo **Seguridad del Agente de registro del LOG** , especifique la cuenta con la que debe ejecutarse el agente:  
  
    -   Indique una nueva cuenta de Windows en el cuadro de texto **Cuenta del agente** .  
  
    -   Escriba una nueva contraseña en los cuadros de texto **Contraseña** y **Confirmar contraseña** .  
  
3.  Especifique el contexto en el que el agente deberá conectarse del distribuidor al publicador. Si selecciona **Mediante el siguiente inicio de sesión de SQL Server**, deberá especificar también el inicio de sesión:  
  
    -   Indique un inicio de sesión en el cuadro de texto **Inicio de sesión** .  
  
    -   Escriba una nueva contraseña en los cuadros de texto **Contraseña** y **Confirmar contraseña** .  
  
    > [!NOTE]  
    >  Si el publicador es un Publicador de Oracle, el contexto de conexión se especifica en el cuadro de diálogo **Propiedades del distribuidor: \<Distributor>** . Cambie el contexto siguiendo el procedimiento que se indica a continuación.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
    > [!NOTE]  
    >  Existe un Agente de registro del LOG para cada base de datos publicada. El cambio de la configuración de seguridad del agente de una publicación afecta a la configuración de todas las publicaciones de la base de datos.  
  
#### <a name="to-change-the-context-under-which-the-snapshot-agent-and-log-reader-agent-for-an-oracle-publication-make-connections-to-the-publisher"></a>Para cambiar el contexto en el que el Agente de instantáneas y el Agente de registro del LOG para una publicación de Oracle realizan las conexiones con el publicador  
  
1.  En la página **Publicadores** del cuadro de diálogo **Propiedades del distribuidor: \<Distributor>** , haga clic en el botón de propiedades ( **...** ) situado junto a un publicador.  
  
2.  En la sección **Conexión del agente al publicador** , especifique el inicio de sesión y la contraseña utilizados por el esquema de usuario administrativo de replicación que ha configurado. Para obtener más información, vea [Configurar un publicador de Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-change-security-settings-for-the-distribution-agent-for-a-push-subscription"></a>Para cambiar la configuración de seguridad del Agente de distribución para una suscripción de inserción  
  
1.  En el cuadro de diálogo **Propiedades de suscripción: \<Subscription>** del publicador, puede realizar los cambios siguientes:  
  
    -   Para cambiar la cuenta con la que el Agente de distribución se ejecuta y realiza conexiones al distribuidor, haga clic en la fila **Cuenta de proceso del agente** y, después, haga clic en el botón de propiedades ( **...** ) de la fila. Especifique una cuenta y una contraseña en el cuadro de diálogo **Seguridad del Agente de distribución** .  
  
    -   Para cambiar el contexto en el que el Agente de distribución se conecta al suscriptor, haga clic en la fila **Conexión de suscriptor** y después en el botón de propiedades ( **...** ) de la fila. Especifique el contexto en el cuadro de diálogo **Escribir información de conexión** .  
  
         Si utiliza suscripciones de actualización en cola, el Agente de lectura de cola utiliza también el contexto especificado aquí para las conexiones al suscriptor.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-change-security-settings-for-the-distribution-agent-for-a-pull-subscription"></a>Para cambiar la configuración de seguridad del Agente de distribución para una suscripción de extracción  
  
1.  En el cuadro de diálogo **Propiedades de suscripción: \<Subscription>** del suscriptor, puede realizar los siguientes cambios:  
  
    -   Para cambiar la cuenta con la que el Agente de distribución se ejecuta y realiza conexiones al suscriptor, haga clic en la fila **Cuenta de proceso del agente** y después en el botón de propiedades ( **...** ) de la fila. Especifique una cuenta y una contraseña en el cuadro de diálogo **Seguridad del Agente de distribución** .  
  
         Si utiliza suscripciones de actualización en cola, el Agente de lectura de cola utiliza también el contexto especificado aquí para las conexiones al suscriptor.  
  
    -   Para cambiar el contexto en el que el Agente de distribución se conecta al distribuidor, haga clic en la fila **Conexión del distribuidor** y después en el botón de propiedades ( **...** ) de la fila. Especifique el contexto en el cuadro de diálogo **Escribir información de conexión** .  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-change-security-settings-for-the-merge-agent-for-a-push-subscription"></a>Para cambiar la configuración de seguridad del Agente de mezcla para una suscripción de inserción  
  
1.  En el cuadro de diálogo **Propiedades de suscripción: \<Subscription>** del publicador, puede realizar los cambios siguientes:  
  
    -   Para cambiar la cuenta con la que el Agente de mezcla se ejecuta y realiza conexiones al publicador y al distribuidor, haga clic en la fila **Cuenta de proceso del agente** y después en el botón de propiedades ( **...** ) de la fila. Especifique una cuenta y una contraseña en el cuadro de diálogo **Seguridad del Agente de mezcla** .  
  
    -   Para cambiar el contexto en el que el Agente de mezcla se conecta al suscriptor, haga clic en la fila **Conexión de suscriptor** y después en el botón de propiedades ( **...** ) de la fila. Especifique el contexto en el cuadro de diálogo **Escribir información de conexión** .  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-change-security-settings-for-the-merge-agent-for-a-pull-subscription"></a>Para cambiar la configuración de seguridad del Agente de mezcla para una suscripción de extracción  
  
1.  En el cuadro de diálogo **Propiedades de suscripción: \<Subscription>** del suscriptor, puede realizar los siguientes cambios:  
  
    -   Para cambiar la cuenta con la que el Agente de mezcla se ejecuta y realiza conexiones al suscriptor, haga clic en la fila **Cuenta de proceso del agente** y después en el botón de propiedades ( **...** ) de la fila. Especifique una cuenta y una contraseña en el cuadro de diálogo **Seguridad del Agente de mezcla** .  
  
    -   Para cambiar el contexto en el que el Agente de mezcla se conecta al publicador y al suscriptor, haga clic en la fila **Conexión de publicador** y después en el botón de propiedades ( **...** ) de la fila. Especifique el contexto en el cuadro de diálogo **Escribir información de conexión** .  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-change-the-account-under-which-the-queue-reader-agent-runs"></a>Para cambiar la cuenta con la que se ejecuta el Agente de lectura de cola  
  
1.  En la página **General** del cuadro de diálogo **Propiedades del distribuidor: \<Distributor>** , haga clic en el botón de propiedades ( **…** ) situado junto a la base de datos de distribución.  
  
2.  En el cuadro de diálogo **Propiedades de base de datos de distribución: \<Database>** , haga clic en el botón **Configuración de seguridad** situado junto al cuadro de texto **Cuenta de proceso del agente**.  
  
3.  En el cuadro diálogo **Seguridad del Agente de lectura de cola** , especifique la cuenta con la que el agente debe ejecutarse y realizar conexiones al distribuidor:  
  
    -   Indique una nueva cuenta de Windows en el cuadro de texto **Cuenta de proceso** .  
  
    -   Escriba una nueva contraseña en los cuadros de texto **Contraseña** y **Confirmar contraseña** .  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
    > [!NOTE]  
    >  Existe un Agente de lectura de cola para cada base de datos de distribución. El cambio de la configuración de seguridad del agente afecta a la configuración de todas las publicaciones de todos los publicadores que usan esta base de datos de distribución.  
  
#### <a name="to-change-the-context-under-which-the-queue-reader-agent-makes-connections-to-the-publisher"></a>Para cambiar el contexto en el que el Agente de lectura de cola realiza conexiones al publicador  
  
1.  En la página **Publicadores** del cuadro de diálogo **Propiedades del distribuidor: \<Distributor>** , haga clic en el botón de propiedades ( **...** ) situado junto al publicador.  
  
2.  En la sección **Conexión del agente al publicador** , especifique el valor **Suplantar la cuenta de proceso del agente** o **autenticación de SQL Server** para la opción **Modo de conexión del agente** . Si especifica **Autenticación de SQL Server**, indique también valores para **Inicio de sesión** y **Contraseña**.  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
    > [!NOTE]  
    >  Existe un Agente de lectura de cola para cada base de datos de distribución. El cambio de la configuración de seguridad del agente afecta a la configuración de todas las publicaciones de todos los publicadores que usan esta base de datos de distribución.  
  
#### <a name="to-change-the-context-under-which-the-queue-reader-agent-makes-connections-to-the-subscriber"></a>Para cambiar el contexto en el que el Agente de lectura de cola realiza conexiones al suscriptor  
  
-   El Agente de lectura de cola utiliza el mismo contexto de conexión que el Agente de distribución para la suscripción. Para obtener más información, vea los procedimientos descritos anteriormente para el Agente de distribución.  
  
#### <a name="to-change-security-settings-for-an-immediate-updating-pull-subscription"></a>Para cambiar la configuración de seguridad para una suscripción de extracción de actualización inmediata  
  
1.  En el cuadro de diálogo **Propiedades de suscripción \<Subscription>** del suscriptor, haga clic en la fila **Conexión de publicador** y luego en el botón de propiedades ( **&#x2026;** ) de la fila.  
  
2.  En el cuadro de diálogo **Escribir información de conexión** , seleccione una de las siguientes opciones:  
  
    -   **Utilizar inicio de sesión de un servidor vinculado o remoto**. Seleccione esta opción si ha definido un servidor remoto o un servidor vinculado entre el suscriptor y el publicador mediante [sp_addserver &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md), [sp_addlinkedserver &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md), [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] u otro método.  
  
    -   **Utilizar autenticación de SQL Server con el inicio de sesión y la contraseña siguientes**. Seleccione esta opción si no ha definido un servidor remoto o un servidor vinculado entre el suscriptor y el publicador. La replicación creará un servidor vinculado. La cuenta que especifique debe existir ya en el publicador.  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
> [!NOTE]  
>  Este procedimiento cambia el método que los desencadenadores de replicación utilizan para conectarse del suscriptor al publicador cuando se realizan cambios en el suscriptor. También puede cambiar la configuración asociada al Agente de distribución para una suscripción de actualización inmediata. Para obtener más información, vea los procedimientos que se describen anteriormente en este tema.  
>   
>  Este procedimiento solamente se aplica a las suscripciones de extracción. Para las suscripciones de inserción, use el procedimiento almacenado [sp_link_publication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-link-publication-transact-sql.md).  
  
#### <a name="to-change-the-password-for-the-administrative-connection-from-the-publisher-to-the-distributor"></a>Para cambiar la contraseña de la conexión administrativa del publicador al distribuidor  
  
1.  En la página **Publicadores** del cuadro de diálogo **Propiedades del distribuidor: \<Distributor>** , escriba una contraseña segura en los cuadros de texto **Contraseña** y **Confirmar contraseña**.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
3.  En la página **General** del cuadro de diálogo **Propiedades del publicador: \<Publisher>** , escriba una contraseña segura en los cuadros de texto **Contraseña** y **Confirmar contraseña**.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
> [!IMPORTANT]  
>  En todos los procedimientos siguientes, cuando sea posible, pida a los usuarios que especifiquen las credenciales de seguridad en tiempo de ejecución. Si almacena credenciales en un archivo de script, debe proteger el archivo para evitar el acceso no autorizado.  
  
#### <a name="to-change-all-instances-of-a-stored-password-at-a-replication-server"></a>Para cambiar todas las instancias de una contraseña almacenada en un servidor de replicación  
  
1.  En una topología de replicación de un servidor, en la base de datos maestra, ejecute [sp_changereplicationserverpasswords](../../../relational-databases/system-stored-procedures/sp-changereplicationserverpasswords-transact-sql.md). Especifique la cuenta de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows o el inicio de sesión de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cuya contraseña se va a cambiar para `@login` y la nueva contraseña para la cuenta o el inicio de sesión de `@password`. Esto cambia cada instancia de la contraseña que usan todos los agentes del servidor al conectarse a otros servidores de la topología.  
  
    > [!NOTE]  
    >  Para cambiar solo el inicio de sesión y la contraseña de una conexión a un servidor determinado de la topología (como el distribuidor o el suscriptor), especifique el nombre de este servidor para `@server`.  
  
2.  Repita el paso 1 en cada servidor de la topología de replicación en el que se deba actualizar la contraseña.  
  
    > [!NOTE]  
    >  Después de cambiar una contraseña de replicación, es necesario detener y reiniciar cada agente que utilice la contraseña para que el cambio surta efecto en dicho agente.  
  
#### <a name="to-change-security-settings-for-the-snapshot-agent"></a>Para cambiar la configuración de seguridad del Agente de instantáneas  
  
1.  En el publicador, ejecute [sp_helppublication_snapshot](../../../relational-databases/system-stored-procedures/sp-helppublication-snapshot-transact-sql.md), y especifique `@publication`. Esto devuelve la configuración de seguridad actual para el Agente de instantáneas.  
  
2.  En el publicador, ejecute [sp_changepublication_snapshot](../../../relational-databases/system-stored-procedures/sp-changepublication-snapshot-transact-sql.md), y especifique `@publication` y una o varias de las opciones de configuración de seguridad siguientes que se vayan a cambiar:  
  
    -   Para cambiar la cuenta de Windows con la que se ejecuta el agente o simplemente la contraseña de esa cuenta, especifique `@job_login` y `@job_password`.  
  
    -   Para cambiar el modo de seguridad que se usa al conectarse al publicador, especifique un valor de **1** o **0** para `@publisher_security_mode`.  
  
    -   Al cambiar el modo de seguridad que se usa para conectarse al publicador de **1** a **0**, o bien al cambiar un inicio de sesión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que se usa para esta conexión, especifique `@publisher_login` y `@publisher_password`.  
  
    > [!IMPORTANT]  
    >  Al configurar un publicador con un distribuidor remoto, los valores suministrados para todos los parámetros, incluidos *job_login* y *job_password*, se envían al distribuidor como texto sin formato. Antes de ejecutar este procedimiento almacenado, se recomienda cifrar la conexión entre el publicador y su distribuidor remoto. Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
#### <a name="to-change-security-settings-for-the-log-reader-agent"></a>Para cambiar la configuración de seguridad del Agente de registro del LOG  
  
1.  En el publicador, ejecute [sp_helplogreader_agent](../../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md), y especifique `@publisher`. Esto devuelve la configuración de seguridad actual para el Agente de registro del LOG.  
  
2.  En el publicador, ejecute [sp_changelogreader_agent](../../../relational-databases/system-stored-procedures/sp-changelogreader-agent-transact-sql.md), y especifique `@publication` y una o varias de las opciones de configuración de seguridad siguientes que se vayan a cambiar:  
  
    -   Para cambiar la cuenta de Windows con la que se ejecuta el agente o simplemente la contraseña de esa cuenta, especifique `@job_login` y `@job_password`.  
  
    -   Para cambiar el modo de seguridad que se usa al conectarse al publicador, especifique un valor de **1** o **0** para `@publisher_security_mode`.  
  
    -   Al cambiar el modo de seguridad que se usa para conectarse al publicador de **1** a **0**, o bien al cambiar un inicio de sesión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que se usa para esta conexión, especifique `@publisher_login` y `@publisher_password`.  
  
    > [!NOTE]  
    >  Después de cambiar un inicio de sesión o una contraseña de agente, debe detener y reiniciar el agente para que el cambio surta efecto.  
  
    > [!IMPORTANT]  
    >  Al configurar un publicador con un distribuidor remoto, los valores suministrados para todos los parámetros, incluidos *job_login* y *job_password*, se envían al distribuidor como texto sin formato. Antes de ejecutar este procedimiento almacenado, se recomienda cifrar la conexión entre el publicador y su distribuidor remoto. Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
#### <a name="to-change-security-settings-for-the-distribution-agent-for-a-push-subscription"></a>Para cambiar la configuración de seguridad del Agente de distribución para una suscripción de inserción  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_helpsubscription](../../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md) y especifique `@publication` y `@subscriber`. Esto devuelve propiedades de suscripción, incluida la configuración de seguridad para el Agente de distribución que se ejecuta en el distribuidor.  
  
2.  En la base de datos de publicación del publicador, ejecute [sp_changesubscription](../../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md), y especifique `@publication`, `@subscriber`, `@subscriber_db**`, un valor de `all` para `@article`, el nombre de la propiedad de seguridad para `@property` y el nuevo valor de la propiedad para `@value`.  
  
3.  Repita el paso 2 para cada una de las propiedades de seguridad siguientes que se vayan a cambiar:  
  
    -   Para cambiar la cuenta de Windows con la que se ejecuta el agente o simplemente la contraseña de esa cuenta, especifique un valor de **distrib_job_password** para `@property` y una contraseña nueva para `@value`. Al cambiar la propia cuenta, repita el paso 2 y especifique un valor de **distrib_job_login** para `@property` y la nueva cuenta de Windows para `@value`.  
  
    -   Para cambiar el modo de seguridad que se usa al conectarse al suscriptor, especifique un valor de **subscriber_security_mode** para `@property` y un valor de **1** (Autenticación integrada de Windows) o **0** (Autenticación de SQL Server) para `@value`.  
  
    -   Al cambiar el modo de seguridad del suscriptor a la autenticación de SQL Server o al cambiar la información de inicio de sesión para la autenticación de SQL Server, especifique un valor de **subscriber_password** para `@property` y la contraseña nueva para `@value`. Repita el paso 2, y especifique un valor de **subscriber_login** para `@property` y el nuevo inicio de sesión para `@value`.  
  
    > [!NOTE]  
    >  Después de cambiar un inicio de sesión o una contraseña de agente, debe detener y reiniciar el agente para que el cambio surta efecto.  
  
    > [!IMPORTANT]  
    >  Al configurar un publicador con un distribuidor remoto, los valores suministrados para todas las propiedades, incluidas **distrib_job_login** y **distrib_job_password**, se envían al distribuidor como texto simple. Antes de ejecutar este procedimiento almacenado, se recomienda cifrar la conexión entre el publicador y su distribuidor remoto. Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
#### <a name="to-change-security-settings-for-the-distribution-agent-for-a-pull-subscription"></a>Para cambiar la configuración de seguridad del Agente de distribución para una suscripción de extracción  
  
1.  En el suscriptor, ejecute [sp_helppullsubscription](../../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md), y especifique `@publication`. Esto devuelve propiedades de suscripción, incluida la configuración de seguridad para el Agente de distribución que se ejecuta en el suscriptor.  
  
2.  En la base de datos de suscripciones del suscriptor, ejecute [sp_change_subscription_properties](../../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md), y especifique `@publisher`, `@publisher_db`, `@publication`, el nombre de la propiedad de seguridad para `@property` y el nuevo valor de la propiedad para `@value`.  
  
3.  Repita el paso 2 para cada una de las propiedades de seguridad siguientes que se vayan a cambiar:  
  
    -   Para cambiar la cuenta de Windows con la que se ejecuta el agente o simplemente la contraseña de esa cuenta, especifique un valor de **distrib_job_password** para `@property` y una contraseña nueva para `@value`. Al cambiar la propia cuenta, repita el paso 2 y especifique un valor de **distrib_job_login** para `@property` y la nueva cuenta de Windows para `@value`.  
  
    -   Para cambiar el modo de seguridad que se usa al conectarse al distribuidor, especifique un valor de **distributor_security_mode** para `@property` y un valor de **1** (Autenticación integrada de Windows) o **0** (Autenticación de SQL Server) para `@value`.  
  
    -   Al cambiar el modo de seguridad del distribuidor a la autenticación de SQL Server o al cambiar la información de inicio de sesión para la autenticación de SQL Server, especifique un valor de **distributor_password** para `@property` y la contraseña nueva para `@value`. Repita el paso 2, y especifique un valor de **distributor_login** para `@property` y el nuevo inicio de sesión para `@value`.  
  
    > [!NOTE]  
    >  Después de cambiar un inicio de sesión o una contraseña de agente, debe detener y reiniciar el agente para que el cambio surta efecto.  
  
#### <a name="to-change-security-settings-for-the-merge-agent-for-a-push-subscription"></a>Para cambiar la configuración de seguridad del Agente de mezcla para una suscripción de inserción  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_helpmergesubscription](../../../relational-databases/system-stored-procedures/sp-helpmergesubscription-transact-sql.md), y especifique `@publication`, `@subscriber,` y `@subscriber_db`. Esto devuelve propiedades de suscripción, incluida la configuración de seguridad para el Agente de mezcla que se ejecuta en el distribuidor.  
  
2.  En la base de datos de publicación del publicador, ejecute [sp_changemergesubscription](../../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md), y especifique `@publication`, `@subscriber`, `@subscriber_db`, el nombre de la propiedad de seguridad para `@property` y el nuevo valor de la propiedad para `@value`.  
  
3.  Repita el paso 2 para cada una de las propiedades de seguridad siguientes que se vayan a cambiar:  
  
    -   Para cambiar la cuenta de Windows con la que se ejecuta el agente o simplemente la contraseña de esa cuenta, especifique un valor de **merge_job_password** para `@property` y una contraseña nueva para `@value`. Al cambiar la propia cuenta, repita el paso 2 y especifique un valor de **merge_job_login** para `@property` y la nueva cuenta de Windows para `@value`.  
  
    -   Para cambiar el modo de seguridad que se usa al conectarse al suscriptor, especifique un valor de **subscriber_security_mode** para `@property` y un valor de **1** (Autenticación integrada de Windows) o **0** (Autenticación de SQL Server) para `@value`.  
  
    -   Al cambiar el modo de seguridad del suscriptor a la autenticación de SQL Server o al cambiar la información de inicio de sesión para la autenticación de SQL Server, especifique un valor de **subscriber_password** para `@property` y la contraseña nueva para `@value`. Repita el paso 2, y especifique un valor de **subscriber_login** para `@property` y el nuevo inicio de sesión para `@value`.  
  
    -   Para cambiar el modo de seguridad que se usa al conectarse al publicador, especifique un valor de **publisher_security_mode** para `@property` y un valor de **1** (Autenticación integrada de Windows) o **0** (Autenticación de SQL Server) para `@value`.  
  
    -   Al cambiar el modo de seguridad del publicador a la autenticación de SQL Server o al cambiar la información de inicio de sesión para la autenticación de SQL Server, especifique un valor de **publisher_password** para `@property` y la contraseña nueva para `@value`. Repita el paso 2, y especifique un valor de **publisher_login** para `@property` y el nuevo inicio de sesión para `@value`.  
  
    > [!NOTE]  
    >  Después de cambiar un inicio de sesión o una contraseña de agente, debe detener y reiniciar el agente para que el cambio surta efecto.  
  
    > [!IMPORTANT]  
    >  Al configurar un publicador con un distribuidor remoto, los valores suministrados para todas las propiedades, incluidas **merge_job_login** y **merge_job_password**, se envían al distribuidor como texto simple. Antes de ejecutar este procedimiento almacenado, se recomienda cifrar la conexión entre el publicador y su distribuidor remoto. Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
#### <a name="to-change-security-settings-for-the-merge-agent-for-a-pull-subscription"></a>Para cambiar la configuración de seguridad del Agente de mezcla para una suscripción de extracción  
  
1.  En el suscriptor, ejecute [sp_helpmergepullsubscription](../../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md), y especifique ``@publication``. Esto devuelve propiedades de suscripción, incluida la configuración de seguridad para el Agente de mezcla que se ejecuta en el suscriptor.  
  
2.  En la base de datos de suscripciones del suscriptor, ejecute [sp_change_subscription_properties](../../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md), y especifique `@publisher`, `@publisher_db`, `@publication`, el nombre de la propiedad de seguridad para `@property` y el nuevo valor de la propiedad para `@value`.  
  
3.  Repita el paso 2 para cada una de las propiedades de seguridad siguientes que se vayan a cambiar:  
  
    -   Para cambiar la cuenta de Windows con la que se ejecuta el agente o simplemente la contraseña de esa cuenta, especifique un valor de **merge_job_password** para `@property` y una contraseña nueva para `@value`. Al cambiar la propia cuenta, repita el paso 2 y especifique un valor de **merge_job_login** para `@property` y la nueva cuenta de Windows para `@value`.  
  
    -   Para cambiar el modo de seguridad que se usa al conectarse al distribuidor, especifique un valor de **distributor_security_mode** para `@property` y un valor de **1** (Autenticación integrada de Windows) o **0** (Autenticación de SQL Server) para `@value`.  
  
    -   Al cambiar el modo de seguridad del distribuidor a la autenticación de SQL Server o al cambiar la información de inicio de sesión para la autenticación de SQL Server, especifique un valor de **distributor_password** para `@property` y la contraseña nueva para `@value`. Repita el paso 2, y especifique un valor de **distributor_login** para `@property` y el nuevo inicio de sesión para `@value`.  
  
    -   Para cambiar el modo de seguridad que se usa al conectarse al publicador, especifique un valor de **publisher_security_mode** para `@property` y un valor de **1** (Autenticación integrada de Windows) o **0** (Autenticación de SQL Server) para `@value`.  
  
    -   Al cambiar el modo de seguridad del publicador a la autenticación de SQL Server o al cambiar la información de inicio de sesión para la autenticación de SQL Server, especifique un valor de **publisher_password** para `@property` y la contraseña nueva para `@value`. Repita el paso 2, y especifique un valor de **publisher_login** para `@property` y el nuevo inicio de sesión para `@value`.  
  
    > [!NOTE]  
    >  Después de cambiar un inicio de sesión o una contraseña de agente, debe detener y reiniciar el agente para que el cambio surta efecto.  
  
#### <a name="to-change-security-settings-for-the-snapshot-agent-to-generate-a-filtered-snapshot-for-a-subscriber"></a>Para cambiar la configuración de seguridad para que el Agente de instantáneas genere una instantánea filtrada para un suscriptor  
  
1.  En el publicador, ejecute [sp_helpdynamicsnapshot_job](../../../relational-databases/system-stored-procedures/sp-helpdynamicsnapshot-job-transact-sql.md), y especifique `@publication`. En el conjunto de resultados, tenga en cuenta el valor de **job_name** para que la partición del suscriptor cambie.  
  
2.  En el publicador, ejecute [sp_changedynamicsnapshot_job](../../../relational-databases/system-stored-procedures/sp-changedynamicsnapshot-job-transact-sql.md), y especifique `@publication`, el valor obtenido del paso 1 para `dynamic_snapshot_jobname` y una contraseña nueva para `@job_password`, o bien el inicio de sesión y la contraseña de la cuenta de Windows con la que se ejecuta el agente para `@job_login` y `@job_password`.  
  
    > [!IMPORTANT]  
    >  Al configurar un publicador con un distribuidor remoto, los valores suministrados para todos los parámetros, incluidos *job_login* y *job_password*, se envían al distribuidor como texto sin formato. Antes de ejecutar este procedimiento almacenado, se recomienda cifrar la conexión entre el publicador y su distribuidor remoto. Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
#### <a name="to-change-security-settings-for-the-queue-reader-agent"></a>Para cambiar la configuración de seguridad del Agente de lectura de cola  
  
1.  En el distribuidor, ejecute [sp_helpqreader_agent](../../../relational-databases/system-stored-procedures/sp-helpqreader-agent-transact-sql.md). Esto devuelve la cuenta de Windows actual con la que se ejecuta el Agente de lectura de cola.  
  
    -   En el distribuidor, ejecute [sp_changeqreader_agent](../../../relational-databases/system-stored-procedures/sp-changeqreader-agent-transact-sql.md), y especifique la configuración de la cuenta de Windows para `@job_login` y `@job_password`.  
  
    > [!NOTE]  
    >  Después de cambiar un inicio de sesión o una contraseña de agente, debe detener y reiniciar el agente para que el cambio surta efecto. Existe un Agente de lectura de cola para cada base de datos de distribución. El cambio de la configuración de seguridad del agente afecta a la configuración de todas las publicaciones de todos los publicadores que usan esta base de datos de distribución.  
  
2.  El Agente de lectura de cola establece conexiones al suscriptor con el mismo contexto de conexión que el Agente de distribución para la suscripción.  
  
#### <a name="to-change-security-mode-used-by-an-immediate-updating-subscriber-when-connecting-to-the-publisher"></a>Para cambiar el modo de seguridad que usa un suscriptor de actualización inmediata al conectarse al publicador  
  
1.  En el suscriptor de la base de datos de suscripción, ejecute [sp_link_publication](../../../relational-databases/system-stored-procedures/sp-link-publication-transact-sql.md). Especifique `@publisher`, `@publication`, el nombre de la base de datos de publicación para `@publisher_db`y uno de los valores siguientes para `@security_mode`:  
  
    -   **0** para usar la autenticación de SQL Server al realizar las actualizaciones en el publicador. Esta opción le exige que especifique un inicio de sesión válido en el publicador para `@login` y `@password`.  
  
    -   **1** para usar el contexto de seguridad del usuario que realiza modificaciones en el suscriptor al conectarse al publicador. Consulte [sp_link_publication](../../../relational-databases/system-stored-procedures/sp-link-publication-transact-sql.md) para conocer las restricciones relacionadas con este modo de seguridad.  
  
    -   **2** para usar un inicio de sesión vinculado definido por el usuario ya existente y creado mediante [sp_addlinkedserver &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md).  
  
#### <a name="to-change-the-password-for-a-remote-distributor"></a>Para cambiar la contraseña de un distribuidor remoto  
  
1.  En la base de datos de distribución del distribuidor, ejecute [sp_changedistributor_password](../../../relational-databases/system-stored-procedures/sp-changedistributor-password-transact-sql.md), y especifique la contraseña nueva de este inicio de sesión para `@password`.  
  
    > [!IMPORTANT]  
    >  No cambie directamente la contraseña de **distributor_admin** .  
  
2.  En cada publicador que use este distribuidor remoto, ejecute [sp_changedistributor_password](../../../relational-databases/system-stored-procedures/sp-changedistributor-password-transact-sql.md), y especifique la contraseña del paso 1 para `@password`.  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Uso de Replication Management Objects (RMO)  
  
> [!IMPORTANT]  
>  Cuando sea posible, pida a los usuarios que proporcionen credenciales de seguridad en tiempo de ejecución. Si debe almacenar credenciales, use los [servicios de cifrado](/previous-versions/aa719848(v=vs.71)) (en inglés) proporcionados por [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows .NET Framework.  
  
#### <a name="to-change-all-instances-of-a-password-stored-on-a-replication-server"></a>Para cambiar todas las instancias de una contraseña almacenadas en un servidor de replicación  
  
1.  Cree una conexión al servidor de replicación mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.ReplicationServer> con la conexión del paso 1.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationServer.ChangeReplicationServerPasswords%2A> . Especifique los parámetros siguientes:  
  
    -   *security_mode* - un valor <xref:Microsoft.SqlServer.Replication.ReplicationSecurityMode> que especifica el tipo de autenticación para la que se cambian todas las instancias de la contraseña.  
  
    -   *login* - el inicio de sesión para el que se cambian todas las instancias de la contraseña.  
  
    -   *password* - el nuevo valor de contraseña.  
  
        > [!IMPORTANT]  
        >  Cuando sea posible, pida a los usuarios que proporcionen credenciales de seguridad en tiempo de ejecución. Si debe almacenar credenciales, use los [servicios de cifrado](/previous-versions/aa719848(v=vs.71)) (en inglés) proporcionados por Windows .NET Framework.  
  
        > [!NOTE]  
        >  Solo un miembro del rol fijo de servidor **sysadmin** puede llamar a este método.  
  
4.  Repita los pasos 1-3 repetidos para cada servidor de la topología de replicación en los que se deba actualizar la contraseña.  
  
#### <a name="to-change-security-settings-for-the-distribution-agent-for-a-push-subscription-to-a-transactional-publication"></a>Para cambiar la configuración de seguridad del Agente de distribución para una suscripción de inserción a una publicación transaccional  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransSubscription>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>y <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A> para la suscripción y establezca la conexión del paso 1 para la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto. Si este método devuelve **false**, significa que las propiedades de suscripción del paso 3 se definieron incorrectamente, o bien que la suscripción no existe.  
  
5.  Establezca una o más de las propiedades de seguridad siguientes en la instancia de <xref:Microsoft.SqlServer.Replication.TransSubscription>:  
  
    -   Para cambiar las credenciales de la cuenta de Windows en la que se ejecuta el agente, establezca los campos <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> y <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>.  
  
    -   Para especificar la autenticación de Windows integrada como el tipo de autenticación que utiliza el agente cuando se conecta al suscriptor, establezca el campo <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> de la propiedad <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> en **true**.  
  
    -   Para especificar la autenticación de SQL Server como el tipo de autenticación que utiliza el agente cuando se conecta al suscriptor, establezca el campo <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> de la propiedad <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> en **false** y especifique las credenciales de inicio de sesión del suscriptor para los campos <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> y <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> .  
  
        > [!NOTE]  
        >  La conexión de agente al distribuidor siempre se realiza utilizando las credenciales de Windows especificadas por <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>. Esta cuenta también se utiliza para realizar conexiones remotas mediante la autenticación de Windows.  
  
6.  (Opcional) Si especificara un valor de **true** para <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A>, llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> para confirmar los cambios en el servidor. Si especificó el valor **false** para <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> (predeterminado), los cambios se envían inmediatamente al servidor.  
  
#### <a name="to-change-security-settings-for-the-distribution-agent-for-a-pull-subscription-to-a-transactional-publication"></a>Para cambiar la configuración de seguridad del Agente de distribución para una suscripción de extracción a una publicación transaccional  
  
1.  Cree una conexión al suscriptor mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection>.  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransPullSubscription>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>y <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A> para la suscripción y establezca la conexión del paso 1 para la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto. Si este método devuelve **false**, significa que las propiedades de suscripción del paso 3 se definieron incorrectamente, o bien que la suscripción no existe.  
  
5.  Establezca una o más de las propiedades de seguridad siguientes en la instancia de <xref:Microsoft.SqlServer.Replication.TransPullSubscription>:  
  
    -   Para cambiar las credenciales de la cuenta de Windows en la que se ejecuta el agente, establezca los campos <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> y <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A>.  
  
    -   Para especificar la autenticación de Windows integrada como el tipo de autenticación que utiliza el agente cuando se conecta al distribuidor, establezca el campo <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> de la propiedad <xref:Microsoft.SqlServer.Replication.PullSubscription.DistributorSecurity%2A> en **true**.  
  
    -   Para especificar la autenticación de SQL Server como el tipo de autenticación que utiliza el agente cuando se conecta al distribuidor, establezca el campo <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> de la propiedad <xref:Microsoft.SqlServer.Replication.PullSubscription.DistributorSecurity%2A> en **false** y especifique las credenciales de inicio de sesión del distribuidor para los campos <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> y <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> .  
  
        > [!NOTE]  
        >  La conexión de agente al suscriptor siempre se realiza utilizando las credenciales de Windows especificadas por <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A>. Esta cuenta también se utiliza para realizar conexiones remotas mediante la autenticación de Windows.  
  
6.  (Opcional) Si especificara un valor de **true** para <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A>, llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> para confirmar los cambios en el servidor. Si especificó el valor **false** para <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> (predeterminado), los cambios se envían inmediatamente al servidor.  
  
#### <a name="to-change-security-settings-for-the-merge-agent-for-a-pull-subscription-to-a-merge-publication"></a>Para cambiar la configuración de seguridad del Agente de mezcla para una suscripción de extracción a una publicación de combinación  
  
1.  Cree una conexión al suscriptor mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection>.  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergePullSubscription>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>y <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A> para la suscripción y establezca la conexión del paso 1 para la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto. Si este método devuelve **false**, significa que las propiedades de suscripción del paso 3 se definieron incorrectamente, o bien que la suscripción no existe.  
  
5.  Establezca una o más de las propiedades de seguridad siguientes en la instancia de <xref:Microsoft.SqlServer.Replication.MergePullSubscription>:  
  
    -   Para cambiar las credenciales de la cuenta de Windows en la que se ejecuta el agente, establezca los campos <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> y <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A>.  
  
    -   Para especificar la autenticación de Windows integrada como el tipo de autenticación que utiliza el agente cuando se conecta al distribuidor, establezca el campo <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> de la propiedad <xref:Microsoft.SqlServer.Replication.PullSubscription.DistributorSecurity%2A> en **true**.  
  
    -   Para especificar la autenticación de SQL Server como el tipo de autenticación que utiliza el agente cuando se conecta al distribuidor, establezca el campo <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> de la propiedad <xref:Microsoft.SqlServer.Replication.PullSubscription.DistributorSecurity%2A> en **false** y especifique las credenciales de inicio de sesión del distribuidor para los campos <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> y <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> .  
  
    -   Para especificar la autenticación de Windows integrada como el tipo de autenticación que utiliza el agente cuando se conecta al publicador, establezca el campo <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> de la propiedad <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherSecurity%2A> en **true**.  
  
    -   Para especificar la autenticación de SQL Server como el tipo de autenticación que utiliza el agente cuando se conecta al publicador, establezca el campo <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> de la propiedad <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherSecurity%2A> en **false** y especifique las credenciales de inicio de sesión del publicador para los campos <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> y <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> .  
  
        > [!NOTE]  
        >  La conexión de agente al suscriptor siempre se realiza utilizando las credenciales de Windows especificadas por <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A>. Esta cuenta también se utiliza para realizar conexiones remotas mediante la autenticación de Windows.  
  
6.  (Opcional) Si especificara un valor de **true** para <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A>, llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> para confirmar los cambios en el servidor. Si especificó el valor **false** para <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> (predeterminado), los cambios se envían inmediatamente al servidor.  
  
#### <a name="to-change-security-settings-for-the-merge-agent-for-a-push-subscription-to-a-merge-publication"></a>Para cambiar la configuración de seguridad del Agente de mezcla para una suscripción de inserción a una publicación de combinación  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergeSubscription>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>y <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A> para la suscripción y establezca la conexión del paso 1 para la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto. Si este método devuelve **false**, significa que las propiedades de suscripción del paso 3 se definieron incorrectamente, o bien que la suscripción no existe.  
  
5.  Establezca una o más de las propiedades de seguridad siguientes en la instancia de <xref:Microsoft.SqlServer.Replication.MergeSubscription>:  
  
    -   Para cambiar las credenciales de la cuenta de Windows en la que se ejecuta el agente, establezca los campos <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> y <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A>.  
  
    -   Para especificar la autenticación de Windows integrada como el tipo de autenticación que utiliza el agente cuando se conecta al suscriptor, establezca el campo <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> de la propiedad <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> en **true**.  
  
    -   Para especificar la autenticación de SQL Server como el tipo de autenticación que utiliza el agente cuando se conecta al suscriptor, establezca el campo <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> de la propiedad <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> en **false** y especifique las credenciales de inicio de sesión del suscriptor para los campos <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> y <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> .  
  
    -   Para especificar la autenticación de Windows integrada como el tipo de autenticación que utiliza el agente cuando se conecta al publicador, establezca el campo <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> de la propiedad <xref:Microsoft.SqlServer.Replication.MergeSubscription.PublisherSecurity%2A> en **true**.  
  
    -   Para especificar la autenticación de SQL Server como el tipo de autenticación que utiliza el agente cuando se conecta al publicador, establezca el campo <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> de la propiedad <xref:Microsoft.SqlServer.Replication.MergeSubscription.PublisherSecurity%2A> en **false** y especifique las credenciales de inicio de sesión del publicador para los campos <xref:Microsoft.SqlServer.Replication.PublisherConnectionSecurityContext.SqlStandardLogin%2A> y <xref:Microsoft.SqlServer.Replication.PublisherConnectionSecurityContext.SqlStandardPassword%2A> .  
  
        > [!NOTE]  
        >  La conexión de agente al distribuidor siempre se realiza utilizando las credenciales de Windows especificadas por <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>. Esta cuenta también se utiliza para realizar conexiones remotas mediante la autenticación de Windows.  
  
6.  (Opcional) Si especificara un valor de **true** para <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A>, llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> para confirmar los cambios en el servidor. Si especificó el valor **false** para <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> (predeterminado), los cambios se envían inmediatamente al servidor.  
  
#### <a name="to-change-the-login-information-used-by-an-immediate-updating-subscriber-when-it-connects-to-the-transactional-publisher"></a>Para cambiar la información de inicio de sesión utilizada por un suscriptor de actualización inmediata cuando se conecta al publicador transaccional  
  
1.  Cree una conexión al suscriptor mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection>.  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.ReplicationDatabase> para la base de datos de suscripciones. Especifique <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.Name%2A> y <xref:Microsoft.SqlServer.Management.Common.ServerConnection> del paso 1 para <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto. Si este método devuelve **false**, significa que las propiedades de suscripción del paso 2 se definieron incorrectamente, o bien que la base de datos de suscripciones no existe.  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.LinkPublicationForUpdateableSubscription%2A> , pasando los parámetros siguientes:  
  
    -   *Publisher* - el nombre del publicador.  
  
    -   *PublisherDB* - el nombre de la base de datos de publicación.  
  
    -   *Publication* - el nombre de la publicación a la que se suscribe el suscriptor de actualización inmediata.  
  
    -   *Distributor* - el nombre del distribuidor.  
  
    -   *PublisherSecurity* - A <xref:Microsoft.SqlServer.Replication.PublisherConnectionSecurityContext> que especifica el tipo de modo de seguridad que emplea el suscriptor de actualización inmediata cuando se conecta al publicador y las credenciales de inicio de sesión para la conexión.  
  
###  <a name="example-rmo"></a><a name="PShellExample"></a> Ejemplo (RMO)  
 Este ejemplo comprueba el valor de inicio de sesión proporcionado y cambia todas las contraseñas para el inicio de sesión de Windows proporcionado o el inicio de sesión de SQL Server almacenado por replicación en el servidor.  
  
 [!code-cs[HowTo#rmo_ChangeServerPasswords](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_changeserverpasswords)]  
  
 [!code-vb[HowTo#rmo_vb_ChangeServerPasswords](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_changeserverpasswords)]  
  
##  <a name="follow-up-after-you-modify-replication-security-settings"></a><a name="FollowUp"></a> Seguimiento: después de modificar la configuración de seguridad de la replicación  
 Después de cambiar un inicio de sesión o una contraseña de agente, debe detener y reiniciar el agente para que el cambio surta efecto.  
  
## <a name="see-also"></a>Consulte también  
 [Replication Management Objects Concepts](../../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Actualizar scripts de replicación &#40;programación de la replicación con Transact-SQL&#41;](../../../relational-databases/replication/administration/upgrade-replication-scripts-replication-transact-sql-programming.md)   
 [Identidad y control de acceso (replicación)](../../../relational-databases/replication/security/identity-and-access-control-replication.md)   
 [Modelo de seguridad del Agente de replicación](../../../relational-databases/replication/security/replication-agent-security-model.md)   
 [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Ver y modificar la configuración de seguridad de la replicación](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)   
 [Conceptos de procedimientos almacenados del sistema de replicación](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)  
  
