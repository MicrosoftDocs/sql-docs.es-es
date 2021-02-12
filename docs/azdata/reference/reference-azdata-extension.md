---
title: Referencia de azdata extension
titleSuffix: SQL Server big data clusters
description: Artículo de referencia de los comandos azdata extension.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 45432b3e1c8dda5835b13c2775035382694713ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052286"
---
# <a name="azdata-extension"></a>azdata extension

Se aplica a [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

En el siguiente artículo se proporciona una referencia de los comandos **sql** de la herramienta **azdata**. Para obtener más información sobre otros comandos de **azdata**, vea la [Referencia de azdata](reference-azdata.md).

## <a name="commands"></a>Comandos

|Comando|Descripción|
| --- | --- |
[azdata extension add](#azdata-extension-add) | Agrega una extensión.
[azdata extension remove](#azdata-extension-remove) | Quita una extensión.
[azdata extension list](#azdata-extension-list) | Enumera todas las extensiones instaladas.
## <a name="azdata-extension-add"></a>azdata extension add
Agrega una extensión.
```bash
azdata extension add --source -s 
                     [--index]  
                     
[--pip-proxy]  
                     
[--pip-extra-index-urls]  
                     
[--yes -y]
```
### <a name="examples"></a>Ejemplos
Agregar una extensión desde una dirección URL.
```bash
azdata extension add --source https://contoso.com/some_ext-0.0.1-py2.py3-none-any.whl```
Add extension from local disk.
```bash
azdata extension add --source ~/some_ext-0.0.1-py2.py3-none-any.whl```
Add extension from local disk and use pip proxy for dependencies.
```bash
azdata extension add --source ~/some_ext-0.0.1-py2.py3-none-any.whl --pip-proxy https://user:pass@proxy.server:8080
```
### <a name="required-parameters"></a>Parámetros obligatorios
#### `--source -s`
Ruta de acceso a una rueda de extensión en el disco o la dirección URL a una extensión
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--index`
Dirección URL base del índice de paquetes de Python (valor predeterminado: https://pypi.org/simple). Esto debería apuntar a un repositorio compatible con PEP 503 (la API de repositorio simple) o un directorio local con el mismo formato.
#### `--pip-proxy`
Proxy para que use pip para las dependencias de la extensión con el formato [usuario:contraseña@]proxy.servidor:puerto
#### `--pip-extra-index-urls`
Lista separada por espacios de direcciones URL adicionales de los índices de paquetes que se van a usar. Esto debería apuntar a un repositorio compatible con PEP 503 (la API de repositorio simple) o un directorio local con el mismo formato.
#### `--yes -y`
No solicita confirmación.
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
## <a name="azdata-extension-remove"></a>azdata extension remove
Quita una extensión.
```bash
azdata extension remove --name -n 
                        [--yes -y]
```
### <a name="examples"></a>Ejemplos
Quita una extensión.
```bash
azdata extension remove --name some-ext
```
### <a name="required-parameters"></a>Parámetros obligatorios
#### `--name -n`
Nombre de la extensión.
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--yes -y`
No solicita confirmación.
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
## <a name="azdata-extension-list"></a>azdata extension list
Enumera todas las extensiones instaladas.
```bash
azdata extension list 
```
### <a name="examples"></a>Ejemplos
Enumerar extensiones.
```bash
azdata extension list
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

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre otros comandos de **azdata**, vea [Referencia de azdata](reference-azdata.md). 

Para obtener más información sobre cómo instalar la herramienta **azdata**, vea [Instalación de azdata](..\install\deploy-install-azdata.md).

