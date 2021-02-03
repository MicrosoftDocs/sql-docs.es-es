---
description: MSSQLSERVER_3413
title: MSSQLSERVER_3413 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3413 (Database Engine error)
ms.assetid: 3fa07637-ba93-4633-aaf2-ade7d18bc487
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 14420a895991a4ea9ed1cc7dc259ac16ad55dead
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197766"
---
# <a name="mssqlserver_3413"></a>MSSQLSERVER_3413
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|3413|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|MARKDB|  
|Texto del mensaje|Id. de base de datos %d. No se pudo marcar la base de datos como sospechosa. Error del recorrido con Getnext NC en sys.databases.database_id. Consulte los errores anteriores del registro de errores para identificar la causa y corregir cualquier problema relacionado.|  
  
## <a name="explanation"></a>Explicación  
Se produjo un error inesperado al intentar marcar una base de datos de usuario como SUSPECT en el catálogo. El estado SUSPECT no será persistente.  
  
## <a name="user-action"></a>Acción del usuario  
Vea los errores anteriores y solucione el problema.  
  
