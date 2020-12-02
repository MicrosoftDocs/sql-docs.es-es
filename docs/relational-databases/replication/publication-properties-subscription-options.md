---
description: Propiedades de la publicación, Opciones de suscripción
title: Propiedades de la publicación, Opciones de suscripción | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newpubwizard.pubproperties.subscriptionoptions.f1
ms.assetid: 31abd605-b273-419d-86df-d0ecf539a507
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: 8637bafe6433893b73e3dc2a7980c43f73fe19d4
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "88420499"
---
# <a name="publication-properties-subscription-options"></a>Propiedades de la publicación, Opciones de suscripción
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  La página **Opciones de suscripción** del cuadro de diálogo **Propiedades de la publicación** permite ver y establecer propiedades a nivel de publicación asociadas con suscripciones. Las propiedades se agrupan en las siguientes categorías:  
  
-   Propiedades que se aplican a todas las publicaciones.  
  
-   Propiedades que se aplican a las publicaciones transaccionales y de instantáneas (incluidas las que permiten suscripciones de actualización).  
  
-   Propiedades que se aplican a las publicaciones de combinación.  
  
> [!NOTE]  
>  Algunas propiedades son de solo lectura; los motivos se tratan en las descripciones de propiedad de este tema. Algunos cambios de propiedad requieren una nueva instancia para la publicación, y otros también requieren la reinicialización de todas las suscripciones. Para obtener más información, vea [Cambiar las propiedades de la publicación y de los artículos](../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
## <a name="options-for-all-publications"></a>Opciones para todas las publicaciones  
  
### <a name="creation-and-synchronization"></a>Creación y sincronización  
 **Permitir suscripciones anónimas**  
 Determina si se permiten suscripciones de extracción anónimas. Se admiten suscripciones anónimas para [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssEWEd2005](../../includes/ssewed2005-md.md)], [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssMobileEd2005](../../includes/ssmobileed2005-md.md)] y [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para Windows CE. Para utilizar esta opción en publicaciones transaccionales y de instantáneas, la opción **Instantánea siempre visible** debe establecerse en **True**.  
  
 **Base de datos de suscripciones adjuntables**  
 Determina si se pueden crear suscripciones adjuntando una copia de una base de datos de suscripciones (requiere que la opción **Instantánea siempre visible** esté establecida en **True** en las publicaciones transaccionales y de instantáneas).  
  
> [!IMPORTANT]  
>  Las suscripciones adjuntables no estarán disponibles en futuras versiones. La característica ha quedado desusada.  
  
 **Permitir suscripciones de extracción**  
 Determina si se permite a los suscriptores crear suscripciones de extracción en esta publicación. Para obtener más información, vea [Suscribirse a publicaciones](../../relational-databases/replication/subscribe-to-publications.md).  
  
### <a name="schema-replication"></a>Replicación de esquemas  
 **Replicación de cambios de esquema**  
 Solo para [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] y versiones posteriores. Determina si se replican cambios de esquema (como agregar una columna a una tabla o cambiar el tipo de datos de una columna) en objetos publicados. Para más información, vea [Realizar cambios de esquema en bases de datos de publicaciones](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
## <a name="options-for-snapshot-and-transactional-publications"></a>Opciones de publicaciones transaccionales y de instantáneas  
  
### <a name="creation-and-synchronization"></a>Creación y sincronización  
 **Agente de distribución independiente**  
 Determina si se utiliza un agente que es independiente de otras publicaciones desde esta base de datos. Esta opción es de solo lectura; de forma predeterminada se establece en **True** para publicaciones creadas con el Asistente para nueva publicación y no se puede cambiar después de crear la publicación. Para obtener más información, vea [Administración del Agente de replicación](../../relational-databases/replication/agents/replication-agent-administration.md).  
  
 **Instantánea siempre visible**  
 Determina si los archivos de instantánea se crean cada vez que se ejecuta el Agente de instantáneas (requiere **Agente de distribución independiente**). Esta opción es de solo lectura; se establece en **True** si se selecciona **Crear una instantánea inmediatamente y mantenerla disponible para inicializar suscripciones** en la página **Agente de instantáneas** del Asistente para nueva publicación (es el valor predeterminado). Para obtener más información, vea [Crear y aplicar una instantánea](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
 **Permitir inicialización desde archivos de copia de seguridad**  
 Solo para[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] y versiones posteriores. Determina si se permite utilizar archivos de copia de seguridad para inicializar suscripciones. Para obtener más información, consulte [Initialize a Transactional Subscription Without a Snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
 **Permitir suscriptores que no sean de SQL**  
 Solo para[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] y versiones posteriores. Determina si la publicación admite suscriptores que no sean de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si esta opción se establece en **True**, se configuran otras propiedades de la publicación para admitir suscriptores que no sean de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta opción es de solo lectura si existen suscripciones; no se puede establecer en **True** si las opciones **Permitir suscripciones de actualización inmediata**, **Permitir suscripciones de actualización en cola** o **Permitir suscripciones punto a punto** están establecidas en **True**. Para más información, consulte [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md).  
  
### <a name="data-transformation"></a>Transformación de datos  
 **Permitir transformaciones de datos**  
 Determina si se deben utilizar los Servicios de transformación de datos (DTS) para transformar los datos antes de distribuirlos a un suscriptor. Esta opción es de solo lectura; las transformaciones de datos solo se pueden habilitar si una publicación se crea utilizando procedimientos almacenados.  
  
> [!IMPORTANT]  
>  Las suscripciones de transformación no estarán disponibles en futuras versiones. La característica ha quedado desusada.  
  
### <a name="peer-to-peer-replication"></a>Replicación punto a punto  
 **Permitir suscripciones punto a punto**  
 Se aplica solamente a [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] y versiones posteriores. Determina si la publicación admite la replicación punto a punto. Si se establece esta opción en **True** , se configuran otras propiedades de la publicación para admitir la replicación punto a punto. Esta opción es de solo lectura si existen suscripciones. Esta opción no se puede establecer en **True** si las opciones **Permitir suscripciones de actualización inmediata** , **Permitir suscripciones de actualización en cola** o **Permitir suscriptores que no sean de SQL** están establecidas en **True**. Para obtener más información, consulte [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
 **Permitir detección de conflictos de replicación punto a punto**  
 Se aplica solamente a [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores. Especifica si la detección de conflictos está habilitada para esta publicación. Para utilizar la detección de conflictos, todos los nodos deben estar ejecutando [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o una versión posterior, y debe haberse habilitado la detección para todos los nodos. Para utilizar la detección de conflictos también debe especificar un valor para **Identificador del autor del mismo nivel**. Para más información, consulte [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
 **Identificador del autor del mismo nivel**  
 Se aplica solamente a [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores. Especifica un Id. para un nodo en una topología punto a punto. Este Id. se utiliza para la detección de conflictos si **Permitir detección de conflictos de replicación punto a punto** se establece en **True**. Especifique un id. positivo distinto de cero que no se haya utilizado jamás en la topología. Para obtener una lista de identificadores que ya se hayan utilizado, consulte la tabla del sistema [Mspeer_originatorid_history](../../relational-databases/system-tables/mspeer-originatorid-history-transact-sql.md) .  
  
### <a name="updatable-subscriptions"></a>Suscripciones actualizables  
 **Permitir suscripciones de actualización inmediata**  
 Determina si los cambios de datos del suscriptor se pueden replicar inmediatamente en el publicador. Esta opción es de solo lectura; las suscripciones de actualización solo se pueden habilitar cuando se crea la publicación. Para más información, consulte [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
 **Permitir suscripciones de actualización en cola**  
 Determina si los cambios de datos del suscriptor se pueden poner en cola y replicar más tarde en el publicador. Esta opción es de solo lectura; las suscripciones de actualización solo se pueden habilitar cuando se crea la publicación. Para más información, consulte [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
 **Informar de conflictos centralmente**  
 Determina si se notifican los cambios de datos conflictivos solo al publicador o al publicador y al suscriptor (requiere la opción **Permitir suscripciones de actualización en cola**). Esta opción es de solo lectura; de forma predeterminada se establece en **True** para publicaciones creadas con el Asistente para nueva publicación y no se puede cambiar después de crear la publicación. El valor **True** significa que los conflictos se notifican solo al publicador. Los conflictos solo pueden verse donde se notifican.  
  
 **Directiva de resolución de conflictos**  
 Especifica la acción que debe llevarse a cabo cuando un cambio de suscriptor entra en conflicto con un cambio de publicador (requiere la opción **Permitir suscripciones de actualización en cola**). Para más información, consulte [Queued Updating Conflict Detection and Resolution](../../relational-databases/replication/transactional/updatable-subscriptions-queued-updating-conflict-resolution.md).  
  
 **Tipo de cola**  
 Determina si se utiliza una cola de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queue Server (MSMQ) para poner en cola los cambios en el suscriptor hasta que puedan aplicarse al publicador (requiere la opción **Permitir suscripciones de actualización en cola**). Esta opción solo es relevante para [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]; las versiones posteriores siempre utilizan tablas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para las colas.  
  
## <a name="options-for-merge-publications"></a>Opciones para publicaciones de combinación  
  
### <a name="conflict-reporting"></a>Informes de conflictos  
 **Informar de conflictos centralmente**  
 Determina si se notifican los cambios de datos conflictivos solo en el publicador o en el publicador y el suscriptor. Esta opción es de solo lectura; de forma predeterminada se establece en **True** para publicaciones creadas con el Asistente para nueva publicación y no se puede cambiar después de crear la publicación. El valor **True** significa que los conflictos se notifican solo al publicador. Los conflictos solo pueden verse donde se notifican. Para obtener más información, vea la sección "Ver conflictos" de [Advanced Merge Replication Conflict Detection and Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md).  
  
### <a name="filtering"></a>Filtros  
 **Permitir filtros con parámetros**  
 Se establece en función de si una publicación utiliza filtros con parámetros. Esta opción siempre es de solo lectura. Para obtener más información, consulte [Filtros de fila con parámetros](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 **Validar suscriptores**  
 Determina qué funciones se utilizan al validar si un suscriptor tiene la partición correcta de datos. Debe utilizarse una coma para separar múltiples valores. Para más información, vea [VValidar la información de particiones para un suscriptor de mezcla](../../relational-databases/replication/validate-partition-information-for-a-merge-subscriber.md).  
  
 **Calcular particiones previamente**  
 Solo para[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] y versiones posteriores. Determina si se debe optimizar la sincronización calculando previamente las filas de datos que pertenecen a cada partición. Tiene como valor predeterminado **True** si la publicación cumple los criterios de las particiones calculadas previamente. Para obtener más información, vea [Optimización del rendimiento de los filtros con parámetros con particiones calculadas previamente](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
 **Optimizar sincronización**  
 Determina si se optimiza el proceso de mezcla almacenando metadatos adicionales en cada suscriptor. Esta optimización se ha sustituido con las particiones calculadas previamente; la opción **Optimizar sincronización** solo es relevante si la opción **Calcular particiones previamente** está establecida en **False**. Para obtener más información, consulte [Filtros de fila con parámetros](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
### <a name="merge-processes"></a>Procesos de mezcla  
 **Limitar procesos simultáneos**  
 Determina si se limita el número de Agentes de mezcla que se pueden ejecutar al mismo tiempo. Normalmente se utiliza si una publicación tiene un gran número de suscripciones de inserción que pueden sincronizarse simultáneamente.  
  
 **Máximo de procesos simultáneos**  
 Indica el número máximo de Agentes de mezcla que se pueden ejecutar simultáneamente (requiere **Limitar procesos simultáneos**). Si el número de agentes que se están sincronizando supera el máximo, los agentes se ponen en cola hasta que el número es inferior al máximo.  
  
## <a name="see-also"></a>Consulte también  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Ver y modificar propiedades de publicación](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Publicar datos y objetos de base de datos](../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
