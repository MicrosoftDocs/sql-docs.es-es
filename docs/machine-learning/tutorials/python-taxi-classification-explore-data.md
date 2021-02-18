---
title: 'Tutorial de Python: Explorar y visualizar datos'
titleSuffix: SQL machine learning
description: Explore los datos de ejemplo y genere algunos trazados para preparar el uso de la clasificación binaria en Python con el aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: e36b8f6f2768d4de83c9f23bd264329f6f04d381
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339687"
---
# <a name="python-tutorial-explore-and-visualize-data"></a>Tutorial de Python: Explorar y visualizar datos
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

En la parte dos de esta serie de tutoriales de cinco partes, explorará los datos de ejemplo y generará algunos trazados. Más adelante, aprenderá a serializar objetos gráficos en Python y, después, a deserializarlos y a crear trazados.

En este artículo, hará lo siguiente:

> [!div class="checklist"]
> + Revisar el código de ejemplo
> + Creación de trazados con Python en T-SQL

En la [parte uno](python-taxi-classification-introduction.md), ha instalado los requisitos previos y ha restaurado la base de datos de ejemplo.

En la [tercera](python-taxi-classification-create-features.md), aprenderá a crear características a partir de datos sin procesar mediante una función de Transact-SQL. Después, llamaremos a esa función desde un procedimiento almacenado para crear una tabla que contiene los valores de las características.

En la [parte cuatro](python-taxi-classification-train-model.md), cargará los módulos y llamará a las funciones necesarias para crear y entrenar el modelo mediante un procedimiento almacenado de SQL Server.

En la [parte cinco](python-taxi-classification-deploy-model.md), aprenderá a poner en marcha los modelos entrenados y guardados en la parte cuatro.

## <a name="review-the-data"></a>Revisión de los datos

En primer lugar, dedique un minuto a examinar el esquema de datos, ya que hemos realizado algunos cambios para que sea más fácil usar los datos de NYC Taxi.

+ En el conjunto de datos original, se usaban archivos independientes para los identificadores de taxis y los registros de trayecto. Hemos combinado los dos conjuntos de datos originales en las columnas _medallion_, _hack_license_ y _pickup_datetime_.  
+ El conjunto de datos original abarcaba muchos archivos y era bastante grande. Lo hemos reducido de tamaño para obtener solo un 1 % del número de registros original. La tabla de datos actual tiene 1 703 957 filas y 23 columnas.

**Identificadores de taxis**

La columna _medallion_ representa el número de identificador único del taxi.

La columna _hack_license_ contiene el número de licencia del conductor del taxi (anónimo).

**Registros de viajes y tarifas**

Cada registro de viaje incluye la ubicación y hora de recogida y destino y la distancia del viaje.

Cada registro de tarifa incluye información de pago, como el tipo de pago, el importe total de pago y la cantidad de propina.

Las últimas tres columnas se pueden usar para varias tareas de aprendizaje automático.  La columna _tip_amount_ contiene valores numéricos continuos y se puede usar como la columna **label** para el análisis de regresión. La columna _tipped_ tiene solo valores sí/no y se usa para la clasificación binaria. La columna _tip_class_ tiene varias **etiquetas de clase** y, por tanto, se puede usar como etiqueta para tareas de clasificación de varias clases.

Todos los valores usados en las columnas de etiqueta se basan en la columna `tip_amount`, con estas reglas de negocios:

+ La columna de etiqueta `tipped` tiene los valores posibles 0 y 1.

    Si `tip_amount` > 0, `tipped` = 1; de lo contrario, `tipped` = 0.

+ La columna de etiqueta `tip_class` tiene los valores de clase posibles 0-4.

    Clase 0: `tip_amount` = 0 $

    Clase 1: `tip_amount` > 0 $ y `tip_amount` < = 5 $
    
    Clase 2: `tip_amount` > 5 $ y `tip_amount` < = 10 $
    
    Clase 3: `tip_amount` > 10 $ y `tip_amount` < = 20 $
    
    Clase 4: `tip_amount` > 20 $

## <a name="create-plots-using-python-in-t-sql"></a>Creación de trazados con Python en T-SQL

Desarrollar una solución de ciencia de datos incluye normalmente la exploración de datos intensivos y la visualización de datos. Dado que la visualización es una herramienta muy eficaz para comprender la distribución de los datos y los valores atípicos, Python proporciona muchos paquetes para visualizar los datos. El módulo **matplotlib** es una de las bibliotecas más populares de visualización, e incluye muchas funciones para crear histogramas, gráficos de dispersión, diagramas de caja y otros gráficos de exploración de datos.

En esta sección, aprenderá a trabajar con trazados usando procedimientos almacenados. En lugar de abrir la imagen en el servidor, almacenará el objeto de Python `plot` como datos **varbinary** y, después, los escribirá en un archivo que se puede compartir o ver en cualquier otro lugar.

### <a name="create-a-plot-as-varbinary-data"></a>Creación de un trazado como datos varbinary

El procedimiento almacenado devuelve un objeto de Python serializado `figure` como un flujo de datos **varbinary**. Los datos binarios no se pueden ver directamente, pero se puede usar código de Python en el cliente para deserializar y ver las cifras y, luego, guardar el archivo de imagen en un equipo cliente.

1. Cree el procedimiento almacenado **PyPlotMatplotlib** (si el script de PowerShell no lo ha hecho todavía).

    - La variable `@query` define el texto de consulta (`SELECT tipped FROM nyctaxi_sample`), que se pasa al bloque de código de Python como argumento de la variable de entrada de script `@input_data_1`.
    - El script de Python es bastante sencillo: los objetos `figure` de **matplotlib** se usan para crear el histograma y el gráfico de dispersión, y luego estos objetos se serializan utilizando la biblioteca `pickle`.
    - El objeto de gráficos de Python se serializa en un DataFrame de **pandas** para la salida.
  
    ```sql
    DROP PROCEDURE IF EXISTS PyPlotMatplotlib;
    GO

    CREATE PROCEDURE [dbo].[PyPlotMatplotlib]
    AS
    BEGIN
        SET NOCOUNT ON;
        DECLARE @query nvarchar(max) =
        N'SELECT cast(tipped as int) as tipped, tip_amount, fare_amount FROM [dbo].[nyctaxi_sample]'
        EXECUTE sp_execute_external_script
        @language = N'Python',
        @script = N'
    import matplotlib
    matplotlib.use("Agg")
    import matplotlib.pyplot as plt
    import pandas as pd
    import pickle

    fig_handle = plt.figure()
    plt.hist(InputDataSet.tipped)
    plt.xlabel("Tipped")
    plt.ylabel("Counts")
    plt.title("Histogram, Tipped")
    plot0 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    plt.hist(InputDataSet.tip_amount)
    plt.xlabel("Tip amount ($)")
    plt.ylabel("Counts")
    plt.title("Histogram, Tip amount")
    plot1 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    plt.hist(InputDataSet.fare_amount)
    plt.xlabel("Fare amount ($)")
    plt.ylabel("Counts")
    plt.title("Histogram, Fare amount")
    plot2 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    plt.scatter( InputDataSet.fare_amount, InputDataSet.tip_amount)
    plt.xlabel("Fare Amount ($)")
    plt.ylabel("Tip Amount ($)")
    plt.title("Tip amount by Fare amount")
    plot3 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    OutputDataSet = plot0.append(plot1, ignore_index=True).append(plot2, ignore_index=True).append(plot3, ignore_index=True)
    ',
    @input_data_1 = @query
    WITH RESULT SETS ((plot varbinary(max)))
    END
    GO
    ```

2. Ahora, ejecute el procedimiento almacenado sin argumentos para generar un trazado a partir de los datos codificados de forma rígida como la consulta de entrada.

    ```sql
    EXEC [dbo].[PyPlotMatplotlib]
    ```

3. Los resultados deben tener un aspecto similar al siguiente:
  
    ```sql
    plot
    0xFFD8FFE000104A4649...
    0xFFD8FFE000104A4649...
    0xFFD8FFE000104A4649...
    0xFFD8FFE000104A4649...
    ```

4. Desde un [cliente de Python](../python/setup-python-client-tools-sql.md), ahora puede conectarse a la instancia de SQL Server que generó los objetos de trazado binario y ver los trazados. 

    Para ello, ejecute el siguiente código de Python, reemplazando el nombre del servidor, el nombre de la base de datos y las credenciales según corresponda (para la autenticación de Windows, reemplace los parámetros `UID` y `PWD` por `Trusted_Connection=True`). Asegúrese de que la versión de Python es la misma en el cliente y en el servidor. Asegúrese también de que las bibliotecas de Python en el cliente (por ejemplo, matplotlib) tienen la misma versión o una versión superior de las bibliotecas instaladas en el servidor.
  
    ```python
    %matplotlib notebook
    import pyodbc
    import pickle
    import os
    cnxn = pyodbc.connect('DRIVER=SQL Server;SERVER={SERVER_NAME};DATABASE={DB_NAME};UID={USER_NAME};PWD={PASSWORD}')
    cursor = cnxn.cursor()
    cursor.execute("EXECUTE [dbo].[PyPlotMatplotlib]")
    tables = cursor.fetchall()
    for i in range(0, len(tables)):
        fig = pickle.loads(tables[i][0])
        fig.savefig(str(i)+'.png')
    print("The plots are saved in directory: ",os.getcwd())
    ```

5. Si la conexión se realiza correctamente, debería ver un mensaje similar al siguiente:
  
   *Los trazados se guardan en el directorio: xxxx*
  
6. El archivo de salida se crea en el directorio de trabajo de Python. Para ver el trazado, busque el directorio de trabajo de Python y abra el archivo. En la siguiente imagen se muestra un trazado guardado en el equipo cliente.
  
   ![Importe de propina frente a importe de tarifa](media/sqldev-python-sample-plot.png "Importe de propina frente a importe de tarifa") 

## <a name="next-steps"></a>Pasos siguientes

En este artículo:

> [!div class="checklist"]
> + Revisó los datos de ejemplo.
> + Creó trazados con Python en T-SQL.

> [!div class="nextstepaction"]
> [Tutorial de Python: Creación de características de datos mediante T-SQL](python-taxi-classification-create-features.md)