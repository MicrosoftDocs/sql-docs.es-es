---
description: MSSQLSERVER_21892
title: MSSQLSERVER_21892 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 21892 (Database Engine error)
ms.assetid: 199818e8-28f4-440e-ad85-7bea88f51153
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 56ba58d0166913aab8c789884eb98f1d6ce7fbb6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196366"
---
# <a name="mssqlserver_21892"></a>MSSQLSERVER_21892
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|21892|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|SQLErrorNum21892|  
|Texto del mensaje|No se puede consultar sys.availability_replicas en el principal de grupo de disponibilidad asociado con el nombre de red virtual "%s" para los nombres de servidor de las réplicas de miembro: error = %d, mensaje de error = %s".|  
  
## <a name="explanation"></a>Explicación  
**sp_validate_replica_hosts_as_publishers** consulta el principal actual del grupo de disponibilidad asociado al publicador redireccionado para determinar las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que hospedan las réplicas de miembro.  Cuando se produce un error en esta consulta, se devuelve el error 21892.  
  
**sp_validate_replica_hosts_as_publishers** se encuentra normalmente en el primer uso del servidor vinculado temporal por lo que, si hay problemas de conectividad, probablemente aparecerán primero con **sp_validate_replica_hosts_as_publishers**. A diferencia de **sp_validate_redirected_publisher**, el servidor vinculado que usa **sp_validate_replica_hosts_as_publishers** siempre emplea las credenciales del autor de la llamada al conectarse a cualquiera de los hosts de la réplica de grupo de disponibilidad.  
  
## <a name="user-action"></a>Acción del usuario  
Al ejecutar este procedimiento almacenado, asegúrese de que se ejecutar desde un inicio de sesión que sea válido en todas las réplicas. El inicio de sesión requiere autorización suficiente para consultar las tablas de metadatos del grupo de disponibilidad, así como para consultar las tablas de metadatos de suscripción en la réplica de base de datos del publicador.  
  
Examine el error de referencia original para determinar la causa del error y llevar a cabo la acción correctiva adecuada.  
  
