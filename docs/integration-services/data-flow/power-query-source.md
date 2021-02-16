---
title: Origen de Power Query | Microsoft Docs
description: Obtenga información sobre cómo configurar el origen de Power Query del flujo de datos de SQL Server Integration Services (SSIS).
ms.date: 12/27/2019
ms.prod: sql
ms.prod_service: integration-services
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.designer.powerqueryconnmgr.f1
- sql13.ssis.designer.powerquerysource.queries.f1
- sql13.ssis.designer.powerquerysource.connmgrs.f1
- sql14.ssis.designer.powerqueryconnmgr.f1
- sql14.ssis.designer.powerquerysource.queries.f1
- sql14.ssis.designer.powerquerysource.connmgrs.f1
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 43e02595cc57413eb18ff3e390606ea94493912a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336345"
---
# <a name="power-query-source-preview"></a>Origen de Power Query (versión preliminar)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

En este artículo se describe cómo configurar las propiedades del origen de Power Query del flujo de datos de SQL Server Integration Services (SSIS). Power Query es una tecnología que permite conectarse a varios orígenes de datos y transformar datos mediante Excel o Power BI Desktop. Para obtener más información, consulte el artículo [Power Query - Descripción general y aprendizaje](https://support.office.com/article/power-query-overview-and-learning-ed614c81-4b00-4291-bd3a-55d80767f81d). El script generado que genera Power Query se puede copiar y pegar en el origen de Power Query del flujo de datos de SSIS para configurarlo.
  
> [!NOTE]
> En la versión preliminar actual, el origen de Power Query solo se puede usar en SQL Server 2017/2019 y Azure-SSIS Integration Runtime (IR) de Azure Data Factory (ADF). Puede descargar e instalar el origen de Power Query más reciente para SQL Server 2017/2019 desde [aquí](https://www.microsoft.com/download/details.aspx?id=100619). El origen de Power Query para Azure-SSIS IR ya está preinstalado. Para aprovisionar Azure-SSIS IR, lea el artículo de [aprovisionamiento de SSIS en ADF](/azure/data-factory/tutorial-deploy-ssis-packages-azure).

## <a name="configure-the-power-query-source"></a>Configuración del origen de Power Query

Para abrir el **Editor de orígenes de Power Query** en SSDT, arrastre y coloque el **origen de Power Query** desde el cuadro de herramientas de SSIS al diseñador de flujo de datos y haga doble clic en él.  

![Origen de PQ](media/power-query-source/pq-source.png)

Se muestran tres pestañas en el lado izquierdo. En la pestaña **Consultas**, puede seleccionar el modo de consulta en el menú desplegable.
-   El modo **Consulta única** permite copiar y pegar un único script de Power Query desde Excel o Power BI Desktop.
-   El modo **Consulta única a partir de variable** permite especificar una variable de cadena que contiene la consulta que se ejecutará.

![Pestaña Consulta única del origen de PQ](media/power-query-source/pq-source-queries-tab-single.png)

En la pestaña **Administradores de conexiones**, puede agregar o quitar administradores de conexiones de Power Query que contienen las credenciales de acceso del origen de datos. Al seleccionar el botón **Detectar el tipo de origen de datos**, se identifican los orígenes de datos a los que se hace referencia en la consulta y los muestra para que pueda asignar los administradores de conexiones de Power Query existentes adecuados o crear otros.

![Pestaña para detectar administradores de conexiones del origen de PQ](media/power-query-source/pq-source-connection-managers-tab-detect.png)

![Pestaña para agregar administradores de conexiones del origen de PQ](media/power-query-source/pq-source-connection-managers-tab-add.png)

Finalmente, en la pestaña **Columnas**, puede editar la información de las columnas de salida.

![Pestaña Columnas del origen de PQ](media/power-query-source/pq-source-columns-tab.png)

## <a name="configure-the-power-query-connection-manager"></a>Configuración del administrador de conexiones de Power Query

Al diseñar el flujo de datos con el origen de Power Query en SSDT, puede crear un administrador de conexiones de Power Query de las maneras siguientes:
- Créelo de forma indirecta en la pestaña **Administradores de conexión** de Power Query después de seleccionar el botón **Agregar**/**Detectar el origen de datos** y elegir **<New connection...>** en el menú desplegable, como se describió anteriormente.
- Créelo directamente haciendo clic con el botón derecho en el panel **Administradores de conexiones** y seleccionando **Nueva conexión...**  en el menú desplegable.

![Agregar en el panel Administradores de conexiones del origen de PQ](media/power-query-source/pq-source-connection-managers-panel-add.png)

En el cuadro de diálogo **Agregar administrador de conexiones SSIS**, haga doble clic en **PowerQuery** en la lista de tipos de administradores de conexiones.

![Cuadro de diálogo Agregar del panel Administradores de conexiones del origen de PQ](media/power-query-source/pq-source-connection-managers-panel-add-dialog.png)

En el **Editor del administrador de conexiones de Power Query**, deberá especificar los valores de **Tipo de origen de datos**, **Ruta de acceso del origen de datos** y **Tipo de autenticación**, así como asignar las credenciales de acceso adecuadas. En **Tipo de origen de datos**, actualmente puede seleccionar uno de los 22 tipos en el menú desplegable.

![Tipos del Editor del administrador de conexiones del origen de PQ](media/power-query-source/pq-source-connection-manager-editor-kind.png)

Algunos de estos orígenes (**Oracle**, **DB2**, **MySQL**, **PostgreSQL**, **Teradata** y **Sybase**) requieren más instalaciones de controladores ADO.NET que pueden obtenerse en el artículo de [requisitos previos de Power Query](/power-bi/desktop-data-source-prerequisites). Puede usar la interfaz de configuración personalizada para instalarlos en Azure-SSIS IR. Consulte el artículo de [personalización de Azure-SSIS IR](/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

En **Ruta de acceso del origen de datos**, puede especificar propiedades específicas del origen de datos que forman una cadena de conexión sin la información de autenticación. Por ejemplo, la ruta de acceso del origen de datos **SQL** tiene el formato `<Server>;<Database>`. Puede seleccionar el botón **Editar** para asignar valores a propiedades específicas del origen de datos que forman la ruta de acceso.

![Ruta de acceso del Editor del administrador de conexiones de archivos](media/power-query-source/pq-source-connection-manager-editor-path.png)

Finalmente, en **Tipo de autenticación**, puede seleccionar **Anónimo**/**Autenticación de Windows**/**Nombre de usuario y contraseña**/**Clave** en el menú desplegable, escriba las credenciales de acceso adecuadas y seleccione el botón **Probar conexión** para asegurarse de que el origen de Power Query se ha configurado correctamente.

![Autenticación del Editor del administrador de conexiones de origen de PQ](media/power-query-source/pq-source-connection-manager-editor-authentication.png)

### <a name="current-limitations"></a>Limitaciones actuales

-   El origen de datos de **Oracle** no se puede usar actualmente en Azure-SSIS IR, dado que el controlador ADO.NET de Oracle no se puede instalar ahí; de modo que, instálelo y use por ahora el origen de datos **ODBC** para conectarse a Oracle. Para más información, consulte el ejemplo **ORACLE STANDARD ODBC** del artículo sobre la [personalización de Azure SSIS IR](/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

-   El origen de datos **web** no se puede usar actualmente en Azure-SSIS IR con instalaciones personalizadas; por lo tanto, úselo por ahora en Azure-SSIS IR sin ellas.

## <a name="next-steps"></a>Pasos siguientes
Obtenga información sobre cómo ejecutar paquetes SSIS en Azure-SSIS IR como actividades de primera clase en canalizaciones ADF. Consulte el artículo de [ejecución del entorno de ejecución de actividades de paquetes SSiS](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).