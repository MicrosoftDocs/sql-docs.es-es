---
title: Programar paquetes SSIS en Azure con SSMS | Microsoft Docs
description: Describe cómo programar paquetes SSIS que se implementan en Azure SQL Database usando el comando Schedule en SQL Server Management Studio (SSMS).
ms.date: 09/23/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 2a4460dadef71d61cb01ce67c27438cee2054857
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351985"
---
# <a name="schedule-the-execution-of-ssis-packages-deployed-in-azure-with-sql-server-management-studio-ssms"></a>Programar la ejecución de paquetes SSIS implementados en Azure con SQL Server Management Studio (SSMS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Puede usar SQL Server Management Studio (SSMS) para programar paquetes SSIS implementados en Azure SQL Database. SQL Server local SQL Managed Instance tienen el Agente SQL Server y el Agente de Instancia administrada respectivamente como programador de trabajos SSIS de primera clase. Por otro lado, SQL Database no tiene ningún programador de trabajos SSIS de primera clase integrado. La característica de SSMS descrita en este artículo proporciona una interfaz de usuario conocida que se parece al Agente SQL Server para programar paquetes implementados en SQL Database.

Si usa SQL Database para hospedar el catálogo de SSIS, `SSISDB`, puede usar esta característica de SSMS para generar las canalizaciones, las actividades y los desencadenadores de Data Factory necesarios para programar los paquetes SSIS. Después, tiene la opción de editar y extender estos objetos en Data Factory.

Si usa SSMS para programar un paquete, SSIS crea automáticamente tres objetos de Data Factory, con nombres que se basan en el nombre del paquete seleccionado y en la marca de tiempo. Por ejemplo, si el nombre del paquete SSIS es **MyPackage**, SSMS crea objetos de Data Factory parecidos a los siguientes:

| Object | Nombre |
|---|---|
| Canalización | **Pipeline_MyPackage_2018-05-08T09_00_00Z** |
| Ejecutar una actividad de paquete SSIS | **Activity_MyPackage_2018-05-08T09_00_00Z** |
| Desencadenador | **Trigger_MyPackage_2018-05-08T09_00_00Z** |
|||

## <a name="prerequisites"></a>Prerequisites

La característica descrita en este artículo require SQL Server Management Studio (versión 17.7 o posterior). Para obtener la versión más reciente de SSMS, vea [Download SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) [Descargar SQL Server Management Studio (SSMS)].

## <a name="schedule-a-package-in-ssms"></a>Programar un paquete en SSMS

1. En SSMS, en el Explorador de objetos, seleccione la base de datos SSISDB, seleccione una carpeta, seleccione un proyecto y, después, seleccione un paquete. Haga clic con el botón derecho en el paquete y seleccione **Programar**.

    ![Seleccione un paquete para programarlo.](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image1-schedule.png)

2. Se abrirá el cuadro de diálogo **Nueva programación**. En la página **General** del cuadro de diálogo **Nueva programación**, escriba un nombre y una descripción para el nuevo trabajo programado.

    ![Página General del cuadro de diálogo Nueva programación](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image2-new-schedule.png)

3. En la página **Paquete** del cuadro de diálogo **Nueva programación**, seleccione opciones de tiempo de ejecución opcionales y un entorno de tiempo de ejecución.

    ![Página Paquete del cuadro de diálogo Nueva programación](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image3-new-schedule2.png)

4. En la página **Programación** del cuadro de diálogo **Nueva programación**, indique las opciones de configuración de la programación, como la frecuencia, la hora y la duración.

    ![Página Programación del cuadro de diálogo Nueva programación](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image4-new-schedule3.png)

5. Cuando haya terminado de crear el trabajo en el cuadro de diálogo **Nueva programación**, aparecerá una confirmación para recordarle los nuevos objetos de Data Factory que va a crear SSMS. Si selecciona **Sí** en el cuadro de diálogo de confirmación, se abrirá la nueva canalización de Data Factory en Azure Portal para que la revise y la personalice.

    ![Confirmación de la nueva programación](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image5-confirmation.png)

6. Para personalizar el desencadenador de programación, seleccione **Nuevo/Editar** en el menú **Desencadenador**.

    ![Editar opcionalmente la nueva canalización](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image6-edit.png)

    Se abre la hoja **Editar desencadenador** para que pueda personalizar las opciones de programación.

    ![Editar desencadenador](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image7-edit2.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre otros métodos para programar un paquete SSIS, vea [Programar la ejecución de un paquete SSIS en Azure](ssis-azure-schedule-packages.md).

Para obtener más información sobre las canalizaciones, las actividades y los desencadenadores de Azure Data Factory, vea los siguientes artículos:
-   [Canalizaciones y actividades en Azure Data Factory](/azure/data-factory/concepts-pipelines-activities)
-   [Ejecución y desencadenadores de canalización en Azure Data Factory](/azure/data-factory/concepts-pipeline-execution-triggers)