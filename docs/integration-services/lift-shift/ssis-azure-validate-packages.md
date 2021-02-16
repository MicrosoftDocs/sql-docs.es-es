---
title: Validación de paquetes de SSIS implementados en Azure | Microsoft Docs
description: Obtenga información sobre cómo el Asistente para la implementación de paquetes SSIS busca en los paquetes problemas conocidos que puedan impedir que se ejecuten según lo previsto en Azure.
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 3da89cbe8d0028a2b362e6358d1b30b3075e030c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345680"
---
# <a name="validate-sql-server-integration-services-ssis-packages-deployed-to-azure"></a>Validación de paquetes de SQL Server Integration Services (SSIS) implementados en Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Al implementar un proyecto de SQL Server Integration Services (SSIS) en el Catálogo de SSIS (SSISDB) de un servidor de Azure, el asistente para la implementación de paquetes agrega un paso de validación adicional después de la página **Revisión**. Durante este paso de validación se comprueba si hay errores conocidos en los paquetes del proyecto que puedan evitar que se ejecuten según lo esperado en Azure SSIS Integration Runtime. A continuación, se muestran las advertencias aplicables en la página **Validate** del asistente.

> [!IMPORTANT]
> La validación que de describe en este artículo se produce al implementar un proyecto con SQL Server Data Tools (SSDT) versión 17.4 o una posterior. Para obtener la versión más reciente de SSDT, consulte [Descargar SQL Server Data Tools (SSDT)](../../ssdt/download-sql-server-data-tools-ssdt.md).

Para obtener más información sobre el asistente para implementación de paquetes, consulte [Deploy Integration Services (SSIS) Projects and Packages](../packages/deploy-integration-services-ssis-projects-and-packages.md) (Implementación de paquetes y proyectos de Integration Services (SSIS)).

## <a name="validate-connection-managers"></a>Validación de administradores de conexión

El asistente comprobará si determinados administradores de conexión tienen los siguientes problemas, que pueden hacer que no se produzca la conexión:
- **Autenticación de Windows**. Si se usa la autenticación de Windows en una cadena de conexión, se generará una advertencia durante la validación. La autenticación de Windows requiere pasos de configuración adicionales. Para obtener más información, vea [Conexión a datos y a recursos compartidos de archivos con la autenticación de Windows](/azure/data-factory/ssis-azure-connect-with-windows-auth).
- **Ruta de acceso a archivos**. Si dentro de una cadena de conexión hay una ruta de acceso a archivo local codificada de forma rígida, como `C:\\...`, se generará una advertencia durante la validación. Es posible que se produzcan errores en paquetes que contengan una ruta de acceso absoluta.
- **Ruta de acceso UNC**. Si dentro de una cadena de conexión hay una ruta de acceso de UNC, se generará una advertencia durante la validación. Es posible que se produzcan errores en paquetes con una ruta de acceso UNC, normalmente debido a que la ruta de acceso UNC requiere autenticación de Windows para el acceso.
- **Nombre de host**. Si dentro de una propiedad del servidor hay un nombre de host, en vez de una dirección IP, se generará una advertencia durante la validación. Es posible que se produzcan errores en paquetes con un nombre de host, normalmente debido a que la red virtual de Azure requiere una configuración de DNS correcta para admitir la resolución de nombres DNS.
- **Proveedor o controlador**. Si un proveedor o controlador no se admite, se generará una advertencia durante la validación. Actualmente solo se admite un número reducido de proveedores y controladores integrados.

El asistente realiza las siguientes comprobaciones de validación para los administradores de conexión de la lista.

| Connection Manager | Autenticación de Windows | Ruta de acceso del archivo | Ruta de acceso UNC | Nombre de host | Proveedor o controlador |
|--------------------|----------|-----------|-----|-----------|-------------------|
| Ado                | âœ"        |           |     | âœ"         | âœ"                 |
| AdoNet             | âœ"        |           |     | âœ"         | âœ"                 |
| Cache              |          | âœ"         | âœ"   |           |                   |
| Excel              |          | âœ"         | âœ"   |           |                   |
| Archivo               |          | âœ"         | âœ"   |           |                   |
| FlatFile           |          | âœ"         | âœ"   |           |                   |
| Ftp                |          |           |     | âœ"         |                   |
| MsOLAP100          |          |           |     | âœ"         | âœ"                 |
| MultiFile          |          | âœ"         | âœ"   |           |                   |
| MultiFlatFile      |          | âœ"         | âœ"   |           |                   |
| OData              | âœ"        |           |     | âœ"         |                   |
| Odbc               | âœ"        |           |     | âœ"         | âœ"                 |
| OleDb              | âœ"        |           |     | âœ"         | âœ"                 |
| SmoServer          | âœ"        |           |     | âœ"         |                   |
| Smtp               | âœ"        |           |     | âœ"         |                   |
| SqlMobile          |          | âœ"         | âœ"   |           |                   |
| Wmi                | âœ"        |           |     |           |                   |
|||||||

## <a name="validate-sources-and-destinations"></a>Validación de orígenes y destinos
Los siguientes orígenes y destinos de terceros no se admiten:

-   Origen y destino Oracle de Attunity
-   Origen y destino Teradata de Attunity
-   Origen y destino SAP BW

## <a name="validate-tasks-and-components"></a>Validación de tareas y componentes

### <a name="execute-process-task"></a>Tarea Ejecutar proceso

Se generará una advertencia durante la validación si un comando apunta a un archivo local con una ruta de acceso absoluta o a un archivo con una ruta de acceso UNC. Es posible que estas rutas de acceso produzcan errores en la ejecución en Azure.

### <a name="script-task-and-script-component"></a>Tarea Script y componente Script

Se generará una advertencia durante la validación si un paquete contiene una tarea Script o un componente Script, que pueden contener referencias o llamadas a ensamblados no admitidos. Estas referencias o llamadas pueden producir errores en la ejecución.

### <a name="other-components"></a>Otros componentes

El formato Orc no se admite en el destino de HDFS ni en el destino de Azure Data Lake Store.

## <a name="next-steps"></a>Pasos siguientes
Para obtener información sobre cómo programar la ejecución de paquetes en Azure, vea [Programar paquetes de SSIS en Azure](ssis-azure-schedule-packages.md).