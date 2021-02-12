---
title: Referencia de azdata arc postgres endpoint
titleSuffix: SQL Server big data clusters
description: Artículo de referencia de los comandos azdata arc postgres endpoint.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: cb48072eaee6cdee6498655f05b88ae05c2062a8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049005"
---
# <a name="azdata-arc-postgres-endpoint"></a>azdata arc postgres endpoint

Se aplica a [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

En el siguiente artículo se proporciona una referencia de los comandos **sql** de la herramienta **azdata**. Para obtener más información sobre otros comandos de **azdata**, vea la [Referencia de azdata](reference-azdata.md).

## <a name="commands"></a>Comandos

|Comando|Descripción|
| --- | --- |
[azdata arc postgres endpoint list](#azdata-arc-postgres-endpoint-list) | Enumera puntos de conexión de grupos de servidores de PostgreSQL.
## <a name="azdata-arc-postgres-endpoint-list"></a>azdata arc postgres endpoint list
Enumera puntos de conexión de grupos de servidores de PostgreSQL.
```bash
azdata arc postgres endpoint list --name -n 
                                  [--engine-version -ev]
```
### <a name="examples"></a>Ejemplos
Enumera puntos de conexión de grupos de servidores de PostgreSQL.
```bash
azdata arc postgres endpoint list -n postgres01
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `--name -n`
Nombre del grupo de servidores de PostgreSQL.
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--engine-version -ev`
Puede usar --engine-version junto con --name para identificar un grupo de servidores de Hiperescala de PostgreSQL cuando dos grupos de servidores de una versión de motor diferente tienen el mismo nombre. --engine-version es opcional y, cuando se usa para identificar un grupo de servidores, debe ser 11 o 12.
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

