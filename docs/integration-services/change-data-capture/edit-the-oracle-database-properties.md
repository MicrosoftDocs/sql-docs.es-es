---
description: Editar las propiedades de la base de datos de Oracle
title: Editar las propiedades de la base de datos de Oracle | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- oraProp
ms.assetid: 58dc99f1-ee6b-4508-bb66-2bc589611ff7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b1d5eb987c5dfd62a67bbe6b536b4c2258463741
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130666"
---
# <a name="edit-the-oracle-database-properties"></a>Editar las propiedades de la base de datos de Oracle

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Use la pestaña Oracle del editor de propiedades para realizar cambios en la descripción que proporcionó en la página Crear base de datos CDC del Asistente para nueva instancia y para realizar cambios en la información de conexión a bases de datos de minería de registros de Oracle.  
  
 A continuación se describe la información de la pestaña **Oracle** .  
  
 **Nombre**  
 Nombre de la instancia CDC que especificó en la página Crear base de datos CDC del Asistente para nueva instancia. Este campo es de solo lectura y no puede editar esta información.  
  
 **Descripción**  
 Puede editar la descripción de la nueva instancia o agregar una si no lo hizo al crear la instancia CDC.  
  
 **Cadena de conexión de Oracle**  
 Cadena de conexión de Oracle al equipo que tiene la base de datos de Oracle que está usando. Este campo es de solo lectura y no puede editar esta información. Esto se debe a que algunos cambios en la cadena de conexión pueden hacer que la instancia CDC de Oracle apunte a otra base de datos de Oracle, lo que dañaría el estado de la instancia CDC almacenada en la tabla **cdc.xdbcdc_config** . Si es necesario realizar pequeños cambios, puede cambiar la cadena de conexión directamente en la tabla de configuración mediante SQL Server Management Studio.  
  
 **Autenticación de minería de registros de Oracle**  
 Para especificar las credenciales de autenticación de la base de datos de Oracle que contiene el extractor de registros, seleccione una de las opciones siguientes en **Autenticación**:  
  
-   **Autenticación de Windows**: seleccione esta opción para usar las credenciales del dominio de Windows actual. Solo puede usar esta opción si la base de datos de Oracle está configurada para usar la autenticación de Windows.  
  
-   **Autenticación de Oracle**: si selecciona esta opción, debe escribir el **Nombre de usuario** y la **Contraseña** para el usuario en la base de datos de Oracle a la que se está conectando.  
  
 Puede ver las propiedades de la base de datos de Oracle en el visor. Cuando se usa el visor, la información es de solo lectura. El visor también incluye una lista de las columnas capturadas de la tabla. Para obtener información acerca de cómo obtener acceso al visor, vea [How to Manage a CDC Instance](../../integration-services/change-data-capture/how-to-manage-a-cdc-instance.md).  
  
## <a name="see-also"></a>Vea también  
 [Cómo administrar un servicio CDC desde la Consola del diseñador CDC](../../integration-services/change-data-capture/how-to-manage-a-cdc-service-from-the-cdc-designer-console.md)   
 [Conectarse a una base de datos de origen de Oracle](../../integration-services/change-data-capture/connect-to-an-oracle-source-database.md)   
 [Conectar con Oracle](../../integration-services/change-data-capture/connect-to-oracle.md)  
  
  
