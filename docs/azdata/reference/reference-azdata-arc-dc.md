---
title: Referencia de azdata arc dc
titleSuffix: SQL Server big data clusters
description: Artículo de referencia sobre los comandos de azdata arc dc.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b223b3b69ba3eda1be5f882b4cabd868c345b20c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049015"
---
# <a name="azdata-arc-dc"></a>azdata arc dc

Se aplica a [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

En el siguiente artículo se proporciona una referencia de los comandos **sql** de la herramienta **azdata**. Para obtener más información sobre otros comandos de **azdata**, vea la [Referencia de azdata](reference-azdata.md).

## <a name="commands"></a>Comandos

|Comando|Descripción|
| --- | --- |
[azdata arc dc create](#azdata-arc-dc-create) | Crear un controlador de datos.
[azdata arc dc delete](#azdata-arc-dc-delete) | Eliminar un controlador de datos.
[azdata arc dc endpoint](reference-azdata-arc-dc-endpoint.md) | Comandos de punto de conexión.
[azdata arc dc status](reference-azdata-arc-dc-status.md) | Comandos de estado.
[azdata arc dc config](reference-azdata-arc-dc-config.md) | Comandos de configuración.
[azdata arc dc debug](reference-azdata-arc-dc-debug.md) | Comandos de depuración.
[azdata arc dc export](#azdata-arc-dc-export) | Exportar métricas, registros o utilización.
[azdata arc dc upload](#azdata-arc-dc-upload) | Cargar el archivo de datos exportado.
## <a name="azdata-arc-dc-create"></a>azdata arc dc create
Crear un controlador de datos. Se requiere kubeconfig en el sistema junto con las variables de entorno siguientes: ["AZDATA_USERNAME", "AZDATA_PASSWORD"].
```bash
azdata arc dc create --namespace -ns 
                     --name -n  
                     
--connectivity-mode  
                     
--resource-group -g  
                     
--location -l  
                     
--subscription -s  
                     
[--profile-name]  
                     
[--path -p]  
                     
[--storage-class -sc]
```
### <a name="examples"></a>Ejemplos
Implementación del controlador de datos.
```bash
azdata arc dc create
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `--namespace -ns`
El espacio de nombres de Kubernetes en el que se va a implementar el controlador de datos. Si ya existe, se usará. Si no existe, en primer lugar se hará un intento para crearlo.
#### `--name -n`
El nombre del controlador de datos.
#### `--connectivity-mode`
La conectividad a Azure (indirecta o directa), en la que el controlador de datos debe funcionar.
#### `--resource-group -g`
El grupo de recursos de Azure en el que se debe agregar el recurso del controlador de datos.
#### `--location -l`
La ubicación de Azure en la que se almacenarán los metadatos del controlador de datos (por ejemplo, eastus).
#### `--subscription -s`
El id. de suscripción de Azure en el que se debe agregar el recurso del controlador de datos.
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--profile-name`
El nombre de un perfil de configuración existente. Para ver las opciones disponibles, ejecute `azdata arc dc config list`. Una de las siguientes: ["azure-arc-aks-premium-storage", "azure-arc-ake", "azure-arc-openshift", "azure-arc-gke", "azure-arc-aks-default-storage", "azure-arc-kubeadm", "azure-arc-eks", "azure-arc-azure-openshift", "azure-arc-aks-hci"].
#### `--path -p`
La ruta de acceso a un directorio que contiene un perfil de configuración personalizado que se va a usar. Para crear un perfil de configuración personalizado, ejecute `azdata arc dc config init`.
#### `--storage-class -sc`
La clase de almacenamiento que se va a usar para todos los datos y que registra los volúmenes persistentes de todos los pods del controlador de datos que los necesitan.
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
## <a name="azdata-arc-dc-delete"></a>azdata arc dc delete
Elimina el controlador de datos: se requiere kubeconfig en el sistema.
```bash
azdata arc dc delete --name -n 
                     --namespace -ns  
                     
[--force -f]
```
### <a name="examples"></a>Ejemplos
Implementación del controlador de datos.
```bash
azdata arc dc delete
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `--name -n`
Nombre del controlador de datos.
#### `--namespace -ns`
El espacio de nombres de Kubernetes en el que existe el controlador de datos.
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--force -f`
Fuerza la eliminación de un controlador de datos.
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
## <a name="azdata-arc-dc-export"></a>azdata arc dc export
Exporta métricas, registros o utilización a un archivo.
```bash
azdata arc dc export --type -t 
                     --path -p  
                     
[--force -f]
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `--type -t`
El tipo de datos que se van a exportar. Opciones: registros, métricas y utilización.
#### `--path -p`
La ruta de acceso completa o relativa, incluido el nombre del archivo que se va a exportar.
### <a name="optional-parameters"></a>Parámetros opcionales
#### `--force -f`
Fuerza la creación de un archivo de salida. Sobrescribe cualquier archivo existente en la misma ruta de acceso.
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
## <a name="azdata-arc-dc-upload"></a>azdata arc dc upload
Carga el archivo de datos exportado desde un controlador de datos a Azure.
```bash
azdata arc dc upload --path -p 
                     
```
### <a name="required-parameters"></a>Parámetros requeridos
#### `--path -p`
La ruta de acceso completa o relativa, incluido el nombre del archivo que se va a cargar.
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

