---
title: Referencia de azdata arc postgres server
titleSuffix: SQL Server big data clusters
description: Artículo de referencia de los comandos azdata arc postgres server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 79d2cad23c6aed747837635e4d19d42a5daf41dd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052646"
---
# <a name="azdata-arc-postgres-server"></a>azdata arc postgres server

Se aplica a [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

En el siguiente artículo se proporciona una referencia de los comandos **sql** de la herramienta **azdata**. Para obtener más información sobre otros comandos de **azdata**, vea la [Referencia de azdata](reference-azdata.md).

## <a name="commands"></a>Comandos

|Comando|Descripción|
| --- | --- |
[azdata arc postgres server create](#azdata-arc-postgres-server-create) | Crea un grupo de servidores PostgreSQL.
[azdata arc postgres server edit](#azdata-arc-postgres-server-edit) | Edita la configuración de un grupo de servidores PostgreSQL.
[azdata arc postgres server delete](#azdata-arc-postgres-server-delete) | Elimina un grupo de servidores PostgreSQL.
[azdata arc postgres server show](#azdata-arc-postgres-server-show) | Muestra los detalles de un grupo de servidores PostgreSQL.
[azdata arc postgres server list](#azdata-arc-postgres-server-list) | Muestra una lista de los grupos de servidores de PostgreSQL.
[azdata arc postgres server config](reference-azdata-arc-postgres-server-config.md) | Comandos de configuración.
[azdata arc postgres server backup](reference-azdata-arc-postgres-server-backup.md) | Administra las copias de seguridad de grupos de servidores PostgreSQL.
## <a name="azdata-arc-postgres-server-create"></a>azdata arc postgres server create
Para establecer la contraseña del grupo de servidores, establezca la variable de entorno AZDATA_PASSWORD.
```bash
azdata arc postgres server create --name -n 
                                  [--path]  
                                  
[--cores-limit -cl]  
                                  
[--cores-request -cr]  
                                  
[--memory-limit -ml]  
                                  
[--memory-request -mr]  
                                  
[--storage-class-data -scd]  
                                  
[--storage-class-logs -scl]  
                                  
[--storage-class-backups -scb]  
                                  
[--extensions]  
                                  
[--volume-size-data -vsd]  
                                  
[--volume-size-logs -vsl]  
                                  
[--volume-size-backups -vsb]  
                                  
[--workers -w]  
                                  
[--engine-version -ev]  
                                  
[--no-external-endpoint]  
                                  
[--dev]  
                                  
[--port]  
                                  
[--no-wait]  
                                  
[--engine-settings -e]
```
### <a name="examples"></a>Ejemplos
Crea un grupo de servidores PostgreSQL.
```bash
azdata arc postgres server create -n pg1
```
Crea un grupo de servidores PostgreSQL con la configuración del motor. Los dos ejemplos siguientes son válidos.
```bash
azdata arc postgres server create -n pg1 --engine-settings "key1=val1"
azdata arc postgres server create -n pg1 --engine-settings "key2=val2"
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `--name -n`
Nombre de la instancia. 
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--path`
Ruta de acceso al archivo JSON de origen del grupo de servidores PostgreSQL. De uso opcional.
#### `--cores-limit -cl`
Número máximo de núcleos de CPU de la instancia de Postgres que se puede usar por nodo. Se pueden usar núcleos fraccionarios.
#### `--cores-request -cr`
Número mínimo de núcleos de CPU que debe haber disponibles por nodo para programar el servicio. Se pueden usar núcleos fraccionarios.
#### `--memory-limit -ml`
Límite de memoria de la instancia de Postgres como un número seguido de Ki (kilobytes), Mi (megabytes) o Gi (gigabytes).
#### `--memory-request -mr`
Solicitud de memoria de la instancia de Postgres como un número seguido de Ki (kilobytes), Mi (megabytes) o Gi (gigabytes).
#### `--storage-class-data -scd`
Clase de almacenamiento que se va a usar en los volúmenes persistentes de datos.
#### `--storage-class-logs -scl`
Clase de almacenamiento que se va a usar en los volúmenes persistentes de registro.
#### `--storage-class-backups -scb`
Clase de almacenamiento que se va a usar en los volúmenes persistentes de copia de datos.
#### `--extensions`
Lista separada por comas de las extensiones de Postgres que deben cargarse en el inicio. Vea la documentación de Postgres para obtener información sobre los valores admitidos.
#### `--volume-size-data -vsd`
Tamaño del volumen de almacenamiento que se va a usar en los datos como un número positivo seguido de Ki (kilobytes), Mi (megabytes) o Gi (gigabytes).
#### `--volume-size-logs -vsl`
Tamaño del volumen de almacenamiento que se va a usar en los registros como un número positivo seguido de Ki (kilobytes), Mi (megabytes) o Gi (gigabytes).
#### `--volume-size-backups -vsb`
Tamaño del volumen de almacenamiento que se va a usar en las copias de seguridad como un número positivo seguido de Ki (kilobytes), Mi (megabytes) o Gi (gigabytes).
#### `--workers -w`
Número de nodos de trabajo que se va a aprovisionar en un clúster particionado, o bien cero (valor predeterminado) en Postgres de un solo nodo.
#### `--engine-version -ev`
Debe ser 11 o 12. El valor predeterminado es 12.
#### `--no-external-endpoint`
Si se especifica, no se creará ningún servicio externo. De lo contrario, se creará un servicio externo usando el mismo tipo de servicio que el controlador de datos.
#### `--dev`
Si se especifica, se considera una instancia de desarrollo y no se facturará.
#### `--port`
Opcional.
#### `--no-wait`
Si se especifica, el comando no esperará a que la instancia esté en un estado listo antes de volver.
#### `--engine-settings -e`
Lista separada por comas de los valores del motor de Postgres con el formato '"key1=val1, key2=val2".
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
## <a name="azdata-arc-postgres-server-edit"></a>azdata arc postgres server edit
Edita la configuración de un grupo de servidores PostgreSQL.
```bash
azdata arc postgres server edit --name -n 
                                [--path]  
                                
[--workers -w]  
                                
[--engine-version -ev]  
                                
[--cores-limit -cl]  
                                
[--cores-request -cr]  
                                
[--memory-limit -ml]  
                                
[--memory-request -mr]  
                                
[--extensions]  
                                
[--dev]  
                                
[--port]  
                                
[--no-wait]  
                                
[--engine-settings -e]  
                                
[--replace-engine-settings -re]  
                                
[--admin-password]
```
### <a name="examples"></a>Ejemplos
Edita la configuración de un grupo de servidores PostgreSQL.
```bash
azdata arc postgres server edit --src ./spec.json -n pg1
```
Edita un grupo de servidores PostgreSQL con la configuración del motor.
```bash
azdata arc postgres server edit -n pg1 --engine-settings "key2=val2"
```
Edita un grupo de servidores PostgreSQL y reemplaza la configuración de motor existente por la nueva configuración "key1=val1".
```bash
azdata arc postgres server edit -n pg1 --engine-settings "key1=val1" --replace-engine-settings
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `--name -n`
Nombre del grupo de servidores de PostgreSQL que se está editando. El nombre con el que la instancia se implementa no se puede cambiar.
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--path`
Ruta de acceso al archivo JSON de origen del grupo de servidores PostgreSQL. De uso opcional.
#### `--workers -w`
Número de nodos de trabajo que se va a aprovisionar en un clúster particionado, o bien cero (valor predeterminado) en Postgres de un solo nodo.
#### `--engine-version -ev`
La versión del motor no se puede cambiar. Puede usar --engine-version junto con --name para identificar un grupo de servidores de Hiperescala de PostgreSQL cuando dos grupos de servidores de una versión de motor diferente tienen el mismo nombre. --engine-version es opcional y, cuando se usa para identificar un grupo de servidores, debe ser 11 o 12.
#### `--cores-limit -cl`
Número máximo de núcleos de CPU de la instancia de Postgres que se puede usar por nodo. Se pueden usar núcleos fraccionarios. Para quitar los límites de núcleos, especifique su valor como una cadena vacía.
#### `--cores-request -cr`
Número mínimo de núcleos de CPU que debe haber disponibles por nodo para programar el servicio. Se pueden usar núcleos fraccionarios. Para quitar las solicitudes de núcleos, especifique su valor como una cadena vacía.
#### `--memory-limit -ml`
Límite de memoria de la instancia de Postgres como un número seguido de Ki (kilobytes), Mi (megabytes) o Gi (gigabytes). Para quitar los límites de memoria, especifique su valor como una cadena vacía.
#### `--memory-request -mr`
Solicitud de memoria de la instancia de Postgres como un número seguido de Ki (kilobytes), Mi (megabytes) o Gi (gigabytes). Para quitar la solicitud de memoria, especifique su valor como una cadena vacía.
#### `--extensions`
Lista separada por comas de las extensiones de Postgres que deben cargarse en el inicio. Vea la documentación de Postgres para obtener información sobre los valores admitidos.
#### `--dev`
Si se especifica, se considera una instancia de desarrollo y no se facturará.
#### `--port`
Opcional.
#### `--no-wait`
Si se especifica, el comando no esperará a que la instancia esté en un estado listo antes de volver.
#### `--engine-settings -e`
Lista separada por comas de los valores del motor de Postgres con el formato '"key1=val1, key2=val2". La configuración proporcionada se combinará con la configuración existente. Para quitar una configuración, indique un valor vacío, como "removedKey=". Si se cambia una configuración de motor que requiere un reinicio, el servicio se reiniciará para aplicar la configuración inmediatamente.
#### `--replace-engine-settings -re`
Cuando se especifica con --engine-settings, reemplazará toda la configuración de motor personalizada existente por el nuevo conjunto de valores y configuraciones.
#### `--admin-password`
Si se especifica, la contraseña de administrador del servidor Postgres se establecerá en el valor de la variable de entorno AZDATA_PASSWORD si está presente y, si no, en un valor solicitado.
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
## <a name="azdata-arc-postgres-server-delete"></a>azdata arc postgres server delete
Elimina un grupo de servidores PostgreSQL.
```bash
azdata arc postgres server delete --name -n 
                                  [--engine-version -ev]
```
### <a name="examples"></a>Ejemplos
Elimina un grupo de servidores PostgreSQL.
```bash
azdata arc postgres server delete -n pg1
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
## <a name="azdata-arc-postgres-server-show"></a>azdata arc postgres server show
Muestra los detalles de un grupo de servidores PostgreSQL.
```bash
azdata arc postgres server show --name -n 
                                [--engine-version -ev]  
                                
[--path -p]
```
### <a name="examples"></a>Ejemplos
Muestra los detalles de un grupo de servidores PostgreSQL.
```bash
azdata arc postgres server show -n pg1
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `--name -n`
Nombre del grupo de servidores de PostgreSQL.
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--engine-version -ev`
Puede usar --engine-version junto con --name para identificar un grupo de servidores de Hiperescala de PostgreSQL cuando dos grupos de servidores de una versión de motor diferente tienen el mismo nombre. --engine-version es opcional y, cuando se usa para identificar un grupo de servidores, debe ser 11 o 12.
#### `--path -p`
Ruta de acceso donde se debe escribir la especificación completa del grupo de servidores PostgreSQL. Si se omite, la especificación se escribirá en la salida estándar.
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
## <a name="azdata-arc-postgres-server-list"></a>azdata arc postgres server list
Muestra una lista de los grupos de servidores de PostgreSQL.
```bash
azdata arc postgres server list 
```
### <a name="examples"></a>Ejemplos
Muestra una lista de los grupos de servidores de PostgreSQL.
```bash
azdata arc postgres server list
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

