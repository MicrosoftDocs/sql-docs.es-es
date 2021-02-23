---
title: Propiedades de configuración de la instancia maestra de SQL Server
titleSuffix: SQL Server big data clusters
description: Artículo de referencia para las propiedades de configuración de la instancia maestra de SQL Server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2d986013374e7f69111288d2d0f50b09130a2d68
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343512"
---
# <a name="sql-server-master-instance-configuration-properties----pre-cu9-release"></a>Propiedades de configuración de la instancia maestra de SQL Server (versiones anteriores a CU9)

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]
> [!NOTE]
> La siguiente información solo se aplica a los clústeres de versiones anteriores a CU9 que no están habilitados para la configuración y que requieren el archivo mssql-conf para configurar la instancia maestra de SQL Server. Los clústeres de CU9 y versiones posteriores aprovechan la funcionalidad de administración de configuración y ya no requieren el uso de un archivo mssql-conf. [Aquí](reference-config-bdc-overview.md) puede encontrar las configuraciones disponibles para la instancia maestra de SQL Server y otros componentes de BDC.

## <a name="properties"></a>Propiedades

Puede configurar las siguientes opciones de SQL Server para la instancia maestra en el momento de la implementación.

|Propiedad|Opciones|
| --- | --- |
|`[sqlagent]`|`enabled = { true | false }` |
|`[telemetry]`|`customerfeedback = { true | false }` |
|`[telemetry]`|`userRequestedLocalAuditDirectory = </path/file>`|
|`[licensing]`| `pid = { Enterprise | Developer }`|
|`[traceflag]`|` traceflag<#> = <####>`|

### <a name="examples"></a>Ejemplos

En el siguiente ejemplo se habilita el Agente SQL y la telemetría, se establece un PID para Enterprise Edition y se habilita la marca de seguimiento 1204:

```
[sqlagent]
enabled=true

[telemetry]
customerfeedback=true
userRequestedLocalAuditDirectory = /tmp/audit

[licensing]
pid = Enterprise

[traceflag]
traceflag0 = 1204
```

Para ver las instrucciones, consulte [Configuración de una instancia maestra de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](configure-sql-server-master-instance.md).

## <a name="next-steps"></a>Pasos siguientes

[Configuración de una instancia maestra de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](configure-sql-server-master-instance.md)
