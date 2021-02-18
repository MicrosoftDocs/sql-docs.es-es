---
title: 'Tutorial: Desarrollo de un modelo predictivo en R'
titleSuffix: SQL machine learning
description: En esta serie de tutoriales de cuatro partes, desarrollará datos para entrenar un modelo predictivo en R con el aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/26/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 5376d2b7876914c28d656574a57f5fc80294c63c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348046"
---
# <a name="tutorial-develop-a-predictive-model-in-r-with-sql-machine-learning"></a>Tutorial: Desarrollo de un modelo predictivo en R con el aprendizaje automático de SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
En esta serie de tutoriales de cuatro partes, usará R y un modelo de Machine Learning en [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) o en [clústeres de macrodatos](../../big-data-cluster/machine-learning-services.md) para predecir el número de alquileres de esquíes.
::: moniker-end
::: moniker range="=sql-server-2017"
En esta serie de tutoriales de cuatro partes, usará R y un modelo de Machine Learning en [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) para predecir el número de alquileres de esquíes.
::: moniker-end
::: moniker range="=sql-server-2016"
En esta serie de tutoriales de cuatro partes, usará R y un modelo de Machine Learning en [SQL Server R Services](../r/sql-server-r-services.md) para predecir el número de alquileres de esquíes.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
En esta serie de tutoriales de cuatro partes, usará R y un modelo de Machine Learning en [Machine Learning Services en Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) para predecir el número de alquileres de esquíes.
::: moniker-end

Imagine que es el propietario de una empresa de alquiler de esquíes y quiere predecir el número de alquileres que tendrá en una fecha futura. Esta información le ayudará a preparar las existencias, el personal y las instalaciones.

En la primera parte de esta serie, configurará los requisitos previos. En las partes dos y tres, desarrollará scripts de R en un cuaderno para preparar sus datos y entrenar un modelo de Machine Learning. Luego, en la tercera parte, ejecutará esos scripts de R en una base de datos mediante procedimientos almacenados de T-SQL.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Restauración de una base de datos de ejemplo 

En la [parte dos](r-predictive-model-prepare-data.md), aprenderá a cargar los datos desde una base de datos en una trama de datos de Python y a preparar los datos en R.

En la [parte tres](r-predictive-model-train.md), aprenderá a entrenar un modelo de Machine Learning en R.

En la [parte cuatro](r-predictive-model-deploy.md), aprenderá a almacenar el modelo en una base de datos y, luego, a crear procedimientos almacenados a partir de los scripts de R desarrollados en las partes dos y tres. Los procedimientos almacenados se ejecutarán en el servidor para realizar predicciones basándose en datos nuevos.

## <a name="prerequisites"></a>Prerrequisitos

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
* SQL Server Machine Learning Services: para instalar Machine Learning Services, vea la [Guía de instalación para Windows](../install/sql-machine-learning-services-windows-install.md) o la [Guía de instalación para Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json). También puede [habilitar Machine Learning Services en clústeres de macrodatos de SQL Server](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
* SQL Server Machine Learning Services: para instalar Machine Learning Services, vea la [Guía de instalación para Windows](../install/sql-machine-learning-services-windows-install.md). 
::: moniker-end
::: moniker range="=sql-server-2016"
* SQL Server 2016 R Services. Para instalar R Services, consulte la [Guía de instalación de Windows](../install/sql-r-services-windows-install.md). 
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
* Machine Learning Services en Azure SQL Managed Instance. Para obtener información, vea [Machine Learning Services de Instancia administrada de Azure SQL (versión preliminar)](/azure/azure-sql/managed-instance/machine-learning-services-overview).

* [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) para restaurar la base de datos de ejemplo en Azure SQL Managed Instance.
::: moniker-end

* IDE de R: en este tutorial se usa [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/).

* RODBC: este controlador se usa en los scripts de R que va a desarrollar en este tutorial. Si aún no está instalado, instálelo con el comando `install.packages("RODBC")` de R. Para obtener más información sobre RODBC, vea [CRAN: paquete RODBC](https://CRAN.R-project.org/package=RODBC).

* Herramienta de consultas SQL: en este tutorial, se da por hecho que usa [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md). Para más información, vea [Uso de cuadernos en Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md).

## <a name="restore-the-sample-database"></a>Restauración de la base de datos de ejemplo

La base de datos de ejemplo usada en este tutorial se ha guardado en un archivo de copia de seguridad de base de datos **.bak** para que pueda descargarlo y usarlo.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> Si usa Machine Learning Services en clústeres de macrodatos, consulte [Restauración de una base de datos en la instancia maestra del clúster de macrodatos de SQL Server](../../big-data-cluster/data-ingestion-restore-database.md).
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
1. Descargue el archivo [TutorialDB.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/TutorialDB.bak).

1. Siga las indicaciones de [Restauración de una base de datos a partir de un archivo de copia de seguridad](../../azure-data-studio/tutorial-backup-restore-sql-server.md#restore-a-database-from-a-backup-file) en Azure Data Studio con estos datos:

   * Importación del archivo **TutorialDB.bak** que ha descargado
   * Asignación del nombre "TutorialDB" a la base de datos de destino

1. Para comprobar que la base de datos restaurada existe, consulte la tabla **DBO.rental_data**:

   ```sql
   USE TutorialDB;
   SELECT * FROM [dbo].[rental_data];
   ```
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
1. Descargue el archivo [TutorialDB.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/TutorialDB.bak).

1. Siga las instrucciones de [Restauración de una base de datos en un Instancia administrada](/azure/sql-database/sql-database-managed-instance-get-started-restore) en SQL Server Management Studio, con los detalles siguientes:

   * Importación del archivo **TutorialDB.bak** que ha descargado
   * Asignación del nombre "TutorialDB" a la base de datos de destino

1. Para comprobar que la base de datos restaurada existe, consulte la tabla **DBO.rental_data**:

   ```sql
   USE TutorialDB;
   SELECT * FROM [dbo].[rental_data];
   ```
::: moniker-end

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no quiere continuar con este tutorial, elimine la base de datos TutorialDB.
## <a name="next-steps"></a>Pasos siguientes

En la parte uno de esta serie de tutoriales, ha completado estos pasos:

* Instalación de los requisitos previos
* Restauración de una base de datos de ejemplo

Para preparar los datos para el modelo de aprendizaje automático, siga la parte dos de esta serie de tutoriales:

> [!div class="nextstepaction"]
> [Preparación de los datos para entrenar un modelo predictivo en R](r-predictive-model-prepare-data.md)