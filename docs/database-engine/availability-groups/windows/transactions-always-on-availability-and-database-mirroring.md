---
description: Transacciones - Grupos de disponibilidad y creación de reflejo de la base de datos
title: 'Transacciones: grupos de disponibilidad y creación de reflejo de la base de datos'
descripton: Learn about cross-database and distributed transaction support for SQL Server Always On availability groups and database mirroring.
ms.custom: seo-lt-2019
ms.date: 12/11/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- cross-database transactions [SQL Server]
- transactions [database mirroring]
- Availability Groups [SQL Server], interoperability
- troubleshooting [SQL Server], cross-database transactions
ms.assetid: 9f7ed895-ad65-43e3-ba08-00d7bff1456d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 69f21f459db5e591b6934bd524515c7ce76cd3e5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348999"
---
# <a name="transactions---availability-groups-and-database-mirroring"></a>Transacciones - Grupos de disponibilidad y creación de reflejo de la base de datos
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

En este artículo se describe la compatibilidad de las transacciones entre bases de datos y distribuidas para la creación de reflejo de bases de datos y grupos de disponibilidad Always On.  

## <a name="support-for-distributed-transactions"></a>Compatibilidad con transacciones distribuidas

SQL Server 2017 admite las transacciones distribuidas de bases de datos en los grupos de disponibilidad. Esto engloba tanto las bases de datos que hay en la misma instancia de SQL Server como las bases de datos en distintas instancias de SQL Server. Las transacciones distribuidas no son posibles con bases de datos configuradas para la creación de reflejo de base de datos.

> [!NOTE]
> [!INCLUDE[SQL Server 2016](../../../includes/sssql16-md.md)] Service Pack 2 y versiones posteriores proporcionan compatibilidad total con las transacciones distribuidas en los grupos de disponibilidad. 
> 
> En las versiones de [!INCLUDE[SQL Server 2016](../../../includes/sssql16-md.md)] anteriores al Service Pack 2, las transacciones distribuidas entre bases de datos (es decir, las transacciones que usan bases de datos en la misma instancia de SQL Server) que implican una base de datos en un grupo de disponibilidad no son compatibles.

Para configurar un grupo de disponibilidad para transacciones distribuidas, vea [Configurar un grupo de disponibilidad para las transacciones distribuidas](configure-availability-group-for-distributed-transactions.md).

Obtenga más información en:

- [DTC Administration Guide](/previous-versions/windows/desktop/ms681291(v=vs.85)) (Guía de administración de DTC)
- [DTC Developers Guide](/previous-versions/windows/desktop/ms679938(v=vs.85)) (Guía de desarrolladores de DTC)
- [DTC Programmers Reference](/previous-versions/windows/desktop/ms686108(v=vs.85)) (Referencia de programadores de DTC)

## <a name="sql-server-2016-sp1-and-before-support-for-cross-database-transactions-within-the-same-sql-server-instance"></a>SQL Server 2016 SP1 y versiones anteriores: compatibilidad con transacciones entre bases de datos en la misma instancia de SQL Server  

En SQL Server 2016 SP1 y versiones anteriores, las transacciones entre bases de datos en la misma instancia de SQL Server no son compatibles con los grupos de disponibilidad. La misma instancia de SQL Server no puede hospedar dos bases de datos en una transacción entre bases de datos si una o ambas bases de datos están en un grupo de disponibilidad. Esta limitación también se aplica si esas bases de datos forman parte del mismo grupo de disponibilidad.  
  
Las transacciones entre bases de datos tampoco se admiten para la creación de reflejo de la base de datos.  
  
##  <a name="sql-server-2016-sp1-and-before-support-for-distributed-transactions"></a><a name="dtcsupport"></a> SQL Server 2016 SP1 y versiones anteriores: compatibilidad con transacciones distribuidas  
Las transacciones distribuidas son compatibles con los grupos de disponibilidad si las bases de datos se hospedan en instancias diferentes de SQL Server. También se aplica a las transacciones distribuidas entre las instancias de SQL Server y otro servidor compatible con DTC.  
 
Coordinador de transacciones distribuidas de Microsoft (MSDTC o DTC) es un servicio de Windows que proporciona infraestructura de transacciones para sistemas distribuidos. MSDTC permite que las aplicaciones cliente incluyan varios orígenes de datos en una transacción, que luego se confirma en todos los servidores incluidos en la transacción. Por ejemplo, puede usar MSDTC para coordinar transacciones que abarcan varias bases de datos en servidores diferentes.

SQL Server 2016 ofrece la posibilidad de usar transacciones distribuidas en las que una o varias de las bases de datos de la transacción se encuentran en un grupo de disponibilidad. Antes de SQL Server 2016, no se admitían las transacciones distribuidas para bases de datos de grupos de disponibilidad. SQL Server 2016 puede registrar un administrador de recursos por base de datos. Esta nueva funcionalidad es la razón por la que las transacciones distribuidas pueden incluir bases de datos de grupos de disponibilidad.
  
 Se deben cumplir los requisitos siguientes:  
  
-   Los grupos de disponibilidad deben ejecutarse en Windows Server 2012 R2 o en una versión posterior. Para Windows Server 2012 R2, debe instalar la actualización de KB3090973 disponible en [https://support.microsoft.com/kb/3090973](https://support.microsoft.com/kb/3090973).  
  
-   Los grupos de disponibilidad deben crearse con el comando **CREATE AVAILABILITY GROUP** y la cláusula **WITH DTC\_SUPPORT = PER_DB**. Actualmente no se puede modificar un grupo de disponibilidad existente.  

- Todas las instancias de SQL Server que participan en el grupo de disponibilidad deben ser de SQL Server 2016 o posterior.
 
 ## <a name="non-support-for-distributed-transactions"></a>Sin compatibilidad con las transacciones distribuidas
 Los casos concretos en que no se admiten transacciones distribuidas incluyen:
 
 - En SQL Server 2016 SP1 y versiones anteriores, si hay más de una base de datos implicada en la transacción en el mismo grupo de disponibilidad.
 
 - En SQL Server 2016 SP1 y versiones anteriores, si al menos una base de datos se encuentra en un grupo de disponibilidad y otra base de datos se encuentra en la misma instancia de SQL Server. 
 
 - Si el grupo de disponibilidad no se ha creado con la transacción distribuida habilitada.
 
 - Creación de reflejo de base de datos.
 
 > [!IMPORTANT]
 > Determine la salida predeterminada apropiada de las transacciones que DTC no puede resolver para el entorno.  Para información sobre cómo configurar la salida predeterminada, consulte [in-doubt xact resolution (opción de configuración del servidor)](../../../database-engine/configure-windows/in-doubt-xact-resolution-server-configuration-option.md).
  
## <a name="example-scenario-with-database-mirroring"></a>Escenario de ejemplo con la creación de reflejo de base de datos  
 En el siguiente ejemplo de creación de reflejo de la base de datos se muestra cómo podría producirse una incoherencia lógica. En este ejemplo, una aplicación utiliza una transacción entre bases de datos para insertar dos filas de datos: una fila se inserta en una tabla de una base de datos reflejada, A, y la otra fila se inserta en una tabla de otra base de datos, B. La base de datos A se está reflejando en modo de alta seguridad con conmutación automática por error. Mientras la transacción se confirma, la base de datos A deja de estar disponible y la sesión de creación de reflejo se conmuta por error automáticamente al reflejo de la base de datos A.  
  
 Después de la conmutación por error, la transacción entre bases de datos podría confirmarse correctamente en la base de datos B, pero no en la base de datos conmutada por error. Por ejemplo, si el servidor principal original de la base de datos A no hubiese enviado el registro de transacciones entre bases de datos al servidor reflejado antes del error. Después de la conmutación por error, esa transacción no existiría en el nuevo servidor principal. Las bases de datos A y B se volverían incoherentes porque los datos insertados en la base de datos B se mantienen intactos, pero los datos insertados en la base de datos A se pierden.  
  
 Su puede producir un escenario parecido cuando se usa una transacción de MS DTC. Por ejemplo, después de la conmutación por error, el nuevo servidor principal contacta con MS DTC. Sin embargo, MS DTC.no tiene conocimiento del nuevo servidor principal y termina las transacciones que están "preparadas para confirmar" y que otras bases de datos consideran confirmadas.  
  
> [!NOTE]  
>  No se puede usar la creación de reflejo de la base de datos con DTC ni usar los grupos de disponibilidad con DTC de formas no aprobadas en este artículo.  Esto no implica que los aspectos del producto no relacionados con DTC sean incompatibles; no obstante, no se admiten los problemas derivados del uso incorrecto de las transacciones distribuidas.  
  
## <a name="next-steps"></a>Pasos siguientes  
 [Grupos de disponibilidad Always On: interoperabilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)  
  
