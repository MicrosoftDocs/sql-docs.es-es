---
title: Referencia de sql de azdata
titleSuffix: SQL Server big data clusters
description: Artículo de referencia de los comandos sql de azdata.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a7d142ca6afb77539b1425caaf097aca3959baeb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048705"
---
# <a name="azdata-sql"></a>sql de azdata

Se aplica a [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

En el siguiente artículo se proporciona una referencia de los comandos **sql** de la herramienta **azdata**. Para obtener más información sobre otros comandos de **azdata**, vea la [Referencia de azdata](reference-azdata.md).

## <a name="commands"></a>Comandos

|Get-Help|Descripción|
| --- | --- |
[azdata sql shell](#azdata-sql-shell) | La CLI de SQL permite al usuario interactuar con SQL Server y Azure SQL mediante T-SQL.
[azdata sql query](#azdata-sql-query) | La CLI de SQL permite al usuario interactuar con SQL Server y Azure SQL mediante T-SQL.
## <a name="azdata-sql-shell"></a>azdata sql shell
La CLI de SQL permite al usuario interactuar con SQL Server y Azure SQL mediante T-SQL.
```bash
azdata sql shell [--username -u] 
                 [--database -d]  
                 
[--server -s]  
                 
[--integrated -e]  
                 
[--mssqlclirc]  
                 
[--row-limit]  
                 
[--less-chatty]  
                 
[--auto-vertical-output]  
                 
[--encrypt -n]  
                 
[--trust-server-certificate -c]  
                 
[--connect-timeout -l]  
                 
[--application-intent -k]  
                 
[--multi-subnet-failover -m]  
                 
[--packet-size]  
                 
[--dac-connection -a]  
                 
[--input-file -i]  
                 
[--output-file]  
                 
[--enable-sqltoolsservice-logging]  
                 
[--prompt]
```
### <a name="examples"></a>Ejemplos
Línea de comandos de ejemplo para iniciar la experiencia interactiva.
```bash
azdata sql shell
```
Ejemplo de línea de comandos usando un servidor, un usuario y una base de datos proporcionados
```bash
azdata sql shell --server localhost --username sa --database master         
```
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--username -u`
Nombre de usuario para conectarse a la base de datos.
#### `--database -d`
Nombre de la base de datos a la que conectarse.
#### `--server -s`
Nombre o dirección de la instancia de SQL Server.
#### `--integrated -e`
Usar la autenticación integrada en Windows.
#### `--mssqlclirc`
Ubicación del archivo de configuración mssqlclirc.
#### `--row-limit`
Establecer el umbral de la solicitud de límite de filas. Usar 0 para deshabilitar la solicitud.
#### `--less-chatty`
Omitir introducción al inicio y despedida al salir.
#### `--auto-vertical-output`
Cambiar automáticamente al modo de salida vertical si el resultado es más ancho que el ancho del terminal.
#### `--encrypt -n`
SQL Server usa el cifrado SSL con todos los datos si el servidor tiene un certificado instalado.
#### `--trust-server-certificate -c`
El canal se cifrará mientras se omite el recorrido de la cadena de certificados para validar la confianza.
#### `--connect-timeout -l`
Tiempo en segundos que se debe esperar a una conexión con el servidor antes de finalizar la solicitud.
#### `--application-intent -k`
Declara el tipo de carga de trabajo de la aplicación al conectarse a una base de datos en un grupo de disponibilidad de SQL Server.
#### `--multi-subnet-failover -m`
Si la aplicación se va a conectar al grupo de disponibilidad AlwaysOn en distintas subredes, al establecer este valor la detección del servidor activo y la conexión a este se realizan de forma más rápida.
#### `--packet-size`
Tamaño en bytes de los paquetes de red utilizados para comunicarse con SQL Server.
#### `--dac-connection -a`
Conéctese a SQL Server mediante la conexión de administrador dedicada.
#### `--input-file -i`
Especifica el archivo que contiene un lote de instrucciones SQL para procesar.
#### `--output-file`
Especifica el archivo que recibe la salida de una consulta.
#### `--enable-sqltoolsservice-logging`
Habilita el registro de diagnóstico para SqlToolsService.
#### `--prompt`
Formato de la solicitud (valor predeterminado: \d>
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
## <a name="azdata-sql-query"></a>azdata sql query
La CLI de SQL permite al usuario interactuar con SQL Server y Azure SQL mediante T-SQL.
```bash
azdata sql query -q 
                 [--database -d]  
                 
[--username -u]  
                 
[--server -s]  
                 
[--integrated -e]
```
### <a name="examples"></a>Ejemplos
Ejemplo de línea de comandos para seleccionar la lista de nombres de tablas.
```bash
azdata sql query --server localhost --username sa --database master -q "SELECT name FROM SYS.TABLES"
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `-q`
Consulta T-SQL que se va a ejecutar.
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--database -d`
Nombre de la base de datos a la que conectarse.
`master`
#### `--username -u`
Nombre de usuario para conectarse a la base de datos.
#### `--server -s`
Nombre o dirección de la instancia de SQL Server.
#### `--integrated -e`
Usar la autenticación integrada en Windows.
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

Para más información sobre cómo instalar la herramienta **azdata**, consulte [Instalación de azdata](..\install\deploy-install-azdata.md).

