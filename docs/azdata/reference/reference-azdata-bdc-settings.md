---
title: Referencia de azdata bdc settings
titleSuffix: SQL Server big data clusters
description: Artículo de referencia sobre los comandos azdata bdc settings.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 37688a962fc17679a1a642af1b83609d2b8f480a
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342497"
---
# <a name="azdata-bdc-settings"></a>azdata bdc settings

Se aplica a [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

En el siguiente artículo se proporciona una referencia de los comandos **sql** de la herramienta **azdata**. Para obtener más información sobre otros comandos de **azdata**, vea la [Referencia de azdata](reference-azdata.md).

## <a name="commands"></a>Comandos
|Comando|Descripción|
| --- | --- |
[azdata bdc settings set](#azdata-bdc-settings-set) | Establece la configuración del ámbito del clúster.
[azdata bdc settings apply](#azdata-bdc-settings-apply) | Aplica los cambios de configuración pendientes a BDC.
[azdata bdc settings cancel-apply](#azdata-bdc-settings-cancel-apply) | Cancela la aplicación de la configuración de BDC.
[azdata bdc settings show](#azdata-bdc-settings-show) | Muestra la configuración del ámbito del clúster o todas las configuraciones del clúster mediante el parámetro "--recursive".
[azdata bdc settings revert](#azdata-bdc-settings-revert) | Revierte los cambios de configuración pendientes de BDC en todos los ámbitos.
## <a name="azdata-bdc-settings-set"></a>azdata bdc settings set
Establece una configuración del ámbito del clúster. Debe especificar el nombre completo de la configuración y el valor. Después, ejecute el comando para aplicar y actualizar la configuración de BDC.
```bash
azdata bdc settings set --settings -s 
                        
```
### <a name="examples"></a>Ejemplos
Establezca el valor predeterminado del clúster para "bdc.telemetry.customerFeedback".
```bash
azdata bdc settings set --settings bdc.telemetry.customerFeedback=false
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `--settings -s`
Establece el valor configurado para la configuración proporcionada. Se pueden establecer varios valores de configuración mediante una lista separada por comas.
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
## <a name="azdata-bdc-settings-apply"></a>azdata bdc settings apply
Aplica los cambios de configuración pendientes a BDC.
```bash
azdata bdc settings apply [--force -f] 
                          
```
### <a name="examples"></a>Ejemplos
Aplique los cambios de configuración pendientes a BDC.
```bash
azdata bdc settings apply
```
Fuerce la aplicación; no se le pedirá al usuario ninguna confirmación y todos los problemas se imprimirán como parte de stderr.
```bash
azdata bdc settings apply --force
```
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--force -f`
Fuerce la aplicación; no se le pedirá al usuario ninguna confirmación y todos los problemas se imprimirán como parte de stderr.
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
## <a name="azdata-bdc-settings-cancel-apply"></a>azdata bdc settings cancel-apply
En caso de que se produzca un error durante la aplicación de la configuración, revierta el clúster de macrodatos a su último estado de ejecución. Este comando no se ejecutará si se aplica a un clúster en ejecución. Los cambios de configuración pendientes anteriormente seguirán pendientes después de la cancelación.
```bash
azdata bdc settings cancel-apply [--force -f] 
                                 
```
### <a name="examples"></a>Ejemplos
Cancele la aplicación de la configuración de BDC.
```bash
azdata bdc settings cancel-apply
```
Fuerce la cancelación de la aplicación; no se le pedirá al usuario ninguna confirmación y todos los problemas se imprimirán como parte de stderr.
```bash
azdata bdc settings cancel-apply --force
```
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--force -f`
Fuerce la cancelación de la aplicación; no se le pedirá al usuario ninguna confirmación y todos los problemas se imprimirán como parte de stderr.
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
## <a name="azdata-bdc-settings-show"></a>azdata bdc settings show
Muestra la configuración de BDC a nivel de clúster. De forma predeterminada, este comando muestra la configuración del ámbito del clúster definida por el usuario. Hay otros filtros disponibles para mostrar todos los valores de configuración (administrados por el sistema, configurables por el usuario y heredados), todos los valores configurables o las configuraciones pendientes. Para ver una configuración del ámbito del clúster específica, puede proporcionar el nombre de la configuración. Si quiere ver la configuración de todos los ámbitos (clúster, servicio y recurso), puede especificar "recursive".
```bash
azdata bdc settings show [--settings -s] 
                         [--filter-option -f]  
                         
[--recursive -rec]  
                         
[--include-details -i]  
                         
[--description -d]
```
### <a name="examples"></a>Ejemplos
Muestre si la recopilación de datos de telemetría de BDC está habilitada.
```bash
azdata bdc settings show --settings bdc.telemetry.customerFeedback
```
Muestre los valores configurados por el usuario en todos los ámbitos de BDC.
```bash
azdata bdc settings show --recursive
```
Muestre todas las configuraciones pendientes en todos los ámbitos de BDC.
```bash
azdata bdc settings show –filter-option=pending --recursive
```
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--settings -s`
Muestra información de los nombres de configuración especificados.
#### `--filter-option -f`
Filtra los valores de configuración del ámbito del clúster que se muestran, en lugar de mostrar solo los valores configurados por el usuario. Hay varios filtros disponibles para mostrar todos los valores de configuración (administrados por el sistema y configurables por el usuario), todos los valores configurables o las configuraciones pendientes.
`userConfigured`
#### `--recursive -rec`
Muestra la información de configuración para el ámbito del clúster y todos los componentes con ámbito inferior (servicios y recursos).
#### `--include-details -i`
Incluye detalles adicionales de los valores de configuración que se van a mostrar.
#### `--description -d`
Incluye una descripción del valor de configuración.
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
## <a name="azdata-bdc-settings-revert"></a>azdata bdc settings revert
Revierte los cambios de configuración pendientes de BDC en todos los ámbitos.
```bash
azdata bdc settings revert [--force -f] 
                           
```
### <a name="examples"></a>Ejemplos
Revierta los cambios de configuración pendientes de BDC en todos los ámbitos.
```bash
azdata bdc settings revert
```
Fuerce la reversión; no se le pedirá al usuario ninguna confirmación y todos los problemas se imprimirán como parte de stderr.
```bash
azdata bdc settings revert --force
```
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--force -f`
Fuerza la reversión; no se le pedirá al usuario ninguna confirmación y todos los problemas se imprimirán como parte de stderr.
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

Para más información sobre cómo instalar la herramienta **azdata**, consulte [Instalación de azdata](..\install\deploy-install-azdata.md).
