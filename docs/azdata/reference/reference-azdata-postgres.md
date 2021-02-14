---
title: Referencia de azdata postgres
titleSuffix: SQL Server big data clusters
description: Artículo de referencia sobre los comandos de azdata postgres.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6403db5f9b716684c3a791ade183a8f9c5edad47
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052276"
---
# <a name="azdata-postgres"></a>azdata postgres

Se aplica a [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

En el siguiente artículo se proporciona una referencia de los comandos **sql** de la herramienta **azdata**. Para obtener más información sobre otros comandos de **azdata**, vea la [Referencia de azdata](reference-azdata.md).

## <a name="commands"></a>Comandos

|Comando|Descripción|
| --- | --- |
[azdata postgres shell](#azdata-postgres-shell) | Interfaz de shell de línea de comandos para Postgres. Consulta https://www.pgcli.com/.
[azdata postgres query](#azdata-postgres-query) | El comando query permite la ejecución de comandos de PostgreSQL en una sesión de base de datos.
## <a name="azdata-postgres-shell"></a>azdata postgres shell
Interfaz de shell de línea de comandos para Postgres. Consulta https://www.pgcli.com/.
```bash
azdata postgres shell [--dbname -d] 
                      [--host]  
                      
[--port -p]  
                      
[--password -w]  
                      
[--no-password]  
                      
[--single-connection]  
                      
[--username -u]  
                      
[--pgclirc]  
                      
[--dsn]  
                      
[--list-dsn]  
                      
[--row-limit]  
                      
[--less-chatty]  
                      
[--prompt]  
                      
[--prompt-dsn]  
                      
[--list -l]  
                      
[--auto-vertical-output]  
                      
[--warn]  
                      
[--no-warn]
```
### <a name="examples"></a>Ejemplos
Línea de comandos de ejemplo para iniciar la experiencia interactiva.
```bash
azdata postgres shell
```
Línea de comandos de ejemplo con una base de datos y un usuario proporcionados.
```bash
azdata postgres shell --dbname <database> --username <username> --host <host>
```
Línea de comandos de ejemplo para empezar a usar una cadena de conexión completa.
```bash
azdata postgres shell --dbname postgres://user:passw0rd@example.com:5432/master 
```
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--dbname -d`
Nombre de la base de datos con la que se va a conectar.
#### `--host`
Dirección de host de la base de datos de Postgres.
#### `--port -p`
Número de puerto en el que escucha la instancia de Postgres.
#### `--password -w`
Forzar solicitud de contraseña.
#### `--no-password`
No solicitar contraseña nunca.
#### `--single-connection`
No usar una conexión independiente para las finalizaciones.
#### `--username -u`
Nombre de usuario para la conexión a la base de datos de Postgres.
#### `--pgclirc`
Ubicación del archivo pgclirc.
#### `--dsn`
Usar la DSN configurada en la sección [alias_dsn] del archivo pgclirc.
#### `--list-dsn`
Lista de DSN configuradas en la sección [alias_dsn] del archivo pgclirc.
#### `--row-limit`
Establecer el umbral de la solicitud de límite de filas. Usar 0 para deshabilitar la solicitud.
#### `--less-chatty`
Omitir introducción al inicio y despedida al salir.
#### `--prompt`
Formato de la solicitud (valor predeterminado: "\u@\h:\d> ").
#### `--prompt-dsn`
Formato de la solicitud para las conexiones que usan alias DSN (valor predeterminado: "\u@\h:\d> ").
#### `--list -l`
Indicar bases de datos disponibles, luego salir.
#### `--auto-vertical-output`
Cambiar automáticamente al modo de salida vertical si el resultado es más ancho que el ancho del terminal.
#### `--warn`
Advertir antes de ejecutar una consulta destructiva.
#### `--no-warn`
Advertir antes de ejecutar una consulta destructiva.
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
## <a name="azdata-postgres-query"></a>azdata postgres query
El comando query permite la ejecución de comandos de PostgreSQL en una sesión de base de datos.
```bash
azdata postgres query --q -q 
                      [--host]  
                      
[--dbname -d]  
                      
[--port -p]  
                      
[--username -u]
```
### <a name="examples"></a>Ejemplos
Indica todas las tablas de information_schema.
```bash
azdata postgres query --host <host> --username <username> -q "SELECT * FROM information_schema.tables"
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `--q -q`
Consulta de PostgreSQL que se va a ejecutar.
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--host`
Dirección de host de la base de datos de Postgres.
`localhost`
#### `--dbname -d`
Base de datos en la que se ejecutará la consulta.
#### `--port -p`
Número de puerto en el que escucha la instancia de Postgres.
`5432`
#### `--username -u`
Nombre de usuario para la conexión a la base de datos de Postgres.
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

