---
description: Características en desuso del motor de base de datos de [!INCLUDE[sssql19-md](../includes/sssql19-md.md)]
title: Características en desuso del motor de base de datos de SQL Server 2019 | Microsoft Docs
titleSuffix: SQL Server 2019
ms.custom: seo-lt-2019
ms.date: 02/12/2021
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- deprecated changes 2019 [SQL Server]
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 4e322ff478e9ccdc031882e7c3b5b18fd8506e42
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186477"
---
# <a name="deprecated-database-engine-features-in-sql-server-2019-15x"></a>Características en desuso del motor de base de datos de SQL Server 2019 (15.x)

[!INCLUDE[sqlserver2019](../includes/applies-to-version/sqlserver2019.md)]

[!INCLUDE [sssql19-md](../includes/sssql19-md.md)] no deja de usar ninguna característica más allá de aquellas que han pasado a estar en desuso en versiones anteriores:

- [[!INCLUDE [sssql17-md](../includes/sssql17-md.md)]](deprecated-database-engine-features-in-sql-server-2017.md)
- [[!INCLUDE [sssql16-md](../includes/sssql16-md.md)]](deprecated-database-engine-features-in-sql-server-2016.md)

## <a name="deprecation-guidelines"></a>Instrucciones sobre la fase de desuso

Cuando se establece que una característica está en desuso, significa que:

- Solo está en modo de mantenimiento. No se realizarán cambios nuevos, ni tampoco los relacionados con la resolución de la interoperabilidad con características nuevas.
- Nos esforzamos por no quitar una característica en desuso en las versiones futuras para facilitar las actualizaciones, Sin embargo, en raras ocasiones puede que optemos por interrumpir (quitar) permanentemente la característica de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] si limita las innovaciones futuras.
- En el caso de trabajos nuevos de desarrollo, no utilice características en desuso. En el caso de aplicaciones existentes, planifique modificar lo antes posible las aplicaciones que actualmente usan estas características.     

Puede supervisar el uso de características en desuso utilizando el contador de rendimiento del objeto de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Características desusadas, o bien los eventos ampliados `deprecation_announcement` y `deprecation_final_support`. Para obtener más información, vea [Usar objetos de SQL Server](../relational-databases/performance-monitor/use-sql-server-objects.md).  

## <a name="query-deprecated-features"></a>Consulta de características en desuso

Los valores de estos contadores también están disponibles si se ejecuta la siguiente instrucción:  

```sql
SELECT * FROM sys.dm_os_performance_counters
WHERE object_name = 'SQLServer:Deprecated Features';
```

### <a name="see-also"></a>Consulte también

- [Cambios importantes en las características del motor de base de datos de SQL Server 2019](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-version-15.md)
- [Funcionalidad del motor de base de datos no incluida en SQL Server](../database-engine/discontinued-database-engine-functionality-in-sql-server.md)
- [Compatibilidad con versiones anteriores del Motor de base de datos de SQL Server](./discontinued-database-engine-functionality-in-sql-server.md)
