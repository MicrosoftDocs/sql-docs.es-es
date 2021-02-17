---
description: Creación de un índice personalizado (Master Data Services)
title: Creación de un índice
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: d694a105-69b1-4ff6-99d3-1f408b916b81
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: cad316eb2c1bd4925e72839ede2e29bccb825118
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351889"
---
# <a name="create-an-index-master-data-services"></a>Creación de un índice personalizado (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Cree un índice personalizado en una lista de atributos que se consulta con frecuencia para mejorar el rendimiento de las consultas.  
  
## <a name="prerequisites"></a>Prerrequisitos  
 Para realizar este procedimiento:  
  
-   Debe disponer de permiso para tener acceso al área funcional de Administración del sistema. Para obtener más información, consulte [Permisos del área funcional &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
 **Para crear un índice**  
  
1.  En Master Data Manager, haga clic en **Administración del sistema**.  
  
2.  En la página **Manage Model** (Administrar modelo), seleccione un modelo de la cuadrícula y después haga clic en **Entidades**.  
  
3.  En la página **Manage Entity** (Administrar entidad), en la **cuadrícula** , seleccione la fila de la entidad para la que desea crear un índice.  
  
4.  Haga clic en **Índices**.  
  
5.  En el cuadro **Nombre** , escriba un nombre para el índice.  
  
6.  Seleccione **Is Unique** (Es único) si desea crear un índice único. Para obtener más información sobre los tipos de índices, consulte [Índice personalizado &#40;Master Data Services&#41;](../master-data-services/custom-index-master-data-services.md).  
  
7.  Haga clic en los atributos en el cuadro **Atributos disponibles** y luego haga clic en la flecha **Agregar** . Para agregar todos los atributos, haga clic en la flecha **Agregar todo** .  
  
8.  Haga clic en **Save**(Guardar).  
  
 Por cada índice creado, se agrega una fila con cuatro columnas a la cuadrícula. En la siguiente tabla se describen las columnas.  
  
|Nombre de columna|Descripción|  
|-----------------|-----------------|  
|Estado|El estado del índice.<br /><br /> Al hacer clic en **Guardar**, se muestra la imagen ![icono de estado de actualización](../master-data-services/media/mds-statusicon-updating.png "Icono de estado de actualización") , que indica que el índice se está actualizando.<br /><br /> Si hay errores al crear o editar un índice, se muestra la imagen ![icono de estado de error](../master-data-services/media/mds-statusicon-error.png "Icono de estado de error") .<br /><br /> De lo contrario, el estado es correcto y se muestra la imagen ![icono de estado correcto](../master-data-services/media/mds-statusicon-ok.png "Icono de estado correcto") .|  
|Nombre|Nombre de índice.|  
|Is Unique|Especifica si el índice es único.|  
|En atributos|Muestra los nombres para mostrar de los atributos en los que está definido el índice.|  
  
 Cuando se hace clic en un índice, se muestra la siguiente información.  
  
-   **Creado por:** Nombre del usuario que creó el índice.  
  
-   **El:** Fecha y hora en que se creó el índice.  
  
-   **Actualizado por:** Nombre del usuario que actualizó el índice por última vez.  
  
-   **El:** Fecha y hora en que se el índice se actualizó por última vez.  
  
## <a name="next-steps"></a>Pasos siguientes  
 [Editar y eliminar un índice &#40;Master Data Services&#41;](../master-data-services/edit-and-delete-an-index-master-data-services.md)  
  
## <a name="see-also"></a>Consulte también  
 [Índice personalizado &#40;Master Data Services&#41;](../master-data-services/custom-index-master-data-services.md)  
  
  
