---
title: Administradores
description: 'Obtenga información sobre los tipos de administradores en Master Data Services: administradores de modelos, administradores de entidad y superusuarios.'
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- administrators [Master Data Services], about administrators
- administrators [Master Data Services]
- models [Master Data Services], administrators
ms.assetid: d330aa4e-6ade-4b09-b376-1b15d6c78f7d
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: bfe03a31b5d8f35770f8ded41e4d8fdb86fb9d30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339663"
---
# <a name="administrators-master-data-services"></a>Administradores (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En este artículo se describen los tipos de administradores de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]: administradores de modelos, administradores de entidad y superusuario.  
  
## <a name="model-administrators"></a>Administradores de modelos  
 En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] , un administrador de modelo es un usuario que tiene permiso de **Administrador** asignado al objeto de modelo de nivel superior en la pestaña **objetos de modelo** . Cuando un usuario tiene permiso de administrador en un modelo determinado, cualquier otro permiso en los objetos secundarios del modelo (tanto los permisos de objeto de modelo como de miembro) es superado por el permiso de **Administrador** del modelo y se pasa por alto de forma efectiva.  
  
-   Si el usuario tiene acceso al área funcional del **Explorador** , podrá agregar, eliminar y actualizar todos los datos maestros de esta área.  
  
-   Si el usuario tiene acceso a otras áreas funcionales, puede realizar todas las tareas administrativas disponibles en el área funcional.  
  
 Cada modelo puede tener varios administradores. Cada usuario puede ser administrador de modelo para uno, varios o todos los modelos de la implementación de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
 Un usuario se puede configurar como administrador de modelo en [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] o mediante programación. Para obtener más información, consulte [Crear un administrador de modelo &#40;Master Data Services&#41;](../master-data-services/create-a-model-administrator-master-data-services.md).  
  
## <a name="entity-administrators"></a>Administradores de entidad  
 En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] , un administrador de entidad es un usuario que tiene permisos de administrador asignados al objeto entidad en la pestaña objetos de modelo. Cuando un usuario tiene permisos de administrador para una entidad, cualquier otro permiso en los objetos secundarios de la entidad (tanto los permisos de objeto de modelo como de miembro) es reemplazado por los permisos de administrador y se omite.  
  
-   Si el usuario tiene acceso al área funcional del **Explorador** , podrá agregar, eliminar y actualizar todos los datos maestros de esta área.  
  
-   Si los cambios de entidad exigen aprobación del administrador, un administrador de entidad puede revisar y luego aprobar o rechazar los conjuntos de cambios.  
  
 Cada entidad puede tener varios administradores. Cada usuario puede ser administrador de entidad de una, varias o todas las entidades.  
  
 Un usuario se puede configurar como administrador de entidad en [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] o mediante programación. Para obtener más información, consulte [Creación de un administrador de entidades &#40;Master Data Services&#41;](../master-data-services/create-an-entity-administrator-master-data-services.md).  
  
## <a name="master-data-services-super-user"></a>Superusuario de Master Data Services  
 En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], puede asignar a un usuario permisos para el área funcional de superusuario. Un usuario con permisos al área funcional de superusuario tiene permiso efectivo Admin en todos los modelos y tiene permisos para las demás áreas funcionales. Para obtener información sobre los permisos de las áreas funcionales, consulte [Permisos del área funcional &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
 El superusuario predeterminado se especifica para la **cuenta de administrador** al crear la base de datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] mediante el [Asistente para crear bases de datos &#40;Administrador de configuración de Master Data Services&#41;](../master-data-services/create-database-wizard-master-data-services-configuration-manager.md).  
  
 El superusuario puede hacer lo siguiente:  
  
-   Acceder a todas las áreas funcionales.  
  
-   Agregar, eliminar y actualizar todos los datos maestros de todos los modelos en el área funcional del **Explorador** .  
  
 Puede asignar permisos de superusuario a varios usuarios o grupos de usuarios.  
  
## <a name="comparing-administrator-types"></a>Comparar los tipos de administradores  
  
|Tipo de administrador|Descripción|  
|------------------------|-----------------|  
|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] Superusuario|Los permisos asignados en [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] no tienen ningún efecto sobre el acceso del administrador.<br /><br /> Puede ser un superusuario según los permisos de área funcional que se le asignen explícitamente o los permisos heredados de un grupo.<br /><br /> Tiene automáticamente todos los permisos para todos los modelos.<br /><br /> Tiene acceso a todas las áreas funcionales automáticamente.|  
|Administrador de modelo|Puede ser un administrador de modelo según los permisos de administración que se le asignen explícitamente o los permisos heredados de un grupo.<br /><br /> Solo tiene acceso a áreas funcionales a las que se permita el acceso.<br /><br /> Tiene automáticamente todos los permisos para todos los objetos y miembros en el modelo concreto.|  
|Administrador de la entidad|Puede ser un administrador de entidad según los permisos de administrador que se le asignen explícitamente o los permisos heredados de un grupo.<br /><br /> Solo tiene acceso a áreas funcionales a las que se permita el acceso.<br /><br /> Tiene automáticamente todos los permisos para todos los objetos y miembros en la entidad concreta.<br /><br /> Puede aprobar los conjuntos de cambios pendientes si los cambios de entidad exigen aprobación.|  
  
## <a name="external-resources"></a>Recursos externos  
 Entrada de blog, [Security Improvements](/archive/blogs/e7/improvements-to-autoplay)(Mejoras de seguridad), en msdn.com.  
  
## <a name="see-also"></a>Consulte también  
 [Cree un administrador de modelos &#40;Master Data Services&#41;](../master-data-services/create-a-model-administrator-master-data-services.md)   
 [Crear una base de datos de Master Data Services](../master-data-services/install-windows/create-a-master-data-services-database.md)   
 [Notificaciones &#40;Master Data Services&#41;](../master-data-services/notifications-master-data-services.md)  
  
