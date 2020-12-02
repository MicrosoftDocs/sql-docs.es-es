---
description: Notificar al operador, tarea
title: Tarea Notificar al operador | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.notifyoperatortask.f1
helpviewer_keywords:
- Notify Operator task
- notifications [Integration Services]
- SQL Server Agent [Integration Services]
ms.assetid: 6c816c68-c6d6-44e4-bb34-c8e060a958a1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d6bcdb94260a5fe1190e6f96ada085f004458c54
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "92194237"
---
# <a name="notify-operator-task"></a>Notificar al operador, tarea

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tarea Notificar al operador envía mensajes de notificación a los operadores del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Un operador del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es un alias para una persona o grupo que puede recibir notificaciones electrónicas. Para obtener más información sobre operadores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vea [Operadores](../../ssms/agent/operators.md).  
  
 Si usa la tarea Notificar al operador, un paquete puede notificar a uno o más operadores a través de correo electrónico, buscapersonas o **net send**. Es posible notificar a cada operador por distintos métodos. Por ejemplo, se notifica al Operador A por correo electrónico y mediante buscapersonas, y al Operador B mediante buscapersonas y **net send**. Los operadores que reciben notificaciones de la tarea deben ser miembros de la colección **OperatorNotify** en la tarea Notificar al operador.  
  
 La tarea Notificar al operador es la única tarea de mantenimiento de la base de datos que no encapsula una instrucción Transact-SQL o un comando DBCC.  
  
## <a name="configuration-of-the-notify-operator-task"></a>Configuración de la tarea Notificar al operador  
 Puede establecer propiedades a través del Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] . Esta tarea se encuentra en la sección **Tareas del plan de mantenimiento** del **Cuadro de herramientas** , en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 Para obtener más información acerca de las propiedades que puede establecer en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en el tema siguiente:  
  
-   [Tarea Notificar al operador &#40;Plan de mantenimiento&#41;](../../relational-databases/maintenance-plans/notify-operator-task-maintenance-plan.md)  
  
 Para obtener más información sobre cómo establecer estas propiedades en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en el siguiente tema:  
  
-   [Establecer las propiedades de tareas o contenedores](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
