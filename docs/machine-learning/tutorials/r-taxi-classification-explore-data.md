---
title: 'Tutorial de R: Explorar y visualizar datos'
titleSuffix: SQL machine learning
description: Explore los datos de ejemplo y genere algunos trazados para preparar el uso de la clasificación binaria en R con el aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 10/15/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 8e2e8994f92bfb96fa66c2c6a08c7db198902b45
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354211"
---
# <a name="r-tutorial-explore-and-visualize-data"></a>Tutorial de R: Explorar y visualizar datos
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

En la parte dos de esta serie de tutoriales de cinco partes, explorará los datos de ejemplo y generará algunos trazados. Más adelante, aprenderá a serializar objetos gráficos en Python y, después, a deserializarlos y a crear trazados.

En la parte dos de este tutorial de cinco partes, revisará los datos de ejemplo y, luego, generará algunos trazados mediante las funciones genéricas `barplot` y `hist` en R base.

Uno de los objetivos clave de este artículo es mostrar cómo llamar a las funciones de R desde [!INCLUDE[tsql](../../includes/tsql-md.md)] en los procedimientos almacenados y guardar los resultados en formatos de archivo de aplicación:

+ Cree un procedimiento almacenado mediante `barplot` para generar un trazado de R como datos varbinary. Use **bcp** para exportar el flujo binario a un archivo de imagen.
+ Cree un procedimiento almacenado mediante `hist` para generar un trazado y guardar los resultados como archivos JPG y PDF.

> [!NOTE]
> Dado que la visualización es una herramienta muy eficaz para comprender la distribución y la forma de datos, R proporciona una variedad de funciones y paquetes para generar histogramas, gráficos de dispersión, gráficos de cuadros y otros gráficos de exploración de datos. R normalmente crea imágenes con un dispositivo de R para la salida gráfica, que puede capturar y almacenar como un tipo de datos **varbinary** para la representación en la aplicación. También puede guardar las imágenes en cualquiera de los formatos de archivo admitidos (.JPG, .PDF, etc.).

En este artículo, hará lo siguiente:

> [!div class="checklist"]
> + Revisará los datos de ejemplo
> + Creará trazados con R en T-SQL
> + Generará trazados en varios formatos de archivo

En la [parte uno](r-taxi-classification-introduction.md), ha instalado los requisitos previos y ha restaurado la base de datos de ejemplo.

En la [tercera](r-taxi-classification-create-features.md), aprenderá a crear características a partir de datos sin procesar mediante una función de Transact-SQL. Después, llamaremos a esa función desde un procedimiento almacenado para crear una tabla que contiene los valores de las características.

En la [parte cuatro](r-taxi-classification-train-model.md), cargará los módulos y llamará a las funciones necesarias para crear y entrenar el modelo mediante un procedimiento almacenado de SQL Server.

En la [parte cinco](r-taxi-classification-deploy-model.md), aprenderá a poner en marcha los modelos entrenados y guardados en la parte cuatro.

## <a name="review-the-data"></a>Revisión de los datos

Desarrollar una solución de ciencia de datos incluye normalmente la exploración de datos intensivos y la visualización de datos. Así pues, primero revise los datos de ejemplo si no lo ha hecho ya, solo le llevará un minuto.

En el conjunto de datos públicos original, los identificadores de taxis y los registros de viajes se proporcionaron en archivos independientes. En cambio, para facilitar el uso de los datos de ejemplo, los dos conjuntos de datos originales se han unido en las columnas _medallion_, _hack\_license_ y _pickup\_datetime_.  Los registros también se muestrearon para obtener solo un 1 % del número de registros original. El conjunto de datos muestreado resultante tiene 1 703 957 filas y 23 columnas.

**Identificadores de taxis**
  
+ La columna _medallion_ representa el número de identificador único del taxi.
  
+ La columna _hack\_license_ contiene el número de licencia del conductor del taxi (anonimizado).
  
**Registros de viajes y tarifas**
  
+ Cada registro de viaje incluye la ubicación y hora de recogida y destino y la distancia del viaje.
  
+ Cada registro de tarifa incluye información de pago, como el tipo de pago, el importe total de pago y la cantidad de propina.
  
+ Las últimas tres columnas se pueden usar para varias tareas de aprendizaje automático. La columna _tip\_amount_ contiene valores numéricos continuos y se puede usar como la columna **label** para el análisis de regresión. La columna _tipped_ tiene solo valores sí/no y se usa para la clasificación binaria. La columna _tip\_class_ tiene varias **etiquetas de clase** y, por tanto, se puede usar como etiqueta para tareas de clasificación de varias clases.
  
  Este tutorial muestra solo la tarea de clasificación binaria; puede intentar crear modelos de las otras dos tareas de aprendizaje automático, regresión y clasificación de varias clases.
  
+ Los valores usados para las columnas de etiqueta se basan en la columna _tip\_amount_, usando estas reglas de negocios:
  
  |Nombre de columna derivada|Regla|
  |-|-|
  |tipped|Si tip_amount > 0, tipped = 1, de lo contrario, tipped = 0|
  |tip_class|Clase 0: tip_amount = 0 $<br /><br />Clase 1: tip_amount > 0 $ y tip_amount < = 5 $<br /><br />Clase 2: tip_amount > 5 $ y tip_amount < = 10 $<br /><br />Clase 3: tip_amount > 10 $ y tip_amount < = 20 $<br /><br />Clase 4: tip_amount > 20 $|

## <a name="create-plots-using-r-in-t-sql"></a>Creación de trazados con R en T-SQL

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!IMPORTANT]
> A partir de SQL Server 2019, el mecanismo de aislamiento requiere que se proporcionen los permisos adecuados para el directorio donde se almacena el archivo de trazado. Para más información sobre cómo establecer estos permisos, consulte la [sección sobre los permisos de archivo en SQL Server 2019 en Windows: Cambios de aislamiento para Machine Learning Services](../install/sql-server-machine-learning-services-2019.md#file-permissions).
::: moniker-end

Use la función `barplot` de R para crear el trazado. Este paso representa un histograma en función de los datos de una consulta [!INCLUDE[tsql](../../includes/tsql-md.md)]. Puede ajustar esta función en un procedimiento almacenado, **RPlotHistogram**.

1. En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], en el Explorador de objetos, haga clic con el botón derecho en la base de datos **NYCTaxi_Sample** y seleccione **Nueva consulta**. O en Azure Data Studio, seleccione **Nuevo cuaderno** en el menú **Archivo** y conéctese a la base de datos.

2. Pegue el siguiente script para crear un procedimiento almacenado que trace el histograma. Este ejemplo se denomina **RPlotHistogram**.

   ```sql
   CREATE PROCEDURE [dbo].[RPlotHistogram]
   AS
   BEGIN
     SET NOCOUNT ON;
     DECLARE @query nvarchar(max) =  
     N'SELECT tipped FROM [dbo].[nyctaxi_sample]'  
     EXECUTE sp_execute_external_script @language = N'R',  
                                        @script = N'  
      image_file = tempfile();  
      jpeg(filename = image_file);  
      #Plot histogram  
      barplot(table(InputDataSet$tipped), main = "Tip Histogram", col="lightgreen", xlab="Tipped or not", ylab = "Counts", space=0)
      dev.off();  
      OutputDataSet <- data.frame(data=readBin(file(image_file, "rb"), what=raw(), n=1e6));  
      ',  
      @input_data_1 = @query  
      WITH RESULT SETS ((plot varbinary(max)));  
   END
   GO
   ```

Entre los puntos clave que se deben comprender en este script se incluyen los siguientes:
  
+ La variable `@query` define el texto de consulta (`'SELECT tipped FROM nyctaxi_sample'`), que se pasa al script de R como argumento para la variable de entrada de script `@input_data_1`. En el caso de los scripts de R que se ejecutan como procesos externos, debe tener una asignación de uno a uno entre las entradas del script y las entradas en el procedimiento almacenado del sistema [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) que inicia la sesión de R en SQL Server.
  
+ Dentro del script de R, se define una variable (`image_file`) para almacenar la imagen.

+ Se llama a la función `barplot` para generar el trazado.
  
+ El dispositivo de R está establecido en **desactivado** porque está ejecutando este comando como un script externo en SQL Server. Normalmente en R, al enviar un comando de trazado de alto nivel, R abre una ventana de gráficos, denominada *dispositivo*. Puede desactivar el dispositivo si está escribiendo en un archivo o controlando el resultado de otra manera.
  
+ El objeto de gráficos de R se serializa en un data.frame de R para la salida.

### <a name="execute-the-stored-procedure-and-use-bcp-to-export-binary-data-to-an-image-file"></a>Ejecute el procedimiento almacenado y use bcp para exportar datos binarios a un archivo de imagen

El procedimiento almacenado devuelve la imagen como una secuencia de datos varbinary, que, evidentemente, no puede ver directamente. En cambio, puede usar la utilidad **bcp** para obtener los datos varbinary y guardarlos como un archivo de imagen en un equipo cliente.
  
1. En [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], ejecute la instrucción siguiente:
  
    ```sql
    EXEC [dbo].[RPlotHistogram]
    ```
  
    **Resultados**
    
    *plot* *0xFFD8FFE000104A4649...*
  
2. Abra un símbolo del sistema de PowerShell y ejecute el siguiente comando con el nombre de instancia, el nombre de la base de datos, el nombre de usuario y las credenciales adecuados como argumentos. Para aquellos que usan identidades de Windows, puede reemplazar **-U** y **-P** con **-T**.
  
   ```powershell
   bcp "exec RPlotHistogram" queryout "plot.jpg" -S <SQL Server instance name> -d  NYCTaxi_Sample  -U <user name> -P <password> -T
   ```

   > [!NOTE]
   > Los modificadores de comandos bcp distinguen mayúsculas de minúsculas.
  
3. Si la conexión es correcta, se le pedirá que escriba más información sobre el formato de archivo de gráficos.

   Pulse ENTRAR en cada aviso para aceptar los valores predeterminados, excepto estos cambios:

   + En **prefix-length of field plot**, escriba 0.
  
   + Escriba **Y** si quiere guardar los parámetros de salida para un uso posterior.
  
   ```text
   Enter the file storage type of field plot [varbinary(max)]: 
   Enter prefix-length of field plot [8]: 0
   Enter length of field plot [0]:
   Enter field terminator [none]:
  
   Do you want to save this format information in a file? [Y/n]
   Host filename [bcp.fmt]:
   ```
  
   **Resultados**

   ```text
   Starting copy...
   1 rows copied.
   Network packet size (bytes): 4096
   Clock Time (ms.) Total     : 3922   Average : (0.25 rows per sec.)
   ```

   > [!TIP]
   > Si guarda la información de formato en archivo (bcp.fmt), la utilidad **bcp** genera una definición de formato que puede aplicar a los comandos similares en el futuro sin tener que cambiar las opciones de formato de archivo de gráficos. Para usar el archivo de formato, agregue `-f bcp.fmt` al final de cualquier línea de comandos después del argumento de la contraseña.
  
4. El archivo de salida se creará en el mismo directorio en el que se ha ejecutado el comando de PowerShell. Para ver el trazado, basta con abrir el archivo plot.jpg.
  
   ![viajes de taxi con y sin propinas](media/rsql-devtut-tippedornot.jpg "viajes de taxi con y sin propinas")  
  
## <a name="create-a-stored-procedure-using-hist"></a>Creación de un procedimiento almacenado con `hist`

Normalmente, los científicos de datos generan varias visualizaciones de datos para obtener información sobre los datos desde distintas perspectivas. En este ejemplo, creará un procedimiento almacenado llamado **RPlotHist** para escribir histogramas, gráficos de dispersión y otros gráficos de R en formato .JPG y .PDF.

Este procedimiento almacenado usa la función `hist` para crear el histograma y exportar los datos binarios a formatos populares como .JPG, .PDF y .PNG.

1. En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], en el Explorador de objetos, haga clic con el botón derecho en la base de datos **NYCTaxi_Sample** y seleccione **Nueva consulta**.

2. Pegue el siguiente script para crear un procedimiento almacenado que trace el histograma. Este ejemplo se denomina **RPlotHist**.
  
   ```sql
   CREATE PROCEDURE [dbo].[RPlotHist]  
   AS  
   BEGIN  
     SET NOCOUNT ON;  
     DECLARE @query nvarchar(max) =  
     N'SELECT cast(tipped as int) as tipped, tip_amount, fare_amount FROM [dbo].[nyctaxi_sample]'  
     EXECUTE sp_execute_external_script @language = N'R',  
     @script = N'  
      # Set output directory for files and check for existing files with same names   
       mainDir <- ''C:\\temp\\plots''  
       dir.create(mainDir, recursive = TRUE, showWarnings = FALSE)  
       setwd(mainDir);  
       print("Creating output plot files:", quote=FALSE)
       
       # Open a jpeg file and output histogram of tipped variable in that file.  
       dest_filename = tempfile(pattern = ''rHistogram_Tipped_'', tmpdir = mainDir)  
       dest_filename = paste(dest_filename, ''.jpg'',sep="")  
       print(dest_filename, quote=FALSE);  
       jpeg(filename=dest_filename);  
       hist(InputDataSet$tipped, col = ''lightgreen'', xlab=''Tipped'',   
           ylab = ''Counts'', main = ''Histogram, Tipped'');  
        dev.off();  

       # Open a pdf file and output histograms of tip amount and fare amount.   
       # Outputs two plots in one row  
       dest_filename = tempfile(pattern = ''rHistograms_Tip_and_Fare_Amount_'', tmpdir = mainDir)  
       dest_filename = paste(dest_filename, ''.pdf'',sep="")  
       print(dest_filename, quote=FALSE);  
       pdf(file=dest_filename, height=4, width=7);  
       par(mfrow=c(1,2));  
       hist(InputDataSet$tip_amount, col = ''lightgreen'',   
           xlab=''Tip amount ($)'',   
           ylab = ''Counts'',   
           main = ''Histogram, Tip amount'', xlim = c(0,40), 100);  
       hist(InputDataSet$fare_amount, col = ''lightgreen'',   
           xlab=''Fare amount ($)'',   
           ylab = ''Counts'',   
           main = ''Histogram,   
           Fare amount'',   
           xlim = c(0,100), 100);  
       dev.off();  
  
       # Open a pdf file and output an xyplot of tip amount vs. fare amount using lattice;  
       # Only 10,000 sampled observations are plotted here, otherwise file is large.  
       dest_filename = tempfile(pattern = ''rXYPlots_Tip_vs_Fare_Amount_'', tmpdir = mainDir)  
       dest_filename = paste(dest_filename, ''.pdf'',sep="")  
       print(dest_filename, quote=FALSE);  
       pdf(file=dest_filename, height=4, width=4);  
       plot(tip_amount ~ fare_amount,   
           data = InputDataSet[sample(nrow(InputDataSet), 10000), ],   
           ylim = c(0,50),   
           xlim = c(0,150),   
           cex=.5,   
           pch=19,   
           col=''darkgreen'',    
           main = ''Tip amount by Fare amount'',   
           xlab=''Fare Amount ($)'',   
           ylab = ''Tip Amount ($)'');   
       dev.off();',  
     @input_data_1 = @query  
   END
   ```
  
Entre los puntos clave que se deben comprender en este script se incluyen los siguientes:

+ El resultado de la consulta SELECT en el procedimiento almacenado se almacena en la trama de datos predeterminada de R, `InputDataSet`. Se puede llamar a varias funciones de trazado de R para generar los archivos de gráficos reales. La mayoría de los scripts insertados de R representan las opciones de estas funciones de gráficos, como `plot` o `hist`.
  
+ El dispositivo de R está establecido en **desactivado** porque está ejecutando este comando como un script externo en SQL Server. Normalmente en R, al enviar un comando de trazado de alto nivel, R abre una ventana de gráficos, denominada *dispositivo*. Puede desactivar el dispositivo si está escribiendo en un archivo o controlando el resultado de otra manera.
  
+ Todos los archivos se guardan en la carpeta local C:\temp\Plots. La carpeta de destino se define mediante los argumentos proporcionados para el script de R como parte del procedimiento almacenado. Para generar los archivos en una carpeta diferente, cambie el valor de la variable `mainDir` en el script insertado de R en el procedimiento almacenado. También puede modificar el script para generar diferentes formatos, más archivos, etc.

### <a name="execute-the-stored-procedure"></a>Ejecutar el procedimiento almacenado

Ejecute la siguiente instrucción para exportar datos de trazados binarios a formatos de archivo JPEG y PDF.

```sql
EXEC RPlotHist
```

**Resultados**

```text
STDOUT message(s) from external script:
[1] Creating output plot files:[1] C:\temp\plots\rHistogram_Tipped_18887f6265d4.jpg[1] 

C:\temp\plots\rHistograms_Tip_and_Fare_Amount_1888441e542c.pdf[1]

C:\temp\plots\rXYPlots_Tip_vs_Fare_Amount_18887c9d517b.pdf
```

Los números de los nombres de archivo se generan de forma aleatoria para asegurarse de que no recibe un error al intentar escribir en un archivo existente.

### <a name="view-output"></a>Ver salida

Para ver el trazado, abra la carpeta de destino y revise los archivos creados por el código R en el procedimiento almacenado.

1. Vaya a la carpeta indicada en el mensaje STDOUT (en el ejemplo, es C:\temp\plots\).

2. Abra `rHistogram_Tipped.jpg` para mostrar el número de viajes que obtuvieron una propina frente a los que no obtuvieron propina (este histograma es similar al que ya generó en el paso anterior).

3. Abra `rHistograms_Tip_and_Fare_Amount.pdf` para ver la distribución de las cantidades de las propinas, trazadas con respecto a los importes de las tarifas.

   ![histograma en el que se muestra tip_amount y fare_amount](media/rsql-devtut-tipamtfareamt.PNG "histograma que muestra tip_amount y fare_amount")

4. Abra `rXYPlots_Tip_vs_Fare_Amount.pdf` para ver un gráfico de dispersión con el importe de la tarifa en el eje x y la cantidad de la propina en el eje y.

   ![cantidad de la propina trazada por importe de la tarifa](media/rsql-devtut-tipamtbyfareamt.PNG "cantidad de la propina trazada por importe de la tarifa")

## <a name="next-steps"></a>Pasos siguientes

En este artículo:

> [!div class="checklist"]
> + Revisó los datos de ejemplo
> + Creó trazados con R en T-SQL
> + Generó trazados en varios formatos de archivo

> [!div class="nextstepaction"]
> [Tutorial de R: Creación de características de datos](r-taxi-classification-create-features.md)