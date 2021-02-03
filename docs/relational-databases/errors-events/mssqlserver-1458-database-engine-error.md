---
description: MSSQLSERVER_1458
title: MSSQLSERVER_1458 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1458 (Database Engine error)
ms.assetid: adc78c59-a6f2-432b-9a07-fdd1dc2b9026
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: c9869ae430c0d1b669ced02202642e5eee7a7b58
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196992"
---
# <a name="mssqlserver_1458"></a>MSSQLSERVER_1458
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|1458|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|DBM_FAILREDO_ON_PRIMARY|  
|Texto del mensaje|La copia principal de la base de datos '%.*ls' encontró el error %d, el estado %d y la gravedad %d. Se suspendió la creación de reflejos de la base de datos. Intente resolver la condición de error y reanude la creación de reflejo.|  
  
## <a name="explanation"></a>Explicación  
Este mensaje indica que la base de datos principal encontró un error que ocasionó la suspensión de la creación de reflejo de la base de datos.  
  
## <a name="user-action"></a>Acción del usuario  
En la mayoría de los casos, este error se corrige automáticamente. Si el problema continúa, el reinicio de la instancia de la base de datos o del servidor normalmente corrige el problema. Para obtener más información, busque el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de cada asociado para el error asociado que precedía a este mensaje.  
  
## <a name="see-also"></a>Consulte también  
[Supervisar la creación de reflejo de la base de datos &#40;SQL Server&#41;](~/database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)  
  
