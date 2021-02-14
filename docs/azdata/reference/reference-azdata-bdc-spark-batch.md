---
title: Referencia de azdata bdc spark batch
titleSuffix: SQL Server big data clusters
description: Artículo de referencia sobre los comandos de azdata bdc spark batch.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8e6dea113ff65125f24819b1f579f686e9452ee1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048805"
---
# <a name="azdata-bdc-spark-batch"></a>azdata bdc spark batch

Se aplica a [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

En el siguiente artículo se proporciona una referencia de los comandos **sql** de la herramienta **azdata**. Para obtener más información sobre otros comandos de **azdata**, vea la [Referencia de azdata](reference-azdata.md).

## <a name="commands"></a>Comandos

|Comando|Descripción|
| --- | --- |
[azdata bdc spark batch create](#azdata-bdc-spark-batch-create) | Cree una sesión de Spark.
[azdata bdc spark batch list](#azdata-bdc-spark-batch-list) | Enumere todos los lotes de Spark.
[azdata bdc spark batch info](#azdata-bdc-spark-batch-info) | Obtenga información sobre un lote de Spark activo.
[azdata bdc spark batch log](#azdata-bdc-spark-batch-log) | Obtenga los registros de ejecución de un lote de Spark.
[azdata bdc spark batch state](#azdata-bdc-spark-batch-state) | Obtenga el estado de ejecución de un lote de Spark.
[azdata bdc spark batch delete](#azdata-bdc-spark-batch-delete) | Elimine un lote de Spark.
## <a name="azdata-bdc-spark-batch-create"></a>azdata bdc spark batch create
Este comando crea un nuevo trabajo de Spark de Batch que ejecuta el código proporcionado.
```bash
azdata bdc spark batch create --file -f 
                              [--class-name -c]  
                              
[--arguments -a]  
                              
[--jar-files -j]  
                              
[--py-files -p]  
                              
[--files]  
                              
[--driver-memory]  
                              
[--driver-cores]  
                              
[--executor-memory]  
                              
[--executor-cores]  
                              
[--executor-count]  
                              
[--archives]  
                              
[--queue -q]  
                              
[--name -n]  
                              
[--config]
```
### <a name="examples"></a>Ejemplos
Cree una sesión de Spark.
```bash
azdata bdc spark batch create --code "2+2"
```
### <a name="required-parameters"></a>Parámetros obligatorios
#### `--file -f`
Ruta de acceso al archivo que se va a ejecutar.
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--class-name -c`
Nombre de la clase que se va a ejecutar al pasar uno o varios archivos jar.
#### `--arguments -a`
Lista de argumentos.  Para pasarlos en una lista, JSON codifica los valores.  Ejemplo: '["entry1", "entry2"]'.
#### `--jar-files -j`
Lista de rutas de acceso de archivo jar.  Para pasarlos en una lista, JSON codifica los valores.  Ejemplo: '["entry1", "entry2"]'.
#### `--py-files -p`
Lista de rutas de acceso de archivo de Python.  Para pasarlos en una lista, JSON codifica los valores.  Ejemplo: '["entry1", "entry2"]'.
#### `--files`
Lista de rutas de acceso de archivo.  Para pasarlos en una lista, JSON codifica los valores.  Ejemplo: '["entry1", "entry2"]'.
#### `--driver-memory`
Cantidad de memoria que se va a asignar al controlador.  Especifique las unidades como parte del valor.  Por ejemplo, 512M o 2G.
#### `--driver-cores`
Cantidad de núcleos de CPU que se van a asignar al controlador.
#### `--executor-memory`
Cantidad de memoria que se va a asignar al ejecutor.  Especifique las unidades como parte del valor.  Por ejemplo, 512M o 2G.
#### `--executor-cores`
Cantidad de núcleos de CPU que se van a asignar al ejecutor.
#### `--executor-count`
Número de instancias del ejecutor que se van a ejecutar.
#### `--archives`
Lista de rutas de acceso de los archivos.  Para pasarlos en una lista, JSON codifica los valores.  Ejemplo: '["entry1", "entry2"]'.
#### `--queue -q`
Nombre de la cola de Spark en la que se va a ejecutar la sesión.
#### `--name -n`
Nombre de la sesión de Spark.
#### `--config`
Lista de pares nombre-valor que contienen los valores de configuración de Spark.  Cifrada como un diccionario JSON.  Ejemplo: '{"name":"value", "name2":"value2"}'.
### <a name="global-arguments"></a>Argumentos globales
#### `--debug`
Aumente el nivel de detalle de registro para mostrar todos los registros de depuración.
#### `--help -h`
Muestre este mensaje de ayuda y salga.
#### `--output -o`
Formato de salida.  Valores permitidos: json, jsonc, table y tsv.  Valor predeterminado: json.
#### `--query -q`
Cadena de consulta de JMESPath. Para obtener más información y ejemplos, vea [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos.
## <a name="azdata-bdc-spark-batch-list"></a>azdata bdc spark batch list
Enumere todos los lotes de Spark.
```bash
azdata bdc spark batch list 
```
### <a name="examples"></a>Ejemplos
Enumere todos los lotes activos.
```bash
azdata bdc spark batch list
```
### <a name="global-arguments"></a>Argumentos globales
#### `--debug`
Aumente el nivel de detalle de registro para mostrar todos los registros de depuración.
#### `--help -h`
Muestre este mensaje de ayuda y salga.
#### `--output -o`
Formato de salida.  Valores permitidos: json, jsonc, table y tsv.  Valor predeterminado: json.
#### `--query -q`
Cadena de consulta de JMESPath. Para obtener más información y ejemplos, vea [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos.
## <a name="azdata-bdc-spark-batch-info"></a>azdata bdc spark batch info
Este comando obtiene la información de un lote de Spark con el identificador especificado.  El identificador de lote se devuelve de "spark batch create".
```bash
azdata bdc spark batch info --batch-id -i 
                            
```
### <a name="examples"></a>Ejemplos
Obtenga la información de lote del lote con el identificador 0.
```bash
azdata bdc spark batch info --batch-id 0
```
### <a name="required-parameters"></a>Parámetros obligatorios
#### `--batch-id -i`
Número de identificador de lote de Spark.
### <a name="global-arguments"></a>Argumentos globales
#### `--debug`
Aumente el nivel de detalle de registro para mostrar todos los registros de depuración.
#### `--help -h`
Muestre este mensaje de ayuda y salga.
#### `--output -o`
Formato de salida.  Valores permitidos: json, jsonc, table y tsv.  Valor predeterminado: json.
#### `--query -q`
Cadena de consulta de JMESPath. Para obtener más información y ejemplos, vea [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos.
## <a name="azdata-bdc-spark-batch-log"></a>azdata bdc spark batch log
Este comando obtiene las entradas del registro por lotes de un lote de Spark con el identificador especificado.  El identificador de lote se devuelve de "spark batch create".
```bash
azdata bdc spark batch log --batch-id -i 
                           
```
### <a name="examples"></a>Ejemplos
Obtenga el registro de lote del lote con el identificador 0.
```bash
azdata bdc spark batch log --batch-id 0
```
### <a name="required-parameters"></a>Parámetros obligatorios
#### `--batch-id -i`
Número de identificador de lote de Spark.
### <a name="global-arguments"></a>Argumentos globales
#### `--debug`
Aumente el nivel de detalle de registro para mostrar todos los registros de depuración.
#### `--help -h`
Muestre este mensaje de ayuda y salga.
#### `--output -o`
Formato de salida.  Valores permitidos: json, jsonc, table y tsv.  Valor predeterminado: json.
#### `--query -q`
Cadena de consulta de JMESPath. Para obtener más información y ejemplos, vea [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos.
## <a name="azdata-bdc-spark-batch-state"></a>azdata bdc spark batch state
Este comando obtiene el estado de lote de un lote de Spark con el identificador especificado.  El identificador de lote se devuelve de "spark batch create".
```bash
azdata bdc spark batch state --batch-id -i 
                             
```
### <a name="examples"></a>Ejemplos
Obtenga el estado de lote del lote con el identificador 0.
```bash
azdata bdc spark batch state --batch-id 0
```
### <a name="required-parameters"></a>Parámetros obligatorios
#### `--batch-id -i`
Número de identificador de lote de Spark.
### <a name="global-arguments"></a>Argumentos globales
#### `--debug`
Aumente el nivel de detalle de registro para mostrar todos los registros de depuración.
#### `--help -h`
Muestre este mensaje de ayuda y salga.
#### `--output -o`
Formato de salida.  Valores permitidos: json, jsonc, table y tsv.  Valor predeterminado: json.
#### `--query -q`
Cadena de consulta de JMESPath. Para obtener más información y ejemplos, vea [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos.
## <a name="azdata-bdc-spark-batch-delete"></a>azdata bdc spark batch delete
Este comando elimina un lote de Spark. El identificador de lote se devuelve de "spark batch create".
```bash
azdata bdc spark batch delete --batch-id -i 
                              
```
### <a name="examples"></a>Ejemplos
Elimine un lote.
```bash
azdata bdc spark batch delete --batch-id 0
```
### <a name="required-parameters"></a>Parámetros obligatorios
#### `--batch-id -i`
Número de identificador de lote de Spark.
### <a name="global-arguments"></a>Argumentos globales
#### `--debug`
Aumente el nivel de detalle de registro para mostrar todos los registros de depuración.
#### `--help -h`
Muestre este mensaje de ayuda y salga.
#### `--output -o`
Formato de salida.  Valores permitidos: json, jsonc, table y tsv.  Valor predeterminado: json.
#### `--query -q`
Cadena de consulta de JMESPath. Para obtener más información y ejemplos, vea [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre otros comandos de **azdata**, vea [Referencia de azdata](reference-azdata.md). 

Para más información sobre cómo instalar la herramienta **azdata**, consulte [Instalación de azdata](..\install\deploy-install-azdata.md).

