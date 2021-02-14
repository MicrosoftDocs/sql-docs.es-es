---
title: Referencia de azdata arc sql mi config
titleSuffix: SQL Server big data clusters
description: Artículo de referencia sobre los comandos de azdata arc sql mi config.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 1913f0754001f69436b02f465711ac45245c504c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052576"
---
# <a name="azdata-arc-sql-mi-config"></a>azdata arc sql mi config

Se aplica a [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

En el siguiente artículo se proporciona una referencia de los comandos **sql** de la herramienta **azdata**. Para obtener más información sobre otros comandos de **azdata**, vea la [Referencia de azdata](reference-azdata.md).

## <a name="commands"></a>Comandos

|Comando|Descripción|
| --- | --- |
[azdata arc sql mi config init](#azdata-arc-sql-mi-config-init) | Inicializa los archivos CRD y de especificación de una instancia administrada de SQL.
[azdata arc sql mi config add](#azdata-arc-sql-mi-config-add) | Agrega un valor a una ruta de acceso json en un archivo de configuración.
[azdata arc sql mi config remove](#azdata-arc-sql-mi-config-remove) | Quita un valor de una ruta de acceso json en un archivo de configuración.
[azdata arc sql mi config replace](#azdata-arc-sql-mi-config-replace) | Reemplaza un valor de una ruta de acceso json en un archivo de configuración.
[azdata arc sql mi config patch](#azdata-arc-sql-mi-config-patch) | Aplica una revisión en un archivo de configuración basándose en un archivo de revisión json.
## <a name="azdata-arc-sql-mi-config-init"></a>azdata arc sql mi config init
Inicializa los archivos CRD y de especificación de una instancia administrada de SQL.
```bash
azdata arc sql mi config init --path -p 
                              
```
### <a name="examples"></a>Ejemplos
Inicializa los archivos CRD y de especificación de una instancia administrada de SQL.
```bash
azdata arc sql mi config init --path ./template
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `--path -p`
Ruta de acceso donde se deben escribir los archivos CRD y de especificación de la instancia administrada de SQL.
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
## <a name="azdata-arc-sql-mi-config-add"></a>azdata arc sql mi config add
Agrega el valor a la ruta de acceso json del archivo de configuración.  Todos los ejemplos siguientes se proporcionan en Bash.  Si usa otra línea de comandos, tenga en cuenta que puede que necesite escapar las comillas (según corresponda).  Como alternativa, puede usar la función del archivo de revisión.
```bash
azdata arc sql mi config add --path -p 
                             --json-values -j
```
### <a name="examples"></a>Ejemplos
Ejemplo 1: agregar almacenamiento.
```bash
azdata arc sql mi config add --path custom/spec.json --json-values "spec.storage={"accessMode":"ReadWriteOnce","className":"managed-premium","size":"10Gi"}"
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `--path -p`
Ruta de acceso a la especificación de recursos personalizados, es decir, custom/spec.json.
#### `--json-values -j`
Lista de pares de clave y valor de rutas json a valores: clave1.subclave1=valor1,clave2.subclave2=valor2. Puede proporcionar valores json insertados (como key='{"kind":"cluster","name":"test-cluster"}'), o bien puede especificar una ruta de archivo (por ejemplo, clave=./valores.json). La adición no admite condicionales.  Si el valor en línea facilitado es en sí mismo un par clave-valor con "=" y ",", escape esos caracteres.  Por ejemplo, key1="key2\=val2\,key3\=val3". Vea ejemplos de la apariencia de la ruta en http://jsonpatch.com/.  Para acceder a una matriz, necesita indicar del índice (por ejemplo, clave.0=valor).
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
## <a name="azdata-arc-sql-mi-config-remove"></a>azdata arc sql mi config remove
Quita el valor de la ruta de acceso json del archivo de configuración.  Todos los ejemplos siguientes se proporcionan en Bash.  Si usa otra línea de comandos, tenga en cuenta que puede que necesite escapar las comillas (según corresponda).  Como alternativa, puede usar la función del archivo de revisión.
```bash
azdata arc sql mi config remove --path -p 
                                --json-path -j
```
### <a name="examples"></a>Ejemplos
Ejemplo 1: quitar almacenamiento.
```bash
azdata arc sql mi config remove --path custom/spec.json --json-path ".spec.storage"
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `--path -p`
Ruta de acceso a la especificación de recursos personalizados, es decir, custom/spec.json.
#### `--json-path -j`
Lista de rutas json basadas en la biblioteca jsonpatch que indica qué valores quiere quitar (por ejemplo, “clave1.subclave1,clave2.subclave2”). La eliminación no admite el uso de condicionales. Vea ejemplos de la apariencia de la ruta en http://jsonpatch.com/.  Para acceder a una matriz, necesita indicar del índice (por ejemplo, clave.0=valor).
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
## <a name="azdata-arc-sql-mi-config-replace"></a>azdata arc sql mi config replace
Reemplaza el valor en la ruta de acceso json del archivo de configuración.  Todos los ejemplos siguientes se proporcionan en Bash.  Si usa otra línea de comandos, tenga en cuenta que puede que necesite escapar las comillas (según corresponda).  Como alternativa, puede usar la función del archivo de revisión.
```bash
azdata arc sql mi config replace --path -p 
                                 --json-values -j
```
### <a name="examples"></a>Ejemplos
Ejemplo 1: reemplazar el puerto de un único punto de conexión.
```bash
azdata arc sql mi config replace --path custom/spec.json --json-values "$.spec.endpoints[?(@.name=="Controller")].port=30080"
```
Ejemplo 2: reemplazar almacenamiento.
```bash
azdata arc sql mi config replace --path custom/spec.json --json-values "spec.storage={"accessMode":"ReadWriteOnce","className":"managed-premium","size":"10Gi"}"
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `--path -p`
Ruta de acceso a la especificación de recursos personalizados, es decir, custom/spec.json.
#### `--json-values -j`
Lista de pares de clave y valor de rutas json a valores: clave1.subclave1=valor1,clave2.subclave2=valor2. Puede proporcionar valores json insertados (como key='{"kind":"cluster","name":"test-cluster"}'), o bien puede especificar una ruta de archivo (por ejemplo, clave=./valores.json). El comando para reemplazar admite condicionales mediante la biblioteca jsonpath.  Para usar esto, inicie la ruta con un signo de $. De esta forma, se puede agregar una declaración condicional, como -j $.key1.key2[?(@.key3=="someValue"].key4=value. Si el valor en línea facilitado es en sí mismo un par clave-valor con "=" y ",", escape esos caracteres.  Por ejemplo, key1="key2\=val2\,key3\=val3". A continuación, se muestran algunos ejemplos. Para obtener ayuda, vea: https://jsonpath.com/
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
## <a name="azdata-arc-sql-mi-config-patch"></a>azdata arc sql mi config patch
Aplica una revisión en el archivo de configuración según el archivo de revisión especificado. Para obtener información sobre cómo crear las rutas, vea http://jsonpatch.com/. La operación de reemplazar puede usar declaraciones condicionales en la ruta debido a la biblioteca jsonpath https://jsonpath.com/. Todos los archivos json de revisión tienen que iniciarse con una clave de “patch”, que tiene una matriz de revisiones con la correspondiente operación (add [agregar], replace [reemplazar], remove [quitar]), ruta y valor. La operación “remove” (quitar) no necesita un valor, solo una ruta de acceso. A continuación, se muestran algunos ejemplos.
```bash
azdata arc sql mi config patch --path -p 
                               --patch-file
```
### <a name="examples"></a>Ejemplos
Ejemplo 1: reemplazar el puerto de un único punto de conexión con un archivo de revisión.
```bash
azdata arc sql mi config patch --path custom/spec.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":"$.spec.endpoints[?(@.name=="Controller")].port","value":30080}]}
```
Ejemplo 2: reemplazar el almacenamiento de con un archivo de revisión.
```bash
azdata arc sql mi config patch --path custom/spec.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":".spec.storage","value":{"accessMode":"ReadWriteMany","className":"managed-premium","size":"10Gi"}}]}
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `--path -p`
Ruta de acceso a la especificación de recursos personalizados, es decir, custom/spec.json.
#### `--patch-file`
Ruta a un archivo json de revisión basado en la biblioteca jsonpatch: http://jsonpatch.com/. Tiene que empezar el archivo json de revisión con una clave denominada “patch”, cuyo valor es una matriz de operaciones de revisión que quiere realizar. Para la ruta de una operación de revisión, puede usar la notación de puntos (como clave1.clave2) para la mayoría de las operaciones. Si no quiere realizar una operación de reemplazo y va a reemplazar un valor en una matriz que necesita una condicional, use la notación jsonpath (para hacerlo, agregue un signo de $ al principio de la ruta de acceso). De esta forma, se puede agregar una declaración condicional, como $.key1.key2[?(@.key3=="someValue"].key4. A continuación, se muestran algunos ejemplos. Para obtener más información sobre las declaraciones condicionales, vea: https://jsonpath.com/.
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

