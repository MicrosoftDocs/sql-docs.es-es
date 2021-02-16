---
title: Conectarse al catálogo de SSIS (SSISDB) en Azure | Microsoft Docs
description: Obtenga la información de conexión necesaria para conectarse al catálogo de SSIS (SSISDB) hospedado en un servidor de Azure SQL Database.
ms.date: 09/23/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: e5ce8d83ba29843cc14c6798eb4f550eab62e557
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352016"
---
# <a name="connect-to-the-ssis-catalog-ssisdb-in-azure"></a>Conectarse al catálogo de SSIS (SSISDB) en Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Obtenga la información de conexión necesaria para conectarse al catálogo de SSIS (SSISDB) hospedado en un servidor de Azure SQL Database. Necesita los elementos siguientes para poder conectarse:
- Un nombre completo de servidor
- El nombre de la base de datos
- La información de inicio de sesión 

> [!IMPORTANT]
> En Azure Data Factory, en estos momentos no se puede crear la base de datos del catálogo SSISDB en Azure SQL Database, independientemente de la creación de Azure-SSIS Integration Runtime. Azure SSIS IR es el entorno de ejecución que ejecuta los paquetes SSIS en Azure. Para obtener un tutorial del proceso, vea [Implementar y ejecutar un paquete SSIS en Azure](/azure/data-factory/tutorial-create-azure-ssis-runtime-portal). 

## <a name="prerequisites"></a>Prerrequisitos
Antes de comenzar, asegúrese de que tiene instalada la versión 17.2 (o posterior) de SQL Server Management Studio (SSMS). Si la base de datos del catálogo de SSISDB está hospedada en SQL Managed Instance, asegúrese de tener la versión 17.6 o posterior de SSMS. Para descargar la versión más reciente de SSMS, consulte [Download SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) [Descargar SQL Server Management Studio (SSMS)].

## <a name="get-the-connection-info-from-the-azure-portal"></a>Obtener la información de conexión desde Azure Portal
1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. En Azure Portal, seleccione **Bases de datos SQL** desde el menú izquierdo y, a continuación, seleccione la base de datos `SSISDB` en la página **Bases de datos SQL**. 
3. En la página **Introducción** de la base de datos `SSISDB`, compruebe el nombre completo del servidor tal como se muestra en la siguiente imagen. Mantenga el puntero sobre el nombre del servidor para que aparezca la opción **Haga clic para copiar**.

    ![Información de conexión del servidor](media/ssis-azure-connect-to-catalog-database/server-name.png) 

4. Si ha olvidado la información de inicio de sesión del servidor de SQL Database, vaya a la página del servidor de SQL Database. En ella podrá ver el nombre de administrador del servidor y, si fuera necesario, podrá restablecer la contraseña.

## <a name="connect-with-ssms"></a>Conectarse con SSMS
1. Abra SQL Server Management Studio.

2. **Conéctese al servidor**. En el cuadro de diálogo **Conectar con el servidor**, especifique la siguiente información:

   | Configuración       | Valor sugerido | Descripción | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Tipo de servidor** | Motor de base de datos | Este valor es necesario. |
   | **Nombre del servidor** | Nombre completo del servidor | El nombre debe tener este formato: **mysqldbserver.database.windows.net**. |
   | **Autenticación** | Autenticación de SQL Server | |
   | **Inicio de sesión** | La cuenta de administrador del servidor | Es la cuenta que especificó cuando creó el servidor. |
   | **Contraseña** | La contraseña de la cuenta de administrador del servidor | Es la contraseña que especificó cuando creó el servidor. |

    ![Conexión al servidor con SSMS](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-1.png)

3. **Conéctese a la base de datos de SSISDB**. Seleccione **Opciones** para expandir el cuadro de diálogo **Conectar con el servidor**. En el cuadro de diálogo **Conectar con el servidor** expandido, seleccione la pestaña **Propiedades de conexión**. En el campo **Conectar con base de datos**, seleccione o escriba `SSISDB`.

    > [!IMPORTANT]
    > Si no selecciona `SSISDB` cuando se conecte, es posible que no vea el catálogo de SSIS en el Explorador de objetos.

    ![Selección de la base de datos SSISDB para la conexión](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-2.png)

4. A continuación, seleccione **Conectar**.

5. En el Explorador de objetos, expanda la opción **Catálogos de Integration Services** y, a continuación, expanda **SSISDB** para ver los objetos de la base de datos del catálogo de SSIS.

    ![Búsqueda de la base de datos SSISDB en el Explorador de objetos de SSMS](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-3.png)

## <a name="next-steps"></a>Pasos siguientes
- Implementar un paquete. Para obtener más información, consulte [Deploy an SSIS project with SQL Server Management Studio (SSMS) (Implementar un proyecto de SSIS con SQL Server Management Studio [SSMS])](../ssis-quickstart-deploy-ssms.md).
- Ejecutar un paquete. Para obtener más información, consulte [Run an SSIS package with SQL Server Management Studio (SSMS) (Ejecutar un paquete de SSIS con SQL Server Management Studio [SSMS])](../ssis-quickstart-run-ssms.md).
- Programar un paquete. Para obtener más información, vea [Programar paquetes SSIS en Azure](ssis-azure-schedule-packages.md).