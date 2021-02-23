---
title: Referencia de azdata bdc sql settings
titleSuffix: SQL Server big data clusters
description: Artículo de referencia sobre los comandos azdata bdc sql settings.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 208f136f246cbcd6af612fbd4867a2a50c0f0cae
ms.sourcegitcommit: 129c084add904fd3f7e9ab35a800c3fd8b1a8927
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100567314"
---
# <a name="azdata-bdc-sql-settings"></a>azdata bdc sql settings

Se aplica a [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

En el siguiente artículo se proporciona una referencia sobre los comandos **sql settings** de la herramienta **azdata**. Para obtener más información sobre otros comandos de **azdata**, vea la [Referencia de azdata](reference-azdata.md).

## <a name="commands"></a>Comandos
|Comando|Descripción|
| --- | --- |
[azdata bdc sql settings set](#azdata-bdc-sql-settings-set) | Establece la configuración del ámbito del servicio SQL.
[azdata bdc sql settings show](#azdata-bdc-sql-settings-show) | Muestra la configuración del ámbito del servicio SQL y, opcionalmente, la configuración de SQL para los recursos especificados.

## <a name="azdata-bdc-sql-settings-set"></a>azdata bdc sql settings set
Proporciona la capacidad de establecer una configuración del ámbito del servicio o del ámbito del recurso. Debe especificar el nombre completo de la configuración y el valor. La configuración no se aplica a un clúster de BDC en ejecución. Para ello, ejecute la actualización.
```bash
azdata bdc sql settings set --settings -s 
                        
```
### <a name="examples"></a>Ejemplos
Habilite el Agente SQL en la instancia maestra de SQL Server.
```bash 
azdata bdc sql settings set --settings mssql.sqlagent.enabled=true --resources master 
``` 
Establezca el número de CPU para SQL Server en el grupo de datos.
```bash 
azdata bdc sql settings set --settings mssql.numberOfCpus=10 –resources data-0 
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

## <a name="azdata-bdc-sql-settings-show"></a>azdata bdc sql settings show
Muestra la configuración del ámbito del servicio (opcionalmente, del ámbito del recurso) de `sql` para la instancia de BDC. De forma predeterminada, este comando muestra la configuración del ámbito del servicio definida por el usuario. Hay varios filtros disponibles para mostrar todos los valores de configuración (administrados por el sistema y configurables), los valores configurables o las configuraciones pendientes. Para ver un valor específico del ámbito del servicio o del ámbito del recurso, puede proporcionar el nombre del valor. Use "recursive" para mostrar la configuración de todos los recursos como parte del servicio. 
```bash

azdata bdc sql settings show 
[--settings -s]
[--filter-option -f]  
[--recursive -rec]
[--include-details -i]  
[--description -d]
```
### <a name="examples"></a>Ejemplos
Muestre la configuración del ámbito del servicio SQL configurada por el usuario. 
```bash
azdata bdc sql settings show
```
Muestre la configuración de memoria máxima del servidor de SQL en el grupo de datos.
```bash
azdata bdc sql settings show --settings mssql.maxServerMemory --resources data-0 
```
Muestre los cambios de configuración pendientes para la configuración del ámbito del servicio y del ámbito del recurso de SQL.
```bash
azdata bdc sql settings show --filter-options=pending --recursive --include-details
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
