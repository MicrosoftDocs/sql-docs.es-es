---
title: Entidades
description: Las entidades son objetos contenidos en Master Data Services modelos. Cada entidad contiene miembros, que son las filas de datos maestros que administra.
ms.custom: ''
ms.date: 04/01/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- entities [Master Data Services], about entities
- entities [Master Data Services]
ms.assetid: 0af057d5-6b73-472b-99eb-9f5eb61a9b5b
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 2862edbc70fdd2fd4d26ba580a04b18ebe1c9ce3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341777"
---
# <a name="entities-master-data-services"></a>Entidades (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Las entidades son objetos contenidos en modelos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Cada entidad contiene miembros, que son las filas de datos maestros que administra.  
  
## <a name="how-many-entities-are-appropriate"></a>¿Cuántas entidades son adecuadas?  
 Los modelos pueden contener tantas entidades como desee administrar. Cada entidad debe agrupar un tipo similar de datos. Por ejemplo, podría tener una entidad para todas las cuentas corporativas o una entidad para la lista maestra de empleados.  
  
 Normalmente, hay una o más entidades centrales importantes para su negocio con las que se relacionan otros objetos del modelo. Por ejemplo, en un modelo Product, podría tener una entidad central denominada Product y entidades denominadas Subcategory y Category relacionadas con la entidad Product. Sin embargo, no necesita tener una entidad central. Según sus necesidades, puede tener varias entidades que considere igual de importantes.  
  
## <a name="how-entities-relate-to-other-model-objects"></a>Cómo se relacionan las entidades con otros objetos del modelo  
 Una entidad es como una tabla que contiene datos maestros, donde las filas representan miembros y las columnas representan atributos.  
  
 ![Entidad de Master Data Services representada como una tabla](../master-data-services/media/mds-conc-entity-table.gif "Entidad de Master Data Services representada como una tabla")  
  
 Rellene la entidad con una lista de datos maestros que desee administrar.  
  
 Las entidades se pueden usar para crear jerarquías derivadas, que son jerarquías de nivel basadas en varias entidades. Para obtener más información, consulte [Jerarquías derivadas &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md).  
  
 Las entidades también se pueden habilitar para contener jerarquías explícitas (distintas estructuras basadas en una sola entidad) y colecciones (combinaciones únicas de subconjuntos de miembros). Para obtener más información, consulte [Jerarquías explícitas &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md) y [Colecciones &#40;Master Data Services&#41;](../master-data-services/collections-master-data-services.md).  
  
## <a name="using-entities-as-constrained-lists"></a>Usar entidades como listas restringidas  
 Cuando los usuarios están asignando atributos a los miembros de una entidad, puede hacer que elijan en una lista restringida de valores. Para ello, use una entidad para rellenar la lista de valores para el atributo. Esto se denomina un atributo basado en dominio. Para obtener más información, consulte [Atributos basados en dominios &#40;Master Data Services&#41;](../master-data-services/domain-based-attributes-master-data-services.md).  
  
## <a name="base-entities"></a>Entidades base  
 Una entidad base es un punto de partida para los usuarios cuando navegan por los objetos del modelo. Una entidad base determina el diseño de la pantalla cuando un usuario abre el área funcional de **Explorador** y hace clic en **Explorador** en la barra de menús. Para especificar una entidad como entidad base, navegue hasta el área funcional de **Administración del sistema** . En la página **Vista de modelo** , arrastre la entidad del control de árbol hasta la derecha el nombre del modelo en el control de árbol de la izquierda.  
  
## <a name="entity-security"></a>Seguridad de la entidad  
 Puede proporcionar a los usuarios permiso en una entidad, que incluye objetos del modelo relacionados. Para obtener más información, consulte [Permisos de entidad &#40;Master Data Services&#41;](../master-data-services/entity-permissions-master-data-services.md).  
  
## <a name="entity-examples"></a>Ejemplos de entidad  
 En el ejemplo siguiente se muestra una entidad que tiene estos atributos: Name, Code, Subcategory, StandardCost, ListPrice y FilePhoto. Estos atributos describen los miembros. Cada miembro está representado por una fila única de valores de atributo.  
  
 ![Tabla de entidades de producto de bicicleta](../master-data-services/media/mds-conc-entity-table-w-data.gif "Tabla de entidades de producto de bicicleta")  
  
 En el ejemplo siguiente, la entidad Product es la entidad central. La entidad Subcategory es un atributo basado en domino de la entidad Product. La entidad Category es un atributo basado en domino de la entidad Subcategory. StandardCost y ListPrice son atributos de forma libre de la entidad Product y FilePhoto es un atributo de archivo de la entidad Product.  
  
 ![Estructura de árbol de entidad de producto](../master-data-services/media/mds-conc-entity-ui.gif "Estructura de árbol de entidad de producto")  
  
> [!NOTE]  
>  Este es un ejemplo basado en la interfaz de usuario (IU) de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] . La estructura de árbol jerárquica muestra las relaciones entre las entidades y los atributos basados en dominio. El objetivo es mostrar las relaciones más que representar niveles de importancia.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Descripción de la tarea|Tema|  
|----------------------|-----------|  
|Crear una entidad nueva.|[Crear una entidad &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md)|  
|Cambiar el nombre de una entidad existente.|[Edición de una entidad &#40;Master Data Services&#41;](../master-data-services/edit-an-entity-master-data-services.md)|  
|Eliminar una entidad existente.|[Eliminar una entidad &#40;Master Data Services&#41;](../master-data-services/delete-an-entity-master-data-services.md)|  
|Asignar permisos a entidades.|[Asignar permisos de objeto de modelo &#40;Master Data Services&#41;](../master-data-services/assign-model-object-permissions-master-data-services.md)|  
  
## <a name="related-content"></a>Contenido relacionado  
  
-   [Modelos &#40;Master Data Services&#41;](../master-data-services/models-master-data-services.md)  
  
-   [Miembros &#40;Master Data Services&#41;](../master-data-services/members-master-data-services.md)  
  
-   [Atributos &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)  
  
  
