---
description: MSSQLSERVER_17300
title: MSSQLSERVER_17300 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17300 (Database Engine error)
ms.assetid: c1d6bfb6-28af-4df6-8087-25807602d282
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: ef6de567f503cd836ee0ef4ed20d9aa601b57250
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196684"
---
# <a name="mssqlserver_17300"></a>MSSQLSERVER_17300
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |
| :-------- | :---- |
|Nombre de producto|SQL Server|  
|Id. de evento|17300|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|PROC_OUT_OF_SYSTASK_SESSIONS|  
|Texto del mensaje|SQL Server no pudo ejecutar una nueva tarea de sistema porque la memoria no es suficiente o porque el número de sesiones configuradas supera el máximo permitido en el servidor. Compruebe que el servidor tiene la memoria necesaria. Utilice sp_configure con la opción 'user connections' para comprobar el número máximo de conexiones de usuario permitido. Utilice sys.dm_exec_sessions para comprobar el número de sesiones actual, incluidos los procesos de usuario.|  
  
## <a name="explanation"></a>Explicación  
Se ha producido un error en un intento de ejecutar una nueva tarea de sistema porque la memoria no es suficiente o porque se ha superado el número de sesiones configuradas en el servidor.  
  
## <a name="user-action"></a>Acción del usuario  
Compruebe que el servidor tiene memoria suficiente. Compruebe el número actual de tareas de sistema utilizando sys.dm_exec_sessions, y compruebe el valor configurado de conexiones de usuario máximas utilizando sp_configure.  
  
Realice las siguientes tareas, según corresponda:  
  
-   Agregue más memoria al servidor.  
  
-   Finalice una o más sesiones.  
  
-   Aumente el número máximo de conexiones de usuario permitido en el servidor.  
  
## <a name="see-also"></a>Consulte también  
[sp_configure &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
[Opciones de configuración de servidor &#40;SQL Server&#41;](~/database-engine/configure-windows/server-configuration-options-sql-server.md)  
[sys.dm_exec_sessions &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
[Establecer la opción de configuración del servidor Conexiones de usuario](~/database-engine/configure-windows/configure-the-user-connections-server-configuration-option.md)  
[KILL &#40;Transact-SQL&#41;](~/t-sql/language-elements/kill-transact-sql.md)  
  
