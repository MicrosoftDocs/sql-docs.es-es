---
title: Replicación de SQL Server en Linux
description: Obtenga información sobre cómo SQL Server 2017 (14.x) (CU18) y versiones posteriores admiten Replicación de SQL Server para instancias de SQL Server en Linux.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 12/09/2019
ms.topic: article
ms.prod: sql
ms.prod_service: database-engine
ms.technology: linux
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: bb322f11b866a7593d22e0d722fad84765cb5139
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346453"
---
# <a name="sql-server-replication-on-linux"></a>Replicación de SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

[!INCLUDE[SQL Server 2017](../includes/sssql17-md.md)] ([CU18](https://support.microsoft.com/help/4527377)) y versiones posteriores admiten Replicación de SQL Server para instancias de SQL Server en Linux.

Configure la replicación en Linux con [procedimientos almacenados de replicación](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md) de SQL Server Management Studio (SSMS).

Una instancia de SQL Server puede participar en cualquier rol de replicación:

* Publicador
* Distribuidor.
* Suscriptor

Un esquema de replicación puede mezclar y hacer coincidir plataformas de sistema operativo. Por ejemplo, un esquema de replicación puede incluir una instancia de SQL Server en Linux para el publicador y el distribuidor, y los suscriptores incluir instancias de SQL Server en Windows y Linux.

Las instancias de SQL Server en Linux pueden participar en cualquier tipo de replicación.

* Transaccional
* Instantánea

Para obtener más información sobre la replicación, vea la [documentación sobre Replicación de SQL Server](../relational-databases/replication/sql-server-replication.md).

## <a name="supported-features"></a>Características admitidas

Se admiten las características de replicación siguientes:

* Replicación de instantáneas
* Replicación transaccional
* Replicación con puertos no predeterminados <!--Add link to explanation-->
* Replicación con autenticación de AD
* Configuraciones de replicación en Windows y Linux
* Actualizaciones inmediatas para la replicación transaccional

## <a name="limitations"></a>Limitaciones

No se admiten las siguientes características:

* Replicación de mezcla
* Replicación punto a punto
* Publicación de Oracle

## <a name="next-steps"></a>Pasos siguientes

[Configuración de la replicación de SQL Server en Linux](sql-server-linux-replication-tutorial-tsql.md)

[Ejemplo: Configuración de la replicación de SQL Server en Linux](sql-server-linux-replication-configure.md)
