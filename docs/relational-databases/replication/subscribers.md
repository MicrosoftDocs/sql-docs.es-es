---
description: Suscriptores
title: Suscriptores | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newsubwizard.subscribers.f1
helpviewer_keywords:
- Subscribers [SQL Server replication]
ms.assetid: 43fb2454-c220-4d25-a826-83c332eb00d2
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: be8ccf0bbaf2efa80c9b0e48e39a0743d63b0c75
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479616"
---
# <a name="subscribers"></a>Suscriptores
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  Especifique los suscriptores de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o que no sean de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vayan a recibir una suscripción a la publicación seleccionada.


[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]
  
## <a name="options"></a>Opciones  
 **Suscriptores**  
 Active la casilla de la cuadrícula para habilitar el origen de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o que no sea de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] correspondiente como suscriptor a la publicación seleccionada en la página **Publicación**. Si el suscriptor no aparece, haga clic en **Agregar suscriptor** o **Agregar suscriptor de SQL Server**.  
  
 **Base de datos de suscripciones**  
 La información mostrada y las acciones disponibles en esta columna dependen del tipo de suscriptor que aparece en la columna **Suscriptores** :  
  
-   Para suscriptores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , seleccione una base de datos de suscripciones de la lista **Base de datos de suscripciones** o cree una nueva base de datos seleccionando el comando **Base de datos nueva** de la misma lista.  
  
    > [!NOTE]  
    >  Si va a habilitar el publicador como suscriptor, la base de datos de suscripciones debe ser diferente de la base de datos de publicaciones.  
  
-   Para suscriptores que no sean de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la base de datos de suscripciones no aparece. Especifique la base de datos, junto con otra información de conexión, en el campo **Nombre del origen de datos** del cuadro de diálogo **Agregar suscriptor que no sea de SQL Server** . Este cuadro de diálogo está disponible si hace clic en **Agregar suscriptor** y, a continuación, en **Agregar suscriptor que no sea de SQL Server**.  
  
 **Agregar suscriptor**  
 Agregue un servidor a la lista de servidores que pueda habilitarse como suscriptores. Este botón aparece cuando se cumplen todas las condiciones siguientes:  
  
-   La publicación que ha seleccionado es una publicación de instantáneas o transaccional que no admite la actualización de suscripciones.  
  
    > [!NOTE]  
    >  Si la publicación a la que se va a suscribir tiene suscripciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y no está habilitada todavía para suscriptores que no sean de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , no se puede agregar una suscripción que no sea de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   La suscripción es una suscripción de inserción.  
  
-   El publicador de la publicación seleccionada es [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o posterior.  
  
 Al hacer clic en **Agregar suscriptor** , aparece un menú con dos opciones: **Agregar suscriptor de SQL Server** y **Agregar suscriptor que no sea de SQL Server**. Haga clic en **Agregar suscriptor que no sea de SQL Server** para agregar un suscriptor de Oracle o IBM DB2.  
  
 **Agregar suscriptor de SQL Server**  
 Agregue un servidor a la lista de servidores que pueda habilitarse como suscriptores. Este botón aparece cuando se cumplen una o varias de las condiciones siguientes:  
  
-   La publicación que ha seleccionado es una publicación de combinación, o una publicación de instantáneas o transaccional que admite actualización de suscripciones.  
  
-   La suscripción es una suscripción de extracción.  
  
-   El publicador de la publicación seleccionada es anterior a [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Para versiones anteriores, este botón solamente aparece cuando se cumplen una o varias de las condiciones siguientes:  
  
    -   Es miembro del rol fijo de servidor **sysadmin** en el publicador.  
  
    -   Se ha agregado el suscriptor a la página **Suscriptores** del cuadro de diálogo **Propiedades del publicador** .  
  
    -   La publicación permite suscripciones anónimas.  
  
## <a name="see-also"></a>Consulte también  
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md)   
 [Suscribirse a publicaciones](../../relational-databases/replication/subscribe-to-publications.md)  
  
  
