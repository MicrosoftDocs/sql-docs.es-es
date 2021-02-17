---
description: sp_execute_external_script (Transact-SQL)
title: sp_execute_external_script (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: reference
f1_keywords:
- sp_execute_external_script_TSQL
- sys.sp_execute_external_script
- sys.sp_execute_external_script_TSQL
- sp_execute_external_script
dev_langs:
- TSQL
helpviewer_keywords:
- sp_execute_external_script
ms.assetid: de4e1fcd-0e1a-4af3-97ee-d1becc7f04df
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: f7c407416b8181864e7cc2fecdc4c95f97ab03cc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355256"
---
# <a name="sp_execute_external_script-transact-sql"></a>sp_execute_external_script (Transact-SQL)
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
El **sp_execute_external_script** procedimiento almacenado ejecuta un script proporcionado como argumento de entrada para el procedimiento, y se usa con [las extensiones](../../language-extensions/language-extensions-overview.md)de [Machine Learning Services](../../machine-learning/sql-server-machine-learning-services.md) y de lenguaje. 

Por Machine Learning Services, [Python](../../machine-learning/concepts/extension-python.md) y [R](../../machine-learning/concepts/extension-r.md) son lenguajes admitidos. En el caso de las extensiones de lenguaje, se admite Java pero debe definirse con [Create external Language](../../t-sql/statements/create-external-language-transact-sql.md).

Para ejecutar **sp_execute_external_script**, primero debe instalar Machine Learning Services o las extensiones de lenguaje. Para obtener más información, consulte [instalación de SQL Server Machine Learning Services (Python y R) en Windows](../../machine-learning/install/sql-machine-learning-services-windows-install.md) y [Linux](../../linux/sql-server-linux-setup-machine-learning.md), o [instalación de extensiones de lenguaje de SQL Server en Windows](../../language-extensions/install/windows-java.md) y [Linux](../../linux/sql-server-linux-setup-language-extensions-java.md).
::: moniker-end

::: moniker range="=sql-server-2017"
El **sp_execute_external_script** procedimiento almacenado ejecuta un script proporcionado como argumento de entrada para el procedimiento y se usa con [Machine Learning Services](../../machine-learning/sql-server-machine-learning-services.md) en SQL Server 2017.

Por Machine Learning Services, [Python](../../machine-learning/concepts/extension-python.md) y [R](../../machine-learning/concepts/extension-r.md) son lenguajes admitidos.

Para ejecutar **sp_execute_external_script**, primero debe instalar Machine Learning Services. Para obtener más información, consulte [instalación de SQL Server Machine Learning Services (Python y R) en Windows](../../machine-learning/install/sql-machine-learning-services-windows-install.md).
::: moniker-end

::: moniker range="=sql-server-2016"
El **sp_execute_external_script** procedimiento almacenado ejecuta un script proporcionado como argumento de entrada para el procedimiento y se usa con [R Services](../../machine-learning/r/sql-server-r-services.md) en SQL Server 2016.

Para R Services,  [r](../../machine-learning/concepts/extension-r.md) es el idioma admitido.

Para ejecutar **sp_execute_external_script**, primero debe instalar R Services. Para obtener más información, consulte [instalación de SQL Server Machine Learning Services (Python y R) en Windows](../../machine-learning/install/sql-r-services-windows-install.md).
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
El **sp_execute_external_script** procedimiento almacenado ejecuta un script proporcionado como argumento de entrada para el procedimiento y se usa con [Machine Learning Services en Azure SQL instancia administrada](/azure/azure-sql/managed-instance/machine-learning-services-overview).

Por Machine Learning Services, [Python](../../machine-learning/concepts/extension-python.md) y [R](../../machine-learning/concepts/extension-r.md) son lenguajes admitidos.

Para ejecutar **sp_execute_external_script**, primero debe habilitar Machine Learning Services. Para obtener más información, consulte la [Machine Learning Services en la documentación de Azure SQL instancia administrada](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
## <a name="syntax"></a>Sintaxis

```
sp_execute_external_script
    @language = N'language',
    @script = N'script'  
    [ , @input_data_1 = N'input_data_1' ]
    [ , @input_data_1_name = N'input_data_1_name' ]  
    [ , @input_data_1_order_by_columns = N'input_data_1_order_by_columns' ]
    [ , @input_data_1_partition_by_columns = N'input_data_1_partition_by_columns' ]  
    [ , @output_data_1_name = N'output_data_1_name' ]  
    [ , @parallel = 0 | 1 ]  
    [ , @params = N'@parameter_name data_type [ OUT | OUTPUT ] [ ,...n ]' ] 
    [ , @parameter1 = 'value1' [ OUT | OUTPUT ] [ ,...n ] ]
```
::: moniker-end
::: moniker range="=sql-server-2016"
## <a name="syntax-for-sql-server-2017-and-earlier"></a>Sintaxis para SQL Server 2017 y versiones anteriores

```
sp_execute_external_script   
    @language = N'language',   
    @script = N'script'  
    [ , @input_data_1 = N'input_data_1' ]   
    [ , @input_data_1_name = N'input_data_1_name' ]  
    [ , @output_data_1_name = N'output_data_1_name' ]  
    [ , @parallel = 0 | 1 ]  
    [ , @params = N'@parameter_name data_type [ OUT | OUTPUT ] [ ,...n ]' ] 
    [ , @parameter1 = 'value1' [ OUT | OUTPUT ] [ ,...n ] ]
```
::: moniker-end

## <a name="arguments"></a>Argumentos
 **\@ Language** = N '*Language*'  
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
 Indica el lenguaje del script. *Language* es de **tipo sysname**. Los valores válidos son **R**, **Python** y cualquier lenguaje definido con [Create external Language](../../t-sql/statements/create-external-language-transact-sql.md) (por ejemplo, Java).
::: moniker-end
::: moniker range="=sql-server-2017"
 Indica el lenguaje del script. *Language* es de **tipo sysname**. En SQL Server 2017, los valores válidos son **R** y **Python**.
::: moniker-end
::: moniker range="=sql-server-2016"
 Indica el lenguaje del script. *Language* es de **tipo sysname**. En SQL Server 2016, el único valor válido es **R**.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
 Indica el lenguaje del script. *Language* es de **tipo sysname**. En Azure SQL Instancia administrada, los valores válidos son **R** y **Python**.
::: moniker-end

 **\@ script** = N script de lenguaje externo de '*script*' especificado como una entrada literal o de variable. el *script* es de tipo **nvarchar (Max)**.  

`[ @input_data_1 =  N'input_data_1' ]` Especifica los datos de entrada utilizados por el script externo en forma de [!INCLUDE[tsql](../../includes/tsql-md.md)] consulta. El tipo de datos de *input_data_1* es **nvarchar (Max)**.

`[ @input_data_1_name = N'input_data_1_name' ]` Especifica el nombre de la variable utilizada para representar la consulta definida por @input_data_1 . El tipo de datos de la variable en el script externo depende del lenguaje. En el caso de R, la variable de entrada es una trama de datos. En el caso de Python, la entrada debe ser tabular. *input_data_1_name* es de **tipo sysname**.  El valor predeterminado es *InputDataSet*.  

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
`[ @input_data_1_order_by_columns = N'input_data_1_order_by_columns' ]` Se usa para compilar modelos por partición. Especifica el nombre de la columna utilizada para ordenar el conjunto de resultados, por ejemplo por nombre de producto. El tipo de datos de la variable en el script externo depende del lenguaje. En el caso de R, la variable de entrada es una trama de datos. En el caso de Python, la entrada debe ser tabular.

`[ @input_data_1_partition_by_columns = N'input_data_1_partition_by_columns' ]` Se usa para compilar modelos por partición. Especifica el nombre de la columna utilizada para segmentar los datos, como la región geográfica o la fecha. El tipo de datos de la variable en el script externo depende del lenguaje. En el caso de R, la variable de entrada es una trama de datos. En el caso de Python, la entrada debe ser tabular. 
::: moniker-end

`[ @output_data_1_name =  N'output_data_1_name' ]` Especifica el nombre de la variable del script externo que contiene los datos que se van a devolver al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] completarse la llamada al procedimiento almacenado. El tipo de datos de la variable en el script externo depende del lenguaje. Para R, la salida debe ser una trama de datos. En el caso de Python, la salida debe ser una trama de datos pandas. *output_data_1_name* es de **tipo sysname**.  El valor predeterminado es *OutputDataSet*.  

`[ @parallel = 0 | 1 ]` Habilite la ejecución en paralelo de los scripts de R estableciendo el `@parallel` parámetro en 1. El valor predeterminado para este parámetro es 0 (sin paralelismo). Si `@parallel = 1` y la salida se transmite directamente al equipo cliente, `WITH RESULT SETS` se requiere la cláusula y se debe especificar un esquema de salida.  

 + En el caso de los scripts de R que no usan funciones de RevoScaleR, el uso del  `@parallel` parámetro puede ser beneficioso para el procesamiento de grandes conjuntos de archivos, suponiendo que el script se puede paralelizar de forma trivial. Por ejemplo, al usar la función de R `predict` con un modelo para generar nuevas predicciones, establezca `@parallel = 1` como sugerencia en el motor de consultas. Si la consulta se puede ejecutar en paralelo, las filas se distribuyen según el valor de **MAXDOP** .  
  
 + En el caso de los scripts de R que usan funciones RevoScaleR, el procesamiento en paralelo se administra automáticamente y no debe especificarse `@parallel = 1` en la llamada **sp_execute_external_script** .  
  
`[ @params = N'@parameter_name data_type [ OUT | OUTPUT ] [ ,...n ]' ]` Una lista de declaraciones de parámetros de entrada que se usan en el script externo.  
  
`[ @parameter1 = 'value1' [ OUT | OUTPUT ] [ ,...n ] ]` Una lista de valores para los parámetros de entrada utilizados por el script externo.  

## <a name="remarks"></a>Observaciones

> [!IMPORTANT]
> El árbol de consultas se controla mediante el aprendizaje automático de SQL y los usuarios no pueden realizar operaciones arbitrarias en la consulta.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Use **sp_execute_external_script** para ejecutar scripts escritos en un lenguaje compatible. Los idiomas admitidos son **Python** y **R** usados con Machine Learning Services y cualquier lenguaje definido con [Create external Language](../../t-sql/statements/create-external-language-transact-sql.md) (por ejemplo, Java) usado con extensiones de lenguaje.
::: moniker-end
::: moniker range="=sql-server-2017"
Use **sp_execute_external_script** para ejecutar scripts escritos en un lenguaje compatible. Los idiomas admitidos son **Python** y **R** en SQL Server 2017 Machine Learning Services.
::: moniker-end
::: moniker range="=sql-server-2016"
Use **sp_execute_external_script** para ejecutar scripts escritos en un lenguaje compatible. El único idioma admitido es **R** en SQL Server 2016 r Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Use **sp_execute_external_script** para ejecutar scripts escritos en un lenguaje compatible. Los idiomas admitidos son **Python** y **R** en Azure SQL instancia administrada Machine Learning Services.
::: moniker-end

De forma predeterminada, los conjuntos de resultados devueltos por este procedimiento almacenado se generan con columnas sin nombre. Los nombres de columna que se usan en un script son locales para el entorno de scripting y no se reflejan en el conjunto de resultados de salida. Para asignar un nombre a las columnas del conjunto de resultados, use la `WITH RESULT SET` cláusula de [`EXECUTE`](../../t-sql/language-elements/execute-transact-sql.md) .

Además de devolver un conjunto de resultados, puede devolver valores escalares a mediante parámetros de salida.

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
Puede controlar los recursos utilizados por los scripts externos mediante la configuración de un grupo de recursos externos. Para obtener más información, vea [Create external Resource POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md). La información sobre la carga de trabajo se puede obtener a partir de las vistas de catálogo del regulador de recursos, las DMV y los contadores. Para obtener más información, vea [Resource Governor vistas de catálogo &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md), [Resource Governor vistas de administración dinámica relacionadas &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md)y [SQL Server objeto de scripts externos](../../relational-databases/performance-monitor/sql-server-external-scripts-object.md).  
::: moniker-end

### <a name="monitor-script-execution"></a>Supervisión de la ejecución del script

Supervise la ejecución de scripts mediante [Sys.dm_external_script_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md) y [Sys.dm_external_script_execution_stats](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-execution-stats.md).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
### <a name="parameters-for-partition-modeling"></a>Parámetros para el modelado de particiones

Puede establecer dos parámetros adicionales que permitan el modelado de datos con particiones, donde las particiones se basan en una o varias columnas que se proporcionan para segmentar lógicamente un conjunto de datos en particiones lógicas creadas y usadas únicamente durante la ejecución del script. Las columnas que contienen valores de repetición de edad, sexo, región geográfica, fecha u hora, son algunos ejemplos que se prestan a conjuntos de datos con particiones.

Los dos parámetros son **input_data_1_partition_by_columns** y **input_data_1_order_by_columns**, donde el segundo parámetro se utiliza para ordenar el conjunto de resultados. Los parámetros se pasan como entradas a `sp_execute_external_script` con el script externo que se ejecuta una vez para cada partición. Para obtener más información y ejemplos, vea [Tutorial: crear modelos basados en particiones](../../machine-learning/tutorials/r-tutorial-create-models-per-partition.md).

Puede ejecutar un script en paralelo especificando `@parallel=1` . Si la consulta de entrada puede ejecutarse en paralelo, debe establecer `@parallel=1` como parte de los argumentos en `sp_execute_external_script` . De forma predeterminada, el optimizador de consultas opera en `@parallel=1` las tablas que tienen más de 256 filas, pero si desea controlar esto explícitamente, este script incluye el parámetro como una demostración.

> [!Tip]
> En las cargas de trabajo de entrenamiento, puede usar `@parallel` con cualquier script de entrenamiento arbitrario, incluso aquellos que usan algoritmos rx que no son de Microsoft. Normalmente, solo los algoritmos de RevoScaleR (con el prefijo rx) ofrecen paralelismo en escenarios de entrenamiento de SQL Server. Pero con los nuevos parámetros en SQL Server 2019 y versiones posteriores, puede paralelizar un script que llama a funciones que no se han diseñado específicamente con esa capacidad.
::: moniker-end

### <a name="streaming-execution-for-python-and-r-scripts"></a>Ejecución de streaming para Python y scripts de R  

Streaming permite que el script de Python o R funcione con más datos de los que caben en la memoria. Para controlar el número de filas que se pasan durante la transmisión por secuencias, especifique un valor entero para el parámetro `@r_rowsPerRead` en la `@params` colección.  Por ejemplo, si va a entrenar un modelo que utiliza datos muy anchos, puede ajustar el valor para que se lean menos filas, para asegurarse de que todas las filas se pueden enviar en un fragmento de datos. También puede usar este parámetro para administrar el número de filas que se leen y procesan al mismo tiempo, a fin de mitigar los problemas de rendimiento del servidor. 
  
Tanto el `@r_rowsPerRead` parámetro para la transmisión por secuencias como el `@parallel` argumento deben considerarse como sugerencias. Para que se aplique la sugerencia, debe ser posible generar un plan de consulta SQL que incluya el procesamiento en paralelo. Si esto no es posible, no se puede habilitar el procesamiento en paralelo.  
  
> [!NOTE]  
> La transmisión por secuencias y el procesamiento paralelo solo se admiten en la edición Enterprise. Puede incluir los parámetros en las consultas en Standard Edition sin producir un error, pero los parámetros no tienen ningún efecto y los scripts de R se ejecutan en un único proceso.  
  
## <a name="restrictions"></a>Restricciones  

### <a name="data-types"></a>Tipos de datos

Los siguientes tipos de datos no se admiten cuando se usan en la consulta de entrada o los parámetros de **sp_execute_external_script** procedimiento y devuelven un error de tipo no compatible.  

Como solución alternativa, **convierta** la columna o el valor a un tipo compatible en [!INCLUDE[tsql](../../includes/tsql-md.md)] antes de enviarlo al script externo.  
  
+ **cursor**  
  
+ **timestamp**  
  
+ **datetime2**, **DateTimeOffset**, **Time**  
  
+ **sql_variant**  
  
+ **texto**, **imagen**  
  
+ **xml**  
  
+ **hierarchyid**, **Geometry**, **Geography**  
  
+ Tipos definidos por el usuario CLR

En general, los conjuntos de resultados que no se pueden asignar a un [!INCLUDE[tsql](../../includes/tsql-md.md)] tipo de datos se generan como valores NULL.  

### <a name="restrictions-specific-to-r"></a>Restricciones específicas de R

Si la entrada incluye valores de **fecha y hora** que no se ajustan al intervalo de valores permitido en R, los valores se convierten en **na**. Esto es necesario porque el aprendizaje automático de SQL permite un intervalo mayor de valores que se admite en el lenguaje R.

Los valores Float (por ejemplo, `+Inf` , `-Inf` , `NaN` ) no se admiten en el aprendizaje automático de SQL aunque ambos lenguajes usen IEEE 754. El comportamiento actual solo envía los valores directamente a SQL. como resultado, el cliente SQL produce un error. Por lo tanto, estos valores se convierten en **null**.

## <a name="permissions"></a>Permisos

Requiere el permiso ejecutar cualquier base de datos de **script externa** .  

## <a name="examples"></a>Ejemplos

Esta sección contiene ejemplos de cómo se puede usar este procedimiento almacenado para ejecutar scripts de R o Python mediante [!INCLUDE[tsql](../../includes/tsql-md.md)] .

### <a name="a-return-an-r-data-set-to-sql-server"></a>A. Devuelve un conjunto de datos de R a SQL Server  

En el ejemplo siguiente se crea un procedimiento almacenado que utiliza **sp_execute_external_script** para devolver el conjunto de resultados de iris incluido con R.  

```sql
DROP PROC IF EXISTS get_iris_dataset;  
go  
CREATE PROC get_iris_dataset
AS  
BEGIN  
 EXEC   sp_execute_external_script  
       @language = N'R'  
     , @script = N'iris_data <- iris;'
     , @input_data_1 = N''  
     , @output_data_1_name = N'iris_data'
     WITH RESULT SETS (("Sepal.Length" float not null,
           "Sepal.Width" float not null,  
        "Petal.Length" float not null,
        "Petal.Width" float not null, "Species" varchar(100)));  
END;
GO
```

### <a name="b-create-a-python-model-and-generate-scores-from-it"></a>B. Creación de un modelo de Python y generar puntuaciones a partir de él

En este ejemplo se muestra cómo usar `sp_execute_external_script` para generar puntuaciones en un modelo de Python sencillo.

```sql
CREATE PROCEDURE [dbo].[py_generate_customer_scores]
AS
BEGIN

-- Input query to generate the customer data
DECLARE @input_query NVARCHAR(MAX) = N'SELECT customer, orders, items, cost FROM dbo.Sales.Orders'

EXEC sp_execute_external_script @language = N'Python', @script = N'
import pandas as pd
from sklearn.cluster import KMeans

# Get data from input query
customer_data = my_input_data

# Define the model
n_clusters = 4
est = KMeans(n_clusters=n_clusters, random_state=111).fit(customer_data[["orders","items","cost"]])
clusters = est.labels_
customer_data["cluster"] = clusters

OutputDataSet = customer_data
'
, @input_data_1 = @input_query
, @input_data_1_name = N'my_input_data'
WITH RESULT SETS (("CustomerID" int, "Orders" float,"Items" float,"Cost" float,"ClusterResult" float));
END;
GO
```

Los encabezados de columna que se usan en el código de Python no se muestran en SQL Server; por lo tanto, use la instrucción WITH RESULT para especificar los nombres de columna y los tipos de datos que SQL va a utilizar.

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
### <a name="c-generate-an-r-model-based-on-data-from-sql-server"></a>C. Generar un modelo de R basado en datos de SQL Server  

En el ejemplo siguiente se crea un procedimiento almacenado que utiliza **sp_execute_external_script** para generar un modelo de iris y devolver el modelo.  

> [!NOTE]
> Este ejemplo requiere la instalación anticipada del paquete e1071. Para obtener más información, consulte [instalación de paquetes de R adicionales en SQL Server](../../machine-learning/package-management/install-additional-r-packages-on-sql-server.md).

```sql
DROP PROC IF EXISTS generate_iris_model;
GO
CREATE PROC generate_iris_model
AS
BEGIN
 EXEC sp_execute_external_script  
      @language = N'R'  
     , @script = N'  
          library(e1071);  
          irismodel <-naiveBayes(iris_data[,1:4], iris_data[,5]);  
          trained_model <- data.frame(payload = as.raw(serialize(irismodel, connection=NULL)));  
'  
     , @input_data_1 = N'select "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "Species" from iris_data'  
     , @input_data_1_name = N'iris_data'  
     , @output_data_1_name = N'trained_model'  
    WITH RESULT SETS ((model varbinary(max)));  
END;
GO
```

Para generar un modelo similar mediante Python, tendría que cambiar el identificador de idioma de `@language=N'R'` a `@language = N'Python'`y realizar las modificaciones necesarias para el argumento `@script`. En caso contrario, todos los parámetros funcionan del mismo modo que para R.
::: moniker-end

Para puntuar, también puede usar la función nativa [PREDICT](../../t-sql/queries/predict-transact-sql.md), que es normalmente más rápida porque evita llamar al runtime de Python o R.

## <a name="see-also"></a>Consulte también

+ [Aprendizaje automático de SQL](../../machine-learning/index.yml)
+ [Extensiones de lenguaje de SQL Server](../../language-extensions/language-extensions-overview.md). 
+ [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
+ [CREAR biblioteca externa &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-library-transact-sql.md)  
+ [sp_prepare &#40;Transact SQL&#41;](../../relational-databases/system-stored-procedures/sp-prepare-transact-sql.md)   
+ [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
+ [Opción de configuración de servidor Scripts externos habilitados](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)   
+ [SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)   
+ [SQL Server, objeto External Scripts](../../relational-databases/performance-monitor/sql-server-external-scripts-object.md)  
+ [sys.dm_external_script_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md)  
+ [sys.dm_external_script_execution_stats](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-execution-stats.md)