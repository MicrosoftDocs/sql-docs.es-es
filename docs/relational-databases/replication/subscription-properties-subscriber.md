---
title: Propiedades de suscripción, cuadro de diálogo
description: Describe el cuadro de diálogo "Propiedades de la suscripción" en SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newsubwizard.subproperties.publisher.f1
- sql13.rep.newsubwizard.subproperties.subscriber.f1
ms.assetid: db2be511-c76e-4f21-8be4-6a8c60a50d30
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 1d0af8d38957286080b6e01c4ed73149ee43bc36
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272336"
---
# <a name="sql-server-replication-subscription-properties-dialog-box"></a>Cuadro de diálogo Propiedades de la suscripción de Replicación de SQL Server 
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]

### <a name="publisher-properties"></a>Propiedades del publicador
El cuadro de diálogo **Propiedades de la suscripción** del publicador permite ver y definir las propiedades para las suscripciones de inserción. También se pueden ver algunas propiedades para las suscripciones de extracción, pero el cuadro de diálogo **Propiedades de la suscripción** del suscriptor muestra propiedades adicionales y permite modificarlas.  
  
 Cada una de las propiedades del cuadro de diálogo **Propiedades de la suscripción** incluye una descripción. Haga clic en una propiedad para mostrar la descripción en la parte inferior del cuadro de diálogo. Este tema proporciona información adicional sobre una serie de propiedades, la mayoría de las cuales se muestran en el publicador únicamente para las suscripciones de inserción. Las propiedades se agrupan en las siguientes categorías:  
  
-   Propiedades que se aplican a todas las suscripciones.    
-   Propiedades que se aplican a las suscripciones transaccionales.  
-   Propiedades que se aplican a las suscripciones de mezcla.  
  
 Si una opción se muestra como de solo lectura, solamente se puede establecer durante la creación de la suscripción. Si desea establecer opciones que no están disponibles en el Asistente para nueva suscripción, cree la suscripción con los procedimientos almacenados. Para obtener más información, consulte [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md) y [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md).  

### <a name="subscriber-properties"></a>Propiedades del suscriptor
El cuadro de diálogo **Propiedades de suscripción** del suscriptor le permite ver y establecer las propiedades de las suscripciones de extracción.  
  
 Cada una de las propiedades del cuadro de diálogo **Propiedades de la suscripción** incluye una descripción. Haga clic en una propiedad para mostrar la descripción en la parte inferior del cuadro de diálogo. Este tema ofrece información adicional acerca de varias propiedades. Las propiedades se agrupan en las siguientes categorías:    
-   Propiedades que se aplican a todas las suscripciones.    
-   Propiedades que se aplican a las suscripciones transaccionales.   
-   Propiedades que se aplican a las suscripciones de mezcla.  
  
 Si una opción se muestra como de solo lectura, solamente se puede establecer durante la creación de la suscripción. Si desea establecer opciones que no están disponibles en el Asistente para nueva suscripción, cree la suscripción con los procedimientos almacenados. Para obtener más información, consulte [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md) y [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md).  
  
> [!NOTE]  
>  - Si aún no ha creado un Agente de distribución o un Agente de mezcla para la suscripción, no se mostrarán muchas de las propiedades de la suscripción. Para crear un trabajo de agente para una suscripción de extracción, ejecute [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md) (para una suscripción a una publicación de instantáneas o transaccional) o [sp_addmergepullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md) (para una suscripción a una publicación de mezcla).  
> - Las bases de datos de Azure SQL Managed Instance pueden ser un publicador, un distribuidor y un suscriptor para la replicación transaccional y de instantáneas. Las bases de datos de Azure SQL Database solo pueden ser suscriptores de extracción para la replicación transaccional y de instantáneas. Para más información, consulte [Replicación transaccional con Azure SQL Database](/azure/sql-database/sql-database-managed-instance-transactional-replication). 
  
## <a name="publisher-options-for-all-subscriptions"></a>Opciones de publicador para todas las suscripciones  
 **Seguridad**  
 Haga clic en la fila **Cuenta de proceso del agente** y, a continuación, haga clic en el botón de propiedades ( **...** ) para cambiar la cuenta con la que el Agente de distribución o el Agente de mezcla se ejecutan en el distribuidor. Para cambiar la cuenta en la que el Agente de distribución o el Agente de mezcla realizan conexiones al suscriptor, haga clic en **Conexión de suscriptor** y, a continuación, haga clic en el botón de propiedades ( **…** ).  
  
 Para obtener más información acerca de los permisos necesarios para cada agente, vea [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
## <a name="publisher-options-for-transactional-subscriptions"></a>Opciones de publicador para las suscripciones transaccionales  
 **Impedir bucles de transacciones**  
 Determina si el agente de distribución envía las transacciones originadas en el suscriptor al mismo suscriptor. Esta opción se utiliza para la replicación transaccional bidireccional. Para más información, consulte [Bidirectional Transactional Replication](../../relational-databases/replication/transactional/bidirectional-transactional-replication.md).  
  
 **Suscripción actualizable**  
 Determina si los cambios del suscriptor se vuelven a replicar en el publicador. Es posible replicar los cambios con una actualización en cola o una actualización inmediata. La opción **Método de actualización del suscriptor** determina el método que se debe utilizar. Para más información, consulte [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
## <a name="publisher-options-for-merge-subscriptions"></a>Opciones de publicador para las suscripciones de mezcla  
 **Definición de partición (HOST_NAME)**  
 Para una publicación que utiliza filtros con parámetros, la replicación de mezcla evalúa una de las dos funciones del sistema (o ambas si el filtro hace referencia a las dos) durante la sincronización para determinar los datos que un suscriptor debe recibir: **SUSER_SNAME()** o **HOST_NAME()** . De manera predeterminada, **HOST_NAME()** devuelve el nombre del equipo en el que se ejecuta el Agente de mezcla, pero puede reemplazar este valor en el Asistente para nueva suscripción. Para obtener más información acerca de los filtros con parámetros y de cómo reemplazar **HOST_NAME()** , vea [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 **Tipo de suscripción** y **Prioridad**  
 Muestra si la suscripción es una suscripción de cliente o de servidor (no podrá cambiarse una vez creada la suscripción). Las suscripciones de servidor pueden volver a publicar datos en otros suscriptores y es posible asignarles una prioridad para la resolución de conflictos.  
  
 Si ha seleccionado un tipo de suscripción de servidor en el Asistente para nueva suscripción, el suscriptor recibe la prioridad que se utiliza durante la resolución de conflictos.  
  
 **Solucionar conflictos de manera interactiva**  
 Establece si se va a utilizar la interfaz de usuario Solucionador interactivo para solucionar conflictos durante la sincronización de mezcla. Requiere un valor de **Habilitado** en **Utilizar el Administrador de sincronización de Windows**. Para obtener más información, consulte [Interactive Conflict Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md).  

  
## <a name="subscriber-options-for-all-subscriptions"></a>Opciones de suscriptor para todas las suscripciones  
 **Inicializar datos publicados desde una instantánea**  
 Determina si las suscripciones se inicializan con una instantánea (opción predeterminada) o con otro método. Para más información sobre cómo inicializar suscripciones, vea [Initialize a Subscription](../../relational-databases/replication/initialize-a-subscription.md) (Inicializar una suscripción).  
  
 **Ubicación de la instantánea**  
 Determina la ubicación desde la que se tiene acceso a los archivos de instantáneas durante la inicialización o la reinicialización. La ubicación puede ser uno de los siguientes valores:  
  
-   **Ubicación predeterminada**: muestra la ubicación predeterminada, definida al configurar el distribuidor. Para más información, vea [Especificación de las opciones de instantáneas](../../relational-databases/replication/snapshot-options.md).  
  
-   **Carpeta alternativa**: muestra una ubicación alternativa, que puede especificarse en el cuadro de diálogo **Propiedades de la publicación** . Para más información, vea [Especificación de las opciones de instantáneas](../../relational-databases/replication/snapshot-options.md).  
  
-   **Carpeta de instantáneas dinámicas**: muestra una ubicación de instantáneas para las publicaciones de combinación que utilizan filtros de fila con parámetros. Para más información, consulte [Instantáneas para publicaciones de combinación con filtros con parámetros](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
-   **Carpeta FTP**: carpeta desde la que se tiene acceso al servidor Protocolo de transferencia de archivos (FTP). Para más información, vea [Entrega de una instantánea mediante FTP](../../relational-databases/replication/publish/deliver-a-snapshot-through-ftp.md).  
  
 **Carpeta de instantáneas**  
 Si selecciona un valor distinto de **Ubicación predeterminada** en la opción **Ubicación de la instantánea** , deberá especificar la ruta a la carpeta de instantáneas.  
  
 **Utilizar el Administrador de sincronización de Windows**  
 Determina si es posible sincronizar la suscripción con el Administrador de sincronización de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 **Seguridad**  
 Haga clic en la fila **Cuenta de proceso del agente** y, a continuación, haga clic en el botón de propiedades ( **...** ) para cambiar la cuenta en la que se ejecuta el Agente de distribución o el Agente de mezcla en el suscriptor. Las opciones de seguridad relativas a las conexiones dependen del tipo de suscripción:  
  
-   Para las sucripciones a una publicación transaccional: para cambiar la cuenta en la que el Agente de distribución realiza la conexión al distribuidor, haga clic en **Conexión del distribuidor** y, a continuación, en el botón de propiedades ( **...** ).  
  
-   Para las suscripción de actualización inmediatas a una publicación transaccional: además de la conexión Distributor describió sobre, puede cambiar el método utilizado para propagar los cambios en el Suscriptor al Publicador: haga clic en **Conexión del publicador** y, a continuación, haga clic en el botón ( **...** ) de propiedades.  
  
-   En las suscripciones a publicaciones de combinación, haga clic en **Conexión de publicador** y, a continuación, haga clic en el botón de propiedades ( **...** ).  
  
 Para obtener más información acerca de los permisos necesarios para cada agente, vea [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
## <a name="subscriber-options-for-transactional-subscriptions"></a>Opciones de suscriptor para las suscripciones transaccionales  
 **Suscripción actualizable**  
 Determina si los cambios del suscriptor se vuelven a replicar en el publicador. Es posible replicar los cambios con una actualización en cola o una actualización inmediata. La opción **Método de actualización del suscriptor** determina el método que se debe utilizar. Para más información, consulte [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
## <a name="options-for-merge-subscriptions"></a>Opciones para suscripciones de mezcla  
 **Definición de partición (HOST_NAME)**  
 Para una publicación que utiliza filtros con parámetros, la replicación de mezcla evalúa una de las dos funciones del sistema (o ambas si el filtro hace referencia a las dos) durante la sincronización para determinar los datos que un suscriptor debe recibir: **SUSER_SNAME()** o **HOST_NAME()** . De manera predeterminada, **HOST_NAME()** devuelve el nombre del equipo en el que se ejecuta el Agente de mezcla, pero puede reemplazar este valor en el Asistente para nueva suscripción. Para obtener más información acerca de los filtros con parámetros y de cómo reemplazar **HOST_NAME()** , vea [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 **Tipo de suscripción** y **Prioridad**  
 Muestra si la suscripción es una suscripción de cliente o de servidor (no podrá cambiarse una vez creada la suscripción). Las suscripciones de servidor pueden volver a publicar datos en otros suscriptores y es posible asignarles una prioridad para la resolución de conflictos.  
  
 Si ha seleccionado un tipo de suscripción de servidor en el Asistente para nueva suscripción, el suscriptor recibe la prioridad que se usa durante la resolución de conflictos.  
  
 **Solucionar conflictos de manera interactiva**  
 Establece si se va a utilizar la interfaz de usuario Solucionador interactivo para solucionar conflictos durante la sincronización de mezcla. Requiere un valor de **Habilitado** en **Utilizar el Administrador de sincronización de Windows**. Para obtener más información, consulte [Interactive Conflict Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md).  
  
 **Sincronización web**  
 **Usar sincronización web** determina si se va a establecer la conexión con un servidor de Internet Information Services (IIS) de [!INCLUDE[msCoName](../../includes/msconame-md.md)] para sincronizar la suscripción. Esta opción solo está disponible si la sincronización web está habilitada en la publicación. Para más información, consulte [Web Synchronization for Merge Replication](../../relational-databases/replication/web-synchronization-for-merge-replication.md).  
  
 Si selecciona **True** para **Usar sincronización web**:  
  
-   Escriba la dirección completa del servidor IIS en **Dirección de servidor web**.    
-   Haga clic en la fila **Conexión de servidor web** y, a continuación, haga clic en el botón de propiedades ( **...** ) para establecer o cambiar la cuenta con la que el suscriptor se conecta al servidor IIS.    
-   Si fuese necesario, cambie el **Tiempo de espera del servidor web** . El tiempo de espera es la cantidad de tiempo, en segundos, que transcurre antes de que expire la solicitud de sincronización web. 

 
Para obtener más información acerca de la configuración, vea [Configure Web Synchronization](../../relational-databases/replication/configure-web-synchronization.md). 
  
## <a name="see-also"></a>Consulte también  
 [View and Modify Pull Subscription Properties](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)  (Ver y modificar las propiedades de una suscripción de extracción)  
 [Ver y modificar las propiedades de una suscripción de inserción](../../relational-databases/replication/view-and-modify-push-subscription-properties.md)   
 [Suscribirse a publicaciones](../../relational-databases/replication/subscribe-to-publications.md)   

  
  
