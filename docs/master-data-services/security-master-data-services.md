---
title: Seguridad
description: Obtenga información sobre la seguridad en Master Data Services, incluidos los tipos de usuarios, cómo establecer la seguridad, la seguridad en el complemento para Excel y las tareas relacionadas.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 56bc41ea-de28-4184-aa7e-99111ae55af5
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 97209bd6d906a73f6199f06a09e14e1ab4d0ac22
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352991"
---
# <a name="security-master-data-services"></a>Seguridad (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], use la seguridad para asegurarse de que los usuarios tengan acceso a los datos maestros específicos necesarios para realizar su trabajo y para impedir que tengan acceso a datos que no deben estar disponibles para ellos.  
  
 También puede usar la seguridad para convertir a un usuario en administrador de un modelo concreto y un área funcional (por ejemplo, para permitir que alguien cree versiones del modelo Customer o para ofrecer a alguien la posibilidad de establecer permisos de seguridad).  
  
 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] se basa en usuarios y grupos de dominio locales o de Active Directory. La seguridad de MDS permite usar un nivel de detalle específico para determinar los datos a los que un usuario puede tener acceso. Debido a la granularidad, la seguridad puede llegar a complicarse fácilmente y debe tener cuidado al usar usuarios y grupos superpuestos. Para obtener más información, consulte [Superponer permisos de usuario y de grupo &#40;Master Data Services&#41;](../master-data-services/overlapping-user-and-group-permissions-master-data-services.md).  
  
 Puede asignar acceso de seguridad en el área funcional de **Permisos de usuario y de grupo** de la aplicación web de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] o usando el servicio web.  
  
## <a name="types-of-users"></a>Tipos de usuarios  
 Hay dos tipos de usuarios en [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]:  
  
-   Los que tienen acceso a los datos en el área funcional de **Explorador** .  
  
-   Los que tienen la posibilidad de realizar tareas administrativas en áreas distintas de **Explorador**. Estos usuarios se denominan [Administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
## <a name="how-to-set-security"></a>Cómo establecer la seguridad  
 Para conceder a un usuario o a un grupo permiso de acceso a datos o a la funcionalidad de MDS, debe asignar:  
  
-   [Acceso a áreas funcionales](../master-data-services/functional-area-permissions-master-data-services.md), que determina a cuál de las cinco áreas funcionales de la interfaz de usuario puede tener acceso un usuario.  
  
-   [Permisos de objeto de modelo](../master-data-services/model-object-permissions-master-data-services.md), que determinan los atributos a los que puede tener acceso un usuario, y el tipo de acceso (lectura, creación o actualización) que tiene el usuario para esos atributos. El usuario puede también asignar permisos de administrador en el nivel de modelo.  
  
-   Opcionalmente, [permisos de los miembros de jerarquía](../master-data-services/hierarchy-member-permissions-master-data-services.md), que determinan los miembros a los que puede tener acceso un usuario, y el tipo de acceso (lectura, actualización o eliminación) que tiene el usuario para esos miembros.  
  
 Cuando asigna permisos a atributos y miembros, los permisos se cruzan y las reglas determinan qué permiso tiene prioridad. Para obtener más información, consulte [Cómo se determinan los permisos &#40;Master Data Services&#41;](../master-data-services/how-permissions-are-determined-master-data-services.md).  
  
## <a name="security-in-the-add-in-for-excel"></a>Seguridad del complemento de Excel  
 La seguridad establecida en la aplicación web de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] también se aplica a [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)]. Los usuarios solo pueden ver y trabajar con datos para los que tienen permiso. Los administradores pueden realizar tareas administrativas.  
  
 La única advertencia es que toda la seguridad asignada en [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] no surtirá efecto en Excel hasta que transcurrido un intervalo de 20 minutos. El intervalo se define mediante la configuración de *MdsMaximumUserInformationCacheInterval* en el archivo web.config. Para cambiar el intervalo, puede cambiar el valor y reiniciar IIS.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Descripción de la tarea|Tema|  
|----------------------|-----------|  
|Crear un usuario que tenga permiso completo para un modelo.|[Crear un administrador de modelo &#40;Master Data Services&#41;](../master-data-services/create-a-model-administrator-master-data-services.md)|  
|Agregar un grupo de Active Directory a [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]; este es el primer paso para conceder a un grupo permiso de acceso a datos de la aplicación web de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .|[Agregar un grupo &#40;Master Data Services&#41;](../master-data-services/add-a-group-master-data-services.md)|  
|Asignar permisos a un área funcional de la aplicación web de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .|[Asignar permisos del área funcional &#40;Master Data Services&#41;](../master-data-services/assign-functional-area-permissions-master-data-services.md)|  
|Asignar permisos a los valores de atributo asignando el permiso a los objetos del modelo.|[Asignar permisos de objeto de modelo &#40;Master Data Services&#41;](../master-data-services/assign-model-object-permissions-master-data-services.md)|  
|Asignar permisos a los valores de miembro asignando el permiso a los nodos de la jerarquía.|[Asignar los permisos de los miembros de una jerarquía &#40;Master Data Services&#41;](../master-data-services/assign-hierarchy-member-permissions-master-data-services.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)   
 [Usuarios y grupos &#40;Master Data Services&#41;](../master-data-services/users-and-groups-master-data-services.md)   
 [&#40;Master Data Services permisos de área funcional&#41;](../master-data-services/functional-area-permissions-master-data-services.md)   
 [Permisos del objeto de modelo &#40;Master Data Services&#41;](../master-data-services/model-object-permissions-master-data-services.md)   
 [Permisos de miembros de la jerarquía &#40;Master Data Services&#41;](../master-data-services/hierarchy-member-permissions-master-data-services.md)   
 [Cómo se determinan los permisos &#40;Master Data Services&#41;](../master-data-services/how-permissions-are-determined-master-data-services.md)  
  
  
