---
title: Usuarios y grupos
description: Averigüe cómo conceder acceso a la aplicación Web de Master Data Manager. Un usuario debe tener una cuenta adecuada.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- users [Master Data Services]
- groups [Master Data Services]
- users [Master Data Services], about users
- groups [Master Data Services], about groups
ms.assetid: ed08dd2d-248e-4b68-91d4-e9961cb50eed
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 580b4f7f098f65391f88cb2e39b57642cbc0eafa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336197"
---
# <a name="users-and-groups-master-data-services"></a>Usuarios y grupos (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Para tener acceso a la aplicación web de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] un usuario debe tener una cuenta de dominio de Windows o una cuenta en el equipo servidor donde se instale [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Para conceder acceso a [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] puede:  
  
-   Agregar la cuenta de usuario a un dominio o grupo local y, a continuación, agregar el grupo a la lista de grupos de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)].  
  
-   Agregar la cuenta de usuario a la lista de usuarios de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)].  
  
    > [!NOTE]  
    >  Cuando un usuario pertenece a un grupo que tiene acceso a [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], el nombre del usuario se agrega de forma automática a la lista de usuarios la primera vez que el usuario obtiene acceso a [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] o al [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)]MDS.  
  
 Para realizar una acción en el área de funcionalidad del **Explorador** de la IU, se debe asignar al grupo o usuario acceso al área funcional del **Explorador** y permiso a los objetos del modelo.  
  
 Si un usuario o grupo necesita tener acceso a otras áreas funcionales, se debe asignar al usuario o grupo acceso al área funcional específica.  
  
## <a name="best-practice"></a>Procedimiento recomendado  
 Para simplificar la administración, cree grupos y asigne a cada uno permiso a las áreas funcionales y a los objetos de modelo. Puede agregar usuarios a los grupos y quitarlos a continuación sin tener acceso a la interfaz de usuario de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] .  
  
 No asigne ningún permiso adicional a ningún usuario individual y no incluya usuarios en varios grupos que tengan acceso a [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]. Además, no use los permisos de miembros de la jerarquía a menos que desee que un grupo tenga limitado el acceso a miembros concretos.  
  
## <a name="see-also"></a>Consulte también  
 [Agregar un usuario &#40;Master Data Services&#41;](../master-data-services/add-a-user-master-data-services.md)   
 [Agregar un grupo &#40;Master Data Services&#41;](../master-data-services/add-a-group-master-data-services.md)   
 [Eliminar usuarios o grupos &#40;Master Data Services&#41;](../master-data-services/delete-users-or-groups-master-data-services.md)   
 [Probar los permisos de un usuario &#40;Master Data Services&#41;](../master-data-services/test-a-user-s-permissions-master-data-services.md)  
  
  
