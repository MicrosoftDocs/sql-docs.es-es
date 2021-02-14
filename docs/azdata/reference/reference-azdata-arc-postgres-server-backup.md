---
title: Referencia de azdata arc postgres server backup
titleSuffix: SQL Server big data clusters
description: Artículo de referencia sobre los comandos de azdata arc postgres server backup.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 00519fa62e0813bc25cee6599cd2faca9ed1489d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052656"
---
# <a name="azdata-arc-postgres-server-backup"></a>azdata arc postgres server backup

Se aplica a [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

En el siguiente artículo se proporciona una referencia de los comandos **sql** de la herramienta **azdata**. Para obtener más información sobre otros comandos de **azdata**, vea la [Referencia de azdata](reference-azdata.md).

## <a name="commands"></a>Comandos

|Comando|Descripción|
| --- | --- |
[azdata arc postgres server backup create](#azdata-arc-postgres-server-backup-create) | Crea una copia de seguridad de un grupo de servidores de PostgreSQL.
[azdata arc postgres server backup delete](#azdata-arc-postgres-server-backup-delete) | Elimina una copia de seguridad de un grupo de servidores de PostgreSQL.
[azdata arc postgres server backup restore](#azdata-arc-postgres-server-backup-restore) | Restaura una copia de seguridad de un grupo de servidores de PostgreSQL.
[azdata arc postgres server backup restorestatus](#azdata-arc-postgres-server-backup-restorestatus) | Obtiene el estado de la operación de restauración más reciente, si la hubiera.
[azdata arc postgres server backup show](#azdata-arc-postgres-server-backup-show) | Muestra detalles sobre una copia de seguridad de un grupo de servidores de PostgreSQL.
## <a name="azdata-arc-postgres-server-backup-create"></a>azdata arc postgres server backup create
Crea una copia de seguridad de un grupo de servidores de PostgreSQL.
```bash
azdata arc postgres server backup create 
```
### <a name="examples"></a>Ejemplos
Crea una copia de seguridad del servicio "pg".
```bash
azdata arc postgres server backup create -sn pg
```
Crea una copia de seguridad con nombre del servicio "pg".
```bash
azdata arc postgres server backup create -sn pg -n backup1
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
## <a name="azdata-arc-postgres-server-backup-delete"></a>azdata arc postgres server backup delete
Elimina una copia de seguridad de un grupo de servidores de PostgreSQL.
```bash
azdata arc postgres server backup delete 
```
### <a name="examples"></a>Ejemplos
Elimina una copia de seguridad de un grupo de servidores de PostgreSQL.
```bash
azdata arc postgres backup delete -sn pg -id e07dd3940e374bd9acbc86869cbc123d
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
## <a name="azdata-arc-postgres-server-backup-restore"></a>azdata arc postgres server backup restore
Restaura una copia de seguridad de un grupo de servidores de PostgreSQL.
```bash
azdata arc postgres server backup restore 
```
### <a name="examples"></a>Ejemplos
Restaura la copia de seguridad más reciente.
```bash
azdata arc postgres server restore -sn pg```
Restore a backup by ID
```bash
azdata arc postgres server restore -sn pg -id 123e4567e89b12d3a456426655440000
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
## <a name="azdata-arc-postgres-server-backup-restorestatus"></a>azdata arc postgres server backup restorestatus
Obtiene el estado de la operación de restauración más reciente, si la hubiera.
```bash
azdata arc postgres server backup restorestatus 
```
### <a name="examples"></a>Ejemplos
Obtiene el estado de restauración reciente del servicio "pg" por identificador.
```bash
azdata arc postgres server backup restorestatus -sn pg -id 123e4567e89b12d3a456426655440000
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
## <a name="azdata-arc-postgres-server-backup-show"></a>azdata arc postgres server backup show
Muestra detalles sobre una copia de seguridad de un grupo de servidores de PostgreSQL.
```bash
azdata arc postgres server backup show 
```
### <a name="examples"></a>Ejemplos
Obtiene una copia de seguridad del servicio "pg" por identificador
```bash
azdata arc postgres server backup show -sn pg -id 123e4567e89b12d3a456426655440000
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

