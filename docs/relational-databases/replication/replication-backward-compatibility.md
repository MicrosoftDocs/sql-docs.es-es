---
title: Compatibilidad con versiones anteriores de replicación | Microsoft Docs
description: Revise estos recursos para mantener la compatibilidad con versiones anteriores en la replicación antes de actualizar o si tiene varias versiones de SQL Server en una topología de replicación.
ms.custom: ''
ms.date: 03/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- transactional replication, backward compatibility
- backward compatibility [SQL Server replication]
- merge replication backward compatibility [SQL Server replication]
- replication [SQL Server], backward compatibility
- backward compatibility [SQL Server], replication
- snapshot replication [SQL Server], backward compatibility
- compatibility [SQL Server replication]
ms.assetid: 091c51dc-8b32-4b4f-847e-b317456c8394
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: a48ca05acd680cfd89f4438a64cac83c5d00dd0f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468826"
---
# <a name="replication-backward-compatibility"></a>Compatibilidad con versiones anteriores de replicación
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Es importante entender la compatibilidad con versiones anteriores si se va a realizar una actualización, o bien si tiene más de una versión de SQL Server en una topología de replicación. 

Las reglas generales son las siguientes: 

-   Un distribuidor puede ser de cualquier versión siempre que ésta sea mayor o igual que la versión del publicador (en muchos casos el distribuidor es la misma instancia que el publicador).    
-   Un publicador puede ser de cualquier versión siempre que ésta sea menor o igual que la versión del distribuidor.    
-   La versión del suscriptor depende del tipo de publicación:    
    - Un suscriptor de una publicación transaccional puede ser de cualquiera de las dos versiones del publicador. Por ejemplo: un publicador de SQL Server 2012 (11.x) puede tener suscriptores de SQL Server 2014 (12.x) y SQL Server 2016 (13.x); y un publicador de SQL Server 2016 (13.x) puede tener suscriptores de SQL Server 2014 (12.x) y SQL Server 2012 (11.x).     
    - En un suscriptor a una publicación de combinación todas las versiones pueden ser iguales o inferiores a la versión del publicador que se admite según el ciclo de soporte del ciclo de vida de las versiones.  


## <a name="replication-matrix"></a>Matriz de replicación
[!INCLUDE[repl matrix](../../includes/replication-compat-matrix.md)]


## <a name="additional-resources"></a>Recursos adicionales
 [Características que ya no se usan en la replicación de SQL Server](../../relational-databases/replication/deprecated-features-in-sql-server-replication.md)  
 En [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], se han conservado las características de replicación para la compatibilidad con versiones anteriores, pero se van a quitar en una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Principales cambios en la replicación de SQL Server](../../relational-databases/replication/breaking-changes-in-sql-server-replication.md)  
 Cambios en las características de replicación que pueden requerir cambios en las aplicaciones. 

 [Actualizar bases de datos replicadas](../../database-engine/install-windows/upgrade-replicated-databases.md)  
 Pasos y consideraciones al actualizar los servidores de SQL Server que participan en una topología de replicación. 
  
  
