---
title: Referencia de azdata bdc spark settings
titleSuffix: SQL Server big data clusters
description: Artículo de referencia sobre los comandos azdata bdc spark settings.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 66c80bc7d77ebc1f1f5c18e5fe972e4c6d61419d
ms.sourcegitcommit: 129c084add904fd3f7e9ab35a800c3fd8b1a8927
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100567321"
---
# <a name="azdata-bdc-spark-settings"></a>azdata bdc spark settings

Se aplica a [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

En el siguiente artículo se proporciona una referencia sobre los comandos **spark settings** de la herramienta **azdata**. Para obtener más información sobre otros comandos de **azdata**, vea la [Referencia de azdata](reference-azdata.md).

## <a name="commands"></a>Comandos
|Comando|Descripción|
| --- | --- |
[azdata bdc spark settings set](#azdata-bdc-spark-settings-set) | Establece la configuración del ámbito del servicio Spark.
[azdata bdc spark settings show](#azdata-bdc-spark-settings-show) | Muestra la configuración del ámbito del servicio Spark y, opcionalmente, la configuración de Spark para los recursos especificados.

## <a name="azdata-bdc-spark-settings-set"></a>azdata bdc spark settings set
Proporciona la capacidad de establecer una configuración del ámbito del servicio o del ámbito del recurso. Debe especificar el nombre completo de la configuración y el valor. La configuración no se aplica a un clúster de BDC en ejecución. Para ello, ejecute la actualización.
```bash
azdata bdc spark settings set --settings -s 
                        
```
### <a name="examples"></a>Ejemplos
Establezca el número predeterminado de núcleos de los controladores en 1 y la memoria predeterminada del controlador en 1664m para el servicio Spark. 
```bash 
azdata bdc spark settings set --settings spark-defaults-conf.spark.driver.cores=1,spark-defaults-conf.spark.driver.memory=1664m 
``` 
Establezca el número predeterminado de núcleos de los ejecutores en 1 para el bloque de almacenamiento. 
```bash 
azdata bdc spark  settings set --settings spark-defaults-conf.spark.executor.cores=1 –resources storage-0 
``` 

### <a name="required-parameters"></a>Parámetros requeridos
#### `--settings -s`
Establece el valor configurado para la configuración proporcionada. Se pueden establecer varios valores de configuración mediante una lista separada por comas.
### <a name="optional-parameters"></a>Parámetros opcionales 
#### `--resources` 
Establece las configuraciones especificadas para los recursos proporcionados. Los recursos se pueden enumerar en una lista separada por comas. 

### <a name="global-arguments"></a>Argumentos globales
#### `--debug`
Aumente el nivel de detalle de registro para mostrar todos los registros de depuración.
#### `--help -h`
Muestre este mensaje de ayuda y salga.
#### `--output -o`
Formato de salida.  Valores permitidos: json, jsonc, table y tsv.  Valor predeterminado: json.
#### `--query -q`
Cadena de consulta de JMESPath. Para obtener más información y ejemplos, vea [http://jmespath.org/](http://jmespath.org/).
#### `--verbose`
Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos.

## <a name="azdata-bdc-spark-settings-show"></a>azdata bdc spark settings show
Muestra la configuración del ámbito del servicio (opcionalmente, del ámbito del recurso) de `spark` para la instancia de BDC. De forma predeterminada, este comando muestra la configuración del ámbito del servicio definida por el usuario. Hay varios filtros disponibles para mostrar todos los valores de configuración (administrados por el sistema y configurables), los valores configurables o las configuraciones pendientes. Para ver un valor específico del ámbito del servicio o del ámbito del recurso, puede proporcionar el nombre del valor. Use "recursive" para mostrar la configuración de todos los recursos como parte del servicio. 
```bash

azdata bdc spark settings show 
[--settings -s]
[--filter-option -f]  
[--recursive -rec]
[--include-details -i]  
[--description -d]
```
### <a name="examples"></a>Ejemplos
Muestre la configuración del ámbito del servicio Spark configurada por el usuario.
```bash
azdata bdc spark settings show
```
Muestre el valor en ejecución y el valor configurado para los núcleos del controlador de Spark en el bloque de almacenamiento. 
```bash
azdata bdc spark settings show --settings spark-defaults-conf.spark.driver.cores --resources storage-0 --include-details
```
Muestre cualquier opción configurable relacionada con la memoria para el servicio Spark.
```bash
azdata bdc spark settings show --settings *memory* --resources storage-0 
```
Muestre los cambios de configuración pendientes para el ámbito del servicio y del ámbito del recurso de Spark.
```bash
azdata bdc spark settings show --filter-options=pending --recursive --include-details
```
### <a name="optional-parameters"></a>Parámetros opcionales 
#### `--filter-options | -f` 
Opciones para filtrar los valores de nivel de servicio o del ámbito del recurso que se muestran, en lugar de solo los valores configurados por el usuario. Hay varios filtros disponibles para mostrar todos los valores de configuración (administrados por el sistema y configurables por el usuario), todos los valores configurables o las configuraciones pendientes. Opciones: `userConfigured`, `all`, `pending` y `configurable`.
#### `--settings | -s` 
Muestra información de los nombres de configuración especificados. 
#### `--include-details | -i` 
Incluye detalles adicionales de los valores de configuración que se van a mostrar. 
#### `--description | -d` 
Incluye la descripción del valor de configuración. Se debe usar con "--include-details". 
#### `--resources | -r` 
Muestra la información de configuración de los recursos especificados. Los recursos se pueden enumerar en una lista separada por comas. 
#### `--recursive | -rec` 
Muestra la información de configuración del ámbito especificado (servicio o recurso de servicio), así como todos los componentes de ámbito inferior (recursos). 

### <a name="global-arguments"></a>Argumentos globales
#### `--debug`
Aumente el nivel de detalle de registro para mostrar todos los registros de depuración.
#### `--help -h`
Muestre este mensaje de ayuda y salga.
#### `--output -o`
Formato de salida.  Valores permitidos: json, jsonc, table y tsv.  Valor predeterminado: json.
#### `--query -q`
Cadena de consulta de JMESPath. Para obtener más información y ejemplos, vea [http://jmespath.org/](http://jmespath.org/).
#### `--verbose`
Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre otros comandos de **azdata**, vea [Referencia de azdata](reference-azdata.md). 

Para obtener más información sobre cómo instalar la herramienta **azdata**, vea [Instalación de azdata para administrar clústeres de macrodatos de SQL Server 2019](../install/deploy-install-azdata.md).
