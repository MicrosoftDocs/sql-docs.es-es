---
title: Servicio de creación de reflejo de base de datos forzado
description: Si se produce un error en el servidor principal mientras el servidor reflejado está disponible, para hacer que la base de datos esté disponible se debe forzar al servicio a realizar la conmutación por error a la base de datos reflejada.
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- forced service [SQL Server]
- database mirroring [SQL Server], forcing service
ms.assetid: 8b6ffe77-35f3-4e2a-a658-8a38a8e1c794
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 401865dd09df7c14c0101629896abc6eb958f994
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342337"
---
# <a name="force-service-in-a-database-mirroring-session-transact-sql"></a>Forzar el servicio en una sesión de creación de reflejo de la base de datos (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En los modos de alto rendimiento y de alta seguridad sin conmutación automática por error, si se produce un error en el servidor principal mientras el servidor reflejado está disponible, el propietario de la base de datos puede hacer que ésta esté disponible forzando la conmutación por error del servicio (con una posible pérdida de datos) para la base de datos reflejada. Esta opción está disponible solamente si se cumplen todas las condiciones siguientes:  
  
-   El servidor principal está inactivo.  
  
-   WITNESS está establecido en OFF o está conectado al servidor reflejado.  
  
> [!CAUTION]  
>  El servicio forzado es estrictamente un método de recuperación de desastres. Si se fuerza el servicio, pueden perderse datos. Por consiguiente, fuerce el servicio solo si es aceptable el riesgo de perder datos para restaurar el servicio en la base de datos inmediatamente. Si forzar el servicio supone una posible pérdida de un gran volumen de datos, se recomienda detener la creación del reflejo y sincronizar manualmente las bases de datos. Para obtener más información acerca de los riesgos que supone forzar el servicio, vea [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).  
  
 Al forzar el servicio se suspende la sesión y se inicia un nuevo punto de bifurcación de recuperación. El efecto de forzar el servicio es parecido al de quitar la creación del reflejo y recuperar la base de datos principal anterior. No obstante, forzar el servicio facilita la resincronización de las bases de datos (con posible pérdida de datos) cuando se reanuda la creación del reflejo.  
  
### <a name="to-force-service-in-a-database-mirroring-session"></a>Para forzar el servicio en una sesión de creación de reflejo de la base de datos  
  
1.  Conéctese al servidor reflejado.  
  
2.  Emita la instrucción siguiente:  
  
     ALTER DATABASE *<database_name>* SET PARTNER FORCE_SERVICE_ALLOW_DATA_LOSS  
  
     Donde *<database_name>* es la base de datos reflejada.  
  
     El servidor reflejado pasa inmediatamente al servidor principal y la creación del reflejo se suspende.  
  
## <a name="see-also"></a>Consulte también  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)  
  
  
