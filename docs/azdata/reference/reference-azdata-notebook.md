---
title: Referencia de azdata notebook
titleSuffix: SQL Server big data clusters
description: Artículo de referencia sobre los comandos de azdata notebook.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: ece761617495c2d4b5d002541ac2d3838ec58d6f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048715"
---
# <a name="azdata-notebook"></a>azdata notebook

Se aplica a [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

En el siguiente artículo se proporciona una referencia de los comandos **sql** de la herramienta **azdata**. Para obtener más información sobre otros comandos de **azdata**, vea la [Referencia de azdata](reference-azdata.md).

## <a name="commands"></a>Comandos

|Comando|Descripción|
| --- | --- |
[azdata notebook view](#azdata-notebook-view) | Sirve para ver un cuaderno.  Opción para detenerse en el primer error de ejecución de celda.
[azdata notebook run](#azdata-notebook-run) | Sirve para ejecutar un cuaderno.  La ejecución se detiene en el primer error.
## <a name="azdata-notebook-view"></a>azdata notebook view
Este comando puede tomar un archivo de cuaderno y convertir el Markdown, el código y la salida en formato de terminal de color.
```bash
azdata notebook view --path -p 
                     [--continue-on-error -c]
```
### <a name="examples"></a>Ejemplos
Sirve para ver cuaderno.  Esto muestra todas las celdas.
```bash
azdata notebook view --path "/home/me/notebooks/demo_notebook.ipynb"
```
Sirve para ver cuaderno.  Esto muestra todas las celdas, a menos que se detecte una celda con un error en la salida.  En ese caso, la salida se detiene.
```bash
azdata notebook view --path "/home/me/notebooks/demo_notebook.ipynb" --stop-on-error
```
### <a name="required-parameters"></a>Parámetros obligatorios
#### `--path -p`
Ruta de acceso al cuaderno que se va a ver.
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--continue-on-error -c`
Se siguen mostrando más celdas y se pasan por alto los errores de celda que se detecten en la salida del cuaderno.  El comportamiento predeterminado es detenerse al encontrar un error.  Al detenerse, será más fácil localizar la primera celda en la que se encontró un error.
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
## <a name="azdata-notebook-run"></a>azdata notebook run
Este comando crea un directorio temporal y ejecuta en él el cuaderno en cuestión como directorio de trabajo.
```bash
azdata notebook run --path -p 
                    [--output-path]  
                    
[--output-html]  
                    
[--arguments -a]  
                    
[--interactive -i]  
                    
[--clear -c]  
                    
[--timeout -t]
```
### <a name="examples"></a>Ejemplos
Sirve para ejecutar el cuaderno.
```bash
azdata notebook run --path "/home/me/notebooks/demo_notebook.ipynb"
```
### <a name="required-parameters"></a>Parámetros obligatorios
#### `--path -p`
Ruta de acceso de archivo al cuaderno que se va a ejecutar.
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--output-path`
Ruta de acceso al directorio que se va a usar para la salida del cuaderno.  Los cuadernos con datos de salida y los archivos generados por el cuaderno se generan en relación con este directorio.
#### `--output-html`
Marca opcional que indica si el cuaderno de salida debe convertirse además en formato HTML.  Crea un segundo archivo de salida.
#### `--arguments -a`
Lista opcional de argumentos de cuaderno para insertar en la ejecución del cuaderno.  Cifrada como un diccionario JSON.  Ejemplo: '{"name":"value", "name2":"value2"}'
#### `--interactive -i`
Ejecuta un cuaderno en un modo interactivo.
#### `--clear -c`
En modo interactivo borra la consola antes de representar una celda.
#### `--timeout -t`
Segundos que es necesario esperar para que termine la ejecución. El valor -1 indica una espera indefinida.
`600`
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

