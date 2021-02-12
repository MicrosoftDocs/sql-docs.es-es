---
title: Referencia de azdata bdc
titleSuffix: SQL Server big data clusters
description: Artículo de referencia sobre los comandos de azdata bdc.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: f10ba617642d42b4b1d091374305c5f97ba1b474
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052326"
---
# <a name="azdata-bdc"></a>azdata bdc

Se aplica a [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

En el siguiente artículo se proporciona una referencia de los comandos **sql** de la herramienta **azdata**. Para obtener más información sobre otros comandos de **azdata**, vea la [Referencia de azdata](reference-azdata.md).

## <a name="commands"></a>Comandos

|Comando|Descripción|
| --- | --- |
[azdata bdc spark](reference-azdata-bdc-spark.md) | Los comandos de Spark permiten al usuario interactuar con el sistema de Spark mediante la creación y administración de sesiones, instrucciones y lotes.
[azdata bdc hdfs](reference-azdata-bdc-hdfs.md) | El módulo HDFS proporciona comandos para acceder a un sistema de archivos HDFS.
[azdata bdc create](#azdata-bdc-create) | Crea el clúster de macrodatos.
[azdata bdc delete](#azdata-bdc-delete) | Elimina el clúster de macrodatos.
[azdata bdc upgrade](#azdata-bdc-upgrade) | Actualiza las imágenes implementadas en cada contenedor en el clúster de macrodatos de SQL Server.
[azdata bdc config](reference-azdata-bdc-config.md) | Comandos de configuración.
[azdata bdc endpoint](reference-azdata-bdc-endpoint.md) | Comandos de punto de conexión.
[azdata bdc debug](reference-azdata-bdc-debug.md) | Comandos de depuración.
[azdata bdc status](reference-azdata-bdc-status.md) | Comandos de estado del BDC.
[azdata bdc control](reference-azdata-bdc-control.md) | Comandos de servicio de control.
[azdata bdc sql](reference-azdata-bdc-sql.md) | Comandos de servicio de SQL.
[azdata bdc hdfs](reference-azdata-bdc-hdfs.md) | Comandos de servicio de HDFS.
[azdata bdc spark](reference-azdata-bdc-spark.md) | Comandos de servicio de Spark.
[azdata bdc gateway](reference-azdata-bdc-gateway.md) | Comandos de servicio de la puerta de enlace.
[azdata bdc app](reference-azdata-bdc-app.md) | Comandos de servicio de la aplicación.
## <a name="azdata-bdc-create"></a>azdata bdc create
Crea un clúster de macrodatos de SQL Server. Es necesario que Kubernetes esté configurado en el sistema junto con las siguientes variables de entorno: ['AZDATA_USERNAME', 'AZDATA_PASSWORD'].
```bash
azdata bdc create [--name -n] 
                  [--config-profile -c]  
                  
[--accept-eula -a]  
                  
[--node-label -l]  
                  
[--force -f]
```
### <a name="examples"></a>Ejemplos
Experiencia de implementación de BDC guiada: se reciben mensajes sobre los valores necesarios.
```bash
azdata bdc create
```
Implementación de BDC con argumentos y perfil de configuración personalizada que se ha inicializado a través de `azdata bdc config init`.
```bash
azdata bdc create --accept-eula yes --config-profile ./path/to/config/profile
```
Implementación de BDC con el nombre de clúster personalizado especificado y un perfil de configuración predeterminado aks-dev-test.
```bash
azdata bdc create --name <cluster_name> --accept-eula yes --config-profile aks-dev-test
```
Implementación de BDC con argumentos: no se proporcionan mensajes, ya que se usa la marca --force.
```bash
azdata bdc create --accept-eula yes --config-profile aks-dev-test --force
```
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--name -n`
Nombre del clúster de macrodatos, se usa para espacios de nombres de kubernetes.
#### `--config-profile -c`
Perfil de configuración del clúster de macrodatos, usado para implementar el clúster: ["openshift-prod", "aks-dev-test-ha", "aro-dev-test-ha", "aks-dev-test", "kubeadm-prod", "aro-dev-test", "openshift-dev-test", "kubeadm-dev-test"]
#### `--accept-eula -a`
¿Acepta los términos de licencia? [sí/no]. Si no quiere usar este argumento, puede establecer la variable de entorno ACCEPT_EULA en “yes”. Los términos de licencia de azdata se pueden ver en https://aka.ms/eula-azdata-en.
#### `--node-label -l`
Etiqueta de nodo del clúster de macrodatos, se usa para designar los nodos en los que se va a implementar.
#### `--force -f`
Fuerza la creación, no se le pide al usuario ningún valor y todos los problemas se imprimen como parte de stderr.
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
## <a name="azdata-bdc-delete"></a>azdata bdc delete
Elimina el clúster de macrodatos de SQL Server. Es necesario que Kubernetes esté configurado en el sistema.
```bash
azdata bdc delete --name -n 
                  [--force -f]
```
### <a name="examples"></a>Ejemplos
Eliminación del BDC.
```bash
azdata bdc delete --name <cluster_name>
```
### <a name="required-parameters"></a>Parámetros obligatorios
#### `--name -n`
Nombre del clúster de macrodatos; se usa para el espacio de nombres de Kubernetes.
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--force -f`
Fuerza la eliminación del clúster de macrodatos.
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
## <a name="azdata-bdc-upgrade"></a>azdata bdc upgrade
Actualiza las imágenes implementadas en cada contenedor en el clúster de macrodatos de SQL Server. Las imágenes actualizadas se basan en la imagen de Docker que se ha pasado. Si las imágenes actualizadas proceden de un repositorio de imágenes de Docker diferente al de las imágenes implementadas actualmente, también se requiere el parámetro "repository".
```bash
azdata bdc upgrade --name -n 
                   --tag -t  
                   
[--repository -r]  
                   
[--controller-timeout -k]  
                   
[--stability-threshold -s]  
                   
[--component-timeout -p]
```
### <a name="examples"></a>Ejemplos
Actualización del BDC a una nueva etiqueta de imagen "cu2" del mismo repositorio.
```bash
azdata bdc upgrade -t cu2
```
Actualización del BDC a una nueva imagen con la etiqueta "cu2" de un nuevo repositorio "foo/bar/baz".
```bash
azdata bdc upgrade -t cu2 -r foo/bar/baz
```
Actualización de BDC a una nueva imagen con la etiqueta "cu2" del mismo repositorio. La actualización esperará 30 minutos para que el controlador se actualice y 30 minutos más para que se actualice la base de datos del controlador. Después esperará a que el controlador y la base de la base del controlador se ejecuten durante tres minutos sin que se bloquee la actualización del resto del clúster. Cada fase posterior de la actualización tardará 40 minutos en completarse.
```bash
azdata bdc upgrade -t cu2 --controller-timeout=30 --component-timeout=40 --stability-threshold=3
```
### <a name="required-parameters"></a>Parámetros obligatorios
#### `--name -n`
Nombre del clúster de macrodatos, se usa para espacios de nombres de kubernetes.
#### `--tag -t`
Etiqueta de imagen de Docker de destino a la que actualizar todos los contenedores del clúster.
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--repository -r`
Repositorio de Docker del que todos los contenedores del clúster extraen sus imágenes.
#### `--controller-timeout -k`
Número de minutos que hay que esperar para que se actualice el controlador o la base de datos del controlador antes de revertir la actualización.
#### `--stability-threshold -s`
Número de minutos que hay que esperar tras una actualización antes de marcarla como estable.
#### `--component-timeout -p`
Número de minutos que hay que esperar para cada fase de la actualización (después de la actualización del controlador) a fin de que se complete antes de pausarla.
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

