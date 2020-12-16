---
description: Propiedades de la publicación, Artículos
title: Propiedades de la publicación, Artículos | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newpubwizard.pubproperties.articles.f1
ms.assetid: bdeea318-a153-44b8-9e51-9155f3bad18b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 4dd8faada9a1290595ed03b4b863a75fe02fbec3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473206"
---
# <a name="publication-properties-articles"></a>Propiedades de la publicación, Artículos
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  La página **Artículos** del cuadro de diálogo **Propiedades de la publicación** contiene información acerca de los artículos incluidos en una publicación, le permite agregar artículos a publicaciones existentes o quitarlos de las mismas y le permite cambiar las propiedades de los artículos y los filtros de columnas.  
  
> [!NOTE]  
>  Una vez creada una publicación, algunos cambios de las propiedades requieren una nueva instantánea. Si una publicación tiene suscripciones, algunos cambios también requieren reinicializar todas las suscripciones. Para más información, vea [Change Publication and Article Properties](../../relational-databases/replication/publish/change-publication-and-article-properties.md) (Cambiar las propiedades de la publicación y de los artículos) y [Agregar y quitar artículos de publicaciones existentes](../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md).  
  
 Para publicar un objeto de base de datos que depende de uno o más objetos de base de datos, debe publicar todos los objetos a los que se hace referencia. Por ejemplo, si publica una vista que depende de una tabla, también debe publicar la tabla.  
  
 Los objetos que no se pueden publicar presentan un icono rojo al lado, con una explicación en el panel de información en la parte inferior de la página del asistente. No es posible publicar los siguientes objetos:  
  
-   Objetos cifrados.  
  
-   Vistas indizadas que contienen columnas que permiten valores NULL.  
  
-   Las tablas sin claves principales no se pueden publicar en las publicaciones transaccionales.  
  
-   No es posible publicar tablas en publicaciones de combinación o publicaciones transaccionales habilitadas para suscripciones de actualización en cola. Para más información sobre cómo publicar un artículo en más de una publicación, vea la sección sobre cómo publicar tablas en varias publicaciones en [Publicar datos y objetos de base de datos](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
## <a name="oracle-publishers"></a>Publicadores de Oracle  
 Existen consideraciones adicionales para los publicadores de Oracle:  
  
-   Para obtener una lista de objetos que se pueden publicar desde Oracle, vea [Design Considerations and Limitations for Oracle Publishers](../../relational-databases/replication/non-sql/design-considerations-and-limitations-for-oracle-publishers.md). No se mostrarán los objetos que no se pueden publicar.  
  
-   Para obtener una lista de los tipos de datos que se pueden publicar, vea [Data Type Mapping for Oracle Publishers](../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md). No se mostrarán columnas con tipos de datos que no se pueden publicar.  
  
## <a name="column-filters"></a>Filtros de columnas  
 Para filtrar columnas en esta página, debe expandir una tabla en el panel **Objetos para publicar** y seleccionar solo las columnas que desea filtrar (puede filtrar filas en la página **Filtrar filas de tabla** de este asistente). Filtrar columnas es útil por diversas razones, entre las que se incluye la seguridad (evitar replicaciones de datos confidenciales) y el rendimiento (por ejemplo, evitar la replicación de columnas de objetos binarios grandes (BLOB)). Para más información sobre cómo filtrar columnas, incluida una lista de tipos de columnas que no pueden filtrarse, vea [Filtrar datos publicados](../../relational-databases/replication/publish/filter-published-data.md).  
  
## <a name="options"></a>Opciones  
 El panel **Objetos para publicar** le permite:  
  
-   Ver todos los objetos disponibles para la replicación.  
  
-   Agregar un artículo a una publicación si activa la casilla mostrada junto al objeto.  
  
-   Quitar un objeto de una publicación si desactiva la casilla mostrada junto al objeto. Para obtener información cuándo se pueden quitar artículos, vea [Agregar y quitar artículos de publicaciones existentes](../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md).  
  
-   Incluir todos los objetos de un tipo concreto (como una tabla) en la publicación si activa la casilla mostrada junto al tipo de objeto (como **Tablas**).  
  
-   Expandir los nodos de tabla para ver las columnas en la tabla.  
  
-   Filtrar las columnas de tabla de una publicación si desactiva la casilla mostrada junto a la columna o incluir columnas no publicadas si activa la casilla.  
  
-   Haga clic con el botón secundario en un objeto del panel para ver un menú de comandos para dicho objeto.  
  
 **Propiedades del artículo**  
 Haga clic en **Propiedades del artículo** y, a continuación, haga clic en una de las siguientes opciones:  
  
-   Haga clic en **Establecer propiedades del artículo de \<ObjectType> resaltado** para iniciar el cuadro de diálogo **Propiedades del artículo: \<ObjectName>** ; los cambios de propiedad realizados en este cuadro de diálogo solo se aplican al objeto que está resaltado en el panel de objetos de la página **Artículos**.  
  
-   Haga clic en **Establecer propiedades de todos los artículos de \<ObjectType>** para iniciar el cuadro de diálogo **Propiedades de todos los artículos de \<ObjectType>** . Los cambios de propiedad realizados en este cuadro de diálogo se aplican a todos los objetos de ese tipo en el panel de objetos de la página **Artículos**, incluidos los que todavía no se hayan seleccionado para la publicación.  
  
    > [!NOTE]  
    >  Los cambios de propiedades realizados en el cuadro de diálogo **Propiedades de todos los artículos de \<ObjectType>** reemplazan los que se hicieran anteriormente en el cuadro de diálogo **Propiedades del artículo: \<ObjectName>** . Por ejemplo, si desea establecer varios valores predeterminados para todos los artículos de un tipo de objeto, pero solamente desea establecer algunas propiedades para objetos individuales, establezca primero los valores predeterminados para todos los artículos. A continuación, establezca las propiedades de los objetos individuales.  
  
 **La tabla resaltada es de solo descarga**  
 Solo replicación de mezcla. Solo para [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] y versiones posteriores. Seleccione esta opción para especificar qué cambios no desea permitir en el suscriptor si usa una suscripción de cliente. Los artículos de solo descarga no se pueden actualizar en el suscriptor, y por ello los metadatos de seguimiento no se envían a los suscriptores. Esto puede reducir la capacidad de almacenamiento en los suscriptores y aumentar el rendimiento, sobre todo si la conexión de red es lenta. Esta opción se corresponde con un valor de **Solo descargar en suscriptor y prohibir cambios del suscriptor** para la opción **Dirección de la sincronización** en el cuadro de diálogo **Propiedades del artículo** . Para más información, vea [Optimizar el rendimiento de la replicación de mezcla con artículos de solo descarga](../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md).  
  
 **Mostrar solamente los objetos seleccionados en la lista**  
 Active esta casilla para mostrar solamente aquellos artículos seleccionados en el panel de objetos.  
  
## <a name="see-also"></a>Consulte también  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Ver y modificar propiedades de publicación](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Crear y aplicar la instantánea inicial](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)   
 [Reinicializar una suscripción](../../relational-databases/replication/reinitialize-a-subscription.md)   
 [Publicar datos y objetos de base de datos](../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
