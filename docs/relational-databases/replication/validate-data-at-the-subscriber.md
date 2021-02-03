---
description: Validar datos replicados
title: Validar datos replicados | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Subscribers [SQL Server replication], data validation
- replication [SQL Server], validating data
- transactional replication, validating data
- validating data
- merge replication data validation [SQL Server replication], SQL Server Management Studio
ms.assetid: 215b4c9a-0ce9-4c00-ac0b-43b54151dfa3
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 1b5109c2bf93a49370f005e94a824fd5bfb70921
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250628"
---
# <a name="validate-replicated-data"></a>Validar datos replicados
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  En este tema se describe cómo validar datos en el suscriptor en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o Replication Management Objects (RMO).  
  
La replicación transaccional y la replicación de mezcla le permiten validar que los datos del suscriptor coinciden con los del publicador. Es posible realizar la validación de determinadas suscripciones o de todas las suscripciones a una publicación. Especifique uno de los siguientes tipos de validación y el Agente de distribución o el Agente de mezcla validarán los datos la próxima vez que se ejecuten:  
  
-   **Solo de número de filas.** Esta opción valida si la tabla del suscriptor tiene las mismas filas que la tabla del publicador pero no valida la coincidencia del contenido de las filas. La validación del recuento de filas proporciona una idea sobre validación que puede ponerle al corriente de problemas con los datos.   
-   **Recuento de filas y suma de comprobación binaria**. Además de llevar a cabo un recuento de filas en el publicador y en el suscriptor, se calcula una suma de comprobación de todos los datos utilizando el algoritmo de suma de comprobación. Si el número de filas da un error, no se lleva a cabo la suma de comprobación.  
  
 Además de validar que los datos en el suscriptor y en el publicador coincidan, la replicación de mezcla ofrece la posibilidad de validar que los datos presenten las particiones correctas para cada suscriptor. Para más información, vea [VValidar la información de particiones para un suscriptor de mezcla](../../relational-databases/replication/validate-partition-information-for-a-merge-subscriber.md).  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]
   
## <a name="how-data-validation-works"></a>Cómo funciona la validación de datos  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valida los datos calculando un recuento de filas o una suma de comprobación en el Publicador y, a continuación, compara estos valores con el recuento de filas o suma de comprobación calculado en el suscriptor. Se calcula un valor para toda la tabla de publicación y otro valor para toda la tabla de suscripción, pero en los cálculos no se incluyen los datos de las columnas **text**, **ntext** ni **image** .  
  
 Mientras se realizan los cálculos, se colocan bloqueos compartidos temporalmente en las tablas en las que se ejecutan los recuentos de filas y sumas de comprobación, pero los cálculos se completan rápidamente y los bloqueos compartidos se quitan normalmente en unos segundos.  
  
 Cuando se utilizan sumas de comprobación binarias, se produce una comprobación de redundancia cíclica (CRC) de 32 bits columna por columna en vez de una comprobación CRC en la fila física de la página de datos. Esto permite que las columnas de la tabla estén en cualquier orden físicamente en la página de datos, pero se calcula la misma comprobación CRC para la fila. Es posible utilizar la validación de suma de comprobación binaria cuando hay filtros de fila o de columna en la publicación.  

 La validación de datos es un proceso de tres partes:  
  
1.  Una sola suscripción o todas las suscripciones en una publicación se *marcan* para validación. Marque las suscripciones para la validación en los cuadros de diálogo **Validar suscripción**, **Validar suscripciones** y **Validar todas las suscripciones**, que están disponibles en la carpeta **Publicaciones locales** y en la carpeta **Suscripciones locales** en [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. También puede marcar suscripciones desde la pestaña **Todas las suscripciones** , la pestaña **Lista de supervisión de suscripciones** y el nodo de publicaciones del Monitor de replicación. Para información sobre cómo iniciar el Monitor de replicación, vea [Iniciar el Monitor de replicación](../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
2.  Una suscripción se valida la próxima vez que la sincroniza el Agente de distribución (en la replicación transaccional) o el Agente de mezcla (en la replicación de mezcla). El Agente de distribución normalmente se ejecuta de forma continua, en cuyo caso la validación se produce inmediatamente; el Agente de mezcla normalmente se ejecuta a petición, en cuyo caso la validación se produce después de ejecutar el agente.  
  
3.  Vea los resultados de la validación:   
    -   En la ventana de detalles del Monitor de replicación: en la pestaña **Historial de Distribuidor a suscriptor** para la replicación transaccional y en la pestaña **Historial de sincronizaciones** en la replicación de mezcla.    
    -   En el cuadro de diálogo **Ver estado de sincronización** en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
 
## <a name="considerations-and-restrictions"></a>Consideraciones y restricciones  
 Tenga en cuenta las siguientes cuestiones a la hora de validar los datos:  
  
-   Debe detener todas las actividades de actualización en los suscriptores antes de validar los datos (no es necesario detener todas las actividades en el publicador durante la validación).  
-   Dado que las sumas de comprobación y las sumas de comprobación binarias requieren grandes cantidades de recursos del procesador para validar un conjunto de datos de gran tamaño, debe programar la validación para que se produzca cuando la actividad sea mínima en los servidores que se utilizan en la replicación.   
-   La replicación solo valida tablas; no valida si los artículos solo de esquema (como los procedimientos almacenados) son iguales en el publicador y en el suscriptor.   
-   La suma de comprobación binaria se puede utilizar en cualquier tabla publicada. La suma de comprobación no puede validar tablas con filtros de columna ni estructuras de tabla lógicas donde los desplazamientos de columnas son distintos (debido a las instrucciones ALTER TABLE que quitan o agregan columnas).   
-   La validación de replicación usa las funciones **checksum** y **binary_checksum** . Para obtener información sobre este comportamiento, vea [CHECKSUM &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-transact-sql.md) y [BINARY_CHECKSUM  &#40;Transact-SQL&#41;](../../t-sql/functions/binary-checksum-transact-sql.md).   
-   La validación mediante suma de comprobación binaria o suma de comprobación puede informar incorrectamente sobre un error si los tipos de datos son diferentes en el suscriptor y en el publicador. Esto se puede producir si lleva a cabo una de las siguientes acciones:    
    -   Establecer de forma explícita opciones del esquema para asignar tipos de datos de versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
    -   Establecer el nivel de compatibilidad de la publicación de una publicación de combinación en una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cuando las tablas publicadas contienen uno o más tipos de datos que se deben asignados a esta versión.    
    -   Inicializar de forma manual una suscripción, si usa diferentes tipos de datos en el suscriptor.   
-   Las validaciones de suma de comprobación binaria y de suma de comprobación no son compatibles con suscripciones transformables en la replicación transaccional.   
-   La validación no se admite para los datos replicados en suscriptores que no son de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .    
-   Los procedimientos para el Monitor de replicación solamente son para suscripciones de inserción porque las suscripciones de extracción no se pueden sincronizar en el Monitor de replicación. No obstante, en el Monitor de replicación puede marcar una suscripción para su validación y ver los resultados de la validación para las suscripciones de extracción.    
-   Los resultados de la validación indican si la validación se ha realizado correctamente o si ha tenido errores, pero no se especifica en qué filas se produjo el error de validación si se ha producido. Para comparar datos en el publicador y el suscriptor, use la [tablediff Utility](../../tools/tablediff-utility.md). Para más información sobre el uso de esta utilidad con datos replicados, vea [Compare Replicated Tables for Differences &#40;Replication Programming&#41;](../../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md) (Comparar tablas replicadas para buscar diferencias &#40;Programación de la replicación&#41;).  


## <a name="data-validation-results"></a>Resultados de la validación de datos  
 Cuando la validación se ha completado, el Agente de distribución o el Agente de mezcla registran mensajes sobre si ha sido correcta o se han producido errores (la replicación no informa sobre las filas que han dado error). Estos mensajes se pueden ver en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], en el Monitor de replicación y en las tablas del sistema de replicación. En el tema de procedimientos indicado anteriormente se explica cómo ejecutar la validación y ver los resultados.  
  
 Para controlar errores de validación, tenga en cuenta lo siguiente:  
  
-   Configure la alerta de replicación **Replicación: el suscriptor no ha superado la validación de datos** para recibir una notificación del error. Para más información, vea [Configure Predefined Replication Alerts &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/administration/configure-predefined-replication-alerts-sql-server-management-studio.md) (Cómo configurar alertas de replicación predefinidas [SQL Server Management Studio]).  
  
-   ¿Son los errores de validación un problema para su aplicación? Si los errores de validación suponen un problema, actualice manualmente los datos para que se sincronicen o reinicialice la suscripción:  
  
    -   Los datos se pueden actualizar con la [utilidad tablediff](../../tools/tablediff-utility.md). Para más información sobre el uso de esta utilidad, vea [Compare Replicated Tables for Differences &#40;Replication Programming&#41;](../../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md) (Comparar tablas replicadas para buscar diferencias &#40;programación de la replicación&#41;).  
  
    -   Para más información sobre la reinicialización, vea [Reinitialize Subscriptions](../../relational-databases/replication/reinitialize-subscriptions.md) (Reinicializar suscripciones).  

  
  
## <a name="articles-in-transactional-replication"></a>Artículos en la replicación transaccional 

### <a name="using-sql-server-management-studio"></a>Uso de SQL Server Management Studio
  
1.  Conéctese al publicador en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]y luego expanda el nodo del servidor.    
2.  Expanda la carpeta **Replicación** y, a continuación, expanda la carpeta **Publicaciones locales** .    
3.  Haga clic con el botón secundario en la publicación en la que desea validar las suscripciones y, a continuación, haga clic en **Validar suscripciones**.    
4.  En el cuadro de diálogo **Validar suscripciones** , seleccione las suscripciones que desea validar:   
    -   Seleccione **Validar todas las suscripciones de SQL Server**.    
    -   Seleccione **Validar las siguientes suscripciones:** y, a continuación, seleccione una o varias suscripciones.    
5.  Para especificar el tipo de validación que se va a realizar (recuento de filas o recuento de filas y suma de comprobación), haga clic en **Opciones de validación** y, a continuación, especifique las opciones en el cuadro de diálogo **Opciones de validación de suscripciones** .  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]   
7.  Vea los resultados de la validación en el Monitor de replicación o en el cuadro de diálogo **Ver estado de sincronización** . Para cada suscripción:   
    1.  Expanda la publicación, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Ver estado de sincronización**.    
    2.  Si el agente no se está ejecutando, haga clic en **Iniciar** en el cuadro de diálogo **Ver estado de sincronización** . En el cuadro de diálogo se mostrarán mensajes informativos relacionados con la validación.    
     Si no ve ningún mensaje relacionado con la validación, el agente ya ha registrado un mensaje posterior. En este caso, vea los resultados de la validación en el Monitor de replicación. Para obtener más información, vea los procedimientos del Monitor de replicación en este tema.  

### <a name="using-transact-sql"></a>Usar Transact-SQL

#### <a name="all-articles"></a>Todos los artículos 
  
1.  En el publicador de la base de datos de publicaciones, ejecute [sp_publication_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md). Especifique `@publication` y uno de los valores siguientes para `@rowcount_only`:  
  
    -   **1** : solo comprobación del recuento de filas (el valor predeterminado)    
    -   **2** : recuento de filas y suma de comprobación binaria.  
  
    > [!NOTE]  
    >  Cuando se ejecuta [sp_publication_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md), se ejecuta [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md) para todos los artículos de la publicación. Para ejecutar correctamente [sp_publication_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md), debe tener los permisos establecidos en SELECT en todas las columnas de las tablas base publicadas.    
2.  (Opcional) Iniciar el Agente de distribución de cada suscripción si aún no se está ejecutando. Para obtener más información, consulte [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) y [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).    
3.  Compruebe el resultado de la validación en la salida del agente. 
  
#### <a name="single-article"></a>Artículo único  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md). Especifique `@publication`, el nombre del artículo para `@article` y uno de los valores siguientes para `@rowcount_only`:  
  
    -   **1** : solo comprobación del recuento de filas (el valor predeterminado)    
    -   **2** : recuento de filas y suma de comprobación binaria.  
  
    > [!NOTE]  
    >  Para ejecutar correctamente [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md), debe tener los permisos establecidos en SELECT en todas las columnas de la tabla base publicada.  
  
2.  (Opcional) Iniciar el Agente de distribución de cada suscripción si aún no se está ejecutando. Para obtener más información, consulte [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) y [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).    
3.  Compruebe el resultado de la validación en la salida del agente.
  
#### <a name="single-subscriber"></a>Suscriptor único 
  
1.  En el publicador de la base de datos de publicación, abra una transacción explícita mediante [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md).    
2.  En la base de datos de publicación del publicador, ejecute [sp_marksubscriptionvalidation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-marksubscriptionvalidation-transact-sql.md). Especifique la publicación para `@publication`, el nombre del suscriptor para `@subscriber` y el nombre de la base de datos de suscripciones para `@destination_db`.    
3.  (Opcional) Repita el paso 2 para cada suscripción que se está validando.    
4.  En la base de datos de publicación del publicador, ejecute [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md). Especifique `@publication`, el nombre del artículo para `@article` y uno de los valores siguientes para `@rowcount_only`:    
    -   **1** : solo comprobación del recuento de filas (el valor predeterminado)    
    -   **2** : recuento de filas y suma de comprobación binaria.  
  
    > [!NOTE]  
    >  Para ejecutar correctamente [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md), debe tener los permisos establecidos en SELECT en todas las columnas de la tabla base publicada.  
  
5.  En la base de datos de publicación del publicador, confirme la transacción mediante [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md).    
6.  (Opcional) Repita los pasos 1 a 5 para cada artículo que se está validando.   
7.  (Opcional) Inicie el Agente de distribución si aún no se está ejecutando. Para obtener más información, consulte [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) y [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).    
8.  Compruebe el resultado de la validación en la salida del agente. Para más información, consulte [Validate Data at the Subscriber](../../relational-databases/replication/validate-data-at-the-subscriber.md).  

## <a name="all-push-subscriptions-to-a-transactional-publication"></a>Todas las suscripciones de inserción para una publicación transaccional 

### <a name="using-replication-monitor"></a>Uso del Monitor de replicación
  
1.  En el Monitor de replicación, expanda un grupo de publicador en el panel izquierdo y, a continuación, expanda un publicador.   
2.  Haga clic con el botón secundario en la publicación en la que desea validar las suscripciones y, a continuación, haga clic en **Validar suscripciones**.   
3.  En el cuadro de diálogo **Validar suscripciones** , seleccione las suscripciones que desea validar:  
  
    -   Seleccione **Validar todas las suscripciones de SQL Server**.    
    -   Seleccione **Validar las siguientes suscripciones:** y, a continuación, seleccione una o varias suscripciones.    
4.  Para especificar el tipo de validación que se va a realizar (recuento de filas o recuento de filas y suma de comprobación), haga clic en **Opciones de validación** y, a continuación, especifique las opciones en el cuadro de diálogo **Opciones de validación de suscripciones** .    
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
6.  Haga clic en la pestaña **Todas las suscripciones** .  
7.  Vea los resultados de la validación. Para cada suscripción de inserción:    
    1.  Si no se está ejecutando el agente, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Iniciar sincronización**.    
    2.  Haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Ver detalles**.   
    3.  Vea información en la pestaña **Historial de Distribuidor a suscriptor** en el área de texto **Acciones en la sesión seleccionada** .  
  
## <a name="for-a-single-subscription-to-a-merge-publication"></a>Para una única suscripción a una publicación de mezcla

### <a name="using-sql-server-management-studio"></a>Uso de SQL Server Management Studio
  
1.  Conéctese al publicador en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]y luego expanda el nodo del servidor.    
2.  Expanda la carpeta **Replicación** y, a continuación, expanda la carpeta **Publicaciones locales** .   
3.  Expanda la publicación en la que desea validar las suscripciones, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Validar suscripción**.    
4.  En el cuadro de diálogo **Validar suscripción** , seleccione **Validar esta suscripción**.    
5.  Para especificar el tipo de validación que se va a realizar (recuento de filas o recuento de filas y suma de comprobación), haga clic en **Opciones** y, a continuación, especifique las opciones en el cuadro de diálogo **Opciones de validación de suscripciones** .    
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
7.  Vea los resultados de la validación en el Monitor de replicación o en el cuadro de diálogo **Ver estado de sincronización** :  
  
    1.  Expanda la publicación, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Ver estado de sincronización**.    
    2.  Si el agente no se está ejecutando, haga clic en **Iniciar** en el cuadro de diálogo **Ver estado de sincronización** . En el cuadro de diálogo se mostrarán mensajes informativos relacionados con la validación.  
  
     Si no ve ningún mensaje relacionado con la validación, el agente ya ha registrado un mensaje posterior. En este caso, vea los resultados de la validación en el Monitor de replicación. Para obtener más información, vea los procedimientos del Monitor de replicación en este tema.  
  
## <a name="for-all-subscriptions-to-a-merge-publication"></a>Para todas las suscripciones a una publicación de mezcla

### <a name="using-sql-server-management-studio"></a>Uso de SQL Server Management Studio  
1.  Conéctese al publicador en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]y luego expanda el nodo del servidor.    
2.  Expanda la carpeta **Replicación** y, a continuación, expanda la carpeta **Publicaciones locales** .   
3.  Haga clic con el botón secundario en la publicación en la que desea validar las suscripciones y, a continuación, haga clic en **Validar todas las suscripciones**.    
4.  En el cuadro de diálogo **Validar todas las suscripciones** , especifique el tipo de validación que se va a realizar (recuento de filas o recuento de filas y suma de comprobación).   
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
6.  Vea los resultados de la validación en el Monitor de replicación o en el cuadro de diálogo **Ver estado de sincronización** . Para cada suscripción:    
    1.  Expanda la publicación, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Ver estado de sincronización**.    
    2.  Si el agente no se está ejecutando, haga clic en **Iniciar** en el cuadro de diálogo **Ver estado de sincronización** . En el cuadro de diálogo se mostrarán mensajes informativos relacionados con la validación.  
  
     Si no ve ningún mensaje relacionado con la validación, el agente ya ha registrado un mensaje posterior. En este caso, vea los resultados de la validación en el Monitor de replicación. Para obtener más información, vea los procedimientos del Monitor de replicación en este tema.  
  
 
## <a name="for-a-single-push-subscription-to-a-merge-publication"></a>Para una única suscripción de inserción a una publicación de mezcla 

### <a name="using-replication-monitor"></a>Uso del Monitor de replicación  
1.  En el Monitor de replicación, expanda un grupo de publicador en el panel izquierdo, expanda un publicador y, a continuación, haga clic en una publicación.    
2.  Haga clic en la pestaña **Todas las suscripciones** .    
3.  Haga clic con el botón secundario en la suscripción que desea validar y, a continuación, haga clic en **Validar suscripción**.    
4.  En el cuadro de diálogo **Validar suscripción** , seleccione **Validar esta suscripción**.    
5.  Para especificar el tipo de validación que se va a realizar (recuento de filas o recuento de filas y suma de comprobación), haga clic en **Opciones** y, a continuación, especifique las opciones en el cuadro de diálogo **Opciones de validación de suscripciones** .    
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
7.  Haga clic en la pestaña **Todas las suscripciones** .    
8.  Vea los resultados de la validación:    
    1.  Si no se está ejecutando el agente, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Iniciar sincronización**.    
    2.  Haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Ver detalles**.    
    3.  Vea información en la pestaña **Historial de sincronizaciones** en el área de texto **Último mensaje de la sesión seleccionada:** .  

### <a name="using-transact-sql"></a>Usar Transact-SQL
1.  En el publicador de la base de datos de publicaciones, ejecute [sp_validatemergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validatemergesubscription-transact-sql.md). Especifique `@publication`, el nombre del suscriptor para `@subscriber`, el nombre de la base de datos de suscripciones para `@subscriber_db` y uno de los valores siguientes para `@level`:   
    -   **1** : validación solo del recuento de filas.    
    -   **3** : validación de la suma de comprobación binaria del recuento de filas.  
  
     Esto marca la suscripción seleccionada para validación.  
  
2.  Inicie el Agente de mezcla para cada suscripción. Para obtener más información, consulte [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) y [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
3.  Compruebe el resultado de la validación en la salida del agente.   
4.  Repita los pasos 1 a 3 para cada suscripción que se está validando.  
  
> [!NOTE]  
>  Una suscripción a una publicación de combinación también se puede validar al final de una sincronización especificando el parámetro **-Validate** al ejecutar [Replication Merge Agent](../../relational-databases/replication/agents/replication-merge-agent.md).  
  
## <a name="for-all-push-subscriptions-to-a-merge-publication"></a>Para todas las suscripciones de inserción a una publicación de mezcla
  
### <a name="using-replication-monitor"></a>Uso del Monitor de replicación    
1.  En el Monitor de replicación, expanda un grupo de publicador en el panel izquierdo y, a continuación, expanda un publicador.    
2.  Haga clic con el botón secundario en la publicación en la que desea validar las suscripciones y, a continuación, haga clic en **Validar todas las suscripciones**.    
3.  En el cuadro de diálogo **Validar todas las suscripciones** , especifique el tipo de validación que se va a realizar (recuento de filas o recuento de filas y suma de comprobación).    
4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
5.  Haga clic en la pestaña **Todas las suscripciones** .    
6.  Vea los resultados de la validación. Para cada suscripción de inserción:    
    1.  Si no se está ejecutando el agente, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Iniciar sincronización**.    
    2.  Haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Ver detalles**.    
    3.  Vea información en la pestaña **Historial de sincronizaciones** en el área de texto **Último mensaje de la sesión seleccionada:** . 
  
### <a name="using-transact-sql"></a>Usar Transact-SQL
1.  En el publicador de la base de datos de publicaciones, ejecute [sp_validatemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validatemergepublication-transact-sql.md). Especifique `@publication` y uno de los valores siguientes para `@level`:    
    -   **1** : validación solo del recuento de filas.   
    -   **3** : validación de la suma de comprobación binaria del recuento de filas.  
  
     Esto marca todas las suscripciones para validación.   
2.  Inicie el Agente de mezcla para cada suscripción. Para obtener más información, consulte [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) y [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
3.  Compruebe el resultado de la validación en la salida del agente. Para más información, consulte [Validate Data at the Subscriber](../../relational-databases/replication/validate-data-at-the-subscriber.md).  

  
## <a name="validate-data-using-merge-agent-parameters"></a>Validación de datos mediante parámetros del Agente de mezcla  
  
1.  Inicie el Agente de mezcla en el suscriptor (suscripción de extracción) o en el distribuidor (suscripción de inserción) del símbolo del sistema de una de las siguientes maneras.   
    -   Especificando un valor de **1** (número de filas) o **3** (número de filas y suma de comprobación binaria) para el parámetro **-Validate** .    
    -   Especificando la **validación del recuento de filas** o la **validación del recuento de filas y de la suma de comprobación** para el parámetro **- ProfileName** .  
  
     Para obtener más información, consulte [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) o [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Uso de Replication Management Objects (RMO)  
 La replicación permite usar Replication Management Objects (RMO) para validar mediante programación si los datos del suscriptor coinciden con los datos del publicador. Los objetos que se usan dependen del tipo de topología de replicación. La replicación transaccional requiere la validación de todas las suscripciones a una publicación.  
  
> [!NOTE]  
>  Para obtener un ejemplo, vea [Ejemplo (RMO)](#RMOExample)más adelante en esta sección.  
  
#### <a name="to-validate-data-for-all-articles-in-a-transactional-publication"></a>Para validar los datos de todos los artículos de una publicación transaccional  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransPublication>. Establezca las propiedades <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> y <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> de la publicación. Establezca la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en la conexión creada en el paso 1.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades restantes del objeto. Si este método devuelve **false**, significa que las propiedades de publicación del paso 2 se definieron incorrectamente, o bien que la publicación no existe.  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.TransPublication.ValidatePublication%2A> . Pase lo siguiente:  
  
    -   <xref:Microsoft.SqlServer.Replication.ValidationOption>  
  
    -   <xref:Microsoft.SqlServer.Replication.ValidationMethod>  
  
    -   Un valor booleano que indique si se debe detener el Agente de distribución una vez completada la validación.  
  
     De esta forma se marcan los artículos para la validación.  
  
5.  Si no se está ejecutando, inicie el Agente de distribución para sincronizar cada suscripción. Para obtener más información, consulte [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md) o [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md). El resultado de la operación de la validación se escribe en el historial del agente. Para más información, consulte [Monitoring Replication](../../relational-databases/replication/monitor/monitoring-replication.md).  
  
#### <a name="to-validate-data-in-all-subscriptions-to-a-merge-publication"></a>Para validar los datos de todas las suscripciones a una publicación de combinación  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergePublication>. Establezca las propiedades <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> y <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> de la publicación. Establezca la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en la conexión creada en el paso 1.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades restantes del objeto. Si este método devuelve **false**, significa que las propiedades de publicación del paso 2 se definieron incorrectamente, o bien que la publicación no existe.  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.MergePublication.ValidatePublication%2A> . Pase el valor de <xref:Microsoft.SqlServer.Replication.ValidationOption>que desee.  
  
5.  Ejecute el Agente de mezcla en cada suscripción para iniciar la validación o espere hasta la siguiente ejecución programada del agente. Para obtener más información, consulte [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) y [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md). El resultado de la operación de la validación se escribe en el historial del agente, que se puede consultar con el Monitor de replicación. Para más información, consulte [Monitoring Replication](../../relational-databases/replication/monitor/monitoring-replication.md).  
  
#### <a name="to-validate-data-in-a-single-subscription-to-a-merge-publication"></a>Para validar los datos de una única suscripción a una publicación de combinación  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergePublication>. Establezca las propiedades <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> y <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> de la publicación. Establezca la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en la conexión creada en el paso 1.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades restantes del objeto. Si este método devuelve **false**, significa que las propiedades de publicación del paso 2 se definieron incorrectamente, o bien que la publicación no existe.  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.MergePublication.ValidateSubscription%2A> . Pase el nombre del suscriptor y la base de datos de suscripciones que se validan y el valor de <xref:Microsoft.SqlServer.Replication.ValidationOption>deseado.  
  
5.  Ejecute el Agente de mezcla en la suscripción para iniciar la validación o espere hasta la siguiente ejecución programada del agente. Para obtener más información, consulte [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) y [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md). El resultado de la operación de la validación se escribe en el historial del agente, que se puede consultar con el Monitor de replicación. Para más información, consulte [Monitoring Replication](../../relational-databases/replication/monitor/monitoring-replication.md).  
  
###  <a name="example-rmo"></a><a name="RMOExample"></a> Ejemplo (RMO)  
 Este ejemplo marca todas las suscripciones a una publicación transaccional para la validación del recuento de filas.  
  
 [!code-cs[HowTo#rmo_ValidateTranPub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_validatetranpub)]  
  
 [!code-vb[HowTo#rmo_vb_ValidateTranPub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_validatetranpub)]  
  
 Este ejemplo marca una suscripción específica a una publicación de combinación para la validación del recuento de filas.  
  
 [!code-cs[HowTo#rmo_ValidateMergeSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_validatemergesub)]  
  
 [!code-vb[HowTo#rmo_vb_ValidateMergeSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_validatemergesub)]  
  
 ## <a name="see-also"></a>Consulte también  
[Best Practices for Replication Administration](../../relational-databases/replication/administration/best-practices-for-replication-administration.md)  
  
  
