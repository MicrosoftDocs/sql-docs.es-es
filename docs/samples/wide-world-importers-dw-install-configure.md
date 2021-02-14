---
title: Instalar & configurar la base de datos de ejemplo DW WideWorldImporters
description: Siga estas instrucciones para descargar, instalar y configurar la base de datos de ejemplo WideWorldImportersDW con SQL Server Management Studio.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.date: 08/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||>=aps-pdw-2016||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 70128430c6ea0943605bd29f00597d51dee71d86
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077756"
---
# <a name="wideworldimportersdw-installation-and-configuration"></a>Instalación y configuración de WideWorldImportersDW
[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]
Instrucciones de instalación y configuración para la base de datos WideWorldImportersDW.

- [SQL Server 2016](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016) (o posterior) o [Azure SQL Database](https://azure.microsoft.com/services/sql-database/). Para usar la versión completa del ejemplo, use SQL Server Evaluation/Developer/Enterprise Edition.
- [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md). Para obtener los mejores resultados, use la versión de junio de 2016 o posterior.

## <a name="download"></a>Descargar

La versión más reciente del ejemplo:

[Wide-World-Importers-versión](https://go.microsoft.com/fwlink/?LinkID=800630)

Descargue la copia de seguridad o BacPac de la base de datos WideWorldImportersDW de ejemplo correspondiente a su edición de SQL Server o Azure SQL Database.

El código fuente para volver a crear la base de datos de ejemplo está disponible en la siguiente ubicación. Tenga en cuenta que el rellenado de datos se basa en ETL de la base de datos OLTP (WideWorldImporters):

[Wide-World-Importers-Source](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/sample-scripts)

## <a name="install"></a>Instalar


### <a name="sql-server"></a>SQL Server

Para restaurar una copia de seguridad en una instancia de SQL Server, puede usar Management Studio.

1. Abra SQL Server Management Studio y conéctese a la instancia de SQL Server de destino.
2. Haga clic con el botón secundario en el nodo **bases** de datos y seleccione **restaurar base de datos**.
3. Seleccione **dispositivo** y haga clic en el botón **..** .
4. En el cuadro de diálogo **seleccionar dispositivos de copia de seguridad**, haga clic en **Agregar**, desplácese hasta la copia de seguridad de base de datos en el sistema de archivos del servidor y seleccione la copia de seguridad. Haga clic en **OK**.
5. Si es necesario, cambie la ubicación de destino de los archivos de datos y de registro, en el panel **archivos** . Tenga en cuenta que se recomienda colocar los archivos de datos y de registro en unidades diferentes.
6. Haga clic en **OK**. Se iniciará la restauración de la base de datos. Una vez finalizado, tendrá la base de datos WideWorldImporters instalada en la instancia de SQL Server.

### <a name="azure-sql-database"></a>Azure SQL Database

Para importar un BacPac en un nuevo SQL Database, puede usar Management Studio.

1. opta Si aún no tiene un SQL Server en Azure, vaya al [Azure portal](https://portal.azure.com/) y cree un nuevo SQL Database. En el proceso de creación de una base de datos, creará un servidor. Tome nota del servidor.
   - Vea [este tutorial](/azure/azure-sql/database/single-database-create-quickstart) para crear una base de datos en minutos
2. Abra SQL Server Management Studio y conéctese al servidor en Azure.
3. Haga clic con el botón secundario en el nodo **bases de datos** y seleccione **importar Data-Tier aplicación**.
4. En la **configuración de importación** , seleccione **Importar desde el disco local** y seleccione el BacPac de la base de datos de ejemplo en el sistema de archivos.
5. En **configuración de base de datos** , cambie el nombre de la base de datos a *WideWorldImportersDW* y seleccione la edición de destino y el objetivo de servicio que se va a usar.
6. Haga clic en **siguiente** y en **Finalizar** para comenzar la implementación. Esta operación tardará algunos minutos en completarse. Al especificar un objetivo de servicio inferior a S2, puede tardar más tiempo.

## <a name="configuration"></a>Configuración

[Se aplica a SQL Server 2016 (y versiones posteriores) Developer/Evaluation/Enterprise Edition]

La base de datos de ejemplo puede hacer uso de polybase para consultar archivos en Hadoop o en Azure BLOB Storage. Sin embargo, esa característica no se instala de forma predeterminada con SQL Server: debe seleccionarla durante la instalación de SQL Server. Por lo tanto, se requiere un paso posterior a la instalación.

1. En SQL Server Management Studio, conéctese a la base de datos WideWorldImportersDW y abra una nueva ventana de consulta.
2. Ejecute el siguiente comando de T-SQL para habilitar el uso de polybase en la base de datos:

   Ejecute [Application]. [Configuration_ApplyPolyBase]