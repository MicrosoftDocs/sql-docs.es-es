---
description: Perfiles de agente
title: Perfiles de agente | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.profiles.perfprofiles.f1
helpviewer_keywords:
- Agent Profiles dialog box
ms.assetid: 0422e99c-e688-419b-bb4c-c7bebeef1d8d
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 95bbfe95e957353f5bcf559038476a90f11167b0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467386"
---
# <a name="agent-profiles"></a>Perfiles de agente
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
   Utilice el cuadro de diálogo **Perfiles de agente** para administrar los perfiles de agente. Los perfiles de agente proporcionan una manera cómoda de administrar los parámetros en tiempo de ejecución para cada agente. Cada agente tiene un perfil predeterminado y algunos tienen perfiles adicionales predefinidos. Por ejemplo, el Agente de mezcla tiene un perfil de "vínculo lento" diseñado para conexiones de banda ancha lentas. Los perfiles predefinidos son suficientes para la mayoría de las aplicaciones, pero también pueden crearse perfiles definidos por el usuario, que permiten personalizar el comportamiento del agente.  
  
## <a name="options"></a>Opciones  
 **Seleccionar una página**  
 Seleccione un agente en el panel izquierdo y los perfiles del agente se mostrarán en el panel derecho.  
  
 **Predeterminado para nuevos**  
 Seleccione el perfil que se utilizará al crear los trabajos de un determinado tipo de agente. Por ejemplo, si crea varias suscripciones para una publicación de combinación, el trabajo del Agente de mezcla de cada suscripción utilizará el perfil seleccionado. Si desea cambiar el perfil de los trabajos existentes, seleccione un perfil y, a continuación, haga clic en **Cambiar agentes existentes**.  
  
 **Nombre**  
 Nombre del perfil.  
  
 **Tipo**  
 Indica el tipo de perfil: **Usuario** (definido por el usuario) o **Sistema** (predefinido).  
  
 **Propiedades (...)**  
 Haga clic para ver los valores utilizados por cada parámetro en el perfil del agente.  
  
 **Nuevo**  
 Haga clic para crear un perfil nuevo.  
  
 **Eliminar**  
 Seleccione un perfil definido por el usuario y, a continuación, haga clic en **Eliminar** para eliminar ese perfil. Los perfiles predefinidos no se pueden eliminar.  
  
 **Cambiar agentes existentes**  
 Seleccione un perfil y, a continuación, haga clic en **Cambiar agentes existentes** para especificar que todos los trabajos existentes de un determinado tipo de agente deben utilizar el perfil seleccionado. Por ejemplo, si ha creado varias suscripciones para una publicación de combinación y desea cambiar el perfil para especificar que el trabajo del Agente de mezcla de cada una de esas suscripciones debe utilizar el **Perfil de agente de conexión lenta**, seleccione ese perfil y, a continuación, haga clic en **Cambiar agentes existentes**.  
  
## <a name="see-also"></a>Consulte también  
 [Trabajar con perfiles del Agente de replicación](../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)   
 [Replication Agents Overview](../../relational-databases/replication/agents/replication-agents-overview.md)  (Información general sobre los agentes de replicación)  
 [Perfiles del Agente de replicación](../../relational-databases/replication/agents/replication-agent-profiles.md)  
  
  
