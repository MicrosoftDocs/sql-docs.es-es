---
title: 'Inicio rápido: estructuras de datos, tipos de datos y objetos de R'
titleSuffix: SQL machine learning
description: En este inicio rápido, aprenderá a usar estructuras de datos, tipos de datos y objetos mediante R con aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e005d979e1c2b2de531979d18d2a7fc882f2b734
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339632"
---
# <a name="quickstart-data-structures-data-types-and-objects-using-r-with-sql-machine-learning"></a>Inicio rápido: Estructuras de datos, tipos de datos y objetos mediante R con aprendizaje automático de SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
En este inicio rápido, aprenderá a usar estructuras de datos y tipos de datos cuando use R en [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) o en [clústeres de macrodatos](../../big-data-cluster/machine-learning-services.md). Aprenderá a mover datos entre R y SQL Server, así como los problemas comunes que pueden producirse.
::: moniker-end
::: moniker range="=sql-server-2017"
En este inicio rápido, aprenderá a usar estructuras de datos y tipos de datos cuando use R en [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Aprenderá a mover datos entre R y SQL Server, así como los problemas comunes que pueden producirse.
::: moniker-end
::: moniker range="=sql-server-2016"
En este inicio rápido, aprenderá a usar estructuras de datos y tipos de datos cuando use R en [SQL Server R Services](../r/sql-server-r-services.md). Aprenderá a mover datos entre R y SQL Server, así como los problemas comunes que pueden producirse.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
En este inicio rápido, obtendrá información sobre cómo usar estructuras de datos y tipos de datos cuando use R en [Machine Learning Services en Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview). Obtendrá información sobre cómo mover datos entre R y SQL Managed Instance, así como las incidencias comunes que pueden producirse.
::: moniker-end

Algunos de los problemas más comunes son:

- A veces, los tipos de datos no coinciden
- Pueden producirse conversiones implícitas
- En ocasiones se requieren operaciones de conversión
- En R y SQL se usan objetos de datos distintos

## <a name="prerequisites"></a>Prerrequisitos

Para ejecutar este inicio rápido, debe cumplir los siguientes requisitos previos.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
- SQL Server Machine Learning Services. Para instalar Machine Learning Services, vea la [Guía de instalación para Windows](../install/sql-machine-learning-services-windows-install.md) o la [Guía de instalación para Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json). También puede [habilitar Machine Learning Services en clústeres de macrodatos de SQL Server](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
- SQL Server Machine Learning Services. Para instalar Machine Learning Services, vea la [Guía de instalación para Windows](../install/sql-machine-learning-services-windows-install.md). 
::: moniker-end
::: moniker range="=sql-server-2016"
- SQL Server 2016 R Services. Para instalar R Services, consulte la [Guía de instalación de Windows](../install/sql-r-services-windows-install.md). 
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
- Machine Learning Services en Azure SQL Managed Instance. Para obtener información, vea [Machine Learning Services de Instancia administrada de Azure SQL (versión preliminar)](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

- Una herramienta para ejecutar consultas de SQL que contengan scripts de R. En este inicio rápido se utiliza [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

## <a name="always-return-a-data-frame"></a>Devolver siempre una trama de datos

Cuando el script devuelve resultados de R a SQL Server, debe devolverlos como una trama de datos (**data.frame**). Cualquier otro tipo de objeto que se genere en el script (sea una lista, un factor, un vector o datos binarios) debe convertirse en una trama de datos si se quiere incluir en los resultados del procedimiento almacenado. Afortunadamente, hay varias funciones de R que permiten convertir otros objetos en una trama de datos. Se puede incluso serializar un modelo binario y devolverlo en una trama de datos, algo que haremos más adelante en esta guía de inicio rápido.

En primer lugar, experimentaremos con algunos objetos básicos de R (vectores, matrices y listas) y comprobaremos cómo al convertirlos en una trama de datos, el resultado pasado a SQL Server cambia.

Compararemos estos dos scripts "Hola mundo" en R. Los scripts son casi idénticos, pero el primero devuelve una única columna de tres valores, mientras que el segundo devuelve tres columnas con un valor único cada una.

**Ejemplo 1**

```sql
EXECUTE sp_execute_external_script
       @language = N'R'
     , @script = N' mytextvariable <- c("hello", " ", "world");
       OutputDataSet <- as.data.frame(mytextvariable);'
     , @input_data_1 = N' ';
```

**Ejemplo 2**

```sql
EXECUTE sp_execute_external_script
        @language = N'R'
      , @script = N' OutputDataSet<- data.frame(c("hello"), " ", c("world"));'
      , @input_data_1 = N'  ';
```

## <a name="identify-schema-and-data-types"></a>Identificación de tipos de datos y esquemas

¿Por qué son tan diferentes los resultados?

Normalmente la respuesta se puede encontrar mediante el comando `str()` de R. Agregue la función `str(object_name)` en cualquier lugar del script de R para que el esquema de datos del objeto de R especificado se devuelva como un mensaje informativo.

Para averiguar por qué en los ejemplos 1 y 2 los resultados son tan diferentes, inserte la línea `str(OutputDataSet)` al final de la definición de variable `@script`de cada instrucción, de esta forma:

**Ejemplo 1 con la función str agregada**

```sql
EXECUTE sp_execute_external_script
        @language = N'R'
      , @script = N' mytextvariable <- c("hello", " ", "world");
      OutputDataSet <- as.data.frame(mytextvariable);
      str(OutputDataSet);'
      , @input_data_1 = N'  '
;
```

**Ejemplo 2 con la función str agregada**

```sql
EXECUTE sp_execute_external_script
  @language = N'R', 
  @script = N' OutputDataSet <- data.frame(c("hello"), " ", c("world"));
    str(OutputDataSet);' , 
  @input_data_1 = N'  ';
```

Ahora, revise el texto de **Mensajes** para ver por qué el resultado es diferente.

**Resultados: Ejemplo 1**

```sql
STDOUT message(s) from external script:
'data.frame':   3 obs. of  1 variable:
$ mytextvariable: Factor w/ 3 levels " ","hello","world": 2 1 3
```

**Resultados: Ejemplo 2**

```sql
STDOUT message(s) from external script:
'data.frame':   1 obs. of  3 variables:
$ c..hello..: Factor w/ 1 level "hello": 1
$ X...      : Factor w/ 1 level " ": 1
$ c..world..: Factor w/ 1 level "world": 1
```

Como puede ver, un pequeño cambio en la sintaxis de R ha tenido un gran efecto en el esquema de los resultados. No entraremos en los motivos, pero las diferencias en los tipos de datos de R se explican en los detalles de la sección *Estructuras de datos* en ["R avanzada" de Hadley Wickham](http://adv-r.had.co.nz).

Por ahora, simplemente tenga presente que deberá comprobar los resultados esperados al convertir objetos de R en tramas de datos.

> [!TIP]
> También puede usar funciones de identidad de R, como `is.matrix`, `is.vector`, para devolver información sobre la estructura de datos interna.

## <a name="implicit-conversion-of-data-objects"></a>Conversión implícita de objetos de datos

Cada objeto de datos de R tiene sus propias reglas sobre cómo controlar los valores cuando se combinan con otros objetos de datos si los dos objetos de datos tienen el mismo número de dimensiones, o bien si un objeto de datos contiene tipos de datos heterogéneos.

En primer lugar, cree una tabla pequeña de datos de prueba.

```sql
CREATE TABLE RTestData (col1 INT NOT NULL)

INSERT INTO RTestData
VALUES (1);

INSERT INTO RTestData
VALUES (10);

INSERT INTO RTestData
VALUES (100);
GO
```

Por ejemplo, supongamos que ejecutamos la siguiente instrucción para multiplicar matrices mediante R. Una matriz de una sola columna con tres valores se multiplica por una matriz con cuatro valores, de lo que se espera como resultado una matriz 4x3.

```sql
EXECUTE sp_execute_external_script
    @language = N'R'
    , @script = N'
        x <- as.matrix(InputDataSet);
        y <- array(12:15);
    OutputDataSet <- as.data.frame(x %*% y);'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
    WITH RESULT SETS (([Col1] int, [Col2] int, [Col3] int, Col4 int));
```

En segundo plano, la columna de tres valores se convierte en una matriz de una sola columna. Como una matriz es simplemente un caso especial de una matriz de R, la matriz `y` se convierte de forma implícita en una matriz de una sola columna para que los dos argumentos coincidan.

**Resultados**

|Col1|Col2|Col3|Col4|
|---|---|---|---|
|12|13|14|15|
|120|130|140|150|
|1200|1300|1400|1.500|

Con todo, observe lo que ocurre cuando se cambia el tamaño de la matriz `y`.

```sql
execute sp_execute_external_script
   @language = N'R'
   , @script = N'
        x <- as.matrix(InputDataSet);
        y <- array(12:14);
   OutputDataSet <- as.data.frame(y %*% x);'
   , @input_data_1 = N' SELECT [Col1]  from RTestData;'
   WITH RESULT SETS (([Col1] int ));
```

Ahora R devuelve un valor único como resultado.

**Resultados**

|Col1|
|---|
|1542|

¿Por qué? En este caso, dado que los dos argumentos se pueden tratar como vectores de la misma longitud, R devuelve el producto interior como una matriz.  Este es el comportamiento esperado según las reglas de álgebra lineal, pero podría causar problemas si la aplicación auxiliar espera que el esquema de salida no cambie nunca.

> [!TIP]
>
> ¿Obtiene errores? Asegúrese de que está ejecutando el procedimiento almacenado en el contexto de la base de datos que contiene la tabla y no en **maestro** u otra base de datos.
>
> Además, se recomienda evitar el uso de tablas temporales en estos ejemplos. Algunos clientes de R finalizarán una conexión entre lotes y eliminarán tablas temporales.

## <a name="merge-or-multiply-columns-of-different-length"></a>Combinación o multiplicación de columnas de longitud diferente

R proporciona una gran flexibilidad a la hora de trabajar con vectores de diferentes tamaños y de combinar estas estructuras (similares a las columnas) en tramas de datos. Las listas de vectores pueden ser similares a una tabla, pero no siguen todas las reglas que rigen las tablas de base de datos.

Por ejemplo, en el script siguiente se define una matriz numérica de longitud 6 y se almacena en la variable de R `df1`. Después, la matriz numérica se combina con los enteros de la tabla RTestData, que contiene tres (3) valores para crear una nueva trama de datos, `df2`.

```sql
EXECUTE sp_execute_external_script
    @language = N'R'
    , @script = N'
               df1 <- as.data.frame( array(1:6) );
               df2 <- as.data.frame( c( InputDataSet , df1 ));
               OutputDataSet <- df2'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
    WITH RESULT SETS (( [Col2] int not null, [Col3] int not null ));
```

Para rellenar la trama de datos, R repite los elementos recuperados de RTestData tantas veces como sea necesario para que coincidan con el número de elementos de la matriz `df1`.

**Resultados**

|*Col2*|*Col3*|
|----|----|
|1|1|
|10|2|
|100|3|
|1|4|
|10|5|
|100|6|

Recuerde que una trama de datos solo es similar a una tabla en cuanto al aspecto. En realidad es una lista de vectores.

## <a name="cast-or-convert-data"></a>Conversión de datos

R y SQL Server no usan los mismos tipos de datos, de modo que cuando se ejecuta una consulta en SQL Server para obtener datos y luego se pasa al tiempo de ejecución de R, suele producirse algún tipo de conversión implícita. Otra serie de conversiones ocurre cuando se devuelven datos de R a SQL Server.

- SQL Server inserta los datos de la consulta en el proceso de R administrado por el servicio Launchpad y los convierte en una representación interna para una mayor eficacia.
- El runtime de R carga los datos en una variable data.frame y realiza sus propias operaciones en los datos.
- El motor de base de datos devuelve los datos a SQL Server por medio de una conexión segura interna y los presenta como tipos de datos de SQL Server.
- Los datos se obtienen estableciendo una conexión a SQL Server con una biblioteca de red o de cliente capaz de emitir consultas SQL y tratar conjuntos de datos tabulares. Esta aplicación cliente puede afectar a los datos de otras maneras.

Para ver cómo funciona esto, ejecute una consulta como esta en el almacén de datos [AdventureWorksDW](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks). Esta vista devuelve datos de ventas que se usan en la creación de previsiones.

```sql
USE AdventureWorksDW
GO

SELECT ReportingDate
         , CAST(ModelRegion as varchar(50)) as ProductSeries
         , Amount
           FROM [AdventureWorksDW].[dbo].[vTimeSeries]
           WHERE [ModelRegion] = 'M200 Europe'
           ORDER BY ReportingDate ASC
```

> [!NOTE]
>
> Puede usar cualquier versión de AdventureWorks o crear una consulta diferente con una base de datos propia. Lo importante es intentar controlar algunos datos que contienen valores numéricos, de texto y de fecha y hora.

Ahora, intente pegar esta consulta como entrada para el procedimiento almacenado.

```sql
EXECUTE sp_execute_external_script
       @language = N'R'
      , @script = N' str(InputDataSet);
      OutputDataSet <- InputDataSet;'
      , @input_data_1 = N'
           SELECT ReportingDate
         , CAST(ModelRegion as varchar(50)) as ProductSeries
         , Amount
           FROM [AdventureWorksDW].[dbo].[vTimeSeries]
           WHERE [ModelRegion] = ''M200 Europe''
           ORDER BY ReportingDate ASC ;'
WITH RESULT SETS undefined;
```

Si se produce un error, probablemente tendrá que realizar algunas modificaciones en el texto de consulta. Por ejemplo, el predicado de cadena en la cláusula WHERE se debe incluir entre dos conjuntos de comillas simples.

Después de conseguir que la consulta funcione, revise los resultados de la función `str` para ver cómo R procesa los datos de entrada.

**Resultados**

```text
STDOUT message(s) from external script: 'data.frame':    37 obs. of  3 variables:
STDOUT message(s) from external script: $ ReportingDate: POSIXct, format: "2010-12-24 23:00:00" "2010-12-24 23:00:00"
STDOUT message(s) from external script: $ ProductSeries: Factor w/ 1 levels "M200 Europe",..: 1 1 1 1 1 1 1 1 1 1
STDOUT message(s) from external script: $ Amount       : num  3400 16925 20350 16950 16950
```

- La columna de fecha y hora se ha procesado con el tipo de datos de R, **POSIXct**.
- La columna de texto "ProductSeries" se ha identificado como un **factor** esto es, como una variable categórica. Los valores de cadena se procesan como factores de forma predeterminada. Si pasa una cadena a R, se convierte en un entero para uso interno y después se vuelve a asignar a la cadena de salida.

### <a name="summary"></a>Resumen

Incluso en estos pequeños ejemplos, puede ver la necesidad de comprobar los efectos de la conversión de datos al pasar consultas SQL como entrada. Dado que R no admite algunos tipos de datos de SQL Server, tenga en cuenta estas formas de evitar cualquier tipo de error:

- Compruebe los datos de antemano y discierna qué columnas o valores del esquema pueden ser problemáticos cuando se pasen al código de R.
- En el origen de datos de entrada, especifique las columnas de forma individual, en lugar de usar `SELECT *`, y sepa cómo se va a procesar cada columna.
- Para evitar sorpresas, realice las conversiones explícitas necesarias al preparar los datos de entrada.
- Evite pasar columnas de datos (como GUID o ROWGUID) que causan errores y no son útiles para el modelado.

Para obtener más información sobre los tipos de datos admitidos y no admitidos, vea [Bibliotecas de R y tipos de datos](../r/r-libraries-and-data-types.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo escribir funciones avanzadas de R con aprendizaje automático de SQL, siga este inicio rápido:

> [!div class="nextstepaction"]
> [Escritura de funciones de R avanzadas con aprendizaje automático de SQL](quickstart-r-functions.md)