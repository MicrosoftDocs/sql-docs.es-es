---
description: MSSQL_ENG020596
title: MSSQL_ENG020596 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG020596 error
ms.assetid: fa33627c-2e99-4be3-9424-52ab83446e07
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: e5762cff5b74df960ed366d6058fc43f857bacd3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473356"
---
# <a name="mssql_eng020596"></a>MSSQL_ENG020596
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Detalles del mensaje  
  
|Atributo|Value|  
|-|-|  
|Nombre de producto|SQL Server|  
|Id. de evento|20596|  
|Origen de eventos|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nombre simbólico||  
|Texto del mensaje|Solo '%1!' o los miembros de db_owner pueden quitar el agente anónimo.|  
  
## <a name="explanation"></a>Explicación  
 No tiene suficientes permisos para quitar el agente de la suscripción anónima. El inicio de sesión usado al llamar a [sp_dropanonymousagent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropanonymousagent-transact-sql.md) debe ser un miembro del rol fijo de servidor **sysadmin** del distribuidor o del rol fijo de base de datos **db_owner** de la base de datos de distribución, o bien el usuario debe ser el que inició la primera ejecución del agente.  
  
## <a name="user-action"></a>Acción del usuario  
 Inicie sesión con las credenciales correctas y ejecute **sp_dropanonymousagent**.  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de errores y eventos &#40;replicación&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
