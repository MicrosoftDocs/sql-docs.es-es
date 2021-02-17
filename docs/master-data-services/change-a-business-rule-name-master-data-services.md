---
description: Cambiar el nombre de una regla de negocios (Master Data Services)
title: Cambiar el nombre de una regla de negocios
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], changing name
ms.assetid: cffcae43-a208-443f-9f43-a0ec9e05f79c
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: cada2e43ffb4d987c54a0e8b5079b32879a47bf2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351954"
---
# <a name="change-a-business-rule-name-master-data-services"></a>Cambiar el nombre de una regla de negocios (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], cambie el nombre de una regla de negocios cuando el nombre asignado no satisfaga sus necesidades empresariales.  
  
## <a name="prerequisites"></a>Prerrequisitos  
 Para realizar este procedimiento:  
  
-   Debe disponer de permiso para tener acceso al área funcional de **Administración del sistema** .  
  
-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Una regla de negocios debe existir. Para obtener más información, consulte [Crear y publicar una regla de negocios &#40;Master Data Services&#41;](../master-data-services/create-and-publish-a-business-rule-master-data-services.md).  
  
### <a name="to-change-the-name-of-a-business-rule"></a>Cambiar el nombre de una regla de negocios  
  
1.  En [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], haga clic en **Administración del sistema**.  
  
2.  En la barra de menús, seleccione **Administrar** y haga clic en **Reglas de negocios**.  
  
3.  En la página **reglas de negocios** , en la lista desplegable **modelo** , seleccione un modelo.  
  
4.  En la lista desplegable **Entidad** , seleccione una entidad.  
  
5.  En la lista desplegable **Member Types** (Tipos de miembros), seleccione un tipo de miembro.  
  
6.  En la cuadrícula, seleccione la fila de la regla de negocios cuyo nombre desea cambiar y haga clic en **Editar**.  
  
7.  Escriba un nuevo nombre para la regla de negocios.  
  
8.  Haga clic en **Save**(Guardar).  
  
9. Haga clic en **Publish All**(Publicar todo).  
  
10. En el cuadro de diálogo de confirmación, haga clic en **Aceptar**. El valor de la columna **Business Rule State**(Estado de la regla de negocio) es **Activo**.  
  
## <a name="see-also"></a>Consulte también  
 [Reglas de negocios &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)  
  
  
