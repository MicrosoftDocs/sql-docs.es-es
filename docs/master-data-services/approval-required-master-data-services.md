---
description: Aprobación necesaria (Master Data Services)
title: Aprobación necesaria
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: b475a53d-269d-49f3-bb42-965c555f80be
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 25de1b29a2d19457d6c74a02ec1937bcd654c023
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339583"
---
# <a name="approval-required-master-data-services"></a>Aprobación necesaria (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], el administrador puede establecer una entidad en Aprobación necesaria. Todos los cambios de esta entidad requerirán que uno de los administradores de la entidad revise y apruebe los cambios.  
  
> [!NOTE]  
>  Los cambios realizados en los miembros hoja requieren aprobación. Los cambios realizados en colecciones y jerarquías explícitas en desuso omiten la aprobación.  
>   
>  Los cambios realizados por el proceso de tablas de almacenamiento provisional omiten la aprobación.  
>   
>  Los cambios realizados por una regla de negocio omiten la aprobación.  
  
## <a name="prerequisites"></a>Prerrequisitos  
 Para realizar este procedimiento:  
  
-   Debe disponer de permiso para acceder al área funcional de Administración del sistema.  
  
-   Debe ser administrador de modelo. Para obtener más información, consulte [Administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)  
  
-   Debe existir una entidad. Para obtener más información, consulte [Crear una entidad &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md)  
  
## <a name="to-enable-approval-required-for-an-entity"></a>Para habilitar la opción Aprobación necesaria para una entidad  
  
1.  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], haga clic en **Administración del sistema**.  
  
2.  En la página **administrar modelo** , seleccione un modelo de la cuadrícula y, a continuación, haga clic en **entidades**.  
  
3.  En la página **Manage Entity** (Administrar entidad), en la cuadrícula, seleccione la fila de la entidad para la que desea habilitar  **Aprobación necesaria** .  
  
4.  Haga clic en **Editar**, seleccione **Aprobación necesaria** y luego haga clic en **Guardar**.  
  
## <a name="see-also"></a>Consulte también  
 [Conjuntos de cambios &#40;Master Data Services&#41;](../master-data-services/changesets-master-data-services.md)  
  
  
