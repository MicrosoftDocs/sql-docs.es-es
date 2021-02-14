---
title: Referencia de azdata arc dc status
titleSuffix: SQL Server big data clusters
description: Artículo de referencia sobre los comandos de azdata arc dc status.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 91bcda5ac4fc7cb39887698cdea19a55b47d8445
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052676"
---
# <a name="azdata-arc-dc-status"></a>azdata arc dc status

Se aplica a [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

En el siguiente artículo se proporciona una referencia de los comandos **sql** de la herramienta **azdata**. Para obtener más información sobre otros comandos de **azdata**, vea la [Referencia de azdata](reference-azdata.md).

## <a name="commands"></a>Comandos

|Comando|Descripción|
| --- | --- |
[azdata arc dc status show](#azdata-arc-dc-status-show) | Muestra el estado del controlador de datos.
## <a name="azdata-arc-dc-status-show"></a>azdata arc dc status show
Muestra el estado del controlador de datos.
```bash
azdata arc dc status show [--namespace -ns] 
                          
```
### <a name="examples"></a>Ejemplos
Muestra el estado del controlador de datos en un espacio de nombres determinado.
```bash
azdata arc dc status show --namespace <ns>
```
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--namespace -ns`
El espacio de nombres de Kubernetes en el que existe el controlador de datos.
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

