---
description: Permisos del área funcional (Master Data Services)
title: Permisos del área funcional
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- functional area permissions [Master Data Services], about functional area permissions
- functional area permissions [Master Data Services]
- permissions [Master Data Services], functional areas
ms.assetid: a80b87b3-b904-4cda-8582-0761c2617c57
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 45386aaa746522c18f7e3de293ecced923cfa807
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272526"
---
# <a name="functional-area-permissions-master-data-services"></a>Permisos del área funcional (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Puede asignar permisos a cada una de las áreas funcionales de la interfaz de usuario de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] . Las siguientes son las áreas funcionales:  
  
-   **Explorador**  
  
-   **Administración de versiones**  
  
-   **Administración de integraciones**  
  
-   **Administración del sistema**  
  
-   **Permisos de usuario y de grupo**  
  
-   **Superusuario**  
  
 Cuando se asignan permisos a un área funcional, está haciendo que el área de la IU sea visible al usuario o grupo.  
  
 Dentro del área funcional del **Explorador** , los permisos adicionales asignados a los objetos de modelo y a los miembros de la jerarquía determinan a qué datos puede tener acceso un usuario. Dentro de todas las demás áreas funcionales, un usuario debe ser administrador de modelo para ver un modelo y actuar en él. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
> [!IMPORTANT]  
>  Un usuario con permisos de superusuario tiene permiso efectivo de administrador en todos los modelos y tiene todos los demás permisos funcionales.  
  
 Un usuario o grupo deben disponer de permisos para al menos un área funcional y un modelo en la pestaña **Modelos** con el fin de tener acceso a [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)].  
  
## <a name="see-also"></a>Consulte también  
 [Asignar permisos de área funcional &#40;Master Data Services&#41;](../master-data-services/assign-functional-area-permissions-master-data-services.md)   
 [Permisos del objeto de modelo &#40;Master Data Services&#41;](../master-data-services/model-object-permissions-master-data-services.md)   
 [Permisos de miembros de la jerarquía &#40;Master Data Services&#41;](../master-data-services/hierarchy-member-permissions-master-data-services.md)   
 [Cómo se determinan los permisos &#40;Master Data Services&#41;](../master-data-services/how-permissions-are-determined-master-data-services.md)  
  
  
