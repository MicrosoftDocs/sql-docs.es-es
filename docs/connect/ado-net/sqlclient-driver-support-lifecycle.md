---
title: Ciclo de vida de soporte del controlador SqlClient
description: Página que contiene información del ciclo de vida de soporte técnico.
ms.date: 02/26/2021
dev_langs:
- csharp
- vb
ms.assetid: 6f5ff56a-a57e-49d7-8ae9-bbed697e42e3
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: d0fc4a58dbd8375bd988fbee851cab68368b28d1
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836992"
---
# <a name="sqlclient-driver-support-lifecycle"></a>Ciclo de vida de soporte del controlador SqlClient

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

La biblioteca Microsoft.Data.SqlClient sigue la directiva de soporte técnico de .NET Core más reciente para todas las versiones.

[Visualización de la directiva de compatibilidad de .NET Core](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)

## <a name="microsoftdatasqlclient-release-cadence"></a>Cadencia de versión de Microsoft.Data.SqlClient

Las nuevas versiones estables (GA) se publican cada 6 meses a partir de una cadencia regular a partir de la versión 1.2, junto con 2 a 3 versiones preliminares entre medio. Las partes interesadas y los mantenedores elegirán las versiones de soporte técnico a largo plazo (LTS) en función de unas cuantas calificaciones y de la respuesta de los clientes.

### <a name="actively-supported-releases"></a>Versiones admitidas activamente

| Versión | Fecha oficial de lanzamiento | Última versión de revisión | Fecha de publicación de la actualización acumulativa | Nivel de soporte técnico  | Finalización del soporte |
| -- | -- | -- | -- | -- | -- |
| 2.1 | 19 de noviembre de 2020 | 2.1.1 | 18 de diciembre de 2020 | LTS | 20 de noviembre de 2023 |
| 1.1 | 20 de noviembre de 2019 | 1.1.3 | 15 de mayo de 2020 | LTS | 21 de noviembre de 2022 |

### <a name="out-of-support-releases"></a>Versiones fuera de soporte técnico

| Versión | Fecha de publicación de la revisión más reciente | Última versión de revisión | Fin de soporte técnico |
| -- | -- | -- | -- |
| 2.0 | 16 de junio de 2020 | 2.0.1 | 25 de agosto de 2020 |
| 1.0 | 26 de septiembre de 2019 | 1.0.19269.1 | 20 de febrero de 2020 |

### <a name="long-term-support-lts-releases"></a>Versiones de soporte técnico a largo plazo (LTS)

Las versiones de LTS se admiten durante tres años después de la versión inicial.

### <a name="current-releases"></a>Versiones actuales

Las versiones actuales se admiten durante tres meses después de la versión actual o LTS posterior.

## <a name="sql-version-compatibility-with-microsoftdatasqlclient"></a>Compatibilidad de versiones de SQL con Microsoft.Data.SqlClient

|Versión de la base de datos&nbsp;&#8594;<br />&#8595; versión del controlador|Azure SQL Database|Azure Synapse Analytics|Instancia administrada de Azure SQL|SQL Server 2019|SQL Server 2017|SQL Server 2016|SQL Server 2014|SQL Server 2012|
|---|---|---|---|---|---|---|---|---|
|2.1|Sí|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|2.0|Sí|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|1,1|Sí|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|1.0|Sí|Sí|Sí|Sí|Sí|Sí|Sí|Sí|

## <a name="supported-os-versions"></a>Versiones de SO admitidas

### <a name="support-for-net-framework-applications"></a>Compatibilidad con aplicaciones de .NET Framework

Microsoft.Data.SqlClient es compatible con todos los sistemas operativos compatibles con .NET Framework 4.6 y versiones posteriores.

[Requisitos de sistema de .NET Framework](/dotnet/framework/get-started/system-requirements).

### <a name="support-for-net-core-applications"></a>Compatibilidad con aplicaciones .NET Core

Microsoft.Data.SqlClient es compatible con todos los sistemas operativos compatibles con .NET Core v2.1 y versiones posteriores.

[Directiva de ciclo de vida del sistema operativo compatible con .NET Core](https://github.com/dotnet/core/blob/master/os-lifecycle-policy.md).

> [!NOTE]
> El modo invariable de globalización no se admite actualmente.
