---
title: 'Tutorial de Python: Crear características de datos'
titleSuffix: SQL machine learning
description: En la parte tres de esta serie de tutoriales de cinco partes, agregará cálculos a los procedimientos almacenados para usarlos en los modelos de aprendizaje automático de Python con el aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 630a5db712e3bd6e288c0ac12ca767a07c3ac4ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339712"
---
# <a name="python-tutorial-create-data-features-using-t-sql"></a>Tutorial de Python: Creación de características de datos mediante T-SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

En la parte tres de esta serie de tutoriales de cinco partes, aprenderá a crear características a partir de datos sin procesar mediante una función [!INCLUDE[tsql](../../includes/tsql-md.md)]. Después, llamará a esa función desde un procedimiento almacenado de SQL para crear una tabla que contenga los valores de las características.

El proceso de *caracterización*, la creación de características a partir de los datos sin procesar, puede ser un paso crítico en el modelado de análisis avanzados.

En este artículo, hará lo siguiente:

> [!div class="checklist"]
> + Modificará una función personalizada para calcular la distancia de la carrera
> + Guardará las características mediante otra función personalizada

En la [parte uno](python-taxi-classification-introduction.md), ha instalado los requisitos previos y ha restaurado la base de datos de ejemplo.

En la [parte dos](python-taxi-classification-explore-data.md), ha explorado los datos de ejemplo y ha generado algunos trazados.

En la [parte cuatro](python-taxi-classification-train-model.md), cargará los módulos y llamará a las funciones necesarias para crear y entrenar el modelo mediante un procedimiento almacenado de SQL Server.

En la [parte cinco](python-taxi-classification-deploy-model.md), aprenderá a poner en marcha los modelos entrenados y guardados en la parte cuatro.

## <a name="define-the-function"></a>Definir la función

Los valores de distancia notificados en los datos originales se basan en la distancia notificada del taxímetro y no representan necesariamente la distancia geográfica o la distancia recorrida. Por tanto, debe calcular la distancia directa entre los puntos de origen y destino, usando las coordenadas disponibles en el conjunto de datos de origen NYC Taxi. Puede hacerlo mediante la [fórmula Haversine](https://en.wikipedia.org/wiki/Haversine_formula) en un función personalizada de [!INCLUDE[tsql](../../includes/tsql-md.md)] .

Usará una función personalizada de T-SQL, _fnCalculateDistance_, para calcular la distancia usando la fórmula Haversine, y una segunda función personalizada de T-SQL, _fnEngineerFeatures_, para crear una tabla que contiene todas las características.

### <a name="calculate-trip-distance-using-fncalculatedistance"></a>Cálculo de la distancia del trayecto con fnCalculateDistance

1. La función _fnCalculateDistance_ se incluye en la base de datos de ejemplo. Tómese un minuto para revisar el código.
  
2. En [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], expanda **Programación**, expanda **Funciones** y, después, **Funciones escalares**.
   Haga clic con el botón derecho en _fnCalculateDistance_ y seleccione **Modificar** para abrir el script de [!INCLUDE[tsql](../../includes/tsql-md.md)] en una nueva ventana de consulta.
  
   ```sql
   CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)
   -- User-defined function that calculates the direct distance between two geographical coordinates
   RETURNS float
   AS
   BEGIN
     DECLARE @distance decimal(28, 10)
     -- Convert to radians
     SET @Lat1 = @Lat1 / 57.2958
     SET @Long1 = @Long1 / 57.2958
     SET @Lat2 = @Lat2 / 57.2958
     SET @Long2 = @Long2 / 57.2958
     -- Calculate distance
     SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
     --Convert to miles
     IF @distance <> 0
     BEGIN
       SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
     END
     RETURN @distance
   END
   GO
   ```

**Notas:**

+ La función es una función escalar y devuelve un único valor de datos de un tipo predefinido.
+ Toma los valores de latitud y longitud como entradas, obtenidos de las ubicaciones de origen y destino de los viajes. La fórmula Haversine convierte ubicaciones en radianes y usa esos valores para calcular la distancia directa en millas entre las dos ubicaciones.

Para agregar el valor calculado a una tabla y poder usarlo para entrenar el modelo, deberá usar otra función, _fnEngineerFeatures_.

### <a name="save-the-features-using-_fnengineerfeatures_"></a>Guardado de las características mediante _fnEngineerFeatures_

1. Dedique un minuto a revisar el código de la función T-SQL personalizada, _fnEngineerFeatures_, que se incluye en la base de datos de ejemplo.
  
   Esta función es una función con valores de tabla que toma varias columnas como entradas y genera una tabla con varias columnas de características.  El propósito de esta función es crear un conjunto de características para usar en la creación de un modelo. La función _fnEngineerFeatures_ llama a la función de T-SQL creada anteriormente, _fnCalculateDistance_, para obtener la distancia directa entre las ubicaciones de origen y destino.
  
   ```sql
   CREATE FUNCTION [dbo].[fnEngineerFeatures] (
   @passenger_count int = 0,
   @trip_distance float = 0,
   @trip_time_in_secs int = 0,
   @pickup_latitude float = 0,
   @pickup_longitude float = 0,
   @dropoff_latitude float = 0,
   @dropoff_longitude float = 0)
   RETURNS TABLE
   AS
     RETURN
     (
     -- Add the SELECT statement with parameter references here
     SELECT
       @passenger_count AS passenger_count,
       @trip_distance AS trip_distance,
       @trip_time_in_secs AS trip_time_in_secs,
       [dbo].[fnCalculateDistance](@pickup_latitude, @pickup_longitude, @dropoff_latitude, @dropoff_longitude) AS direct_distance
     )
   GO
   ```
  
2. Para comprobar que esta función funciona, puede usarla para calcular la distancia geográfica de los viajes en los que la distancia medida era 0 pero las ubicaciones de origen y destino eran diferentes.
  
   ```sql
       SELECT tipped, fare_amount, passenger_count,(trip_time_in_secs/60) as TripMinutes,
       trip_distance, pickup_datetime, dropoff_datetime,
       dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) AS direct_distance
       FROM nyctaxi_sample
       WHERE pickup_longitude != dropoff_longitude and pickup_latitude != dropoff_latitude and trip_distance = 0
       ORDER BY trip_time_in_secs DESC
   ```
  
   Como puede ver, la distancia notificada por el taxímetro no siempre se corresponde con la distancia geográfica. Por eso es importante la ingeniería de características.

En la parte siguiente, aprenderá a usar estas características de datos para crear y entrenar un modelo de aprendizaje automático con Python.

## <a name="next-steps"></a>Pasos siguientes

En este artículo:

> [!div class="checklist"]
> + Se modificó una función personalizada para calcular la distancia de la carrera
> + Se guardaron las características mediante otra función personalizada

> [!div class="nextstepaction"]
> [Tutorial de Python: Entrenar y guardar un modelo de Python mediante T-SQL](python-taxi-classification-train-model.md)