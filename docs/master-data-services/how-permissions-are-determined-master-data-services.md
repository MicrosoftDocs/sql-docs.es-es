---
description: Cómo se determinan los permisos (Master Data Services)
title: Cómo se determinan los permisos
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- permissions [Master Data Services], determining permissions
ms.assetid: 1dc0b43a-d023-4e7d-b027-8b1459fd058c
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 17f9c8a9bc6a871e00df31a2171244984c3d49fb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272516"
---
# <a name="how-permissions-are-determined-master-data-services"></a>Cómo se determinan los permisos (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], la manera más sencilla de configurar la seguridad es asignar permisos del objeto de modelo a un grupo al que pertenece el usuario.  
  
 La seguridad se hace más compleja cuando:  
  
-   Se asignan permisos del objeto de modelo y permisos de los miembros de una jerarquía.  
  
-   El usuario pertenece a grupos y el permiso se asigna al usuario y los grupos.  
  
-   El usuario pertenece a grupos y el permiso se asigna a varios grupos.  
  
## <a name="permissions-assigned-to-a-single-group-or-user"></a>Permisos asignados a un solo grupo o usuario  
 Si asigna permisos a un solo grupo o usuario, los permisos se determinan en función del siguiente flujo de trabajo.  
  
 ![mds_conc_security_no_overlap](../master-data-services/media/mds-conc-security-no-overlap.gif "mds_conc_security_no_overlap")  
  
### <a name="step-1-effective-attribute-permissions-are-determined"></a>Paso 1: se determinan los permisos de atributo efectivos.  
 En la siguiente lista se describe cómo se determinan los permisos de atributo efectivos:  
  
-   Los permisos asignados a objetos de modelo determinan a qué atributos puede tener acceso un usuario.  
  
-   Todos los objetos de modelo heredan automáticamente el permiso del objeto más cercano en un nivel superior de la estructura del modelo.  
  
-   Todos los objetos del mismo nivel que la entidad se deniegan implícitamente.  
  
-   Los objetos de un nivel superior reciben lectura inferida. Para obtener más información sobre la lectura inferida, consulte [Acceso por navegación &#40;Master Data Services&#41;](../master-data-services/navigational-access-master-data-services.md).  
  
 En este ejemplo, el permiso de **lectura** se asigna a una entidad y su atributo, que está en un nivel inferior de la estructura del modelo, lo hereda. El modelo proporciona lectura inferida a esta entidad y su atributo. La otra entidad del modelo no tiene ningún permiso explícito asignado y no hereda ningún permiso, de modo que se deniega implícitamente.  
  
 ![mds_conc_inheritance_model](../master-data-services/media/mds-conc-inheritance-model.gif "mds_conc_inheritance_model")  
  
### <a name="step-2-if-hierarchy-member-permissions-are-assigned-effective-member-permissions-are-determined"></a>Paso 2: si se asignan los permisos de los miembros de una jerarquía, se determinan los permisos de miembros efectivos.  
 En la siguiente lista se describe cómo se determinan los permisos de los miembros de una jerarquía efectivos:  
  
-   Los permisos asignados a los nodos de la jerarquía determinan a qué miembros puede tener acceso un usuario.  
  
-   Todos los nodos de una jerarquía heredan automáticamente el permiso del objeto más cercano en un nivel superior en la estructura de la jerarquía.  
  
-   Todos los nodos del mismo nivel se deniegan implícitamente.  
  
-   Todos los nodos de niveles superiores a los que no se asignan los permisos se deniegan implícitamente.  
  
 En este ejemplo, el permiso de **lectura** se asigna a un nodo de la jerarquía y un nodo de un nivel inferior de la estructura de la jerarquía lo hereda. La raíz no tiene ningún permiso asignado, de modo que se deniega implícitamente. El otro nodo de la estructura de la jerarquía no tiene ningún permiso explícito asignado y no hereda ningún permiso, de modo que se deniega implícitamente.  
  
 ![mds_conc_inheritance_hierarchy](../master-data-services/media/mds-conc-inheritance-hierarchy.gif "mds_conc_inheritance_hierarchy")  
  
### <a name="step-3-the-intersection-of-attribute-and-member-permissions-is-determined"></a>Paso 3: se determina la intersección los permisos de atributo y de miembro.  
 Si los permisos de atributo efectivos son diferentes que los permisos de miembro efectivos, los permisos se deben determinar para cada valor de atributo individual. Para obtener más información, consulte [Superponer permisos de modelo y de miembro &#40;Master Data Services&#41;](../master-data-services/overlapping-model-and-member-permissions-master-data-services.md).  
  
## <a name="permissions-assigned-to-multiple-groups"></a>Permisos asignados a varios grupos  
 Si un usuario pertenece a uno o más grupos y se asignan permisos al usuario y a los grupos, el flujo de trabajo se vuelve más complejo.  
  
 ![mds_conc_security_group_overlap](../master-data-services/media/mds-conc-security-group-overlap.gif "mds_conc_security_group_overlap")  
  
 En este caso, los permisos de usuario y de grupo que se superpongan se deben resolver para poder comparar los permisos del objeto de modelo y del miembro de jerarquía. Para obtener más información, consulte [Superponer permisos de usuario y de grupo &#40;Master Data Services&#41;](../master-data-services/overlapping-user-and-group-permissions-master-data-services.md).  
  
## <a name="see-also"></a>Consulte también  
 [Los permisos de usuario y de grupo superpuestos &#40;Master Data Services&#41;](../master-data-services/overlapping-user-and-group-permissions-master-data-services.md)   
 [Superponer permisos de modelo y de miembro &#40;Master Data Services&#41;](../master-data-services/overlapping-model-and-member-permissions-master-data-services.md)  
  
  
