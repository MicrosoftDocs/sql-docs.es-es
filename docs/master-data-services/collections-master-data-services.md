---
description: Colecciones (Master Data Services)
title: Colecciones
ms.custom: ''
ms.date: 04/01/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- collections [Master Data Services]
- collections [Master Data Services], about collections
ms.assetid: 5aa1d1e0-b4e5-4897-8e74-01dcf418df73
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 574a99b245764a504297829ba3c037186d88672d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272796"
---
# <a name="collections-master-data-services"></a>Colecciones (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Una colección es un grupo de miembros hoja y consolidados de una única entidad. Use colecciones cuando no necesite una jerarquía completa y desee ver agrupaciones diferentes de los miembros para elaborar informes o análisis o cuando necesite crear una taxonomía.  
  
> [!NOTE]  
>  Colección está en desuso.  
  
## <a name="what-collections-contain"></a>Qué contienen las colecciones  
 Las colecciones no limitan el número o el tipo de miembros que puede incluir, siempre y cuando los miembros pertenezcan a la misma entidad. Una colección puede contener miembros hoja y consolidados de varias jerarquías explícitas obligatorias y no obligatorias.  
  
 Al crear una colección, no está creando una estructura jerárquica; crea una lista plana de miembros. Al seleccionar un nodo de una jerarquía y agregarlo a la colección, el miembro consolidado seleccionado es el único miembro que se agregará a la colección.  
  
 Una colección también puede contener otras colecciones. Puede usar colecciones de colecciones para crear taxonomías.  
  
 Al crear una colección, aparecerá automáticamente como el propietario. Si es administrador, puede crear otros atributos para la colección según sea necesario.  
  
## <a name="subscription-views-for-collections"></a>Vistas de suscripciones para colecciones  
 Hay dos tipos de vistas de suscripciones que muestran colecciones. El formato **Atributos de colección** muestra una lista de colecciones y todos los atributos relacionados con las colecciones (como la descripción o el propietario). El formato **Colecciones** muestra todos los miembros en todas las colecciones, junto con cada ponderación y criterio de ordenación de los miembros. Para obtener más información, consulte [Información general: exportar datos &#40;Master Data Services&#41;](../master-data-services/overview-exporting-data-master-data-services.md).  
  
 Si establece valores de ponderación para los miembros específicos de una colección, estos valores estarán disponibles en las vistas de suscripción relacionadas.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Descripción de la tarea|Tema|  
|----------------------|-----------|  
|Crear una nueva colección.|[Crear una colección &#40;Master Data Services&#41;](../master-data-services/create-a-collection-master-data-services.md)|  
|Agregar miembros a una colección existente.|[Agregar miembros a una colección &#40;Master Data Services&#41;](../master-data-services/add-members-to-a-collection-master-data-services.md)|  
  
## <a name="related-content"></a>Contenido relacionado  
  
-   [Jerarquías explícitas &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md)  
  
-   [Información general: exportar datos &#40;Master Data Services&#41;](../master-data-services/overview-exporting-data-master-data-services.md)  
  
  
