---
title: 'Tutorial: Desarrollo de un modelo de agrupación en clústeres en R'
titleSuffix: SQL Machine Learning
description: En esta serie de tutoriales de cuatro partes, desarrollará un modelo para realizar la agrupación en clústeres en R con el aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/26/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 6a02b31b6427659b93e28c666fb0efdb471a1fcf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348117"
---
# <a name="tutorial-develop-a-clustering-model-in-r-with-sql-machine-learning"></a>Tutorial: Desarrollo de un modelo de agrupación en clústeres en R con el aprendizaje automático de SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
En esta serie de tutoriales de cuatro partes, usará R para desarrollar e implementar un modelo de agrupación en clústeres k-means en [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) o [clústeres de macrodatos](../../big-data-cluster/machine-learning-services.md) para categorizar datos de clientes.
::: moniker-end
::: moniker range="=sql-server-2017"
En esta serie de tutoriales de cuatro partes, usará R para desarrollar e implementar un modelo de agrupación en clústeres k-means en [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) para agrupar en clústeres datos de clientes.
::: moniker-end
::: moniker range="=sql-server-2016"
En esta serie de tutoriales de cuatro partes, usará R para desarrollar e implementar un modelo de agrupación en clústeres k-means en [SQL Server R Services](../r/sql-server-r-services.md) para agrupar en clústeres datos de clientes.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
En esta serie de tutoriales de cuatro partes, usará R para desarrollar e implementar un modelo de agrupación en clústeres k-means en [Machine Learning Services en Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) para agrupar en clústeres datos de clientes.
::: moniker-end

En la primera parte de esta serie, configurará los requisitos previos para el tutorial y, después, restaurará un conjunto de datos de ejemplo en una base de datos. En las partes dos y tres, desarrollará scripts de R en un cuaderno de Azure Data Studio para analizar y preparar los datos de ejemplo y entrenar un modelo de Machine Learning. Luego, en la parte cuatro, ejecutará esos scripts de R en una base de datos mediante procedimientos almacenados.

*Agrupar en clústeres* es organizar datos en grupos, donde los miembros de un grupo son de alguna forma similares. Para esta serie de tutoriales, imagine que es el propietario de un negocio de venta al por menor. Usará el algoritmo **k-means** para realizar la agrupación de clientes en clústeres en un conjunto de datos de compras y devoluciones de productos. Al agrupar los clientes en clústeres, puede centrar sus actividades de marketing de forma más eficaz al dirigirse a grupos específicos. La agrupación en clústeres k-means es un algoritmo de *aprendizaje no supervisado* que analiza patrones en datos basándose en similitudes.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Restauración de una base de datos de ejemplo

En la [parte dos](r-clustering-model-prepare-data.md), aprenderá a preparar los datos de una base de datos para realizar la agrupación en clústeres.

En la [parte tres](r-clustering-model-build.md), aprenderá a crear y entrenar un modelo de agrupación en clústeres k-means en R.

En la [parte cuatro](r-clustering-model-deploy.md), aprenderá a crear un procedimiento almacenado en una base de datos que pueda realizar la agrupación en clústeres en R basándose en datos nuevos.

## <a name="prerequisites"></a>Prerrequisitos

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
* [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) con la opción de lenguaje de Python: siga las instrucciones de instalación en la [Guía de instalación para Windows](../install/sql-machine-learning-services-windows-install.md) o la [Guía de instalación para Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%252fsql%252fmachine-learning%252ftoc.json&view=sql-server-linux-ver15&preserve-view=true). También puede [habilitar Machine Learning Services en clústeres de macrodatos de SQL Server](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
* [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) con la opción de lenguaje de R: siga las instrucciones de instalación en la [Guía de instalación para Windows](../install/sql-machine-learning-services-windows-install.md).
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
* Machine Learning Services en Azure SQL Managed Instance. Para obtener información, vea [Machine Learning Services de Instancia administrada de Azure SQL (versión preliminar)](/azure/azure-sql/managed-instance/machine-learning-services-overview).

* [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) para restaurar la base de datos de ejemplo en Azure SQL Managed Instance.
::: moniker-end

* [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) Usará un cuaderno en Azure Data Studio para SQL. Para obtener más información sobre los cuadernos, vea [Uso de los cuadernos en Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md).

* IDE de R: en este tutorial se usa [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/).

* RODBC: este controlador se usa en los scripts de R que va a desarrollar en este tutorial. Si aún no está instalado, instálelo con el comando `install.packages("RODBC")` de R. Para obtener más información sobre RODBC, vea [CRAN: paquete RODBC](https://CRAN.R-project.org/package=RODBC).

## <a name="restore-the-sample-database"></a>Restauración de la base de datos de ejemplo

El conjunto de datos de ejemplo usado en este tutorial se ha guardado en un archivo de copia de seguridad de base de datos **.bak** para que pueda descargarlo y usarlo. Este conjunto de datos se basa en el conjunto de datos [tpcx-bb](http://www.tpc.org/tpcx-bb/default5.asp) proporcionado por [Transaction Processing Performance Council (TPC)](http://www.tpc.org/).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> Si usa Machine Learning Services en clústeres de macrodatos, consulte [Restauración de una base de datos en la instancia maestra del clúster de macrodatos de SQL Server](../../big-data-cluster/data-ingestion-restore-database.md).
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
1. Descargue el archivo [tpcxbb_1gb.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bak).

1. Siga las indicaciones de [Restauración de una base de datos a partir de un archivo de copia de seguridad](../../azure-data-studio/tutorial-backup-restore-sql-server.md#restore-a-database-from-a-backup-file) en Azure Data Studio con estos datos:

   * Importe el archivo **tpcxbb_1gb.bak** que ha descargado.
   * Asigne a la base de datos de destino el nombre "tpcxbb_1gb".

1. Para asegurarse de que el conjunto de datos exista después de restaurar la base de datos, ejecute la siguiente consulta en la tabla **dbo.customer**:

    ```sql
    USE tpcxbb_1gb;
    SELECT * FROM [dbo].[customer];
    ```
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
1. Descargue el archivo [tpcxbb_1gb.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bak).

1. Siga las instrucciones de [Restauración de una base de datos en un Instancia administrada](/azure/sql-database/sql-database-managed-instance-get-started-restore) en SQL Server Management Studio, con los detalles siguientes:

   * Importe el archivo **tpcxbb_1gb.bak** que ha descargado.
   * Asigne a la base de datos de destino el nombre "tpcxbb_1gb".

1. Para asegurarse de que el conjunto de datos exista después de restaurar la base de datos, ejecute la siguiente consulta en la tabla **dbo.customer**:

    ```sql
    USE tpcxbb_1gb;
    SELECT * FROM [dbo].[customer];
    ```
::: moniker-end

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no quiere continuar con este tutorial, elimine la base de datos tpcxbb_1gb.

## <a name="next-steps"></a>Pasos siguientes

En la parte uno de esta serie de tutoriales, ha completado estos pasos:

* Instalación de los requisitos previos
* Restauración de una base de datos de ejemplo

Para preparar los datos para el modelo de aprendizaje automático, siga la parte dos de esta serie de tutoriales:

> [!div class="nextstepaction"]
> [Preparación de datos para realizar la agrupación en clústeres](r-clustering-model-prepare-data.md)