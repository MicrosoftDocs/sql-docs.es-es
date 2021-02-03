---
description: MSSQLSERVER_5235
title: MSSQLSERVER_5235 | Microsoft Docs
ms.custom: ''
ms.date: 09/05/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5235 (Database Engine error)
ms.assetid: 1aa7e6a5-7ccb-43c8-a1fd-d50e92e0a798
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1a81590c727a34dfe272a03ec6319f487c02e9f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198141"
---
# <a name="mssqlserver_5235"></a>MSSQLSERVER_5235
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|5235|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|DBCC4_ERRORLOG_SUMMARY_PREMATURE_TERMINATION|  
|Texto del mensaje|[EMERGENCY] DBCC DBCC_COMMAND_DETAILS ejecutado por USER_NAME finalizó de forma anómala debido a un estado de error ERROR_STATE. Tiempo transcurrido: HOURS horas MINUTES minutos SECONDS segundos.|  
  
## <a name="explanation"></a>Explicación  
Este es el mensaje de resumen que DBCC imprime en el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuando se produce una finalización inesperada mientras se ejecuta el comando. El estado de error notificado en el mensaje define el tipo de finalización inesperada.  
  
En la siguiente tabla se muestran y se definen los estados de error.  
  
|Estado de error|Definición|  
|---------------|--------------|  
|State 1|La instrucción finalizó debido a un daño grave de los metadatos. Este mensaje va acompañado de una o varias instancias de error 8930.|  
|State 2|La instrucción finalizó debido a un error interno de comprobación. Este mensaje va acompañado de una o varias instancias de error 8967.|  
|State 3|Error en las comprobaciones básicas de las tablas del sistema del motor de almacenamiento principal. Este mensaje va acompañado de una o varias instancias de error [7984](../../relational-databases/errors-events/mssqlserver-7984-database-engine-error.md), 7985, [7986](~/relational-databases/errors-events/mssqlserver-7986-database-engine-error.md), [7987](~/relational-databases/errors-events/mssqlserver-7987-database-engine-error.md) o [7988](~/relational-databases/errors-events/mssqlserver-7988-database-engine-error.md).|  
|State 4|Error en la reparación del modo de emergencia DBCC debido a que la base de datos no pudo iniciarse después de volver a generar el registro de transacciones. Este mensaje va acompañado del error 7909.|  
|Estado 5|Se produjo un error de aserción o infracción de acceso durante la ejecución del comando.|  
|Estado 6|Se produjo un error desconocido que canceló inesperadamente el comando DBCC.|  
|Estado 7|Una finalización anormal debido al error en la réplica (AlwaysOn).|  
  
## <a name="user-action"></a>Acción del usuario  
En la siguiente tabla se muestra la acción apropiada que debe llevar a cabo el usuario para el estado de error especificado.  
  
|Estado de error|Acción del usuario|  
|---------------|---------------|  
|State 1|Restaure mediante la copia de seguridad.|  
|State 2|Póngase en contacto con el Servicio de atención al cliente y soporte técnico (CSS) de [!INCLUDE[msCoName](../../includes/msconame-md.md)].|  
|State 3|Restaure mediante la copia de seguridad.|  
|State 4|Restaure mediante la copia de seguridad.|  
|State 4|Póngase en contacto con el CSS.|  
|Estado 6|Vuelva a ejecutar el comando. Si el problema persiste, póngase en contacto con el CSS.|  
  
## <a name="see-also"></a>Consulte también  
[DBCC &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-transact-sql.md)  
  
