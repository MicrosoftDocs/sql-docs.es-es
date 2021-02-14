---
description: Aprobación o rechazo de un conjunto de cambios (Master Data Services)
title: Aprobación o rechazo de un conjunto de cambios
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 45bd01f9-ae15-4fc5-a2ba-eee565a26ef8
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 47ea1da154b6e27be84888361867e370945e7438
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339567"
---
# <a name="approve-or-reject-a-changeset-master-data-services"></a>Aprobación o rechazo de un conjunto de cambios (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Un conjunto de cambios es una colección de cambios pendientes en los datos maestros. Si los cambios de entidad requieren la aprobación del administrador y se envía un conjunto de cambios para su aprobación, puede revisarlos y luego aprobar o rechazar el conjunto de cambios.  
  
## <a name="prerequisites"></a>Requisitos previos  
  
-   Debe disponer de permiso de acceso al área funcional **Explorador** . Para obtener más información, consulte [Permisos del área funcional &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
-   Debe tener permiso de administrador para la entidad.  
  
-   Para los cambios de entidad se necesita la aprobación del administrador.  
  
-   Si el estado del conjunto de cambios es pendiente, puede revisarlo y luego aprobarlo o rechazarlo.  
  
-   No se permite a los usuarios aprobar sus propios cambios. Si usted es el administrador de la entidad, debe asignar un administrador secundario para aprobar su propio conjunto de cambios.  
  
## <a name="to-approve-or-reject-a-changeset"></a>Para aprobar o rechazar un conjunto de cambios  
  
1.  En la página principal de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , seleccione el modelo y la versión y luego haga clic en **Explorador**.  
  
2.  Haga clic en una entidad en el menú **Entidades** .  
  
3.  En el panel derecho, seleccione **Conjuntos de cambios** y haga doble clic en el conjunto de cambios que quiera aprobar o rechazar.  
  
4.  Haga clic en **Aplicar** para aplicar el conjunto de cambios y revisar los cambios pendientes.  
  
5.  Haga clic en **Rechazar** para rechazar el conjunto de cambios y enviarlo de vuelta al propietario.  
  
6.  Haga clic en **Aprobar** para aprobar el conjunto de cambios. El conjunto de cambios se confirma automáticamente.  
  
## <a name="see-also"></a>Consulte también  
 [Crear un conjunto de cambios &#40;Master Data Services&#41;](../master-data-services/create-a-changeset-master-data-services.md)   
 [Aplicar y actualizar un conjunto de cambios &#40;Master Data Services&#41;](../master-data-services/apply-and-update-a-changeset-master-data-services.md)   
 [Confirmación o envío de un conjunto de cambios &#40;Master Data Services&#41;](../master-data-services/commit-or-submit-a-changeset-master-data-services.md)  
  
  
