---
description: Propiedades de Agente SQL Server (página Conexión)
title: Propiedades de Agente SQL Server (página Conexión)
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.agent.connection.f1
ms.assetid: d6a677ff-60ad-47ba-a0cb-df4193b165e0
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 42a7dc1230057e390de4a1afc5ed334fe7c2a655
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474376"
---
# <a name="sql-server-agent-properties-connection-page"></a>Propiedades de Agente SQL Server (página Conexión)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

Utilice esta página para ver y modificar la configuración de la conexión del servicio Agente [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="options"></a>Opciones  
**Servidor de host local del alias**  
Especifica el alias que debe utilizarse para conectarse a la instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si no puede utilizar las opciones de conexión predeterminadas del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , defina un alias para la instancia y especifíquelo aquí.  
  
**Utilizar autenticación de Windows**  
Establece la autenticación de [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows como método de autenticación que se utiliza para conectarse a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se conecta como la cuenta con la que se ejecuta el servicio Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
**Utilizar autenticación de SQL Server**  
Establece la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] como método de autenticación que se utiliza para conectarse a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se proporciona por motivos de compatibilidad con versiones anteriores. Para mejorar la seguridad, utilice la autenticación de Windows siempre que sea posible.  
  
**Inicio de sesión**  
Especifica el nombre de inicio de sesión que debe utilizarse para conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
**Contraseña**  
Especifica la contraseña que debe utilizarse para conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
