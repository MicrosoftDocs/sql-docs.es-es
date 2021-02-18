---
title: 'Tutorial de R: Exploración de datos'
description: Tutorial que muestra cómo visualizar y generar resúmenes estadísticos mediante funciones de R para el análisis en base de datos en SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/26/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 676fd125bf148a5139de1ecee7145c523ee6b4e0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272946"
---
# <a name="view-and-summarize-sql-server-data-using-r-walkthrough"></a>Visualización y resumen de datos de SQL Server mediante R (tutorial)
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

En esta lección se presentan las funciones del paquete **RevoScaleR** y se explican estas tareas:

> [!div class="checklist"]
> * Conectar a SQL Server
> * Definir una consulta con los datos que necesita o especificar una tabla o vista
> * Definir uno o más contextos de cálculo para usarlos al ejecutar código de R
> * Si lo prefiere, puede definir las transformaciones que se aplican al origen de datos mientras se lee desde el origen.

## <a name="define-a-sql-server-compute-context"></a>Definición de un contexto de proceso de SQL Server

Ejecute las siguientes instrucciones de R en un entorno de R en la estación de trabajo del cliente. En esta sección se presupone que usa una [estación de trabajo de ciencia de datos con Microsoft R Client](../r/set-up-a-data-science-client.md), ya que incluye todos los paquetes de RevoScaleR, además de un conjunto básico y ligero de herramientas de R. Por ejemplo, puede usar Rgui.exe para ejecutar el script de R en esta sección.

1. Si aún no está cargado el paquete **RevoScaleR**, ejecute esta línea de código R:

    ```R
    library("RevoScaleR")
    ```

     En este caso, las comillas son opcionales pero recomendables.
     
     Si recibe un error, asegúrese de que el entorno de desarrollo de R usa una biblioteca que contiene el paquete RevoScaleR. Use un comando como `.libPaths()` para ver la ruta de acceso actual a la biblioteca.

2. Cree la cadena de conexión para SQL Server y guárdela en una variable de R, *connStr*.

   Debe cambiar el marcador de posición "your_server_name" por un nombre de instancia de SQL Server válido. En el caso del nombre del servidor, es posible que pueda usar solo el nombre de la instancia, o puede que necesite un nombre completo, en función de la red.
    
   Esta es la sintaxis de conexión para la autenticación de SQL Server:

    ```R
    connStr <- "Driver=SQL Server;Server=your_server_name;Database=nyctaxi_sample;Uid=your-sql-login;Pwd=your-login-password"
    ```

    Para la autenticación de Windows, la sintaxis es un poco diferente:
    
    ```R
    connStr <- "Driver=SQL Server;Server=your_server_name;Database=nyctaxi_sample;Trusted_Connection=True"
    ```

    Normalmente se recomienda usar la autenticación de Windows siempre que sea posible, para no tener que guardar contraseñas en el código de R.

3. Defina las variables que se usarán al crear un nuevo *contexto de proceso*. Después de crear el objeto de contexto de proceso, puede usarlo para ejecutar el código de R en la instancia de SQL Server.

    ```R
    sqlShareDir <- paste("C:\\AllShare\\",Sys.getenv("USERNAME"),sep="")
    sqlWait <- TRUE
    sqlConsoleOutput <- FALSE
    ```

    - R usa un directorio temporal cuando serializa los objetos de R entre la estación de trabajo y el equipo con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Puede especificar el directorio local que se usa como *sqlShareDir* o aceptar el valor predeterminado.
  
    - Use *sqlWait* para indicar si quiere que R espere los resultados del servidor.  Para obtener una explicación sobre si esperar o no los trabajos, vea [Proceso distribuido y en paralelo con RevoScaleR en Microsoft R](/r-server/r/how-to-revoscaler-distributed-computing).
  
    - Use el argumento *sqlConsoleOutput* para indicar que no quiere ver los resultados desde la consola de R.

4. Llame al constructor [RxInSqlServer](/r-server/r-reference/revoscaler/rxinsqlserver) para crear el objeto de contexto de proceso con las variables y las cadenas de conexión ya definidas, y guarde el nuevo objeto en la variable de R *sqlcc*.
  
    ```R
    sqlcc <- RxInSqlServer(connectionString = connStr, shareDir = sqlShareDir, wait = sqlWait, consoleOutput = sqlConsoleOutput)
    ```

5. De manera predeterminada, el contexto de proceso es local, por lo que deberá establecer explícitamente el *contexto de proceso activo*.

    ```R
    rxSetComputeContext(sqlcc)
    ```

    + [rxSetComputeContext](/machine-learning-server/r-reference/revoscaler/rxsetcomputecontext) devuelve el contexto de proceso activo anteriormente de manera invisible para que pueda usarlo.
    + [rxGetComputeContext](/machine-learning-server/r-reference/revoscaler/rxsetcomputecontext) devuelve el contexto de proceso activo.
    
    Tenga que cuenta que establecer un contexto de proceso solo afecta a las operaciones que usan funciones en el paquete **RevoScaleR**; el contexto de proceso no afecta a la forma en que se realizan las operaciones de R de código abierto.

## <a name="create-a-data-source-using-rxsqlserver"></a>Creación de un origen de datos mediante RxSqlServer

Cuando se usan las bibliotecas de Microsoft R como RevoScaleR y MicrosoftML, un *origen de datos* es un objeto que se crea mediante las funciones de RevoScaleR. El objeto de origen de datos especifica algún conjunto de datos que quiera usar para una tarea, como el entrenamiento del modelo o la extracción de características. Puede obtener datos de diversos orígenes, incluido SQL Server. Para obtener la lista de orígenes admitidos actualmente, vea [RxDataSource](/r-server/r-reference/revoscaler/rxdatasource).

Antes definió una cadena de conexión y guardó esa información en una variable de R. Puede volver a usar esa información de conexión para especificar los datos que quiere obtener.

1. Guarde una consulta SQL como una variable de cadena. La consulta define los datos para entrenar el modelo.

    ```R
    sampleDataQuery <- "SELECT TOP 1000 tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime, pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude FROM nyctaxi_sample"
    ```

    Aquí hemos usado una cláusula TOP para que todo se ejecute más rápido, pero las filas reales devueltas por la consulta pueden variar según el orden. Por lo tanto, los resultados de resumen también pueden ser diferentes de los que se enumeran aquí. No dude en quitar la cláusula TOP.

2. Pase la definición de consulta como argumento a la función [RxSqlServerData](/r-server/r-reference/revoscaler/rxsqlserverdata).

    ```R
    inDataSource <- RxSqlServerData(
      sqlQuery = sampleDataQuery,
      connectionString = connStr,
      colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric",
      dropoff_longitude = "numeric", dropoff_latitude = "numeric"),
      rowsPerRead=500
      )
    ```
    
    + El argumento  *colClasses* especifica los tipos de columna que se usarán al mover los datos entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y R. Esto es importante porque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa tipos de datos distintos a los de R y más tipos de datos. Para más información, vea [Bibliotecas de R y tipos de datos](../r/r-libraries-and-data-types.md).
  
    + El argumento *rowsPerRead* es importante para controlar el uso de memoria y la eficacia de los procesos.  La mayoría de las funciones analíticas mejoradas de[!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] procesan los datos en fragmentos y acumulan los resultados intermedios, devolviendo los cálculos finales después de que se han leído todos los datos.  Al agregar el parámetro *rowsPerRead*, se consigue controlar cuántas filas de datos se leen en cada fragmento para su procesamiento.  Si el valor de este parámetro es demasiado grande, es posible que el acceso a los datos sea lento porque no tiene suficiente memoria para procesar de forma eficaz un fragmento de datos tan grande.  En algunos sistemas, si se establece *rowsPerRead* en un valor demasiado pequeño también puede suponer un rendimiento más lento.

3. Hasta ahora, ha creado el objeto *inDataSource*, pero no contiene ningún dato. Los datos no se extraen de la consulta de SQL al entorno local hasta que se ejecuta una función como [rxImport](/r-server/r-reference/revoscaler/rxdatastep) o [rxSummary](/r-server/r-reference/revoscaler/rxsummary).

    Aunque, como ya ha definido los objetos de datos, puede usarlos como argumento para otras funciones.

## <a name="use-the-sql-server-data-in-r-summaries"></a>Uso de los datos de SQL Server en resúmenes de R

En esta sección, probará algunas de las funciones proporcionadas en [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] que admiten contextos de proceso remotos. Al aplicar funciones de R al origen de datos, puede explorar, resumir y mostrar en gráficos los datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

1. Llame a la función [rxGetVarInfo](/r-server/r-reference/revoscaler/rxgetvarinfo) para obtener una lista de las variables del origen de datos y sus tipos de datos.

    **rxGetVarInfo** es una función muy útil: puede llamarla en cualquier trama de datos o en un conjunto de datos en un objeto de datos remotos para obtener información como los valores máximo y mínimo, el tipo de datos y el número de niveles en las columnas de factor.
    
    Considere la posibilidad de ejecutar esta función después de cualquier tipo de entrada de datos, transformación de características o ingeniería de características. Al hacerlo, puede asegurarse de que todas las características que quiere usar en el modelo son del tipo de datos esperado y evitar errores.
  
    ```R
    rxGetVarInfo(data = inDataSource)
    ```

    **Resultados**
    
    ```R
    Var 1: tipped, Type: integer
    Var 2: fare_amount, Type: numeric
    Var 3: passenger_count, Type: integer
    Var 4: trip_time_in_secs, Type: numeric, Storage: int64
    Var 5: trip_distance, Type: numeric
    Var 6: pickup_datetime, Type: character
    Var 7: dropoff_datetime, Type: character
    Var 8: pickup_longitude, Type: numeric
    Var 9: pickup_latitude, Type: numeric
    Var 10: dropoff_longitude, Type: numeric
    ```

2. Ahora, llame a la función [rxSummary](/r-server/r-reference/revoscaler/rxsummary) de RevoScaleR para obtener estadísticas más detalladas sobre las variables individuales.

    rxSummary se basa en la función `summary` de R, pero tiene algunas características y ventajas adicionales. rxSummary funciona en varios contextos de proceso y admite la fragmentación.  También puede usarse rxSummary para transformar valores o resumir en función de los niveles de factor.
    
    En este ejemplo, se resume el importe de la carrera en función del número de pasajeros.
    
    ```R
    start.time <- proc.time()
    rxSummary(~fare_amount:F(passenger_count,1,6), data = inDataSource)
    used.time <- proc.time() - start.time
    print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds,
      Elapsed Time=", round(used.time[3],2),
      " seconds to summarize the inDataSource.", sep=""))
    ```
    + El primer argumento para rxSummary especifica la fórmula o el término que se va a usar para resumir. Aquí se usa la función `F()` para convertir los valores de _passenger\_count_ en factores antes de resumirlos. También debe especificar el valor mínimo (1) y el valor máximo (6) para la variable del factor _passenger\_count_.
    + Si no especifica las estadísticas de salida, rxSummary genera de forma predeterminada Mean, StDev, Min, Max y el número de observaciones válidas y que faltan.
    + Este ejemplo también incluye algo de código para realizar un seguimiento de la hora a la que empieza y finaliza la función, para que pueda comparar el rendimiento.
  
    **Resultados**

    Si la función rxSummary se ejecuta correctamente, se mostrarán resultados como estos, seguidos de una lista de estadísticas por categoría. 

    ```R
    rxSummary(formula = ~fare_amount:F(passenger_count, 1,6), data = inDataSource)
    Data: inDataSource (RxSqlServerData Data Source)
    Number of valid observations: 1000
    ```

### <a name="bonus-exercise-on-big-data"></a>Ejercicio adicional sobre macrodatos

Intente definir una nueva cadena de consulta con todas las filas. Se recomienda configurar un nuevo objeto de origen de datos para este experimento. También puede intentar cambiar el parámetro *rowsToRead* para ver cómo afecta al rendimiento.

```R
bigDataQuery  <- "SELECT tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime, pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude FROM nyctaxi_sample"

bigDataSource <- RxSqlServerData(
      sqlQuery = bigDataQuery,
      connectionString = connStr,
      colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric",
      dropoff_longitude = "numeric", dropoff_latitude = "numeric"),
      rowsPerRead=500
      )

start.time <- proc.time()
rxSummary(~fare_amount:F(passenger_count,1,6), data = bigDataSource)
used.time <- proc.time() - start.time
print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds,
  Elapsed Time=", round(used.time[3],2),
  " seconds to summarize the inDataSource.", sep=""))
```

> [!TIP]
> Mientras se está ejecutando, puede usar una herramienta como el [Explorador de procesos](/sysinternals/downloads/process-explorer) o SQL Server Profiler para ver cómo se establece la conexión y cómo se ejecuta el código de R con los servicios de SQL Server.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Crear gráficos y trazados con R](walkthrough-create-graphs-and-plots-using-r.md)