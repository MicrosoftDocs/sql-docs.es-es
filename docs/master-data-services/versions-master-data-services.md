---
title: Versiones
description: Puede crear varias versiones de los datos maestros dentro de un modelo en Master Data Services. Obtenga información sobre las versiones y cuándo usarlas.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- version flags [Master Data Services], about version flags
- versions [Master Data Services]
- version flags [Master Data Services]
- versions [Master Data Services], version flags
ms.assetid: 752ec96d-53d7-4160-8ed2-92e0324645f3
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 54c250aa3b6a6e803f90522218b1eef854af7903
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348841"
---
# <a name="versions-master-data-services"></a>Versiones (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], puede crear varias versiones de los datos maestros dentro de un modelo. Las versiones se pueden bloquear mientras se validan los datos y confirmarse una vez validados los datos. Las versiones confirmadas forman un registro auditable de cambios. Cada versión que cree contendrá todos los miembros, valores de atributo, miembros de jerarquía, relaciones de jerarquía y colecciones del modelo.  
  
## <a name="when-to-use-versions"></a>Cuándo usar versiones  
 Use versiones para:  
  
-   Conservar un registro auditable de los datos maestros según vayan cambiando a medida que pasa el tiempo.  
  
-   Evitar que los usuarios realicen modificaciones mientras se asegura de que todos los datos se validan correctamente con respecto a las reglas de negocios.  
  
-   Bloquear un modelo para su uso en sistemas de suscripción.  
  
-   Probar diferentes jerarquías sin implementarlas inmediatamente.  
  
> [!NOTE]  
>  Cuando cambie la estructura del modelo, por ejemplo cuando cree una entidad o un atributo basado en dominio nuevos, el cambio se aplicará a todas las versiones. Si ve una versión anterior del modelo, la entidad o el atributo se muestran, pero no hay datos.  
  
## <a name="version-flags"></a>Marcas de versión  
 Cuando una versión está preparada para los usuarios o para un sistema de suscripción, podrá establecer una marca para identificar la versión. Puede mover esta marca de versión a versión según sea necesario. Las marcas ayudan a los usuarios y sistemas de suscripción a identificar la versión del modelo que utilizan.  
  
## <a name="workflow-for-version-management"></a>Flujo de trabajo para la administración de versiones  
 Use el flujo de trabajo siguiente para administrar las versiones:  
  
1.  La versión inicial se crea automáticamente cuando crea un modelo y rellena la base de datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] con los datos maestros de la compañía. Según los permisos, los usuarios pueden realizar cambios en esta versión según sea necesario.  
  
2.  Cuando desee confirmar una versión de un modelo, bloquee la versión para que solo los administradores del mismo puedan actualizar los datos. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md). Si se configuran las notificaciones, se envía una notificación por correo electrónico a los administradores del modelo cada vez que el estado de la versión cambia. Para obtener más información, consulte [Configurar notificaciones por correo electrónico &#40;Master Data Services&#41;](../master-data-services/configure-email-notifications-master-data-services.md).  
  
3.  Aplique reglas de negocios a los datos de la versión bloqueada y revise cualquier problema de la validación. Si fuera necesario, puede rellenar la información que falte o revertir la transacción que provocó el problema. También puede desbloquear la versión para que los usuarios efectúen modificaciones.  
  
4.  Cuando todos los datos se hayan validado correctamente, confirme la versión y asígnele una marca para que la usen los sistemas de suscripción. No se pueden realizar cambios en una versión confirmada.  
  
5.  Copie la versión confirmada y notifique a los usuarios que pueden empezar a trabajar en una nueva versión del modelo.  
  
## <a name="sequential-or-simultaneous-versions"></a>Versiones secuenciales o simultáneas  
 Puede crear versiones secuenciales o simultáneas de su modelo.  
  
-   **Versiones secuenciales.** Cada vez que confirma una versión, crea una copia nueva y asigna a la versión el siguiente número secuencial. Por ejemplo, puede copiar **Versión 7** de su modelo y llamar a la copia **Versión 8**.  
  
-   **Versiones simultáneas.** Cree versiones simultáneas del modelo cuando desee trabajar con dos más versiones de los datos al mismo tiempo. Esto es útil cuando la empresa efectúa reorganizaciones o fusiones que coinciden con la práctica habitual de la actividad empresarial y desea determinar la forma en que los nuevos datos maestros van a ajustarse en las estructuras existentes.  
  
    > [!NOTE]  
    >  Una opción de [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] determina si puede copiar o no todas las versiones o únicamente aquellas que se hayan confirmado. Para crear versiones simultáneas debe configurar [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] para que permita copiar todas las versiones. Este valor también está disponible en la tabla Configuración del sistema. Para obtener más información, vea [Configuración del sistema &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md).  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Descripción de la tarea|Tema|  
|----------------------|-----------|  
|Cambiar el nombre de una versión existente.|[Cambiar un nombre de versión &#40;Master Data Services&#41;](../master-data-services/change-a-version-name-master-data-services.md)|  
|Bloquear una versión para que solo los administradores puedan editar sus datos.|[Bloquear una versión &#40;Master Data Services&#41;](../master-data-services/lock-a-version-master-data-services.md)|  
|Desbloquear una versión para que los usuarios puedan editar sus datos.|[Desbloquear una versión &#40;Master Data Services&#41;](../master-data-services/unlock-a-version-master-data-services.md)|  
|Confirmar una versión después de validar todos los datos.|[Confirmar una versión &#40;Master Data Services&#41;](../master-data-services/commit-a-version-master-data-services.md)|  
|Crear una nueva marca para marcar una versión.|[Crear una marca de versión &#40;Master Data Services&#41;](../master-data-services/create-a-version-flag-master-data-services.md)|  
|Cambiar el nombre de una marca de versión existente.|[Cambiar el nombre de marca de una versión &#40;Master Data Services&#41;](../master-data-services/change-a-version-flag-name-master-data-services.md)|  
|Asignar una marca existente a una versión.|[Asignar una marca a una versión &#40;Master Data Services&#41;](../master-data-services/assign-a-flag-to-a-version-master-data-services.md)|  
|Crear una nueva copia de una versión existente|[Copiar una versión &#40;Master Data Services&#41;](../master-data-services/copy-a-version-master-data-services.md)|  
|Eliminar una versión existente.|[Eliminar una versión &#40;Master Data Services&#41;](../master-data-services/delete-a-version-master-data-services.md)|  
|Purga los miembros eliminados temporalmente de una versión.|[Purga de miembros de versión &#40;Master Data Services&#41;](../master-data-services/purge-version-members-master-data-services.md)|  
  
## <a name="related-content"></a>Contenido relacionado  
  
-   [Invertir una transacción &#40;Master Data Services&#41;](../master-data-services/reverse-a-transaction-master-data-services.md)  
  
-   [Notificaciones &#40;Master Data Services&#41;](../master-data-services/notifications-master-data-services.md)  
  
-   [Reglas de negocios &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)  
  
  
