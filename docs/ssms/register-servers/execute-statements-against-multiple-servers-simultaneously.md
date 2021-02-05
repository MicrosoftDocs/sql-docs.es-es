---
description: Ejecutar instrucciones en varios servidores simultáneamente
title: Ejecutar instrucciones en varios servidores simultáneamente
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- multiserver queries
- executing queries against multiple servers
- queries [SQL Server], multiserver
ms.assetid: 197760f3-0a06-43de-8162-69c27d3fbe56
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 07/18/2016
ms.openlocfilehash: 1c5de69d2a4167d03dcc5ea5c34ed6f0262c4678
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250446"
---
# <a name="execute-statements-against-multiple-servers-simultaneously"></a>Ejecutar instrucciones en varios servidores simultáneamente

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

En este tema se describe cómo consultar al mismo tiempo varios servidores en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]creando un grupo de servidores locales o un Servidor de administración central y uno o varios grupos de servidores, y uno o varios servidores registrados dentro de los grupos, y consultando a continuación el grupo completo. 

Los resultados que devuelve la consulta se pueden combinar en un único panel de resultados o en paneles de resultados independientes. El conjunto de resultados puede incluir columnas adicionales para el nombre de servidor y el inicio de sesión que usa la consulta en cada servidor. Los Servidores de administración central y los servidores secundarios se pueden registrar únicamente utilizando la autenticación de Windows. Los servidores de los grupos de servidores locales se pueden registrar con la autenticación de Windows o con la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> **NOTA:** Antes de ejecutar los procedimientos siguientes, cree un Servidor de administración central y un grupo de servidores. Para obtener más información, vea [Crear un servidor de administración central y un grupo de servidores &#40;SQL Server Management Studio&#41;](./create-a-central-management-server-and-server-group.md).  

  
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Dado que las conexiones que mantiene un Servidor de administración central se ejecutan en el contexto del usuario, si se usara la autenticación de Windows, los permisos vigentes en los servidores registrados podrían variar. Por ejemplo, el usuario podría ser miembro del rol fijo de servidor sysadmin en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] A, pero tener permisos limitados en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] B.  
  
 ## <a name="execute-statements-against-multiple-configuration-targets-simultaneously"></a>Ejecutar instrucciones con varios destinos de configuración simultáneamente  

1.  En SQL Server Management Studio, en el menú **Vista** , haga clic en **Servidores registrados**.  
  
2.  Expanda un Servidor de administración central, haga clic con el botón derecho en un grupo de servidores, seleccione **Conectar** y haga clic en **Nueva consulta**.  
  
3.  En el Editor de consultas, escriba y ejecute una instrucción de [!INCLUDE[tsql](../../includes/tsql-md.md)] , como la siguiente:  
  
    ```  
    USE master  
    GO  
    SELECT * FROM sysdatabases;  
    GO  
    ```  
  
     De forma predeterminada, el panel de resultados combinará los resultados de la consulta de todos los servidores del grupo de servidores.  
  
#### <a name="to-change-the-multiserver-results-options"></a>Para cambiar las opciones de los resultados multiservidor  
  
1.  En [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], en el menú **Herramientas** , haga clic en **Opciones**.  
  
2.  Expanda **Resultados de la consulta**, expanda **SQL Server** y, a continuación, haga clic en **Resultados multiservidor**.  
  
3.  En la página **Resultados multiservidor** , especifique la configuración de las opciones que desee y, a continuación, haga clic en **Aceptar**.  
  
## <a name="see-also"></a>Consulte también  
 [Administrar varios servidores mediante Servidores de administración central](../../relational-databases/administer-multiple-servers-using-central-management-servers.md)  
  
