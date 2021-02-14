---
description: Edición de una entidad (Master Data Services)
title: Edición de una entidad
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- entities [Master Data Services], changing name
ms.assetid: 6a5b9f14-6dfc-49d7-a771-e96461d4feae
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 55cc510ecfb781d1d4366d9df928223139116fde
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341746"
---
# <a name="edit-an-entity-master-data-services"></a>Edición de una entidad (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], puede editar una entidad.  
  
## <a name="prerequisites"></a>Requisitos previos  
 Para realizar este procedimiento:  
  
-   Debe disponer de permiso para tener acceso al área funcional de **Administración del sistema** .  
  
-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-edit-an-entity"></a>Para editar una entidad  
  
1.  En [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], haga clic en **Administración del sistema**.  
  
2.  En la página **administrar modelo** , seleccione un modelo de la cuadrícula y, a continuación, haga clic en **entidades**.  
  
3.  En la página **Manage Entity** (Administrar entidad), en la cuadrícula, seleccione la fila de la entidad que desea cambiar y luego haga clic en **Editar**.  
  
4.  En el cuadro **Nombre** , escriba el nombre actualizado de la entidad.  
  
5.  En el campo **Descripción** , escriba la descripción actualizada de la entidad.  
  
6.  En el campo **Nombre de las tablas de ensayo** , escriba un nombre para la tabla de almacenamiento provisional.  
  
7.  Para el campo **Tipo de registro de transacciones** , elija el tipo de registro de transacciones actualizado en la lista desplegable.  
  
     Para obtener información, consulte [Cambio del tipo de registro de transacciones de entidad &#40;Master Data Services&#41;](../master-data-services/change-the-entity-transaction-log-type-master-data-services.md)  
  
8.  Seleccione la casilla **Crear automáticamente los valores de código** , o anule la selección, según proceda.  
  
     Para obtener más información, consulte [Creación automática de código &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md)  
  
9. Seleccione la casilla **Habilitar compresión de datos** , o anule la selección, según proceda. La compresión de fila está activada de forma predeterminada.  
  
     Para obtener más información, vea [compresión de datos](../relational-databases/data-compression/data-compression.md) .  
  
## <a name="status"></a>Status  
 La columna de estado de la cuadrícula muestra el estado de la operación en la entidad. Al hacer clic en **Guardar entidad**, aparece la imagen siguiente, que indica que la entidad se está actualizando.  
  
 ![Icono de estado de actualización](../master-data-services/media/mds-statusicon-updating.png "Icono de estado de actualización")  
  
 Si se producen errores al crear o editar una entidad, se muestra la imagen siguiente.  
  
 ![Icono de estado de error](../master-data-services/media/mds-statusicon-error.png "Icono de estado de error")  
  
 Si el estado es correcto, se muestra la siguiente imagen.  
  
 ![Icono de estado correcto](../master-data-services/media/mds-statusicon-ok.png "Icono de estado correcto")  
  
## <a name="see-also"></a>Consulte también  
 [Jerarquías explícitas &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md)   
 [Eliminar una entidad &#40;Master Data Services&#41;](../master-data-services/delete-an-entity-master-data-services.md)   
 [Entidades &#40;Master Data Services&#41;](../master-data-services/entities-master-data-services.md)  
  
  
