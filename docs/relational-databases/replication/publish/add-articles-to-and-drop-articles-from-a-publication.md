---
title: Adición y eliminación de artículos de publicación (SSMS)
description: Describe cómo agregar y quitar artículos de una publicación mediante SQL Server Management Studio (SSMS).
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
- dropping articles
- adding articles
- articles [SQL Server replication], adding
ms.assetid: d5a3e536-62d2-4473-a178-877ba52f7d7f
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 03440561eb766779309a868ac5caf7c49a885f73
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482996"
---
# <a name="add-articles-to-and-drop-articles-from-a-publication"></a>Agregar y quitar artículos de una publicación
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Agregue artículos a una publicación al principio, cuando la cree en el Asistente para nueva publicación. Para obtener más información sobre cómo usar este asistente, vea [Crear una publicación](../../../relational-databases/replication/publish/create-a-publication.md).  
  
 Después de crear una publicación, agregue y elimine artículos en la página **Artículos** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** . Para obtener más información sobre el acceso a este cuadro de diálogo, vea [View and Modify Publication Properties](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md). Para obtener información sobre las consideraciones para agregar y quitar artículos, vea [Agregar y quitar artículos de publicaciones existentes](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md).  
  
### <a name="to-add-an-article-after-a-publication-is-created"></a>Para agregar un artículo una vez creada la publicación  
  
1.  En la página **Artículos** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** , desactive la casilla **Mostrar solo los objetos seleccionados en la lista**. De este modo podrá ver los objetos no publicados en la base de datos de publicaciones.  
  
2.  Active la casilla situada junto a cada artículo que desee agregar.  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
### <a name="to-delete-an-article"></a>Para eliminar un artículo  
  
1.  En la página **Artículos** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** , desactive la casilla situada junto a cada artículo que quiera eliminar.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
## <a name="see-also"></a>Consulte también  
 [Define an Article](../../../relational-databases/replication/publish/define-an-article.md)   
 [Publicar datos y objetos de base de datos](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
