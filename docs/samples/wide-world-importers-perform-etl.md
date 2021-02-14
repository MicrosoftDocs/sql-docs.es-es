---
title: 'WideWorldImportersDW: flujo de trabajo ETL | Microsoft Docs'
description: Use el paquete ETL con SQL Server Integration Services (SSIS) para migrar periódicamente datos de la base de datos WideWorldImporters a WideWorldImportersDW.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 04/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: ce9cd8ee5a4153cb1d744d505a26b400497b460b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354056"
---
# <a name="wideworldimportersdw-etl-workflow"></a>Flujo de trabajo ETL de WideWorldImportersDW
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
Use el *WWI_Integration* paquete ETL para migrar datos de la base de datos WideWorldImporters a la base de datos WideWorldImportersDW a medida que los datos cambian. El paquete se ejecuta periódicamente (normalmente diariamente).

El paquete garantiza un alto rendimiento mediante el uso de SQL Server Integration Services para orquestar operaciones de T-SQL masivas (en lugar de transformaciones independientes en Integration Services).

Las dimensiones se cargan primero y, a continuación, se cargan las tablas de hechos. Puede volver a ejecutar el paquete en cualquier momento después de un error.

El flujo de trabajo tiene el siguiente aspecto:

 ![Flujo de trabajo ETL de WideWorldImporters](media/wide-world-importers/wideworldimporters-etl-workflow.png)

El flujo de trabajo se inicia con una tarea expresión que determina el tiempo límite adecuado. La hora límite es la hora actual menos unos minutos. (Este enfoque es más sólido que solicitar datos directamente a la hora actual). Los milisegundos se truncan a partir de la hora.

El procesamiento principal comienza rellenando la tabla de dimensiones Date. El procesamiento garantiza que todas las fechas del año en curso se han rellenado en la tabla.

A continuación, una serie de tareas flujo de datos carga cada dimensión. A continuación, carga cada hecho.

## <a name="prerequisites"></a>Requisitos previos

- SQL Server 2016 (o posterior), con las bases de datos WideWorldImporters y WideWorldImportersDW (en la misma instancia o en instancias diferentes de SQL Server)
- SQL Server Management Studio
- SQL Server 2016 Integration Services
  - Asegúrese de crear un catálogo de Integration Services. Para crear un catálogo de Integration Services, en SQL Server Management Studio Explorador de objetos, haga clic con el botón secundario en **Integration Services** y, a continuación, seleccione **Agregar Catálogo**. Deje las opciones predeterminadas. Se le pedirá que habilite SQLCLR y proporcione una contraseña.


## <a name="download"></a>Descargar

Para obtener la versión más reciente del ejemplo, consulte [Wide-World-Importers-Release](https://go.microsoft.com/fwlink/?LinkID=800630). Descargue el archivo de paquete de Integration Services *ETL diario. ISPAC* .

Para que el código fuente vuelva a crear la base de datos de ejemplo, consulte [Wide-World-Importers](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/wwi-ssis).

## <a name="install"></a>Instalar

1. Implemente el paquete de Integration Services:
   1. En el explorador de Windows, abra el paquete *ETL. ISPAC diario* . Esto inicia el Asistente para la implementación de SQL Server Integration Services.
   2. En **Seleccionar origen**, siga los valores predeterminados para la implementación del proyecto, con la ruta de acceso que apunta al paquete *ETL. ISPAC diario* .
   3. En **Seleccionar destino**, escriba el nombre del servidor que hospeda el catálogo de Integration Services.
   4. Seleccione una ruta de acceso en el catálogo de Integration Services, por ejemplo, en una carpeta nueva denominada *WideWorldImporters*.
   5. Seleccione **implementar** para finalizar el asistente.

2. Cree un trabajo Agente SQL Server para el proceso ETL:
   1. En Management Studio, haga clic con el botón secundario en **Agente SQL Server** y seleccione **nuevo**  >  **trabajo**.
   2. Escriba un nombre, por ejemplo, *WIDEWORLDIMPORTERS ETL*.
   3. Agregue un **paso de trabajo** del tipo **SQL Server Integration Services paquete**.
   4. Seleccione el servidor que contiene el catálogo de Integration Services y, a continuación, seleccione el paquete *ETL diario* .
   5. En   >  **administradores de conexión** de configuración, asegúrese de que las conexiones con el origen y el destino están configuradas correctamente. El valor predeterminado es conectarse a la instancia local.
   6. Seleccione **Aceptar** para crear el trabajo.

3. Ejecute o programe el trabajo.
