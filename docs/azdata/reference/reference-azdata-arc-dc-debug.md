---
title: Referencia de azdata arc dc debug
titleSuffix: SQL Server big data clusters
description: Artículo de referencia sobre los comandos de azdata arc dc debug.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3979dbd8561eeedf3e5c0ab03a51b04b2bd73674
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052686"
---
# <a name="azdata-arc-dc-debug"></a>azdata arc dc debug

Se aplica a [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

En el siguiente artículo se proporciona una referencia de los comandos **sql** de la herramienta **azdata**. Para obtener más información sobre otros comandos de **azdata**, vea la [Referencia de azdata](reference-azdata.md).

## <a name="commands"></a>Comandos

|Comando|Descripción|
| --- | --- |
[azdata arc dc debug copy-logs](#azdata-arc-dc-debug-copy-logs) | Copie registros.
[azdata arc dc debug dump](#azdata-arc-dc-debug-dump) | Volcado de memoria del desencadenador.
## <a name="azdata-arc-dc-debug-copy-logs"></a>azdata arc dc debug copy-logs
Copia los registros de depuración del controlador de datos; es necesario que Kubernetes esté configurado en el sistema.
```bash
azdata arc dc debug copy-logs --namespace -ns 
                              [--container -c]  
                              
[--target-folder -d]  
                              
[--pod]  
                              
[--resource-kind -rk]  
                              
[--resource-name -rn]  
                              
[--timeout -t]  
                              
[--skip-compress -sc]  
                              
[--exclude-dumps -ed]
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `--namespace -ns`
Espacio de nombres de Kubernetes del controlador de datos.
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--container -c`
Copie los registros de los contenedores con un nombre similar. Opcional, de forma predeterminada, copia los registros de todos los contenedores. No se puede especificar varias veces. Si se especifica varias veces, se usará la última.
#### `--target-folder -d`
Ruta de acceso de la carpeta de destino en la que copiar los registros. Opcional, de forma predeterminada, crea el resultado en la carpeta local.  No se puede especificar varias veces. Si se especifica varias veces, se usará la última.
#### `--pod`
Copie los registros de los pods con un nombre similar. Opcional, de forma predeterminada copia los registros de todos los pods. No se puede especificar varias veces. Si se especifica varias veces, se usará la última.
#### `--resource-kind -rk`
Copia los registros para el recurso de un tipo determinado. No se puede especificar varias veces. Si se especifica varias veces, se usará la última. Si se especifica, también se debe especificar--resource-name para identificar el recurso.
#### `--resource-name -rn`
Copia los registros para el recurso del nombre especificado. No se puede especificar varias veces. Si se especifica varias veces, se usará la última. Si se especifica, también se debe especificar--resource-kind para identificar el recurso.
#### `--timeout -t`
Número de segundos que se debe esperar a que se complete el comando. El valor predeterminado es 0, es decir, sin límite.
#### `--skip-compress -sc`
Indica si se omite o no la compresión de la carpeta de resultados. El valor predeterminado es False, que comprime la carpeta de resultados.
#### `--exclude-dumps -ed`
Indica si se excluyen o no los volcados de la carpeta de resultados. El valor predeterminado es False, que incluye los volcados.
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
## <a name="azdata-arc-dc-debug-dump"></a>azdata arc dc debug dump
Desencadene el volcado de registros y cópielo del contenedor; es necesario que Kubernetes esté configurado en el sistema.
```bash
azdata arc dc debug dump --namespace -ns 
                         [--container -c]  
                         
[--target-folder -d]
```
### <a name="required-parameters"></a>Parámetros obligatorios
#### `--namespace -ns`
Espacio de nombres de Kubernetes del controlador de datos.
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--container -c`
Contenedor de destino que se va a desencadenar para volcar los procesos en ejecución.
`controller`
#### `--target-folder -d`
Carpeta de destino en la que se va a copiar el volcado. `./output/dump`
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

