---
title: Configuración de una instancia maestra de SQL Server
titleSuffix: Configure SQL Server master instance of Big Data Cluster
description: Configuración de una instancia maestra de un clúster de macrodatos de SQL Server
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 11/04/2019
ms.topic: overview
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: de9bba220f985522926d610b36418d9607c8a568
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039085"
---
# <a name="configure-master-instance-of-big-data-clusters-2019"></a>Configuración de una instancia maestra de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

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

## <a name="known-limitations"></a>Restricciones conocidas

- Los procedimientos anteriores requieren permisos de administrador de clústeres de Kubernetes.
- La intercalación del servidor de la instancia maestra de SQL Server del clúster de macrodatos no se puede cambiar después de la implementación.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo implementar clústeres de macrodatos de SQL Server, vea [Introducción a clústeres de macrodatos de SQL Server](deploy-get-started.md).