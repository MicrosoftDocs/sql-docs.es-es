---
title: 'Tutorial de R: Crear características de datos'
titleSuffix: SQL machine learning
description: En la parte tres de esta serie de tutoriales de cinco partes, usará funciones de T-SQL para crear y almacenar características a partir de datos de ejemplo con aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 10/15/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 0dfba9b2e9178d0b389d812881d27341fde7c208
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347789"
---
# <a name="r-tutorial-create-data-features"></a>Tutorial de R: Crear características de datos
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

En la parte tres de esta serie de tutoriales de cinco partes, aprenderá a crear características a partir de datos sin procesar mediante una función [!INCLUDE[tsql](../../includes/tsql-md.md)]. Después, llamará a esa función desde un procedimiento almacenado de SQL para crear una tabla que contenga los valores de las características.

En este artículo, hará lo siguiente:

> [!div class="checklist"]
> + Modificará una función personalizada para calcular la distancia de la carrera
> + Guardará las características mediante otra función personalizada

En la [parte uno](r-taxi-classification-introduction.md), ha instalado los requisitos previos y ha restaurado la base de datos de ejemplo.

En la [parte dos](r-taxi-classification-explore-data.md), revisó los datos de ejemplo y generó algunos trazados.

En la [parte cuatro](r-taxi-classification-train-model.md), cargará los módulos y llamará a las funciones necesarias para crear y entrenar el modelo mediante un procedimiento almacenado de SQL Server.

En la [parte cinco](r-taxi-classification-deploy-model.md), aprenderá a poner en marcha los modelos entrenados y guardados en la parte cuatro.

En la [parte cinco](./python-taxi-classification-deploy-model.md), aprenderá a poner en marcha los modelos entrenados y guardados en la parte cuatro.

## <a name="about-feature-engineering"></a>Acerca de la ingeniería de características

Después de varias series de exploración de datos, ha recopilado conocimientos de los datos y está listo para pasar a la *ingeniería de funciones*. Este proceso de creación de características significativas a partir de los datos sin procesar es un paso fundamental a la hora de crear modelos analíticos.

En este conjunto de datos, los valores de distancia se basan en la distancia notificada del taxímetro, y no representan necesariamente la distancia geográfica o la distancia recorrida realmente. Por tanto, debe calcular la distancia directa entre los puntos de origen y destino, usando las coordenadas disponibles en el conjunto de datos de origen NYC Taxi. Puede hacerlo mediante la [fórmula Haversine](https://en.wikipedia.org/wiki/Haversine_formula) en un función personalizada de [!INCLUDE[tsql](../../includes/tsql-md.md)] .

Usará una función personalizada de T-SQL, _fnCalculateDistance_, para calcular la distancia usando la fórmula Haversine, y una segunda función personalizada de T-SQL, _fnEngineerFeatures_, para crear una tabla que contiene todas las características.

El proceso general consiste en lo siguiente:

+ Crear la función de T-SQL que realiza los cálculos

+ Llamar a la función para generar los datos de característica

+ Guardar los datos de característica en una tabla

## <a name="calculate-trip-distance-using-fncalculatedistance"></a>Cálculo de la distancia del trayecto con fnCalculateDistance

La función _fnCalculateDistance_ debe haberse descargado y registrado con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] como parte de la preparación de este tutorial. Tómese un minuto para revisar el código.
  
1. En [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], expanda **Programación**, expanda **Funciones** y, después, **Funciones escalares**.   

2. Haga clic con el botón derecho en _fnCalculateDistance_ y seleccione **Modificar** para abrir el script de [!INCLUDE[tsql](../../includes/tsql-md.md)] en una nueva ventana de consulta.
  
   ```sql
   CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)  
   -- User-defined function that calculates the direct distance between two geographical coordinates.  
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
  
   + La función es una función escalar y devuelve un único valor de datos de un tipo predefinido.
  
   + Toma los valores de latitud y longitud como entradas, obtenidos de las ubicaciones de origen y destino de los viajes. La fórmula Haversine convierte ubicaciones en radianes y usa esos valores para calcular la distancia directa en millas entre las dos ubicaciones.

## <a name="generate-the-features-using-_fnengineerfeatures_"></a>Generación de las características con _fnEngineerFeatures_

Para agregar los valores calculados a una tabla y poder usarlos para entrenar el modelo, deberá usar otra función, _fnEngineerFeatures_. La nueva función llama a la función de T-SQL creada anteriormente, _fnCalculateDistance_, para obtener la distancia directa entre las ubicaciones de origen y destino. 

1. Tómese un minuto para revisar el código de la función personalizada de T-SQL, _fnEngineerFeatures_, que debe haberse creado como parte de la preparación para este tutorial.
  
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

   + Esta función es una función con valores de tabla que toma varias columnas como entradas y genera una tabla con varias columnas de características.

   + El propósito de esta función es crear características para usarlas en la creación de un modelo.

2. Para comprobar que esta función funciona, úsela para calcular la distancia geográfica de los trayectos en los que la distancia medida era 0, pero las ubicaciones de origen y destino eran diferentes.
  
   ```sql
       SELECT tipped, fare_amount, passenger_count,(trip_time_in_secs/60) as TripMinutes,
       trip_distance, pickup_datetime, dropoff_datetime,
       dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) AS direct_distance
       FROM nyctaxi_sample
       WHERE pickup_longitude != dropoff_longitude and pickup_latitude != dropoff_latitude and trip_distance = 0
       ORDER BY trip_time_in_secs DESC
   ```
  
   Como puede ver, la distancia notificada por el taxímetro no siempre se corresponde con la distancia geográfica. Por eso es tan importante la ingeniería de características. Estas características de datos se pueden usar para entrenar un modelo de Machine Learning con R.

## <a name="next-steps"></a>Pasos siguientes

En este artículo:

> [!div class="checklist"]
> + Se modificó una función personalizada para calcular la distancia de la carrera
> + Se guardaron las características mediante otra función personalizada

> [!div class="nextstepaction"]
> [Tutorial de R: Entrenamiento y guardado del modelo](r-taxi-classification-train-model.md)