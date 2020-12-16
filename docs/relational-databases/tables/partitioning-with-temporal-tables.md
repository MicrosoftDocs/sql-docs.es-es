---
description: Creación de particiones con tablas temporales
title: Creación de particiones con tablas temporales | Microsoft Docs
ms.custom: ''
ms.date: 04/26/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 313714b8-4ad1-4c14-93a3-7f628a334a51
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 83180bde505a156c5a6fd5832916e578fb73c52c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484457"
---
# <a name="partitioning-with-temporal-tables"></a>Creación de particiones con tablas temporales


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Puede usar la creación de particiones en la tabla actual y en la tabla de historial de forma independiente. Sin embargo, la creación de particiones no puede utilizarse para cambiar el contenido de los datos sin el control de versiones del sistema.

> [!NOTE]
> La creación de particiones es una característica de SQL Server 2016 Enterprise Edition anterior a Service Pack 1 y versiones anteriores. Esta característica se admite en todas las ediciones de SQL Server 2016 Service Pack 1 y en versiones posteriores.

- **Tabla actual:**

  - **SWITCH IN** a la tabla actual se puede usar para facilitar la carga y la consulta de datos mientras **SYSTEM_VERSIONING** está **ON**
  - **SWITCH OUT** no está permitido mientras **SYSTEM_VERSIONING** está **ON**

- **Tabla de historial:**

  - **SWITCH OUT** desde la tabla de historial se puede realizar mientras **SYSTEM_VERSIONING** está **ON** para purgar las partes de datos del historial que ya no sean de interés.
  - **SWITCH IN** no se permite mientras **SYSTEM_VERSIONING** está **ON** , puesto que puede invalidar la coherencia de los datos temporales.

## <a name="next-steps"></a>Pasos siguientes

- [Tablas temporales](../../relational-databases/tables/temporal-tables.md)
- [Introducción a las tablas temporales con versión del sistema](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [Comprobaciones de coherencia del sistema de la tabla temporal](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [Limitaciones y consideraciones de las tablas temporales](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)
- [Seguridad de la tabla temporal](../../relational-databases/tables/temporal-table-security.md)
- [Administración de la retención de datos históricos en las tablas temporales con control de versiones del sistema](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [Tablas temporales con control de versiones del sistema con tablas con optimización para memoria](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [Funciones y vistas de metadatos de la tabla temporal](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)
