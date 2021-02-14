---
description: Permisos de modelo (Master Data Services)
title: Permisos de modelo
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- models [Master Data Services], permissions
- permissions [Master Data Services], models
ms.assetid: 210f440b-2cc1-4c49-94b1-3a97e2af7bc3
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 029a3f624dea71567f34a26cdd7093cc96be36d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340336"
---
# <a name="model-permissions-master-data-services"></a>Permisos de modelo (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Los permisos de modelo se aplican a todas las entidades, jerarquías derivadas, jerarquías explícitas y colecciones dentro del modelo. Los permisos asignados al modelo se pueden invalidar con respecto a cualquier objeto individual.  
  
> [!NOTE]  
>  Si un usuario es administrador de un modelo, el modelo se muestra en todas las áreas funcionales de la interfaz de usuario. De lo contrario, el modelo solo se mostrará en el área funcional del **Explorador** . Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
|Permiso|Descripción|  
|----------------|-----------------|  
|**Lectura**|El usuario puede leer miembros, atributos, pertenencias a la jerarquía o pertenencias a la colección.|  
|**Crear**|El usuario puede crear miembros hoja y asignar valores de atributo durante la creación.|  
|**Actualizar**|El usuario puede actualizar miembros, atributos, pertenencias a la jerarquía o pertenencias a la colección.|  
|**Eliminar**|El usuario puede eliminar miembros|  
|**Deny**|El usuario puede denegar todo el acceso al modelo.|  
|**Administrador**|Permisos de administrador en el modelo. El permiso de administrador solo está disponible en el nivel de modelo.|  
  
 Los permisos de lectura, creación, actualización y eliminación se pueden combinar. Cuando se asignan permisos de Crear, Actualizar y Eliminar, el permiso de lectura se asigna automáticamente.  
  
## <a name="see-also"></a>Consulte también  
 [Asignar permisos de objeto de modelo &#40;Master Data Services&#41;](../master-data-services/assign-model-object-permissions-master-data-services.md)   
 [Permisos del objeto de modelo &#40;Master Data Services&#41;](../master-data-services/model-object-permissions-master-data-services.md)   
 [Permisos de entidad &#40;Master Data Services&#41;](../master-data-services/entity-permissions-master-data-services.md)   
 [Permisos de colección &#40;Master Data Services&#41;](../master-data-services/collection-permissions-master-data-services.md)  
  
  
