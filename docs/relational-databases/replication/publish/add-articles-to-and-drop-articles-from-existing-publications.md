---
title: Adición o eliminación de artículos (publicación existente)
description: Obtenga información sobre cómo agregar y quitar artículos de publicaciones existentes en SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- articles [SQL Server replication], dropping
- deleting articles
- removing articles
- dropping articles
- adding articles
- administering replication, articles
- publications [SQL Server replication], adding and dropping articles
- articles [SQL Server replication], adding
ms.assetid: b148e907-e1f2-483b-bdb2-59ea596efceb
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 7c3f54328d1c614f04f5f9fb9d2571d78f1b8733
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236642"
---
# <a name="add-articles-to-and-drop-articles-from-existing-publications"></a>Agregar y quitar artículos de publicaciones existentes
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Después de crear una publicación, se le pueden agregar y quitar artículos. Se pueden agregar artículos en cualquier momento, pero las acciones necesarias para quitar artículos dependen del tipo de replicación y del momento en que se quite el artículo.  
  
## <a name="adding-articles"></a>agregar artículos  
 Para agregar un artículo, es necesario agregar el artículo a la publicación, crear una instantánea nueva para la publicación y sincronizar la suscripción para aplicar el esquema y los datos para el nuevo artículo.  
  
> [!NOTE]
>  Si se agrega un artículo a una publicación de combinación y ya hay un artículo que dependa de este nuevo artículo, debe especificar un orden de procesamiento para ambos artículos con el parámetro **\@processing_order** de [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) y [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Considere el caso siguiente: publica una tabla pero no publica una función a la que hace referencia la tabla. Si no publica la función, la tabla no se puede crear en el suscriptor. Al agregar la función a la publicación: especifique el valor **1** para el parámetro **\@processing_order** de **sp_addmergearticle** y el valor **2** para el parámetro **\@processing_order** de **sp_changemergearticle**; asimismo, debe especificar el nombre de la tabla para el parámetro **\@article**. Este orden de procesamiento garantiza que la función se cree en el suscriptor antes que la tabla que depende de él. Puede usar números distintos para cada artículo, siempre que el número de la función sea inferior al de la tabla.  
  
1.  Agregue uno o más artículos con uno de estos métodos:  
  
    -   [Agregar y quitar artículos de una publicación &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-a-publication.md)  
  
    -   [Definir un artículo](../../../relational-databases/replication/publish/define-an-article.md)  
  
2.  Después de agregar un artículo a una publicación, debe crear una nueva instantánea para la publicación (y todas las particiones, si se trata de una publicación de combinación con filtros con parámetros). A continuación, el Agente de distribución o de mezcla copia el esquema y los datos del nuevo artículo al suscriptor (sin reinicializar la publicación completa).  
  
    -   Para crear una instantánea nueva, vea [Create and Apply the Initial Snapshot](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
    -   Para crear una instantánea para una publicación de mezcla con filtros con parámetros, consulte [Crear una instantánea para una publicación de mezcla con filtros con parámetros](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
3.  Una vez creada la instantánea, sincronice la suscripción para copiar el esquema y los datos para el nuevo artículo.  

    -   Para sincronizar una suscripción de inserción, vea [Synchronize a Push Subscription](../../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
    -   Para sincronizar una suscripción de extracción, vea [Synchronize a Pull Subscription](../../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
## <a name="dropping-articles"></a>quitar artículos  
 Puede quitar artículos de una publicación en cualquier momento, pero debe tener en cuenta los siguientes comportamientos:  
  
-   Al quitar un artículo de una publicación no se quita el objeto de la base de datos de publicaciones ni el objeto correspondiente de la base de datos de suscripciones. Use DROP \<Object> para quitar estos objetos, si es necesario. Al quitar un artículo relacionado con otros artículos publicados a través de restricciones de clave externa, se recomienda quitar la tabla del Suscriptor de forma manual o, con la ejecución de un script a petición, especifique un script que incluya las instrucciones DROP \<Object> apropiadas. Para más información, vea [Execute Scripts During Synchronization &#40;Replication Transact-SQL Programming&#41;](../../../relational-databases/replication/execute-scripts-during-synchronization-replication-transact-sql-programming.md) (Ejecutar scripts durante la sincronización (programación de la replicación con Transact-SQL)).  
  
-   En el caso de las publicaciones de combinación con un nivel de compatibilidad igual o superior a 90RTM, puede quitar artículos en cualquier momento, pero necesitará una instantánea nueva. Además:  
  
    -   Si un artículo es el artículo primario de un filtro de combinación o de una relación de registros lógicos, necesitará quitar primero las relaciones; para esto, es necesario reinicializar.  
  
    -   Si un artículo tiene el último filtro con parámetros de una publicación, será necesario reinicializar las suscripciones.  
  
-   En el caso de las publicaciones de combinación con un nivel de compatibilidad inferior a 90RTM, puede quitar artículos sin ningún tipo de consideraciones especiales antes de la sincronización inicial de las suscripciones. Si quita un artículo después de haber sincronizado una o varias suscripciones, deberá quitar, volver a crear y sincronizar estas suscripciones.  
  
-   En las publicaciones de instantáneas o transaccionales, se pueden quitar artículos sin ninguna consideración especial antes de crear las suscripciones. Si quita un artículo después de haber creado una o más suscripciones, deberá quitar, volver a crear y sincronizar estas suscripciones. Para obtener más información sobre cómo quitar suscripciones, consulte [Suscribirse a publicaciones](../../../relational-databases/replication/subscribe-to-publications.md) y [sp_dropsubscription &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md). **sp_dropsubscription** permite quitar un único artículo de la suscripción, en lugar de toda la suscripción.  
  
1.  Para quitar un artículo de una publicación, hay que quitar el artículo y crear una instantánea nueva para la publicación. Al quitar un artículo se invalida la instantánea actual; por lo tanto, es necesario crear una instantánea nueva.  
  
    -   Para quitar un artículo de una publicación, consulte [Agregar y quitar artículos de una publicación &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-a-publication.md) o [Eliminar un artículo](../../../relational-databases/replication/publish/delete-an-article.md).  
  
2.  Después de quitar un artículo de una publicación, debe crear una nueva instantánea para la publicación (y todas las particiones, si se trata de una publicación de combinación con filtros con parámetros).  
  
    -   Para crear una instantánea nueva, vea [Create and Apply the Initial Snapshot](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
    -   Para crear una instantánea para una publicación de mezcla con filtros con parámetros, consulte [Crear una instantánea para una publicación de mezcla con filtros con parámetros](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
 Como se indicó antes, en algunos casos es necesario quitar, volver a crear y sincronizar las suscripciones después de quitar un artículo. Para obtener más información, consulte [Suscribirse a publicaciones](../../../relational-databases/replication/subscribe-to-publications.md) y [Sincronizar datos](../../../relational-databases/replication/synchronize-data.md).  
 
 > [!NOTE]
 > **[!INCLUDE[sssql14-md](../../../includes/sssql14-md.md)] Service Pack 2** o versiones posteriores y **[!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] Service Pack 1** o versiones posteriores permiten eliminar una tabla con el comando **DROP TABLE** DDL para artículos que participan en la replicación transaccional. Si un DROP TABLE DDL es compatible con las publicaciones, la operación DROP TABLE quitará la tabla de la publicación y la base de datos. El agente de registro del LOG enviará un comando de limpieza de la base de datos de distribución de la tabla quitada y realizará la limpieza de los metadatos del publicador. Si el registro del LOG no ha procesado todas las entradas del registro que hacen referencia a la tabla quitada, omitirá los comandos nuevos que estén asociados a la tabla quitada. Los registros ya procesados se entregarán a la base de datos de distribución. Se pueden aplicar a la base de datos del suscriptor si el agente de distribución los procesa antes de que el registro del LOG limpie los artículos obsoletos (quitados). La configuración **predeterminada** para todas las publicaciones de replicación transaccional es no admitir DROP TABLE DDL. En [KB 3170123](https://support.microsoft.com/help/3170123/supports-drop-table-ddl-for-articles-that-are-included-in-transactional-replication-in-sql-server-2014-or-in-sql-server-2016-sp1) se incluyen más detalles sobre esta mejora.

  
## <a name="see-also"></a>Consulte también  
 [Publicar datos y objetos de base de datos](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Reinicializar suscripciones](../../../relational-databases/replication/reinitialize-subscriptions.md)   
 [Realizar cambios de esquema en bases de datos de publicaciones](../../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)  
  
  
