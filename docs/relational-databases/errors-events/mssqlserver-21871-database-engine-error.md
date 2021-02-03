---
description: MSSQLSERVER_21871
title: MSSQLSERVER_21871 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 21871 (Database Engine error)
ms.assetid: d3215378-9282-444f-a18b-00b96fd0133d
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1085b9ad233df60e39dca019bd9cf7b1e424942f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196420"
---
# <a name="mssqlserver_21871"></a>MSSQLSERVER_21871
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|21871|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|SQLErrorNum21871|  
|Texto del mensaje|El publicador %s de la base de datos %s no ha sido redirigido.|  
  
## <a name="explanation"></a>Explicación  
**sp_validate_replica_hosts_as_publishers** busca una entrada para el publicador identificado y la base de datos del publicador en la tabla MSredirected_publishers de la base de datos de distribución.  **sp_validate_replica_hosts_as_publishers** devuelve el error 21871 si no se encuentra ninguna entrada.  
  
## <a name="user-action"></a>Acción del usuario  
**sp_validate_replica_hosts_as_publishers** solo es pertinente para los publicadores redirigidos. Si la base de datos del publicador es miembro de un grupo de disponibilidad, use el procedimiento almacenado **sp_redirect_publisher** para asociar el publicador y la base de datos del publicador con el nombre de escucha de grupo de disponibilidad del grupo de disponibilidad.  
  
