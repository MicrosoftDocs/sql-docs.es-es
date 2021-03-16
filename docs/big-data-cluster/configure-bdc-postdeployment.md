---
title: Información general sobre la configuración de Clústeres de macrodatos de SQL Server después de la implementación
titleSuffix: SQL Server big data clusters
description: Información general sobre la configuración de Clústeres de macrodatos después de la implementación
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 06b01a8fba30178cf8c8eb5842750de4baff1c82
ms.sourcegitcommit: e2d25f265556af92afcc0acde662929e654bf841
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103489476"
---
# <a name="how-to-configure-bdc-settings-post-deployment"></a>Procedimiento para configurar BDC después de la implementación

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

> [!NOTE]
> La configuración posterior a la implementación solo está disponible en BDC CU9 y versiones posteriores. Además, no incluye la configuración de escalado, almacenamiento o puntos de conexión. En [este artículo](configure-bdc-pre-configuration.md) encontrará opciones e instrucciones para configurar versiones anteriores a BDC CU9.

La configuración del ámbito del clúster, el servicio y el recurso para Clústeres de macrodatos se puede configurar después de la implementación mediante la CLI de azdata. Esta funcionalidad permite a los administradores de BDC ajustar las configuraciones para cumplir siempre los requisitos de la carga de trabajo. En este artículo se describen los pasos para configurar una instancia de BDC para que cumpla los requisitos de la carga de trabajo de Spark. La funcionalidad de configuración posterior a la implementación sigue un flujo de establecimiento, comparación y aplicación.

## <a name="step-by-step-configure-bdc-to-meet-your-spark-workload-requirements"></a>Guía paso a paso: Configuración de BDC para cumplir los requisitos de la carga de trabajo de Spark

### <a name="view-the-current-configurations-of-the-big-data-cluster-spark-service"></a>Visualización de las configuraciones actuales del servicio Spark de clústeres de macrodatos
En el ejemplo siguiente se muestra cómo ver los valores de configuración del servicio Spark configurados por el usuario. Puede ver todos los valores de configuración posibles, los valores administrados por el sistema y todos los valores configurables, así como los cambios de configuración pendientes mediante parámetros opcionales. Para obtener más información, consulte el artículo sobre la [instrucción `azdata bdc spark`](../azdata/reference/reference-azdata-bdc-spark-statement.md).

```bash
azdata bdc spark settings show
```
#### <a name="sample-output"></a>Salida de ejemplo
Servicio Spark. 

|Parámetro|Valor actual|
| --- | --- |
|`spark-defaults-conf.spark.driver.cores`|`1` |
|`spark-defaults-conf.spark.driver.memory`|`1664m` |

### <a name="change-the-default-number-of-cores-and-memory-for-the-spark-driver-across-all-resources-with-spark-ie-for-the-spark-service"></a>Cambio del número predeterminado de núcleos y de la memoria predeterminada del controlador de Spark en todos los recursos con Spark (es decir, del servicio Spark)
Actualice el número predeterminado de núcleos a 2 y la memoria predeterminada a 7424m para el servicio Spark.

```bash
azdata bdc spark settings set --settings spark-defaults-conf.spark.driver.cores=2,spark-defaults-conf.spark.driver.memory=7424m
```

### <a name="change-the-default-number-of-cores-and-memory-for-the-spark-executors-in-the-storage-pool"></a>Cambio del número predeterminado de núcleos y de la memoria predeterminada de los ejecutores de Spark en el bloque de almacenamiento
Actualice el número predeterminado de núcleos de los ejecutores a 4 para el bloque de almacenamiento.

```bash
azdata bdc spark settings set --settings spark-defaults-conf.spark.executor.cores=4 --resource=storage-0
```

### <a name="view-the-pending-settings-changes-staged-in-the-bdc"></a>Visualización de los cambios de configuración pendientes almacenados provisionalmente en la instancia de BDC
Vea los cambios de configuración pendientes para el servicio Spark únicamente y en todo el clúster de BDC.

#### <a name="pending-spark-service-settings"></a>Cambios de configuración pendientes del servicio Spark
```bash
azdata bdc spark settings show --filter-option=pending --include-details
```

### <a name="spark-service"></a>Servicio Spark

|Parámetro|Valor actual|Valor configurado|Configurable|Configurado |Última actualización|
| --- | --- | --- | --- | --- | --- |
|`spark-defaults-conf.spark.driver.cores`|`1`| `2` | `true` | `true` |
|`spark-defaults-conf.spark.driver.memory`|`1664m`| `7424m` | `true` | `true` |

#### <a name="all-pending-settings"></a>Todos los cambios de configuración pendientes
```bash
azdata bdc settings show --filter-option=pending --include-details --recursive
```

Cambios de configuración pendientes del servicio Spark

|Parámetro|Valor actual|Valor configurado|Configurable|Configurado|Última actualización|
| --- | --- | --- | --- | --- | --- |
|`spark-defaults-conf.spark.driver.cores`|`1`| `2` | `true` | `true` |
|`spark-defaults-conf.spark.driver.memory`|`1664m`| `7424m` | `true` | `true` |

Cambios de configuración pendientes del recurso storage-0 de Spark

|Parámetro|Valor actual|Valor configurado|Configurable|Configurado|Última actualización|
| --- | --- | --- | --- | --- | --- |
|`spark-defaults-conf.spark.executor.cores`|`1`| `4` | `true` | `true` |

### <a name="apply-the-pending-settings-to-the-bdc"></a>Aplicación de las configuraciones pendientes en BDC

```bash
azdata bdc settings apply
```

### <a name="monitor-the-status-of-the-bdc-configuration-update"></a>Supervisión del estado de la actualización de la configuración de BDC

```bash
azdata bdc status show -all
```

## <a name="optional-steps"></a>Pasos opcionales

### <a name="revert-pending-configuration-settings"></a>Reversión de las configuraciones pendientes

Si determina que ya no quiere cambiar las configuraciones pendientes, puede dejar de almacenarlas provisionalmente. Esto revertirá las configuraciones pendientes en todos los ámbitos.

```bash
azdata bdc settings revert
```

### <a name="abort-the-configuration-upgrade"></a>Anulación de la actualización de la configuración

Si se produce un error en la actualización de la configuración para cualquiera de los componentes, puede cancelar el proceso de actualización y revertir la instancia de BDC a sus configuraciones anteriores. Las configuraciones almacenadas provisionalmente durante la actualización se volverán a mostrar como configuraciones pendientes.

```bash
azdata bdc settings cancel-apply
```

## <a name="next-steps"></a>Pasos siguientes

[Configuración de un clúster de macrodatos de SQL Server](configure-bdc-overview.md)
