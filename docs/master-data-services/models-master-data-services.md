---
title: Modelos
description: Un modelo define la estructura de datos de la solución de administración de datos maestros. Los modelos son el nivel más alto de organización de datos en Master Data Services.
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- models [Master Data Services], about models
- models [Master Data Services]
ms.assetid: 9f862a3d-25ab-41e9-b833-1db99959e825
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 93542a994aacb3d82be79c5a0b18daf5ea61fd09
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340342"
---
# <a name="models-master-data-services"></a>Modelos (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Los modelos son el nivel superior de organización de datos en [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]. Un modelo define la estructura de datos de la solución de administración de datos maestros. Un modelo contiene los siguientes objetos:  
  
-   Entidades  
  
-   Atributos y grupos de atributos  
  
-   Jerarquías derivadas y explícitas  
  
-   Colecciones  
  
 Los modelos organizan la estructura de los datos maestros. Su implementación de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] puede tener uno o varios modelos que agrupan tipos de datos similares. En general, los datos maestros se pueden clasificar en una de estas cuatro categorías: personas, lugares, cosas o conceptos. Por ejemplo, puede crear un modelo Producto que vaya a contener datos relacionados con productos o un modelo Cliente para que contenga datos relacionados con clientes.  
  
 Puede asignar a usuarios y grupos el permiso para ver y actualizar los objetos dentro del modelo. Si no concede permisos al modelo, no se mostrará.  
  
 En un momento determinado, podrá crear copias de los datos maestros dentro de un modelo. Estas copias se denominan versiones.  
  
 Una vez haya definido un modelo en un entorno de prueba, puede implementarlo, con o sin los datos correspondientes, desde el entorno de prueba hasta un entorno de producción. Esto elimina la necesidad de volver a crear los modelos en el entorno de producción.  
  
## <a name="how-models-relate-to-other-objects"></a>Cómo se relacionan los modelos con otros objetos  
 Un modelo contiene entidades. Las entidades contienen atributos, jerarquías explícitas y colecciones. Los atributos pueden estar contenidos en grupos de atributos. Los atributos basados en dominios existen cuando se usa una entidad como atributo para otra entidad.  
  
 Esta imagen muestra las relaciones existentes entre los objetos de un modelo.  
  
 ![Objetos de un modelo de Master Data Services](../master-data-services/media/mds-conc-model-circles.gif "Objetos de un modelo de Master Data Services")  
  
> [!NOTE]  
>  Las jerarquías derivadas también son objetos de modelo, pero no se muestran en la imagen. Las jerarquías derivadas se derivan de las relaciones de atributo basadas en dominios que existen entre las entidades. Vea [Jerarquías derivadas &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md) para más información.  
  
 Los datos maestros son los datos contenidos en los objetos de modelo. En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], los datos maestros se almacenan como miembros de una entidad.  
  
 Los objetos de modelo se mantienen en el área funcional de **Administración del sistema** de la interfaz de usuario de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] .  
  
## <a name="model-example"></a>Ejemplo de modelo  
 En el ejemplo siguiente, los objetos del modelo Product agrupan lógicamente los datos relacionados con los productos.  
  
 ![Ejemplo de datos maestros de modelo de producto](../master-data-services/media/mds-conc-model.gif "Ejemplo de datos maestros de modelo de producto")  
  
 Otros modelos comunes son:  
  
-   Accounts: podrían incluir entidades como cuentas de balance, cuentas de balance de resultados, estadísticas y tipos de cuenta.  
  
-   Customer: podría incluir entidades como género, educación, ocupación y estado civil.  
  
-   Geography: podría incluir entidades como códigos postales, ciudades, municipios, estados, provincias, regiones, territorios, países y continentes.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Descripción de la tarea|Tema|  
|----------------------|-----------|  
|Crear un modelo para organizar los datos maestros.|[Crear un modelo &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md)|  
|Cambiar el nombre de un modelo existente.|[Editar modelo &#40;Master Data Services&#41;](../master-data-services/edit-model-master-data-services.md)|  
|Eliminar un modelo existente.|[Eliminar un modelo &#40;Master Data Services&#41;](../master-data-services/delete-a-model-master-data-services.md)|  
  
## <a name="related-content"></a>Contenido relacionado  
  
-   [Introducción a Master Data Services &#40;MDS&#41;](../master-data-services/master-data-services-overview-mds.md)  
  
-   [Entidades &#40;Master Data Services&#41;](../master-data-services/entities-master-data-services.md)  
  
-   [Atributos &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)  
  
-   [Implementar modelos &#40;Master Data Services&#41;](../master-data-services/deploying-models-master-data-services.md)  
  
-   [Permisos de objeto del modelo &#40;Master Data Services&#41;](../master-data-services/model-object-permissions-master-data-services.md)  
  
  
