---
description: Supervisar y responder a eventos
title: Supervisar y responder a eventos
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- notifications [SQL Server], alert
- events [SQL Server], alerts
- alerts [SQL Server]
- notifications [SQL Server]
- events [SQL Server], automatically responding to
- administrator notifications [SQL Server Agent]
- automatic event responses
- SQL Server Agent alerts
- monitoring [SQL Server], events
- responding to events automatically
ms.assetid: f7fbe155-5b68-4777-bc71-a47637471f32
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 4618bcc2b35a258f24046b2d88ea407742e87b20
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477006"
---
# <a name="monitor-and-respond-to-events"></a>Supervisar y responder a eventos
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El Agente  puede supervisar y responder automáticamente a *eventos*, por ejemplo, mensajes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], condiciones de rendimiento específicas y eventos de Instrumental de administración de Windows (WMI).  
  
## <a name="in-this-section"></a>En esta sección  
[Alertas](../../ssms/agent/alerts.md)  
Contiene información acerca de la nomenclatura de alertas y la selección de los eventos o las condiciones de rendimiento a las que responden las alertas.  
  
[Crear un evento definido por el usuario](../../ssms/agent/create-a-user-defined-event.md)  
Contiene información acerca de cómo crear eventos distintos a los predefinidos por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
[Operadores](../../ssms/agent/operators.md)  
Contiene información acerca de la creación de alias para administradores que el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede utilizar para enviar notificaciones cuando los trabajos se realizan correctamente o generan un error.  
  
## <a name="about-monitoring-and-responding-to-events"></a>Acerca de cómo supervisar y responder a eventos  
Las respuestas automatizadas a los eventos se llaman *alertas*. Puede definir una alerta sobre uno o varios eventos para especificar cómo desea que el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] responda cuando aparezcan. Una alerta puede responder a un evento notificando a un administrador o ejecutando un trabajo, o ambos. Una alerta también puede reenviar un evento al registro de la aplicación de Microsoft Windows en otro equipo. Por ejemplo, puede especificar que se notifique inmediatamente a un operador si se produce un evento de gravedad 19. Si se definen alertas, los administradores de bases de datos pueden supervisar y administrar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]de forma más eficaz.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El Agente solo responde a los eventos para los que se ha definido una alerta. El método que utiliza el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para supervisar eventos depende del tipo de evento.  
  
Cuando se define una alerta del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para un contador de rendimiento, el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supervisa directamente el contador de rendimiento. Para un evento WMI, el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] registra una consulta de evento para el evento WMI.  
  
Para responder a mensajes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supervisa el registro de la aplicación Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El Agente solo puede responder a mensajes que aparecen en este registro. De manera predeterminada, SQL Server registra los siguientes mensajes en el registro de la aplicación de Windows:  
  
-   Errores sysmessages de gravedad 19 o superior.  
  
    Si también desea registrar errores sysmessages específicos que tengan una gravedad inferior a 19, utilice el procedimiento almacenado sp_altermessage para designar tales errores como siempre registrados.  
  
-   Instrucciones RAISERROR invocadas mediante la sintaxis WITH LOG.  
  
    Utilizar RAISERROR WITH LOG es la manera que se recomienda para escribir en el registro de la aplicación Windows desde una instancia de SQL Server.  
  
-   Cualquier evento de aplicación registrado mediante xp_logevent.  
  
    > [!NOTE]  
    > Registrar eventos de aplicación consume espacio de registro y puede provocar que el registro de la aplicación de Windows supere el tamaño máximo. Asegúrese de que el tamaño máximo del registro de la aplicación Windows es suficiente como para evitar la pérdida de información de eventos de SQL Server.  
  
Cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] registra un mensaje, el servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compara el mensaje con las alertas definidas por el administrador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Independientemente del origen del evento, el servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] responde al evento realizando las tareas especificadas en la alerta del evento.  
  
## <a name="see-also"></a>Consulte también  
[sp_altermessage](../../relational-databases/system-stored-procedures/sp-altermessage-transact-sql.md)  
