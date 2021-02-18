---
title: 'Tutorial de R: Crear gráficos y trazados'
description: Aprenda técnicas para generar trazados y mapas mediante el lenguaje R con datos de SQL Server. Creará un histograma sencillo y luego desarrollará un trazado de mapa más complejo.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 06/13/2019
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: edf65095f245e5185b9eeadbd1d8b6c8e87e49cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272956"
---
# <a name="create-graphs-and-plots-using-sql-and-r-walkthrough"></a>Crear gráficos y trazados mediante SQL y R (tutorial)
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

En esta parte del tutorial, aprenderá técnicas para generar trazados y mapas mediante R con datos de SQL Server. Primero creará un histograma sencillo y luego desarrollará un trazado de mapa más complejo.

## <a name="prerequisites"></a>Requisitos previos

En este paso se da por supuesto que hay una sesión de R en curso basada en los pasos anteriores de este tutorial. Usaremos las cadenas de conexión y los objetos de origen de datos creados en esos pasos. También emplearemos las siguientes herramientas y paquetes para ejecutar el script.

+ Rgui.exe para ejecutar comandos de R
+ Management Studio para ejecutar T-SQL
+ googMap
+ paquete ggmap
+ paquete mapproj

## <a name="create-a-histogram"></a>Crear un histograma

1. Ejecute estas líneas para generar el primer trazado mediante la función [rxHistogram](/r-server/r-reference/revoscaler/rxdatasource) .  La función rxHistogram ofrece funciones similares a las de los paquetes de R de código abierto, pero se puede ejecutar en un contexto de ejecución remota.

    ```R
    # Plot fare amount on SQL Server and return the plot
    start.time <- proc.time()
    rxHistogram(~fare_amount, data = inDataSource, title = "Fare Amount Histogram")
    used.time <- proc.time() - start.time
    print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2), " seconds, Elapsed Time=", round(used.time[3],2), " seconds to generate plot.", sep=""))
    ```

2. La imagen se devuelve en el dispositivo de gráficos de R para el entorno de desarrollo.  Por ejemplo, en RStudio, haga clic en la ventana **Trazar** .  En [!INCLUDE[rsql_rtvs](../../includes/rsql-rtvs-md.md)], se abre una ventana de gráficos independiente.

    ![Uso de rxHistogram para trazar importes de tarifas](media/rsql-e2e-rxhistogramresult.png "Uso de rxHistogram para trazar importes de tarifas")

    > [!NOTE]
    > ¿Su gráfico tiene un aspecto diferente?
    >  
    > Esto se debe a que _inDataSource_ usa solo las 1000 primeras filas. El orden de las filas cuando se usa TOP no es determinista en ausencia de una cláusula ORDER BY, por lo que es posible que los datos y el gráfico resultante sean distintos.
    > Esta imagen en particular se generó con alrededor de 10 000 filas de datos. Se recomienda experimentar con distintos números de filas para obtener distintos gráficos y observar cuánto tiempo se tarda en devolver los resultados en su entorno.

## <a name="create-a-map-plot"></a>Crear un trazado de mapa

Normalmente, los servidores de bases de datos bloquean el acceso a Internet. Esto puede ser un inconveniente cuando se usan paquetes de R que necesitan descargar mapas u otras imágenes para generar trazados. Sin embargo, hay una solución que puede serle útil cuando desarrolle sus propias aplicaciones. Consiste, básicamente, en generar la representación del mapa en el cliente y, luego, superponer en el mapa los puntos que se almacenan como atributos en la tabla de SQL Server.

1. Defina la función que crea el objeto de trazado de R. La función personalizada *mapPlot* crea un gráfico de dispersión que usa los puntos de recogida de los taxis y traza el número de carreras que se inician desde cada punto. Usa los paquetes **ggplot2** y **ggmap**, que ya deberían estar [instalados y cargados](walkthrough-data-science-end-to-end-walkthrough.md#add-packages).

    ```R
    mapPlot <- function(inDataSource, googMap){
        library(ggmap)
        library(mapproj)
        ds <- rxImport(inDataSource)
        p <- ggmap(googMap)+
        geom_point(aes(x = pickup_longitude, y =pickup_latitude ), data=ds, alpha =.5,
    color="darkred", size = 1.5)
        return(list(myplot=p))
    }
    ```

    + La función *mapPlot* toma dos argumentos: un objeto de datos existente que se definió anteriormente mediante RxSqlServerData, y la representación del mapa que se pasa desde el cliente.
    + En la línea que empieza con la variable *ds*, se usa rxImport para cargar datos en la memoria desde el origen de datos creado anteriormente, *inDataSource*. (Ese origen de datos solo contiene 1000 filas; si desea crear un mapa con más puntos de datos, puede sustituir un origen de datos diferente).
    + Siempre que use funciones de R de código abierto, hay que cargar los datos en tramas de datos en memoria. No obstante, si llama a la función [rxImport](/r-server/r-reference/revoscaler/rximport), puede ejecutar en la memoria del contexto de proceso remoto.

2. Cambie al contexto de proceso local y cargue las bibliotecas necesarias para crear las asignaciones.

    ```R
    rxSetComputeContext("local")
    library(ggmap)
    library(mapproj)
    gc <- geocode("Times Square", source = "google")
    googMap <- get_googlemap(center = as.numeric(gc), zoom = 12, maptype = 'roadmap', color = 'color');
    ```

    + La variable `gc` almacena un conjunto de coordenadas para Times Square, Nueva York.

    + La línea que comienza con `googmap` genera un mapa con las coordenadas especificadas en el centro.

3. Cambie al contexto de proceso de SQL Server y represente los resultados. Para ello, ajuste la función de trazado en [rxExec](/r-server/r-reference/revoscaler/rxexec) como se muestra aquí. La función rxExec forma parte del paquete **RevoScaleR** y admite la ejecución de funciones arbitrarias de R en un contexto de cálculo remoto.

    ```R
    rxSetComputeContext(sqlcc)
    myplots <- rxExec(mapPlot, inDataSource, googMap, timesToRun = 1)
    plot(myplots[[1]][["myplot"]]);
    ````

    + Los datos de mapa de `googMap` se pasan como un argumento a la función ejecutada de forma remota *mapPlot*. Como los mapas se generaron en el entorno local, deben pasarse a la función para poder crear el gráfico en el contexto de SQL Server.

    + Cuando se ejecuta la línea que empieza con `plot`, los datos representados se vuelven a serializar en el entorno local de R para que pueda verlos en el cliente de R.

    > [!NOTE]
    > Si usa SQL Server en una máquina virtual de Azure, es posible que obtenga un error en este momento. Se produce un error cuando la regla de firewall predeterminada en Azure bloquea el acceso a la red mediante código R. Para saber más sobre cómo corregir este error, vea [Instalación de Machine Learning (R) Services en una máquina virtual de Azure](../install/sql-machine-learning-azure-virtual-machine.md).

4. En la siguiente imagen se muestra el trazado de salida. Las ubicaciones de recogida de taxi se agregan al mapa como puntos rojos. La imagen podría ser diferente, en función del número de ubicaciones que haya en el origen de datos usado.

    ![Trazado de las carreras de los taxis mediante una función personalizada de R](media/rsql-e2e-mapplot.png "Trazado de las carreras de los taxis mediante una función personalizada de R")

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Crear características de datos mediante R y SQL](walkthrough-create-data-features.md)