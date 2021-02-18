---
title: Datos de demostración de los taxis de Nueva York para tutoriales
description: Cree una base de datos que contenga los datos de ejemplo de los taxis de Nueva York. Este conjunto de datos se utiliza en los tutoriales de R y Python para SQL Server Machine Learning Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/31/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 0a62131735fb5612af44f67cdbb03359563019f3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347338"
---
# <a name="nyc-taxi-demo-data-for-sql-server-python-and-r-tutorials"></a>Datos de demostración de los taxis de Nueva York para tutoriales de Python y R en SQL Server
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

En este artículo se explica cómo configurar una base de datos de ejemplo formada por datos públicos procedentes de la [Comisión de taxis y limusinas de la ciudad de Nueva York](http://www.nyc.gov/html/tlc/html/about/trip_record_data.shtml). Estos datos se usan en varios tutoriales de R y Python para el análisis de bases de datos en SQL Server. Para que el código de ejemplo se ejecute más rápidamente, hemos creado una muestra representativa del 1 % de los datos. En el sistema, el archivo de copia de seguridad de base de datos es ligeramente superior a 90 MB, lo que proporciona 1,7 millones de filas en la tabla de datos principal.

Para completar este ejercicio, debe tener [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md?view=sql-server-2017&preserve-view=true) u otra herramienta que pueda restaurar un archivo de copia de seguridad de la base de datos y ejecutar consultas de T-SQL.

Entre los tutoriales y las guías de inicio rápido que usan este conjunto de datos se incluyen los siguientes:

+ [Obtenga información sobre el análisis de bases de datos con R en SQL Server](r-taxi-classification-introduction.md)
+ [Obtenga información sobre el análisis de bases de datos con Python en SQL Server](python-taxi-classification-introduction.md)

## <a name="download-files"></a>Descarga de archivos

La base de datos de ejemplo es un archivo BAK de SQL Server 2016 hospedado por Microsoft. Puede restaurarlo en SQL Server 2016 y versiones posteriores. La descarga de archivos comienza inmediatamente al hacer clic en el vínculo. 

El tamaño del archivo es de aproximadamente 90 MB.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
>[!NOTE]
>Para restaurar la base de datos de ejemplo en [clústeres de macrodatos de SQL Server](../../big-data-cluster/big-data-cluster-overview.md), descargue [NYCTaxi_Sample.bak](https://sqlmldoccontent.blob.core.windows.net/sqlml/NYCTaxi_Sample.bak) y siga las instrucciones de [Restauración de una base de datos en la instancia maestra del clúster de macrodatos de SQL Server](../../big-data-cluster/data-ingestion-restore-database.md).
::: moniker-end

::: moniker range=">=azuresqldb-mi-current"
>[!NOTE]
>Para restaurar la base de datos de ejemplo en [Machine Learning Services, en Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview), siga las instrucciones de [Inicio rápido: Restauración de una base de datos a Azure SQL Managed Instance](/azure/azure-sql/managed-instance/restore-sample-database-quickstart) mediante el archivo. bak de la base de datos de taxis de Nueva York: [https://sqlmldoccontent.blob.core.windows.net/sqlml/NYCTaxi_Sample.bak](https://sqlmldoccontent.blob.core.windows.net/sqlml/NYCTaxi_Sample.bak).
::: moniker-end

1. Haga clic en [NYCTaxi_Sample. bak](https://sqlmldoccontent.blob.core.windows.net/sqlml/NYCTaxi_Sample.bak) para descargar el archivo de copia de seguridad de la base de datos.

2. Copie el archivo en la carpeta C:\Program files\Microsoft SQL Server\MSSQL-instance-name\MSSQL\Backup.

3. En Management Studio, haga clic con el botón derecho en **Bases de datos** y seleccione **Restaurar archivos y grupos de archivos**.

4. Escriba *NYCTaxi_Sample* como nombre de la base de datos.

5. Haga clic en **Desde el dispositivo** y después abra la página de selección de archivos para seleccionar el archivo de copia de seguridad. Haga clic en **Agregar** para seleccionar NYCTaxi_Sample.bak.

6. Active la casilla **Restaurar** y haga clic en **Aceptar** para restaurar la base de datos.

## <a name="review-database-objects"></a>Revisión de los objetos de base de datos
   
Confirme que los objetos de base de datos están en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Tendría que ver la base de datos, las tablas, las funciones y los procedimientos almacenados.
  
   ![rsql_devtut_BrowseTables](media/rsql-devtut-browsetables.png "rsql_devtut_BrowseTables")

### <a name="objects-in-nyctaxi_sample-database"></a>Objetos en la base de datos NYCTaxi_Sample

En la tabla siguiente se resumen los objetos creados en la base de datos de demostración de los taxis de Nueva York.

|**Nombre de objeto**|**Tipo de objeto**|**Descripción**|
|----------|------------------------|---------------|
|**NYCTaxi_Sample** | database | Crea una base de datos y dos tablas:<br /><br />Tabla dbo.nyctaxi_sample: contiene el conjunto de datos NYC Taxi principal. Un índice de almacén de columnas agrupado se agrega a la tabla para mejorar el rendimiento de almacenamiento y de consulta. La muestra del 1 % del conjunto de datos NYC Taxi se inserta en esta tabla.<br /><br />Tabla dbo.nyc_taxi_models: se usa para conservar el modelo de análisis avanzado entrenado.|
|**fnCalculateDistance** |función escalar | Calcula la distancia directa entre las ubicaciones de origen y destino. Esta función se usa al [crear características de datos mediante R y T-SQL](r-taxi-classification-create-features.md), al [entrenar y guardar un modelo](r-taxi-classification-train-model.md) y al [hacer operativo el modelo R](r-taxi-classification-deploy-model.md).|
|**fnEngineerFeatures** |función con valores de tabla | Crea nuevas características de datos para el entrenamiento del modelo. Esta función se usa al [rear características de datos](r-taxi-classification-create-features.md) y al [acer operativo el modelo R](r-taxi-classification-deploy-model.md).|


Los procedimientos almacenados se crean mediante el script de R y Python que se encuentra en varios tutoriales. En la tabla siguiente se resumen los procedimientos almacenados que se pueden agregar opcionalmente a la base de datos de demostración de los taxis de Nueva York al ejecutar un script desde varias lecciones.

|**Procedimiento almacenado**|**Lenguaje**|**Descripción**|
|-------------------------|------------|---------------|
|**RxPlotHistogram** |R | Llama a la función RevoScaleR rxHistogram para trazar el histograma de una variable y, después, devuelve el gráfico como un objeto binario. Este procedimiento almacenado se usa al [xplorar y visualizar los datos](r-taxi-classification-explore-data.md).|
|**RPlotRHist** |R| Crea un gráfico mediante una función de Hist y guarda el resultado como un archivo PDF local. Este procedimiento almacenado se usa al [xplorar y visualizar los datos](r-taxi-classification-explore-data.md).|
|**RxTrainLogitModel**  |R| Entrena un modelo de regresión logística mediante una llamada a un paquete de R. El modelo predice el valor de la columna tipped y se entrena usando un 70 % de los datos seleccionados aleatoriamente. El resultado del procedimiento almacenado es el modelo entrenado, que se guarda en la tabla nyc_taxi_models. Este procedimiento almacenado se usa al [ntrenar y guardar un modelo](r-taxi-classification-train-model.md).|
|**RxPredictBatchOutput**  |R | Llama al modelo entrenado para crear predicciones usando el modelo. El procedimiento almacenado acepta una consulta como su parámetro de entrada y devuelve una columna de valores numéricos que contiene los resultados para las filas de entrada. Este procedimiento almacenado se usa al [redecir posibles resultados](r-taxi-classification-deploy-model.md).|
|**RxPredictSingleRow**  |R| Llama al modelo entrenado para crear predicciones usando el modelo. Este procedimiento almacenado acepta una observación nueva como entrada, con valores de características individuales pasados como parámetros en línea, y devuelve un valor que predice el resultado de la nueva observación. Este procedimiento almacenado se usa al [redecir posibles resultados](r-taxi-classification-deploy-model.md).|

## <a name="query-the-data"></a>Consultar los datos

Como paso de validación, ejecute una consulta para confirmar que se han cargado los datos.

1. En Explorador de objetos, debajo de Bases de datos, haga clic con el botón derecho en la base de datos **NYCTaxi_Sample** e inicie una nueva consulta.

2. Ejecute algunas consultas sencillas:

    ```sql
    SELECT TOP(10) * FROM dbo.nyctaxi_sample;
    SELECT COUNT(*) FROM dbo.nyctaxi_sample;
    ```
La base de datos contiene 1,7 millones de filas.

3. Dentro de la base de datos hay la tabla **nyctaxi_sample**, que contiene el conjunto de datos. Esta tabla se ha optimizado para cálculos basados en conjuntos con la incorporación de un [índice de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-overview.md). Ejecute esta instrucción para generar un resumen rápido en la tabla.

    ```sql
    SELECT DISTINCT [passenger_count]
        , ROUND (SUM ([fare_amount]),0) as TotalFares
        , ROUND (AVG ([fare_amount]),0) as AvgFares
    FROM [dbo].[nyctaxi_sample]
    GROUP BY [passenger_count]
    ORDER BY  AvgFares DESC
    ````
Los resultados deberían ser similares a los que se muestran en la captura de pantalla siguiente.

  ![Tabla con el resumen de la información](media/nyctaxidatatablesummary.png "Resultados de la consulta")

## <a name="next-steps"></a>Pasos siguientes

Ahora están disponibles los datos de ejemplo de los taxis de Nueva York para poder ponerlos en práctica para el aprendizaje.

+ [Obtenga información sobre el análisis de bases de datos con R en SQL Server](r-taxi-classification-introduction.md)
+ [Obtenga información sobre el análisis de bases de datos con Python en SQL Server](python-taxi-classification-introduction.md)