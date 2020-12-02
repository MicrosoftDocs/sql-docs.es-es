---
description: Seleccionar tablas y columnas de Oracle
title: Seleccionar tablas y columnas de Oracle | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- selTabCol
ms.assetid: bf73f80e-a954-4c5f-874e-17fdd4082715
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d418e3d9f8b323c5f0c7905dad8c2ec636f1a3e0
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "88457657"
---
# <a name="select-oracle-tables-and-columns"></a>Seleccionar tablas y columnas de Oracle

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Use la página Seleccionar tablas y columnas de Oracle para seleccionar las tablas de la base de datos de origen de Oracle donde se capturan cambios. Esta página contiene los elementos siguientes:  
  
## <a name="options"></a>Opciones  
 **Lista de la tabla**  
 La lista de tablas tiene tres columnas:  
  
-   **Nombre de tabla de Oracle**: nombre de la tabla, incluido el esquema de tabla.  
  
-   **Instancia de captura**: nombre de la instancia de captura que se usa para denominar los objetos de captura de datos modificados específicos de una instancia. La instancia de captura no puede ser NULL.  
  
     Si no se especifica, el nombre se obtiene del nombre del esquema de origen al que se agrega el nombre de la tabla de origen en el formato `<schema-name>_<table-name>`. El nombre de la instancia de captura no puede tener más de 100 caracteres y debe ser único dentro de la base de datos.  
  
     Puede hacer clic en cualquier celda de esta columna para editar manualmente **capture_instance**.  
  
-   **Rol de seguridad**: nombre del rol de acceso de base de datos usado para controlar el acceso a los datos modificados.  
  
     Puede hacer clic en cualquier celda de esta columna para editar manualmente **security_role**.  
  
 **Agregar tablas**  
 Haga clic en **Agregar tablas** para abrir el cuadro de diálogo Selección de tabla, donde puede [Seleccionar tablas de Oracle para capturar cambios](../../integration-services/change-data-capture/select-oracle-tables-for-capturing-changes.md).  
  
 **Edición**  
 Seleccione una tabla de la lista y seleccione **Editar** para abrir el cuadro de diálogo **Propiedades** de la tabla, donde puede [Realizar cambios en las tablas seleccionadas para capturar cambios](../../integration-services/change-data-capture/make-changes-to-the-tables-selected-for-capturing-changes.md).  
  
 **Remove**  
 Seleccione una tabla de la lista y haga clic en **Quitar** para quitar la tabla de la instancia CDC.  
  
 Después de [Seleccionar tablas de Oracle para capturar cambios](../../integration-services/change-data-capture/select-oracle-tables-for-capturing-changes.md) o [Realizar cambios en las tablas seleccionadas para capturar cambios](../../integration-services/change-data-capture/make-changes-to-the-tables-selected-for-capturing-changes.md) con los cuadros de diálogo correspondientes, haga clic en **Siguiente** para [Generar y ejecutar el script de registro complementario](../../integration-services/change-data-capture/generate-and-run-the-supplemental-logging-script.md).  
  
## <a name="see-also"></a>Consulte también  
 [Cómo crear la instancia de base de datos de cambios de SQL Server](../../integration-services/change-data-capture/how-to-create-the-sql-server-change-database-instance.md)   
 [Editar tablas](../../integration-services/change-data-capture/edit-tables.md)   
 [Agregar tablas a una instancia CDC](../../integration-services/change-data-capture/add-tables-to-a-cdc-instance.md)   
 [Editar las propiedades de tabla](../../integration-services/change-data-capture/edit-the-table-properties.md)  
  
  
