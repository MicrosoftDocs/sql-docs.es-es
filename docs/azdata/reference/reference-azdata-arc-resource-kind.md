---
title: Referencia de azdata arc resource-kind
titleSuffix: SQL Server big data clusters
description: Artículo de referencia sobre los comandos de azdata arc resource-kind.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b13ab0788b0cef102fa5388492ef8ad12836c138
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052606"
---
# <a name="azdata-arc-resource-kind"></a>azdata arc resource-kind

Se aplica a [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

En el siguiente artículo se proporciona una referencia de los comandos **sql** de la herramienta **azdata**. Para obtener más información sobre otros comandos de **azdata**, vea la [Referencia de azdata](reference-azdata.md).

## <a name="commands"></a>Comandos

|Comando|Descripción|
| --- | --- |
[azdata arc resource-kind list](#azdata-arc-resource-kind-list) | Enumera los tipos de recursos personalizados disponibles para Arc que se pueden definir y crear.
[azdata arc resource-kind get](#azdata-arc-resource-kind-get) | Obtiene el archivo de plantilla de la clase de recurso de Arc.
## <a name="azdata-arc-resource-kind-list"></a>azdata arc resource-kind list
Enumera los tipos de recursos personalizados disponibles para Arc que se pueden definir y crear. Después de la lista, puede continuar con la obtención del archivo de plantilla necesario para definir o crear ese recurso personalizado.
```bash
azdata arc resource-kind list 
```
### <a name="examples"></a>Ejemplos
Comando de ejemplo para enumerar los tipos de recursos personalizados disponibles para Arc.
```bash
azdata arc resource-kind list
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
## <a name="azdata-arc-resource-kind-get"></a>azdata arc resource-kind get
Obtiene el archivo de plantilla de la clase de recurso de Arc.
```bash
azdata arc resource-kind get --kind -k 
                             [--dest -d]
```
### <a name="examples"></a>Ejemplos
Comando de ejemplo para obtener un archivo de plantilla de CRD de tipo de recurso de Arc.
```bash
azdata arc resource-kind get --kind sqldb
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `--kind -k`
Tipo de recurso de Arc para el que quiere el archivo de plantilla.
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--dest -d`
Directorio en el que quiere colocar los archivos de plantilla.
`template`
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

