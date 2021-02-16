---
title: Pausa y reanudación del reflejo de la base de datos
description: Aprenda a pausar y luego a reanudar una sesión de creación de reflejo de la base de datos de SQL Server para conservar el estado de sesión a la vez que se suspende la creación de reflejo.
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- sessions [SQL Server], database mirroring
- resuming database mirroring
- database mirroring [SQL Server], pausing
- database mirroring [SQL Server], resuming
- pausing database mirroring
ms.assetid: c67802c6-ee8c-4cbd-a6d4-f7b80413a4ab
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c565c8ca660c0616c061ba70f3c24d3a7d347191
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343732"
---
# <a name="pausing-and-resuming-database-mirroring-sql-server"></a>Pausar y reanudar la creación de reflejo de la base de datos (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  El propietario de la base de datos puede pausar y reanudar posteriormente una sesión de creación de reflejo de la base de datos en cualquier momento. La pausa preserva el estado de la sesión mientras se suspende la creación de reflejo. Durante los cuellos de botella, las pausas pueden ser útiles para mejorar el rendimiento en el servidor principal.  
  
 Cuando se realiza una pausa en una sesión, la base de datos principal sigue estando disponible. La pausa establece el estado de la sesión de creación de reflejo de la base de datos en SUSPENDED y la base de datos reflejada ya no se mantiene al día con la base de datos principal, lo que ocasiona que la base de datos principal se ejecute de una manera expuesta.  
  
 Se recomienda que reanude una sesión en pausa rápidamente, porque siempre que la sesión de creación de reflejo de la base de datos permanezca en pausa, no se puede truncar el registro de transacciones. Por tanto, si se realiza una pausa demasiado larga en la sesión de creación de reflejo de la base de datos, se llena el registro de transacciones, haciendo que la base de datos no esté disponible. Para obtener una explicación de por qué ocurre esto, vea "Cómo afectan la pausa y la reanudación al truncamiento del registro" a continuación.  
  
> [!IMPORTANT]  
>  Después de un servicio forzado, cuando el servidor principal original se conecta de nuevo se suspende la creación de reflejo. Reanudar la creación de reflejo en esta situación puede dar lugar a una pérdida de datos en el servidor principal original. Para obtener información acerca de la administración de la posible pérdida de datos, vea [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).  
  
 **En este tema:**  
  
-   [Cómo afectan la pausa y la reanudación al truncamiento del registro](#EffectOnLogTrunc)  
  
-   [Evitar un registro de transacciones lleno](#AvoidFullLog)  
  
-   [Tareas relacionadas](#RelatedTasks)  
  
##  <a name="how-pausing-and-resuming-affect-log-truncation"></a><a name="EffectOnLogTrunc"></a> Cómo afectan la pausa y la reanudación al truncamiento del registro  
 Normalmente, cuando se lleva a cabo un punto de comprobación automático en una base de datos, su registro de transacciones se trunca en dicho punto de comprobación después de la siguiente copia de seguridad del registro. Mientras que una sesión de creación de reflejo de la base de datos permanece en pausa, todos las entradas de registro actuales permanecen activas porque el servidor principal está esperando para enviarlos al servidor reflejado. Las entradas de registro no enviadas se acumulan en el registro de transacciones de la base de datos principal hasta que se reanuda la sesión y el servidor principal ha enviado las entradas de registro al servidor reflejado.  
  
 Cuando se reanuda la sesión, el servidor principal comienza a enviar inmediatamente las entradas de registro acumuladas al servidor reflejado. Después de que el servidor reflejado confirma que ha puesto en cola la entrada de registro correspondiente al punto de comprobación automático más antiguo, el servidor principal trunca el registro de la base de datos principal en dicho punto de comprobación. El servidor reflejado trunca la cola rehecha en la misma entrada de registro. Conforme este proceso se repite por cada punto de comprobación sucesivo, el registro se trunca por etapas, en cada punto de comprobación.  
  
> [!NOTE]  
>  Para obtener más información sobre los puntos de comprobación y el truncamiento del registro, vea [Puntos de comprobación de base de datos &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md).  
  
##  <a name="avoid-a-full-transaction-log"></a><a name="AvoidFullLog"></a> Evitar un registro de transacciones lleno  
 Si el registro se llena (bien porque alcanza su tamaño máximo o porque la instancia del servidor se queda sin espacio), la base de datos no puede realizar más actualizaciones. Hay dos alternativas para evitar este problema:  
  
-   Reanudar la sesión de creación de reflejo de la base de datos antes de que se llene el registro o agregar más espacio al registro. Reanudar la creación de reflejo de la base de datos permite al servidor principal enviar su registro activo acumulado al servidor reflejado y aplicar el estado SYNCHRONIZING a la base de datos reflejada. A continuación, el servidor reflejado puede reforzar el registro en el disco y comenzar a rehacerlo.  
  
-   Detener la sesión de creación de reflejo de la base de datos quitando la creación de reflejo.  
  
     A diferencia de la pausa de una sesión, al quitar la creación de reflejo se elimina toda la información de la sesión de creación de reflejo. Cada instancia de servidor asociado conserva su propia copia de la base de datos. Si se recupera la copia reflejada anterior, será diferente de la copia principal anterior y estará por detrás en la cantidad de tiempo transcurrido desde que se detuvo la sesión. Para obtener más información, vea [Quitar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
 **Para pausar o reanudar la creación de reflejo de la base de datos**  
  
-   [Pausar o reanudar una sesión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/pause-or-resume-a-database-mirroring-session-sql-server.md)  
  
 **Para detener la creación de reflejo de la base de datos**  
  
-   [Quitar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-database-mirroring-sql-server.md)  
  
## <a name="see-also"></a>Consulte también  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Quitar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md)  
  
  
