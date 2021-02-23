---
title: Referencia de azdata bdc hdfs key
titleSuffix: SQL Server big data clusters
description: Artículo de referencia sobre los comandos azdata bdc hdfs key.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: c208d1c726bae41b349abdf475627afbc82ed2b8
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342507"
---
# <a name="azdata-bdc-hdfs-key"></a>azdata bdc hdfs key

Se aplica a [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

En el siguiente artículo se proporciona una referencia de los comandos **sql** de la herramienta **azdata**. Para obtener más información sobre otros comandos de **azdata**, vea la [Referencia de azdata](reference-azdata.md).

## <a name="commands"></a>Comandos
|Comando|Descripción|
| --- | --- |
[azdata bdc hdfs key create](#azdata-bdc-hdfs-key-create) | Crea una clave de HDFS.
[azdata bdc hdfs key list](#azdata-bdc-hdfs-key-list) | Enumera todas las claves de zona de cifrado de Hadoop.
[azdata bdc hdfs key roll](#azdata-bdc-hdfs-key-roll) | Sustituye una clave de HDFS.
## <a name="azdata-bdc-hdfs-key-create"></a>azdata bdc hdfs key create
Crea una clave de HDFS con el nombre y el tamaño especificados.
```bash
azdata bdc hdfs key create --name -n 
                           [--size -size]
```
### <a name="examples"></a>Ejemplos
Cree una clave de 256 bits con el nombre key1 mediante el comando "azdata hdfs key create --name key1 --size 256".
```bash
azdata hdfs key create --name key1 --size 256
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `--name -n`
El nombre de la clave de la zona de cifrado de Hadoop. 
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--size -size`
La longitud en bits de la clave de cifrado de Hadoop; la longitud predeterminada es de 256.
`256`
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
## <a name="azdata-bdc-hdfs-key-list"></a>azdata bdc hdfs key list
Enumera todas las claves de zona de cifrado de Hadoop.
```bash
azdata bdc hdfs key list 
```
### <a name="examples"></a>Ejemplos
Muestre todas las claves de zona de cifrado de Hadoop mediante "azdata bdc hdfs key list".
```bash
azdata bdc hdfs key list
```
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
## <a name="azdata-bdc-hdfs-key-roll"></a>azdata bdc hdfs key roll
Sustituye una clave de HDFS con un nombre determinado.
```bash
azdata bdc hdfs key roll --name -n 
                         
```
### <a name="examples"></a>Ejemplos
Sustituya una clave con el nombre key1 mediante "azdata hdfs key roll --name key1".
```bash
azdata hdfs key roll --name key1
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `--name -n`
El nombre de la clave de la zona de cifrado que se va a sustituir por una nueva versión. 
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
