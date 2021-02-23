---
title: Configuración de Clústeres de macrodatos de SQL Server (versiones anteriores a CU9)
titleSuffix: SQL Server big data clusters
description: Configuración de Clústeres de macrodatos (versiones anteriores a CU9)
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b00ed57288d19f08555a00eec8c9e62edc0f8cf6
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343969"
---
# <a name="configure-a-sql-server-big-data-cluster---pre-cu9-release"></a>Configuración de un clúster de macrodatos de SQL Server (versiones anteriores a CU9)

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

> [!NOTE]
> Antes de la versión CU9 y la compatibilidad con los clústeres habilitados para la configuración, los clústeres de macrodatos solo se podían configurar en el momento de la implementación, a excepción de la instancia maestra de SQL Server, que solo se podía configurar después de la implementación mediante mssql-conf. [Aquí](configure-bdc-overview.md) encontrará instrucciones para configurar BDC CU9 y versiones posteriores.


En BDC CU8 y versiones anteriores, puede configurar los parámetros de BDC en el momento de la implementación mediante el archivo de implementación `bdc.json`. La instancia maestra de SQL Server solo se puede configurar después de la implementación mediante mssql-conf.

## <a name="configuration-scopes"></a>Ámbitos de configuración
La configuración de los Clústeres de macrodatos (versiones anteriores a CU9) tiene dos niveles de ámbito: `service` y `resource`. La jerarquía de la configuración también sigue este orden, de mayor a menor. Los componentes de BDC tomarán el valor de la configuración que se define en el ámbito inferior. Si la configuración no se define en un ámbito determinado, heredará el valor de su ámbito principal superior.

Por ejemplo, puede que quiera definir el número predeterminado de núcleos que el controlador de Spark usará en el bloque de almacenamiento y en los recursos de `Sparkhead`. Puede hacerlo de dos maneras:

* Especifique un valor predeterminado de núcleos en el ámbito de servicio `Spark` 
* Especifique un valor predeterminado de núcleos en el ámbito de recursos `storage-0` y `sparkhead`

En el primer escenario, todos los recursos con ámbito inferior del servicio Spark (bloque de almacenamiento y `Sparkhead`) *heredarán* el número predeterminado de núcleos del valor predeterminado del servicio Spark.

En el segundo escenario, cada recurso usará el valor definido en su ámbito respectivo.

Si el número predeterminado de núcleos se configura tanto en el ámbito de servicio como en el de ámbito, el valor con ámbito de recurso reemplazará el valor con ámbito de servicio, porque se trata del ámbito **configurado por el usuario** inferior para la configuración determinada.

Para obtener información específica sobre la configuración, vea los artículos siguientes:

## <a name="configure-the-sql-server-master-instance"></a>Configuración de la instancia maestra de SQL Server
Configure una instancia maestra de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].

No se pueden definir opciones de configuración de servidor de una instancia maestra de SQL Server en el momento de la implementación. En este artículo se describe una solución temporal para configurar opciones como la edición de SQL Server, habilitar o deshabilitar el Agente SQL Server, habilitar marcas de seguimiento concretas o habilitar o deshabilitar los comentarios de los clientes.

Haga lo siguiente para cambiar alguna de esas opciones:

1. Cree un archivo `mssql-custom.conf` personalizado que incluya la configuración de destino. En el siguiente ejemplo se habilita el Agente SQL y la telemetría, se establece un PID para Enterprise Edition y se habilita la marca de seguimiento 1204:

   ```
   [sqlagent]
   enabled=true
   
   [telemetry]
   customerfeedback=true
   userRequestedLocalAuditDirectory = /tmp/audit

   [DEFAULT]
   pid = Enterprise

   [traceflag]
   traceflag0 = 1204
   ```

1. Copie el archivo `mssql-custom.conf` en `/var/opt/mssql` en el contenedor `mssql-server` del pod `master-0`. Reemplace `<namespaceName>` por el nombre del clúster de macrodatos.

   ```bash
   kubectl cp mssql-custom.conf master-0:/var/opt/mssql/mssql-custom.conf -c mssql-server -n <namespaceName>
   ```

1. Reinicie la instancia de SQL Server.  Reemplace `<namespaceName>` por el nombre del clúster de macrodatos.

   ```bash
   kubectl exec -it master-0  -c mssql-server -n <namespaceName> -- /bin/bash
   supervisorctl restart mssql-server
   exit
   ```

> [!IMPORTANT]
> Si la instancia maestra de SQL Server está en una configuración de grupos de disponibilidad, copie el archivo `mssql-custom.conf` en todos los pods `master`. Tenga en cuenta que cada reinicio producirá una conmutación por error, por lo que deberá planear esta actividad durante períodos de tiempo de inactividad.

### <a name="known-limitations"></a>Restricciones conocidas

- Los procedimientos anteriores requieren permisos de administrador de clústeres de Kubernetes.
- La intercalación del servidor de la instancia maestra de SQL Server del clúster de macrodatos no se puede cambiar después de la implementación.

## <a name="configure-apache-spark-and-apache-hadoop"></a>Configuración de Apache Spark y Apache Hadoop
Para configurar Apache Spark y Apache Hadoop en clústeres de macrodatos, debe modificar el perfil de clúster en el momento de la implementación.

Un Clúster de macrodatos tiene cuatro categorías de configuración: 

- `sql` 
- `hdfs` 
- `spark` 
- `gateway` 

`sql`, `hdfs`, `spark` y `sql` son servicios. Cada servicio se asigna a la misma categoría de configuración con nombre. Todas las configuraciones de puerta de enlace se corresponden a la categoría `gateway`. 

Por ejemplo, todas las configuraciones del servicio `hdfs` pertenecen a la categoría `hdfs`. Tenga en cuenta que todas las configuraciones de Hadoop (sitio principal), HDFS y Zookeeper pertenecen a la categoría `hdfs`; todas las configuraciones de Livy, Spark, Yarn, Hive y Metastore de Hive pertenecen a la categoría `spark`. 

[Configuraciones admitidas](reference-config-spark-hadoop.md#supported-configurations) muestra propiedades de Apache Spark y Hadoop que se pueden configurar al implementar un clúster de macrodatos de SQL Server.

En las siguientes secciones se enumeran las propiedades que **no puede** modificar en un clúster:

- [Configuraciones de `spark` no admitidas](reference-config-spark-hadoop.md#unsupported-spark-configurations)
- [Configuraciones de `hdfs` no admitidas](reference-config-spark-hadoop.md#unsupported-hdfs-configurations)
- [Configuraciones de `gateway` no admitidas](reference-config-spark-hadoop.md#unsupported-gateway-configurations)

## <a name="next-steps"></a>Pasos siguientes

[Configuración de un clúster de macrodatos de SQL Server](configure-bdc-overview.md)