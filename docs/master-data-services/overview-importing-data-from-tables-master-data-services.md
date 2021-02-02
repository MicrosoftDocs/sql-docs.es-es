---
title: Importar datos desde tablas
description: Importe datos de tablas y realice cambios en los datos después de crear un modelo para los datos en Master Data Services.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- staging process [Master Data Services], about staging process
- importing data [Master Data Services]
- staging process [Master Data Services]
ms.assetid: 181d1e22-379c-45d1-b03c-e1e22ff14164
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: b5290d2e9650acc8ee540e572109b51b377e5235
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236581"
---
# <a name="overview-importing-data-from-tables-master-data-services"></a>Información general: importación de datos de tablas (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Después de que haya creado un modelo para los datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], puede empezar a agregar datos y realizar cambios en ellos.   Puede usar tablas de almacenamiento provisional de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] , procedimientos almacenados y Master Data Manager.  
  
 Para obtener instrucciones sobre cómo agregar y modificar datos, consulte [Importar datos de tablas &#40;Master Data Services&#41;](../master-data-services/import-data-from-tables-master-data-services.md).  
  
> [!NOTE]
>  También puede usar el [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)][!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)] para agregar datos al repositorio de MDS (base de datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]) desde Excel. Para obtener más información, consulte [Información general: Importación de datos desde Excel &#40;complemento MDS para Excel&#41;](../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md).  
  
 Cuando agregue o modifique datos, puede hacer lo siguiente:  
  
-   Cargar y actualizar miembros, así como actualizar valores de atributo  
  
-   Desactivar o eliminar miembros  
  
-   Mover miembros de jerarquías explícitas  
  
 La adición y la actualización de datos incluyen las siguientes tareas principales.  
  
1.  Cargar datos en las tablas de almacenamiento provisional en la base de datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
2.  Cargar datos de las tablas de almacenamiento provisional en las tablas adecuadas de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
     Puede usar los procedimientos almacenados de almacenamiento provisional o [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] para cargar los datos.  
  
> [!NOTE]  
>  En [!INCLUDE[sssql15-md](../includes/sssql16-md.md)], la compatibilidad con los procesos de almacenamiento provisional de [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)] está en desuso.  
  
## <a name="deactivating-and-deleting-members-mds"></a>Desactivación y eliminación de miembros (MSD)  
 La desactivación significa que el miembro se puede reactivar. Si reactiva un miembro, se restauran sus atributos y su pertenencia a jerarquías y colecciones. Todas las transacciones anteriores quedan intactas. Las transacciones desactivación son visibles para los administradores en el área funcional de **Administración de versiones** de Master Data Manager.  
  
 Eliminar significa purgar el miembro del sistema permanentemente. Todas las transacciones del miembro, todas las relaciones y todos los atributos se eliminan de forma permanente.  
  
> [!NOTE]  
>  No se puede utilizar el almacenamiento provisional para reactivar los miembros. Se debe hacer manualmente en Master Data Manager. Para obtener más información, consulte [Reactivar un miembro o una colección &#40;Master Data Services&#41;](../master-data-services/reactivate-a-member-or-collection-master-data-services.md).  
>   
>  No se puede utilizar el almacenamiento provisional para eliminar o desactivar colecciones. Para obtener más información sobre cómo desactivar colecciones de forma manual, consulte [Eliminar un miembro o una colección &#40;Master Data Services&#41;](../master-data-services/delete-a-member-or-collection-master-data-services.md).  
  
## <a name="moving-explicit-hierarchy-members-mds"></a>Movimiento de miembros de jerarquías explícitas (MSD)  
 Al mover la ubicación de los miembros de jerarquías explícitas de forma masiva, puede designar lo siguiente.  
  
-   Un miembro consolidado como elemento primario de un miembro consolidado.  
  
-   Un miembro consolidado como elemento primario de un miembro hoja.  
  
-   Un miembro hoja es un elemento relacionado de una hoja o de un miembro consolidado.  
  
-   Un miembro consolidado es un elemento relacionado de una hoja o de un miembro consolidado.  
  
## <a name="staging-tables-and-stored-procedures-mds"></a>Tablas de almacenamiento provisional y procedimientos almacenados (MDS)  
 La base de datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] incluye los siguientes tipos de tablas de almacenamiento provisional que puede rellenar con sus datos.  
  
-   [Tabla de almacenamiento provisional de miembros hoja &#40;Master Data Services&#41;](../master-data-services/leaf-member-staging-table-master-data-services.md)  
  
-   [Tabla de almacenamiento provisional de miembros consolidados &#40;Master Data Services&#41;](../master-data-services/consolidated-member-staging-table-master-data-services.md)  
  
-   [Tabla de almacenamiento provisional de relaciones &#40;Master Data Services&#41;](../master-data-services/relationship-staging-table-master-data-services.md)  
  
 Para cada entidad del modelo, hay una tabla de almacenamiento provisional. El nombre de la tabla indica la entidad correspondiente y el tipo de entidad, como miembro hoja. La siguiente imagen muestra las tablas de almacenamiento provisional de las entidades de moneda, cliente y producto.  
  
 ![Tablas de almacenamiento provisional en la base de datos MDS](../master-data-services/media/mds-staging-tables.png "Tablas de almacenamiento provisional en la base de datos MDS")  
  
 El nombre de cada tabla se especifica cuando se crea una entidad y no se puede cambiar. Si el nombre de la tabla de ensayo contiene _1 u otro número, otra tabla con ese nombre ya existía cuando se creó la entidad.  
  
 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] incluye los siguientes tipos de procedimientos almacenados de almacenamiento provisional.  
  
-   stg.udp_ \<name> _Leaf  
  
-   stg.udp_ \<name> _Consolidated  
  
-   stg.udp_ \<name> _Relationship  
  
 Para cada entidad del modelo, hay tres procedimientos almacenados que corresponden a las tablas de almacenamiento provisional de miembros hoja, miembros consolidados y relaciones.  La siguiente imagen muestra los procedimientos almacenados de almacenamiento provisional de las entidades de moneda, cliente y producto.  
  
 ![Procedimientos almacenados de almacenamiento provisional en la base de datos de MDS](../master-data-services/media/mds-staging-storedprocedures.png "Procedimientos almacenados de almacenamiento provisional en la base de datos de MDS")  
  
 Para obtener más información sobre los procedimientos almacenados, consulte [Procedimiento almacenado de almacenamiento provisional &#40;Master Data Services&#41;](../master-data-services/staging-stored-procedure-master-data-services.md).  
  
## <a name="logging-transactions-mds"></a>Registro de transacciones (MDS)  
 Se pueden registrar todas las transacciones que se producen cuando se importan o actualizan datos o relaciones. Una opción del procedimiento almacenado permite este registro. Si inicia el proceso de almacenamiento provisional mediante [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], no se realiza ningún registro.  
  
 En [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)], el valor de **Registrar todas las transacciones de almacenamiento provisional** no se aplica a este método de almacenamiento provisional de datos.  
  
## <a name="related-content"></a>Contenido relacionado  
  
-   [Validación &#40;Master Data Services&#41;](../master-data-services/validation-master-data-services.md)  
  
-   [Reglas de negocios &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)  
  
  
