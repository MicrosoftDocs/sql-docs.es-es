---
description: Purga de miembros de versión (Master Data Services)
title: Purga de miembros de versión
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: adecce2d-46bb-49ff-8be9-0b31b8dd3cb6
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 4a23a3827cafd22b23144fa8d0862e25f2afe48b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340229"
---
# <a name="purge-version-members-master-data-services"></a>Purga de miembros de versión (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], la eliminación de un miembro solo lo desactiva, es decir, lo elimina de forma temporal. Los datos aún residen en la base de datos. En este tema se describe cómo purgar (eliminar de forma permanente) todos los miembros eliminados temporalmente en una versión de modelo.  
  
## <a name="prerequisites"></a>Prerrequisitos  
 Para realizar este procedimiento.  
  
-   Debe disponer del permiso para tener acceso al área funcional de Administración de versiones.  
  
-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
## <a name="to-purge-soft-deleted-members"></a>Para purgar los miembros eliminados temporalmente  
  
1.  En [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], haga clic en **Administración de versiones**.  
  
2.  En la página **Administrar versiones** , seleccione el modelo cuya versión desea purgar. Se muestra la lista de versiones del modelo.  
  
3.  Seleccione la fila para la versión que desea purgar.  
  
4.  Haga clic en **Purge Members**(Purgar miembros).  
  
5.  En el mensaje de confirmación, haga clic en "Aceptar".  
  
## <a name="additional-methods-to-purge-members"></a>Métodos adicionales para purgar los miembros  
 Al purgar los miembros de la versión, se eliminarán de manera permanente los miembros eliminados temporalmente de todas las entidades que pertenecen a la versión seleccionada. Una alternativa bastante más precisa consiste en usar el almacenamiento provisional de base de entidades para eliminar permanentemente algunos miembros concretos de una entidad. Además, los administradores de la entidad con permiso funcional de explorador pueden purgar una versión de la entidad en la página del explorador de la entidad.  
  
 Para obtener más información, consulte [Tabla de almacenamiento provisional de miembros hoja &#40;Master Data Services&#41;](../master-data-services/leaf-member-staging-table-master-data-services.md)  
  
  
