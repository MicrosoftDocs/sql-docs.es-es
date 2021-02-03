---
description: MSSQLSERVER_5242
title: MSSQLSERVER_5242 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5242 (Database Engine error)
ms.assetid: 712b1a10-2f87-41df-a111-1ed9f14102d4
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 58ac27fac4af3c8d343102403549758ea428d494
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198127"
---
# <a name="mssqlserver_5242"></a>MSSQLSERVER_5242
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|5242|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico||  
|Texto del mensaje|Se detectó una incoherencia durante una operación interna en la base de datos '%.*ls' (Id.:%d) en la página %S_PGID. Póngase en contacto con el servicio de soporte técnico. Número de referencia %ld.|  
  
## <a name="explanation"></a>Explicación  
Se ha producido una incoherencia estructural en la página de base de datos a la que se hace referencia en el mensaje de error.  
  
## <a name="see-also"></a>Consulte también  
[DBCC CHECKDB &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[Archivos y grupos de archivos de base de datos](~/relational-databases/databases/database-files-and-filegroups.md)  
  
