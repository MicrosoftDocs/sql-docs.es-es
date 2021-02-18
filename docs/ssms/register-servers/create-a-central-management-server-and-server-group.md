---
description: Crear un servidor de administración central y un grupo de servidores
title: Creación de un servidor de administración central
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- configuration server
ms.assetid: da265482-3953-440a-ac23-0ab7e42a55eb
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 843411069a27d658cf9e2b157170d8ca0f14c91c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350554"
---
# <a name="create-a-central-management-server-and-server-group"></a>Crear un servidor de administración central y un grupo de servidores

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

En este tema se describe cómo designar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] como servidor de administración central de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Los servidores de administración central almacenan una lista de instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se organizan en uno o varios grupos de este tipo de servidores. Las acciones que se llevan a cabo mediante un servidor de administración central actúan en todos los servidores del grupo. Esto incluye la conexión a los servidores mediante el Explorador de objetos y la ejecución de instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)] y de directivas de administración basada en directivas en varios servidores al mismo tiempo.  
  
> [!NOTE]  
>  Las versiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anteriores a [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] no se pueden designar como servidores de administración central.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Seguridad](#Security)  
  
-   **Para crear un servidor de administración central y un grupo de servidores, usando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Dos roles de base de datos en la base de datos msdb conceden acceso a los servidores de administración central. Solo los miembros del rol ServerGroupAdministratorRole pueden administrar el servidor de administración central. La pertenencia al rol ServerGroupReaderRole es necesaria para conectarse a un servidor de administración central.  
  
 Dado que las conexiones que mantiene un servidor de administración central se ejecutan en el contexto del usuario, si se usara la autenticación de Windows los permisos vigentes en los servidores registrados podrían variar. Por ejemplo, el usuario podría ser miembro del rol fijo de servidor sysadmin en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] A, pero tener permisos limitados en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] B.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Los procedimientos siguientes describen cómo realizar los pasos siguientes.  
  
1.  Cree un servidor de administración central.  
  
2.  Agregue uno o varios grupos de servidores al servidor de administración central y uno o más servidores registrados a los grupos de servidores.  
  
#### <a name="create-a-central-management-server"></a>Crear un servidor de administración central  
  
1.  En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], en el menú **Ver** , haga clic en **Servidores registrados**.  
  
2.  En Servidores registrados, expanda **Motor de base de datos**, haga clic con el botón derecho en **Servidores de administración central** y, después, haga clic en **Registrar servidor de administración central**.  
  
3.  En el cuadro de diálogo **Nuevo registro de servidor** , seleccione la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que quiera que se convierta en el servidor de administración central en la lista desplegable de servidores. Utilice la autenticación de Windows para el servidor de administración central.  
  
4.  En **Servidor registrado**, escriba un nombre de servidor y una descripción opcional.  
  
5.  En la pestaña **Propiedades de conexión**, revise o modifique las propiedades de conexión y red. Para obtener más información, vea [Conectar al servidor &#40;página Propiedades de conexión del motor de base de datos&#41;](../f1-help/connect-to-server-connection-properties-page-database-engine.md)  
  
6.  Haga clic en **Probar** para probar la conexión.  
  
7.  Haga clic en **Guardar**. La instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aparecerá en la carpeta de **Servidores de administración central** .  
  
#### <a name="create-a-new-server-group-and-add-servers-to-the-group"></a>Crear un nuevo grupo de servidores y agregar servidores el grupo  
  
1.  En **Servidores registrados**, expanda **Servidores de administración central**. Haga clic con el botón derecho en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agregada en el procedimiento anterior y seleccione **Nuevo grupo de servidores**.  
  
2.  En **Propiedades de nuevo grupo de servidores**, escriba un nombre de grupo y una descripción opcional.  
  
3.  En **Servidores registrados**, haga clic con el botón derecho en el grupo de servidores y haga clic en **Nuevo registro de servidor**.  
  
4.  En Nuevo registro de servidor, seleccione una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [Crear un servidor registrado &#40;SQL Server Management Studio&#41;](./create-a-new-registered-server-sql-server-management-studio.md). Agregue más servidores según corresponda.  
  
#### <a name="to-execute-queries-against-several-configuration-targets-at-the-same-time"></a>Para ejecutar al mismo tiempo consultas con varios destinos de configuración  
  
-   Después de crear un servidor de administración central, uno o varios grupos de servidores, y uno o varios servidores registrados, puede ejecutar al mismo tiempo las consultas con un grupo entero. Para obtener más información sobre cómo ejecutar instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] en los servidores de un grupo de servidores al mismo tiempo, vea [Ejecutar instrucciones con varios servidores simultáneamente &#40;SQL Server Management Studio&#41;](./execute-statements-against-multiple-servers-simultaneously.md).  
  
## <a name="see-also"></a>Consulte también  
 [Administrar varios servidores mediante Servidores de administración central](../../relational-databases/administer-multiple-servers-using-central-management-servers.md)  
  
