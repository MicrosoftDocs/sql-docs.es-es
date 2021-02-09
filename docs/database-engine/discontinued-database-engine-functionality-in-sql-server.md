---
title: Funcionalidad del motor de base de datos no incluida
description: Obtenga información sobre la funcionalidad y las características del motor de base de datos que se han suspendido en SQL Server 2019 (15.x), SQL Server 2016 (13.x) y versiones anteriores.
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- VIA protocol
- unsupported features [SQL Server]
- SQL Mail
- discontinued functionality [SQL Server]
- RESTORE WITH DBO_ONLY
- BACKUP WITH PASSWORD
- user instances enabled
- BACKUP WITH MEDIAPASSWORD
- AWE
- SQL-DMO
- '*= and =*'
- 80 compatibility levels
- COMPUTE BY
- user instance timeout
- sp_dropalias
- COMPUTE
- SSL
- WITH APPEND
- sys.database_principal_aliases
- sp_dboption
- DATABASEPROPERTY
- FASTFIRSTROW hint
- SET DISABLE_DEF_CNST_CHK
ms.assetid: d686cdf0-d11d-4dba-9ec8-de1a5f189f25
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>= sql-server-linux-2017  || >= sql-server-2016'
ms.openlocfilehash: 093e20773376cddeea566e202d9f18605f1a1c13
ms.sourcegitcommit: 58e7069b5b2b6367e27b49c002ca854b31b1159d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99552594"
---
# <a name="discontinued-database-engine-functionality-in-sql-server"></a>Funcionalidad del motor de base de datos no incluida en SQL Server
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  En este tema se describen las características del [!INCLUDE[ssDE](../includes/ssde-md.md)] que ya no están disponibles en [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)].  

## <a name="discontinued-features-in-sssql19"></a>Características no incluidas en [!INCLUDE[sssql19](../includes/sssql19-md.md)]  

- Las siguientes opciones de configuración con ámbito de base de datos no se incluyen:

  - `DISABLE_BATCH_MODE_ADAPTIVE_JOIN`
  - `DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK`
  - `DISABLE_INTERLEAVED_EXECUTION_TVF`

Para obtener las opciones de configuración actuales, vea [ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).

>[!NOTE]
>Ninguna característica se ha dejado de incluir en [!INCLUDE[sssql14](../includes/sssql17-md.md)].

## <a name="discontinued-features-in-sssql15-md"></a>Características no incluidas en [!INCLUDE[sssql15-md](../includes/sssql16-md.md)]

- [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] es una aplicación de 64 bits. La instalación de 32 bits está descontinuada, a pesar de que algunos de los elementos se ejecutan como componentes de 32 bits.  

- El nivel de compatibilidad 90 se ha dejado de usar. Para obtener más información, vea [Nivel de compatibilidad de ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  

- No se incluye el subsistema Active X. En su lugar, use scripts de PowerShell o la línea de comandos.

- Parámetros de inicio **-h** y **-g**. Para más información, consulte [Opciones de inicio del servicio de motor de base de datos](/previous-versions/sql/2014/database-engine/configure-windows/database-engine-service-startup-options?view=sql-server-2014&preserve-view=true).

- Ya no se incluye el cifrado de Capa de sockets seguros (SSL). En su lugar use Seguridad de la capa de transporte (TLS). Para más información, vea [Habilitación de conexiones cifradas en el motor de base de datos](../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).

## <a name="previous-versions"></a>Versiones anteriores

- [Funcionalidad del motor de base de datos no incluida en SQL Server 2014](/previous-versions/sql/2014/database-engine/discontinued-database-engine-functionality-in-sql-server-2016?view=sql-server-2014&preserve-view=true)

### <a name="see-also"></a>Consulte también

- [Características en desuso del motor de base de datos de SQL Server 2019](deprecated-database-engine-features-in-sql-server-version-15.md)
- [Características en desuso del motor de base de datos de SQL Server 2017](deprecated-database-engine-features-in-sql-server-2017.md)
- [Características en desuso del motor de base de datos de SQL Server 2016](../database-engine/deprecated-database-engine-features-in-sql-server-2016.md)
- [Cambios importantes en las características del motor de base de datos de SQL Server 2019](breaking-changes-to-database-engine-features-in-sql-server-version-15.md)
- [Cambios importantes en las características del motor de base de datos de SQL Server 2017](breaking-changes-to-database-engine-features-in-sql-server-2017.md)
- [Cambios importantes en las características del motor de base de datos de SQL Server 2016](breaking-changes-to-database-engine-features-in-sql-server-2016.md)
- [Características en desuso en la replicación de SQL Server](../relational-databases/replication/deprecated-features-in-sql-server-replication.md)