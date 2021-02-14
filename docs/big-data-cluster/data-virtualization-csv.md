---
title: Virtualización de datos CSV del bloque de almacenamiento
subtitle: Clústeres de macrodatos de SQL Server
description: Pasos que detallan la creación de una tabla externa para la virtualización de un archivo CSV en un clúster de macrodatos
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 04/24/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: polybase
monikerRange: '>= sql-server-ver15'
ms.metadata: seo-lt-2019
ms.openlocfilehash: 7b78e44ee36e9320688a8a6815fbb267f1663d3d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038675"
---
# <a name="virtualize-csv-data-from-storage-pool-big-data-clusters"></a>Virtualización de datos CSV del bloque de almacenamiento (clústeres de macrodatos)

La característica Clústeres de macrodatos de SQL Server puede virtualizar datos de archivos CSV en HDFS. Este proceso permite que los datos permanezcan en su ubicación original, pero se pueden consultar desde una instancia de SQL Server como cualquier otra tabla. Esta característica usa conectores de PolyBase y minimiza la necesidad de procesos ETL. Vea [¿Qué es PolyBase?](../relational-databases/polybase/polybase-guide.md) para más información sobre la virtualización de datos.

## <a name="prerequisites"></a>Prerrequisitos

- [Un clúster de macrodatos implementado](deployment-guidance.md)
- [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)

## <a name="select-or-upload-a-csv-file-for-data-virtualization"></a>Selección o carga de un archivo CSV para la virtualización de datos 

En Azure Data Studio (ADS), [conéctese a la instancia maestra de SQL Server](connect-to-big-data-cluster.md#master) del clúster de macrodatos. Una vez conectado, expanda los elementos HDFS en el Explorador de objetos para buscar los archivos CSV que quiere virtualizar. 

Para los fines de este tutorial, cree un directorio llamado **Datos**.

1. Haga clic con el botón derecho en el menú contextual del directorio raíz de HDFS.
2. Haga clic en **Nuevo directorio**.
3. Asigne el nombre *Datos* al nuevo directorio.

Cargue datos de ejemplo. Para un tutorial sencillo, puede usar un archivo de datos CSV de ejemplo. En este artículo se usan los datos de la causa del retraso de las líneas aéreas del [Departamento de Transporte de EE. UU.](https://www.transtats.bts.gov/OT_Delay/OT_DelayCause1.asp?pn=1) Descargue los datos sin procesar y extraiga los datos en el equipo. Asigne el nombre *airline_delay_causes.csv* al archivo.

Para cargar el archivo de ejemplo después de extraerlo:

1. En Azure Data Studio, *haga clic con el botón derecho* en el directorio que ha creado. 
2. Haga clic en **Cargar archivos**.

![Archivo CSV de ejemplo en HDFS](media/data-virtualization/100-csv-sample-file-hdfs.png)

Azure Data Studio carga los archivos en HDFS en el clúster de macrodatos.

## <a name="create-the-storage-pool-external-data-source-in-your-target-database"></a>Creación del origen de datos externo del bloque de almacenamiento en la base de datos de destino

Los orígenes de datos externos del bloque de almacenamiento ya no se crean de forma predeterminada en el clúster de macrodatos. Para poder crear la tabla externa, cree el origen de datos externo predeterminado **SqlStoragePool** en la base de datos de destino con la siguiente consulta de Transact-SQL. Asegúrese de cambiar primero el contexto de la consulta a la base de datos de destino.

```sql
-- Create the default storage pool source for SQL Big Data Cluster
IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlStoragePool')
    CREATE EXTERNAL DATA SOURCE SqlStoragePool
    WITH (LOCATION = 'sqlhdfs://controller-svc/default');
```

## <a name="create-the-external-table"></a>Creación de la tabla externa

En ADS, haga clic con el botón en el archivo CSV y seleccione **Create External Table From CSV File** (Crear tabla externa a partir del archivo CSV) en el menú contextual. También puede crear tablas externas a partir de archivos CSV en un directorio de HDFS si los archivos del directorio siguen el mismo esquema. Esto permitiría la virtualización de los datos en un nivel de directorio sin tener que procesar archivos individuales y obtener un conjunto de resultados combinados de los datos combinados. Azure Data Studio le guiará por los pasos necesarios para crear la tabla externa.

Especifique la base de datos, el origen de datos, un nombre de tabla, el esquema y el nombre del formato de archivo externo de la tabla.

Haga clic en **Next**.

## <a name="preview-data"></a>Vista previa de los datos

Azure Data Studio ofrece una vista previa de los datos importados.

![Captura de pantalla que muestra la ventana Create External Table From CSV File (Crear tabla externa a partir del archivo CSV) con una vista previa de los datos importados.](media/data-virtualization/130-csv-preview-data.png)

Cuando haya terminado de consultar la vista previa, haga clic en **Siguiente** para continuar.

## <a name="modify-columns"></a>Modificación de columnas

En la ventana siguiente, puede modificar las columnas de la tabla externa que quiere crear. Puede modificar el nombre de columna, cambiar el tipo de datos y permitir filas que admitan valores NULL. 

![Captura de pantalla de la ventana Create External Table From CSV File (Crear tabla externa a partir del archivo CSV) que muestra el paso 3: Modificación de columnas.](media/data-virtualization/140-csv-modify-columns.png)

Después de comprobar las columnas de destino, haga clic en **Siguiente**.

## <a name="summary"></a>Resumen

Este paso proporciona un resumen de las selecciones. Proporciona el nombre del servidor SQL Server, el nombre de la base de datos, el nombre de la tabla, el esquema de la tabla e información de la tabla externa. En este paso, tiene la opción de generar un script o crear una tabla. **Generar script** crea un script en T-SQL para crear el origen de datos externo. **Crear tabla** crea el origen de datos externo.

![Pantalla de resumen](media/data-virtualization/150-csv-virtualize-data-summary.png)

Si hace clic en **Crear tabla**, SQL Server crea la tabla externa en la base de datos de destino.

Si hace clic en **Generar script**, Azure Data Studio crea la consulta de T-SQL para crear la tabla externa.

Una vez creada la tabla, ahora se puede realizar consultas directamente con T-SQL a partir de la instancia de SQL Server.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre los clústeres de macrodatos de SQL Server y los escenarios relacionados, consulte [What is SQL Server Big Data Cluster?](big-data-cluster-overview.md) (¿Qué son los clústeres de macrodatos de SQL Server?)
