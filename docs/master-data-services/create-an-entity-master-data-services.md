---
title: Crear una entidad
description: Obtenga información sobre cómo crear una entidad en Master Data Services para contener miembros y sus atributos. Debe tener permisos para el área de administración del sistema.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- entities [Master Data Services], creating
- creating entities [Master Data Services]
ms.assetid: d9a6a51e-7b53-4785-a118-3baeb7ca2d48
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 8d47a78e4a7e706e91312ee2eb4686121c2b2532
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341428"
---
# <a name="create-an-entity-master-data-services"></a>Crear una entidad (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], cree una entidad para contener los miembros y sus atributos.  
  
## <a name="prerequisites"></a>Requisitos previos  
 Para realizar este procedimiento:  
  
-   Debe disponer de permiso para tener acceso al área funcional de **Administración del sistema** .  
  
-   Debe ser administrador de modelo. Para obtener más información, vea [administradores &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Debe existir un modelo. Para obtener más información, consulte [Crear un modelo &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md).  
  
### <a name="to-create-an-entity"></a>Crear una entidad  
  
1.  En [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], haga clic en **Administración del sistema**.  
  
2.  En la página **Manage Model** (Administrar modelo), en la cuadrícula, seleccione el modelo para el que desea crear la entidad y luego haga clic en **Entidades**.  
  
3.  En el **Manage Entity** (Administrar entidad) haga clic en **Agregar**.  
  
4.  En el cuadro **Nombre** , escriba el nombre de la entidad.  
  
5.  Opcionalmente, en el campo **Descripción** , escriba la descripción de la entidad.  
  
6.  Opcionalmente, en el cuadro **Nombre de las tablas de ensayo** , escriba un nombre para la tabla de ensayo.  
  
     Si no completa este campo, se usa el nombre de la entidad.  
  
    > [!TIP]  
    >  El nombre del modelo debe formar parte del nombre de la tabla de almacenamiento provisional, por ejemplo *Nombremodelo_Nombreentidad*. Esto hace que resulte más sencillo buscar tablas en la base de datos. Para obtener más información sobre las tablas de almacenamiento provisional, consulte [Información general: importación de datos de tablas &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md).
    > [!TIP]
    > Si se utiliza la nomenclatura predeterminada para las tablas de almacenamiento provisional, MDS agregará automáticamente identificadores (por ejemplo, _1, _2) a los nombres de las tablas de almacenamiento provisional si existe una entidad con el mismo nombre en otro modelo.
  
7.  En el campo **Tipo de registro de transacciones** , elija el tipo de registro de transacciones de la lista desplegable.  
  
     Para obtener información, consulte [Cambio del tipo de registro de transacciones de entidad &#40;Master Data Services&#41;](../master-data-services/change-the-entity-transaction-log-type-master-data-services.md)  
  
8.  Opcional. Active la casilla **Crear automáticamente valores Code** . Para obtener más información, consulte [creación automática de código &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md).  
  
9. Opcional. Active la casilla **Habilitar compresión de datos** . La compresión de fila está activada de forma predeterminada. Para obtener más información, consulte [Data Compression](../relational-databases/data-compression/data-compression.md).  
  
10. Haga clic en **Save**(Guardar).  
  
## <a name="grid-columns"></a>Columnas de la cuadrícula  
 Por cada entidad creada, se agrega una fila con trece columnas a la cuadrícula. Las columnas son las siguientes.  
  
|Nombre|Descripción|  
|----------|-----------------|  
|Estado|El estado de la entidad. Al hacer clic en **Guardar** , aparece la imagen siguiente, que indica que la entidad se está actualizando.<br /><br /> ![Icono de estado de actualización](../master-data-services/media/mds-statusicon-updating.png "Icono de estado de actualización")<br /><br /> Si se producen errores al crear o editar una entidad, se muestra la imagen siguiente.<br /><br /> ![Icono de estado de error](../master-data-services/media/mds-statusicon-error.png "Icono de estado de error")<br /><br /> Si el estado es correcto, se muestra la siguiente imagen.<br /><br /> ![Icono de estado correcto](../master-data-services/media/mds-statusicon-ok.png "Icono de estado correcto")|  
|nombre|El nombre de la entidad.|  
|Descripción|La descripción de la entidad.|  
|Tabla de ensayo|El nombre de prefijo de la tabla que se usa para almacenar datos.|  
|Tipo de registro de transacciones|El tipo de registro de transacciones de la entidad.|  
|Creación automática de código|Especifica si está habilitada la creación automática de código.|  
|Data Compression|Especifica si está habilitada la compresión de datos para la entidad.|  
|Es destino de sincronización|Especifica si la entidad es el destino de una relación de sincronización.|  
|Está habilitada la jerarquía|Especifica si la entidad está habilitada para jerarquías explícitas. Esta columna muestra Sí si se ha creado al menos una jerarquía explícita para la entidad.|  
|Creado por|Nombre del usuario que creó la entidad.|  
|Creada el|Fecha y hora en que se creó la entidad.|  
|Actualizada por|Nombre del usuario que actualizó la entidad por última vez.|  
|Updated On|Fecha y hora en que se actualizó la entidad por última vez.|  
  
## <a name="next-steps"></a>Pasos siguientes  
  
-   [Crear un atributo de texto &#40;Master Data Services&#41;](../master-data-services/create-a-text-attribute-master-data-services.md)  
  
-   [Crear un atributo basado en dominio &#40;Master Data Services&#41;](../master-data-services/create-a-domain-based-attribute-master-data-services.md)  
  
-   [Crear un atributo de archivo &#40;Master Data Services&#41;](../master-data-services/create-a-file-attribute-master-data-services.md)  
  
## <a name="see-also"></a>Consulte también  
 [Entidades &#40;Master Data Services&#41;](../master-data-services/entities-master-data-services.md)   
 [Jerarquías explícitas &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md)   
 [Editar un Master Data Services de &#40;de entidad&#41;](../master-data-services/edit-an-entity-master-data-services.md)   
 [Eliminar una entidad &#40;Master Data Services&#41;](../master-data-services/delete-an-entity-master-data-services.md)  
  
  
