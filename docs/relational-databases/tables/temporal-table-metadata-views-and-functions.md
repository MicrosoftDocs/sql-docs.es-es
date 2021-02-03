---
description: Funciones y vistas de metadatos de la tabla temporal
title: Funciones y vistas de metadatos de la tabla temporal | Microsoft Docs
ms.custom: ''
ms.date: 03/28/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: e5d23ec9-7d18-40f6-add4-bea13132d0b9
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a8c13f9bcbbe501e369e2dd536c90d180bd88f6d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201160"
---
# <a name="temporal-table-metadata-views-and-functions"></a>Funciones y vistas de metadatos de la tabla temporal


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] y [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] incluyen una serie de funciones y vistas de metabase que permiten a los administradores recuperar información acerca de las tablas temporales.

La información acerca de las tablas temporales se expone en las siguientes vistas de metadatos:

- [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)
- [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)
- [sys.periods &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-periods-transact-sql.md)

 La información acerca de las tablas temporales se expone en las siguientes funciones de metadatos:

- [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)

- [OBJECTPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/objectpropertyex-transact-sql.md)

- [COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md)

## <a name="next-steps"></a>Pasos siguientes

- [Tablas temporales](../../relational-databases/tables/temporal-tables.md)
- [Introducción a las tablas temporales con versión del sistema](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [Comprobaciones de coherencia del sistema de la tabla temporal](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [Creación de particiones con tablas temporales](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [Limitaciones y consideraciones de las tablas temporales](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)
- [Seguridad de la tabla temporal](../../relational-databases/tables/temporal-table-security.md)
- [Administración de la retención de datos históricos en las tablas temporales con control de versiones del sistema](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [Tablas temporales con control de versiones del sistema con tablas con optimización para memoria](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
