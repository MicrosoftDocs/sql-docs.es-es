---
description: Configurar el Agente SQL Server para que use el Correo electrónico de base de datos
title: Configurar el Agente SQL Server para que use el Correo electrónico de base de datos
ms.date: 08/05/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- Database Mail [SQL Server], SQL Server Agent Mail
- SQL Server Agent Mail
ms.assetid: 4b8b61bd-4bd1-43cd-b6e5-c6ed2e101dce
author: stevestein
ms.author: sstein
ms.custom: seo-dt-2019
ms.openlocfilehash: d3740fd3c1e6030eda7428bc73a11f6c65fec68f
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250338"
---
# <a name="configure-sql-server-agent-mail-to-use-database-mail"></a>Configurar el Agente SQL Server para que use el Correo electrónico de base de datos
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo configurar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para usar Correo electrónico de base de datos a fin de enviar notificaciones y alertas en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  Para obtener información sobre cómo habilitar y configurar Correo electrónico de base de datos, vea [Configurar Correo electrónico de base de datos](../../relational-databases/database-mail/configure-database-mail.md).  Para un ejemplo de uso de [!INCLUDE[tsql](../../includes/tsql-md.md)], consulte [Crear un perfil de Correo electrónico de base de datos](../../relational-databases/database-mail/create-a-database-mail-profile.md).
  
-   **Antes de empezar:**  
  
-   [Requisitos previos](#Prerequisites)  
  
-   [Seguridad](#Security)  
  
-   [Para configurar el Agente SQL Server para que use el Correo electrónico de base de datos mediante Server Management Studio](#SSMSProcedure)  
  
-   [Tareas de seguimiento](#Follow_Up)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
  > [!NOTE]
  > El Agente SQL de Instancia administrada siempre se configura para usar Correo electrónico de base de datos, por lo que este contenido no es aplicable en Instancia administrada. En Instancia administrada necesita tener un perfil que se debe llamar **[AzureManagedInstance_dbmail_profile](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)** para enlazar el Agente SQL con Correo electrónico de base de datos. 
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   [Habilitar Correo electrónico de base de datos](../../relational-databases/database-mail/configure-database-mail.md).  
  
-    [Crear una cuenta de Correo electrónico de base de datos](../../relational-databases/database-mail/create-a-database-mail-account.md) para la cuenta de servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que desea usar.  
  
-   [Crear un perfil de Correo electrónico de base de datos](../../relational-databases/database-mail/create-a-database-mail-profile.md) para uso de la cuenta de servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y agregar el usuario al rol **DatabaseMailUserRole** de la base de datos **msdb** .
  
-   Establecer el perfil como perfil predeterminado de la base de datos **msdb** .  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 El usuario que crea cuentas de perfil y ejecuta procedimientos almacenados debe ser miembro del rol fijo de servidor sysadmin.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 **Para configurar el Agente SQL Server de modo que use el Correo electrónico de base de datos**  
  
-   En el Explorador de objetos, expanda una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Haga clic con el botón derecho en **Agente SQL Server** y, luego, haga clic en **Propiedades**.  
  
-   Haga clic en **Sistema de alerta**.  
  
-   Seleccione **Habilitar perfil de correo**.  
  
-   En la lista **Sistema de correo** , seleccione **Correo electrónico de base de datos**.  
  
-   En la lista **Perfil de correo**, seleccione un perfil de correo para el Correo electrónico de base de datos. 
  
-   Reinicie el Agente SQL Server.  
  
##  <a name="follow-up-tasks"></a><a name="Follow_Up"></a> Tareas de seguimiento  
 Las siguientes tareas son necesarias para completar la configuración del agente con el fin de que envíe alertas y notificaciones.  
  
-   [Alertas](../../ssms/agent/alerts.md)  
  
     Las alertas se pueden configurar para notificar a un operador de una determinada condición de evento o de sistema operativo de la base de datos.  
  
-   [Operadores](../../ssms/agent/operators.md)  
  
     Los operadores son alias de personas o grupos que pueden recibir la notificación electrónica  
  
