---
title: Requisitos del rol de seguridad para la replicación | Microsoft Docs
description: Obtenga información sobre el nivel de autenticación necesario para las tareas comunes de configuración y mantenimiento de la replicación en SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- security [SQL Server replication], roles
- roles [SQL Server], replication
ms.assetid: b324a80f-4319-4cb2-847b-1910c49d90e0
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 4c715601699b689a41765633a079b9d6a99da8b5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475646"
---
# <a name="security-role-requirements-for-replication"></a>Security Role Requirements for Replication
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  La replicación restringe las acciones específicas que puede llevar a cabo un usuario, basándose en los roles a los que está asignado el nombre de inicio de sesión del usuario. La replicación ha concedido ciertos permisos al rol fijo de servidor **sysadmin** , al rol fijo de base de datos **db_owner** y a los inicios de sesión de la lista de acceso a la publicación (PAL).  
  
## <a name="security-role-requirements-for-replication-setup"></a>Requisitos del rol de seguridad para la configuración de la replicación  
 En la tabla siguiente se resume el nivel de autenticación necesario para las tareas comunes de configuración de la replicación:  
  
|Tarea de configuración|Requisito de pertenencia|  
|----------------|----------------------------|  
|Habilitar un distribuidor, publicador o suscriptor.|Rol de servidor **sysadmin** en el publicador.|  
|Habilitar una base de datos para replicación.|Rol de servidor **sysadmin** en el publicador.|  
|Crear una publicación.|Rol de base de datos **db_owner** en la base de datos de publicaciones del publicador o rol de servidor **sysadmin** en el publicador.|  
|Ver propiedades de publicación.|Miembro de la PAL en el publicador, rol de base de datos **db_owner** en la base de datos de publicaciones del publicador o rol de servidor **sysadmin** en el publicador.|  
|Crear una suscripción.|Rol de base de datos **db_owner** en la base de datos de publicaciones del publicador o rol de servidor **sysadmin** en el publicador.<br /><br /> Rol de base de datos **db_owner** en la base de datos de suscripciones del suscriptor o rol de servidor **sysadmin** en el suscriptor.|  
|Configurar perfiles de agente.|Rol de servidor **sysadmin** en el distribuidor.|  
  
## <a name="security-role-requirements-for-replication-maintenance"></a>Requisitos del rol de seguridad para el mantenimiento de la replicación  
 En la tabla siguiente se resume el nivel de autenticación necesario para las tareas comunes de mantenimiento de la replicación:  
  
|Tarea de mantenimiento|Requisito de pertenencia|  
|----------------------|----------------------------|  
|Modificar o quitar un distribuidor, publicador o suscriptor.|Rol de servidor **sysadmin** en el servidor apropiado.|  
|Modificar o quitar una publicación.|Rol de base de datos **db_owner** en la base de datos de publicaciones del publicador o rol de servidor **sysadmin** en el publicador.|  
|Modificar o quitar una suscripción en el publicador.|Rol de base de datos **db_owner** en la base de datos de publicaciones del publicador o rol de servidor **sysadmin** en el publicador.|  
|Modificar o quitar una suscripción en el suscriptor.|Rol de base de datos **db_owner** en la base de datos de suscripciones del suscriptor o rol de servidor **sysadmin** en el suscriptor.|  
|Marcar una suscripción para reinicializarla.|Suscripción de inserción: rol de base de datos **db_owner** en la base de datos de publicaciones del publicador o rol de servidor **sysadmin** en el publicador.<br /><br /> Suscripción de extracción: rol de base de datos **db_owner** en la base de datos de suscripciones del suscriptor o rol de servidor **sysadmin** en el suscriptor.|  
|Ver la actividad de replicación, los errores y el historial con el Monitor de replicación. Un usuario no puede modificar perfiles de agente, programaciones, etc., a menos que sea miembro del rol de servidor **sysadmin** .|Rol de base de datos **replmonitor** en la base de datos de distribución del distribuidor o rol de servidor **sysadmin** en el distribuidor.|  
|Mantener agentes de replicación.|Rol de base de datos **db_owner** en la base de datos correspondiente o rol de servidor **sysadmin** en el servidor apropiado.<br /><br /> Si un usuario creó el agente en el rol **sysadmin** y no especificó una cuenta de proxy para el agente, éste se ejecutará en el contexto de la cuenta del Agente [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . En este caso, un usuario con el rol **db_owner** no podrá modificar el trabajo asociado con el agente.|  
|Iniciar o detener un agente de replicación.|Propietario del trabajo de agente o rol de servidor **sysadmin** en el servidor apropiado.|  
  
## <a name="see-also"></a>Consulte también  
 [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Ver y modificar la configuración de seguridad de la replicación](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)  
  
  
