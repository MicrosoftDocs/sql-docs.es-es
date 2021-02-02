---
title: Creación de una suscripción para un suscriptor que no sea de SQL
description: Describe cómo crear una suscripción para un suscriptor que no sea de SQL Server en SQL Server mediante SQL Server Management Studio (SSMS) o Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [SQL Server replication], non-SQL Server Subscribers
- Subscribers [SQL Server replication], non-SQL Server Subscribers
- non-SQL Server Subscribers, subscriptions
ms.assetid: 5020ee68-b988-4d57-8066-67d183e61237
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 36c15a9d7de2c613b3a25b16827bfcc037393524
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076227"
---
# <a name="create-a-subscription-for-a-non-sql-server-subscriber"></a>Crear una suscripción para un suscriptor que no sea de SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo crear una suscripción para un suscriptor que no sea de SQL Server en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. La replicación transaccional y la de instantáneas admiten la publicación de datos en suscriptores que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información sobre plataformas de suscriptores admitidos, vea [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md).  
  
 **En este tema**  
  
-   **Para crear una suscripción para un suscriptor que no sea de SQL Server con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Para crear una suscripción para un suscriptor que no sea de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
1.  Instale y configure el software de cliente y el proveedor o proveedores OLE DB adecuados en el distribuidor de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener más información, consulte [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md) y [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md).  
  
2.  Cree una publicación mediante el Asistente para nueva publicación. Para obtener información sobre cómo crear una publicación de una base de datos de Oracle, vea [Crear una publicación a partir de una base de datos de Oracle](../../relational-databases/replication/publish/create-a-publication.md) y [Crear una publicación a partir de una base de datos de Oracle](../../relational-databases/replication/publish/create-a-publication-from-an-oracle-database.md). En el Asistente para nueva publicación, especifique las siguientes opciones:  
  
    -   En la página **Tipo de publicación** , seleccione **Publicación de instantáneas** o **Publicación transaccional**.  
  
    -   En la página **Agente de instantáneas** , desactive **Crear una instantánea inmediatamente**.  
  
         La instantánea se crea una vez habilitada la publicación para los suscriptores que no son de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a fin de garantizar que el Agente de instantáneas genera scripts de inicialización e instantáneas adecuados para los suscriptores que no son de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
3.  Habilite la publicación para los suscriptores que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante el cuadro de diálogo **Propiedades de la publicación: \<PublicationName>** . Para obtener más información acerca de este paso, vea [Publication Properties, Subscription Options](../../relational-databases/replication/publication-properties-subscription-options.md) .  
  
4.  Cree una suscripción mediante el Asistente para nuevas suscripciones. En este tema se proporciona más información acerca de este paso.  
  
5.  (Opcional) Cambie la propiedad de artículo **pre_creation_cmd** para conservar las tablas en el suscriptor. En este tema se proporciona más información acerca de este paso.  
  
6.  Genere una instantánea para la publicación. En este tema se proporciona más información acerca de este paso.  
  
7.  Sincronice la suscripción. Para obtener más información, consulte [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
#### <a name="to-enable-a-publication-for-non-sql-server-subscribers"></a>Para habilitar una publicación para suscriptores que no son de SQL Server  
  
1.  Conéctese al publicador en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]y luego expanda el nodo del servidor.  
  
2.  Expanda la carpeta **Replicación** y, a continuación, expanda la carpeta **Publicaciones locales** .  
  
3.  Haga clic con el botón secundario en la publicación y, a continuación, haga clic en **Propiedades**.  
  
4.  En la página **Opciones de suscripción** , seleccione el valor **True** para la opción **Permitir suscriptores que no sean de SQL**. Al seleccionar esta opción, se modifican una serie de propiedades para que la publicación sea compatible con suscriptores que no son de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    > [!NOTE]  
    >  Al seleccionar **True** se asigna el valor 'drop' a la propiedad de artículo **pre_creation_cmd** . El valor mencionado indica que la replicación debe quitar una tabla en el suscriptor si coincide con el nombre de la tabla del artículo. Si en el suscriptor existen tablas que desea conservar, use el procedimiento almacenado [sp_changearticle](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md) para cada artículo; asigne el valor 'none' a **pre_creation_cmd**: `sp_changearticle @publication= 'MyPublication', @article= 'MyArticle', @property='pre_creation_cmd', @value='none'`.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)] Se le pedirá que cree una nueva instantánea para la publicación. Si no desea crear ninguna en este momento, siga los pasos descritos en el procedimiento correspondiente más adelante.  
  
#### <a name="to-create-a-subscription-for-a-non-sql-server-subscriber"></a>Para crear una suscripción para un suscriptor que no sea de SQL Server  
  
1.  Expanda la carpeta **Replicación** y, a continuación, expanda la carpeta **Publicaciones locales** .  
  
2.  Haga clic con el botón secundario en la publicación correspondiente y, a continuación, haga clic en **Nuevas suscripciones**.  
  
3.  En la página **Ubicación del Agente de distribución** , asegúrese de que la opción **Ejecutar todos los agentes en el distribuidor** está seleccionada. Los suscriptores que no son de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no pueden ejecutar agentes en el suscriptor.  
  
4.  En la página **Suscriptores** , haga clic en **Agregar suscriptor** y, a continuación, haga clic en **Agregar suscriptor que no sea de SQL Server**.  
  
5.  En el cuadro de diálogo **Agregar suscriptor que no sea de SQL Server** , seleccione el tipo de suscriptor.  
  
6.  Especifique un valor en **Nombre del origen de datos**:  
  
    -   Para Oracle, es el nombre TNS (transparent network substrate) configurado.  
  
    -   Para IBM, puede ser cualquier nombre. Lo habitual es especificar la dirección de red del suscriptor.  
  
     El asistente no valida el nombre del origen de datos especificado en este paso ni las credenciales del paso 9. No se utilizan para la replicación hasta que se ejecuta el Agente de distribución para la suscripción. Asegúrese de que haber comprobado todos los valores conectándose al suscriptor mediante una herramienta cliente (como **sqlplus** para Oracle). Para obtener más información, consulte [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md) y [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md).  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)] En la página **Suscriptores** del asistente, el suscriptor aparece en la columna **Suscriptor** con un **(destino predeterminado)** de solo lectura en la columna **Base de datos de suscripciones** :  
  
    -   En Oracle, un servidor tiene como máximo una base de datos, por lo que no es necesario especificarla.  
  
    -   Para IBM DB2, la base de datos se especifica en la propiedad **Catálogo inicial** de la cadena de conexión DB2, la cual puede indicarse en el campo **Opciones de conexión adicionales** que se describe más adelante en este proceso.  
  
8.  En la página **Seguridad del Agente de distribución** , haga clic en el botón de propiedades ( **…** ) situado junto al suscriptor para obtener acceso al cuadro de diálogo **Seguridad del Agente de distribución** .  
  
9. En el cuadro de diálogo **Seguridad del Agente de distribución** :  
  
    -   En los campos **Cuenta de proceso**, **Contraseña** y **Confirmar contraseña** , especifique la cuenta y contraseña de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows con las que se deberá ejecutar el Agente de distribución y establecer las conexiones locales al distribuidor.  
  
         La cuenta requiere los siguientes permisos mínimos: miembro del rol fijo de base de datos **db_owner** de la base de datos de distribución; miembro de la lista de acceso a la publicación (PAL); disponer de permisos de lectura en el recurso compartido de instantáneas y disponer de permiso de lectura en el directorio de instalación del proveedor OLE DB. Para obtener más información sobre la PAL, vea [Secure the Publisher](../../relational-databases/replication/security/secure-the-publisher.md) (Proteger el publicador).  
  
    -   En **Conectar al suscriptor**, en los campos **Inicio de sesión**, **Contraseña** y **Confirmar contraseña** , escriba el nombre de inicio de sesión y la contraseña que deben utilizarse para conectar al suscriptor. Este inicio de sesión ya debería estar configurado y disponer de los permisos suficientes para crear objetos en la base de datos de suscripciones.  
  
    -   En el campo **Opciones de conexión adicionales** , especifique cualquier opción de conexión para el suscriptor en forma de cadena de conexión (Oracle no requiere opciones adicionales). Las opciones deben ir separadas con punto y coma. A continuación se ofrece un ejemplo de una cadena de conexión DB2 (se han incluido saltos de línea para facilitar la lectura):  
  
        ```  
        Provider=DB2OLEDB;Initial Catalog=MY_SUBSCRIBER_DB;Network Transport Library=TCP;Host CCSID=1252;  
        PC Code Page=1252;Network Address=MY_SUBSCRIBER;Network Port=50000;Package Collection=MY_PKGCOL;  
        Default Schema=MY_SCHEMA;Process Binary as Character=False;Units of Work=RUW;DBMS Platform=DB2/NT;  
        Persist Security Info=False;Connection Pooling=True;  
        ```  
  
         La mayoría de las opciones de la cadena son específicas para el servidor DB2 que está configurando, pero la opción **Process Binary as Character** siempre debe establecerse en **False**. Se requiere un valor para que la opción **Catálogo original** identifique la base de datos de suscripciones.  
  
10. En la página **Programación de sincronización** , seleccione una programación para el Agente de distribución en el menú **Programación del agente** (la programación suele ser **Ejecutar continuamente**).  
  
11. En la página **Inicializar suscripciones** , especifique si la suscripción debe inicializarse y, en ese caso, cuándo debe llevarse a cabo:  
  
    -   Desactive **Inicializar** únicamente si ha creado todos los objetos y agregado todos los datos necesarios a la base de datos de suscripciones.  
  
    -   Seleccione **Inmediatamente** en la lista desplegable de la columna **Inicializar cuando** para que el Agente de distribución transfiera los archivos de instantáneas al suscriptor una vez que el asistente haya finalizado. Seleccione **En la primera sincronización** para que el agente transfiera los archivos la próxima vez que esté programado para ejecutarse.  
  
12. En la página **Acciones del asistente** , incluya de forma opcional la suscripción. Para más información, consulte [Scripting Replication](../../relational-databases/replication/scripting-replication.md).  
  
#### <a name="to-retain-tables-at-the-subscriber"></a>Para conservar las tablas en el suscriptor  
  
-   De forma predeterminada, al habilitar una publicación para suscriptores que no son de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , se asigna el valor 'drop' a la propiedad de artículo **pre_creation_cmd** . El valor mencionado indica que la replicación debe quitar una tabla en el suscriptor si coincide con el nombre de la tabla del artículo. Si en el suscriptor existen tablas que desea conservar, use el procedimiento almacenado [sp_changearticle](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md) para cada artículo; asigne el valor 'none' a **pre_creation_cmd**. `sp_changearticle @publication= 'MyPublication', @article= 'MyArticle', @property='pre_creation_cmd', @value='none'`.  
  
#### <a name="to-generate-a-snapshot-for-the-publication"></a>Para generar una instantánea para la publicación  
  
1.  Expanda la carpeta **Replicación** y, a continuación, expanda la carpeta **Publicaciones locales** .  
  
2.  Haga clic con el botón secundario en la publicación y, a continuación, haga clic en **Ver estado del agente de instantáneas**.  
  
3.  En el cuadro de diálogo **Ver estado del Agente de instantáneas: \<Publication>** , haga clic en **Iniciar**.  
  
 Cuando el Agente de instantáneas termina de generar la instantánea, aparece un mensaje del tipo "[100%] Se ha generado una instantánea de 17 artículos".  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Puede crear suscripciones de inserción para suscriptores que no sean de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante programación con procedimientos almacenados de replicación.  
  
> [!IMPORTANT]  
>  Cuando sea posible, pida a los usuarios que proporcionen credenciales de seguridad en tiempo de ejecución. Si debe almacenar las credenciales en un archivo de script, proteja el archivo para evitar el acceso no autorizado.  
  
#### <a name="to-create-a-push-subscription-for-a-transactional-or-snapshot-publication-to-a-non-sql-server-subscriber"></a>Para crear una suscripción de inserción para una publicación transaccional o de instantáneas a un suscriptor que no sea de SQL Server  
  
1.  Instale el proveedor OLE DB más reciente para el Suscriptor que no sea de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el Publicador y el Distribuidor. Para los requisitos de replicación para un proveedor OLE DB, vea [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md), [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md), [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md).  
  
2.  En el publicador de la base de datos de publicaciones, compruebe que la publicación admita suscripciones que no sean de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante la ejecución de [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md).  
  
    -   Si el valor de **enabled_for_het_sub** es 1, se admiten Suscriptores que no sean de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    -   Si el valor de **enabled_for_het_sub** es 0, ejecute [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) y especifique **enabled_for_het_sub** para `@property` y **true** para `@value`.  
  
        > [!NOTE]  
        >  Antes de cambiar **enabled_for_het_sub** a **true**, debe quitar cualquier suscripción existente en la publicación. No puede establecer **enabled_for_het_sub** en **true** cuando la publicación también admite las suscripciones de actualización. El cambio de **enabled_for_het_sub** afectará a otras propiedades de publicación. Para más información, consulte [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md).  
  
3.  En el publicador de la base de datos de publicaciones, ejecute [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Especifique `@publication`, `@subscriber`, un valor de `(default destination)` para `@destination_db`, un valor de **push** para `@subscription_type` y un valor de 3 para `@subscriber_type` (especifica un proveedor OLE DB).  
  
4.  En el publicador de la base de datos de publicaciones, ejecute [sp_addpushsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md). Especifique lo siguiente:  
  
    -   Los parámetros `@subscriber` y `@publication`.  
  
    -   Un valor de **(destino predeterminado)** para `@subscriber_db`,  
  
    -   Las propiedades del origen de datos que no es de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para `@subscriber_provider`, `@subscriber_datasrc`, `@subscriber_location`, `@subscriber_provider_string` y `@subscriber_catalog`.  
  
    -   Los credenciales de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows con las que se ejecuta el Agente de distribución en el distribuidor para `@job_login` y `@job_password`.  
  
       > [!NOTE]  
       > Las conexiones realizadas con la Autenticación integrada de Windows siempre usan las credenciales de Windows especificadas por `@job_login` y `@job_password`. El Agente de distribución siempre realiza la conexión local con el distribuidor mediante la autenticación integrada de Windows. De forma predeterminada, el agente se conectará con el suscriptor mediante la autenticación integrada de Windows.  
  
    -   Un valor de **0** para `@subscriber_security_mode` y la información de inicio de sesión del proveedor OLE DB para `@subscriber_login` y `@subscriber_password`.  
  
    -   Una programación para el trabajo del Agente de distribución de esta suscripción. Para obtener más información, consulte [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
    > [!IMPORTANT]  
    >  Al crear una suscripción de inserción en un publicador con un distribuidor remoto, los valores suministrados para todos los parámetros, incluidos *job_login* y *job_password*, se envían al distribuidor como texto simple. Antes de ejecutar este procedimiento almacenado, se recomienda cifrar la conexión entre el publicador y su distribuidor remoto. Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
## <a name="see-also"></a>Consulte también  
 [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md)   
 [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md)   
 [Otros suscriptores que no son de SQL Server](../../relational-databases/replication/non-sql/other-non-sql-server-subscribers.md)   
 [Replication System Stored Procedures Concepts](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Procedimientos recomendados de seguridad de replicación](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
