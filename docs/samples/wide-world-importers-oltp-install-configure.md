---
title: Instalar y configurar la base de datos de ejemplo WideWorldImporters
description: Siga estas instrucciones para descargar, instalar y configurar la base de datos de ejemplo WideWorldImporters con SQL Server Management Studio.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.date: 04/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: 588597398c11b9b374bdecd272162fe82053cd8f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354086"
---
# <a name="installation-and-configuration"></a>Instalación y configuración
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
Instrucciones de configuración e instalación de bases de datos OLTP de Wide World Importers.

## <a name="prerequisites"></a>Requisitos previos

- [SQL Server 2016](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016) (o posterior) o [Azure SQL Database](https://azure.microsoft.com/services/sql-database/). Para obtener la versión completa del ejemplo, use SQL Server Evaluation/Developer/Enterprise Edition.
- [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md). Para obtener los mejores resultados, use la versión de junio de 2016 o posterior.

## <a name="download"></a>Descargar

La versión más reciente del ejemplo:

[Wide-World-Importers-versión](https://go.microsoft.com/fwlink/?LinkID=800630)

Descargue la copia de seguridad o BacPac de la base de datos WideWorldImporters de ejemplo correspondiente a su edición de SQL Server o Azure SQL Database.

El código fuente para volver a crear la base de datos de ejemplo está disponible en la siguiente ubicación. Tenga en cuenta que al volver a crear el ejemplo se producirán pequeñas diferencias en los datos, ya que hay un factor aleatorio en la generación de datos:

[Wide-World-Importers](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/sample-scripts)

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
5. En **configuración de base de datos** , cambie el nombre de la base de datos a *WideWorldImporters* y seleccione la edición de destino y el objetivo de servicio que se va a usar.
6. Haga clic en **siguiente** y en **Finalizar** para comenzar la implementación. Tardará unos minutos en completarse en P1. Si se desea un plan de tarifa inferior, se recomienda importar en una nueva base de datos P1 y, a continuación, cambiar el plan de tarifa al nivel deseado.

## <a name="configuration"></a>Configuración

### <a name="full-text-indexing"></a>Indización de texto completo

La base de datos de ejemplo puede hacer uso de la indización de Full-Text. Sin embargo, esa característica no se instala de forma predeterminada con SQL Server: debe seleccionarla durante la instalación de SQL Server (está habilitada de forma predeterminada en Azure SQL Database). Por lo tanto, se requiere un paso posterior a la instalación.

1. En SQL Server Management Studio, conéctese a la base de datos WideWorldImporters y abra una nueva ventana de consulta.
2. Ejecute el siguiente comando de T-SQL para habilitar el uso de la indización de Full-Text en la base de datos:  `EXECUTE Application.Configuration_ApplyFullTextIndexing`


### <a name="sql-server-audit"></a>SQL Server Audit

Se aplica a: SQL Server

La habilitación de la auditoría en SQL Server requiere la configuración del servidor. Para habilitar SQL Server Audit para el ejemplo WideWorldImporters, ejecute la siguiente instrucción en la base de datos:

```sql
EXECUTE [Application].[Configuration_ApplyAuditing]
```

En Azure SQL Database, la auditoría se configura a través del [Azure portal](https://portal.azure.com/).

### <a name="row-level-security"></a>Seguridad de nivel de fila

Se aplica a: Azure SQL Database

Row-Level seguridad no está habilitada de forma predeterminada en la descarga de BacPac de WideWorldImporters. Para habilitar la seguridad de Row-Level en la base de datos, ejecute el siguiente procedimiento almacenado:

```sql
EXECUTE [Application].[Configuration_ApplyRowLevelSecurity]
```