---
description: Registrar servidores
title: Registrar servidores
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.sqlserverregisteredserver.dhelp
helpviewer_keywords:
- connections [SQL Server], registered servers
- registering servers
- servers [SQL Server], registering
- server management [SQL Server], registering servers
- server registration [SQL Server]
ms.assetid: c2a2513e-fa09-419c-99e7-a12d57c5a0db
author: markingmyname
ms.author: maghan
ms.manageR: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 51c90ad84344c98ff0e144bd611ab243715e1787
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341059"
---
# <a name="register-servers"></a>Registrar servidores

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Registrar un servidor en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] permite almacenar la información de conexión del servidor para futuras conexiones. Hay tres formas de registrar un servidor en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
1.  Las instancias locales de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se registran automáticamente durante el primer inicio de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] después de su instalación.  
  
2.  También puede iniciar el proceso de registro automático en cualquier momento para restaurar el registro de las instancias de los servidores locales.  
  
3.  Por último, puede registrar un servidor mediante la herramienta Servidores registrados de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="benefits-of-registered-servers"></a>Ventajas de los servidores registrados  
 Con Servidores registrados puede:  
  
-   Registrar servidores para conservar la información de conexión.  
  
-   Determinar si un servidor registrado está funcionando.  
  
-   Conectar fácilmente el Explorador de objetos y el Editor de consultas a un servidor registrado.  
  
-   Modificar o eliminar la información de registro de un servidor registrado.  
  
-   Crear grupos de servidores.  
  
-   Proporcionar nombres descriptivos a los servidores registrados escribiendo en el cuadro **Nombre del servidor registrado** un valor distinto al de la lista **Nombre del servidor** .  
  
-   Proporcionar descripciones detalladas de los servidores registrados.  
  
-   Proporcionar descripciones detalladas de los grupos de servidores registrados.  
  
-   Exportar grupos de servidores registrados.  
  
-   Importar grupos de servidores registrados.  
  
-   Vea los archivos de registro de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de las instancias con conexión o sin conexión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="related-tasks"></a>Related Tasks  
 Use los temas siguientes para empezar a trabajar con servidores registrados:  
  
|**Descripción**|**Tema.**|  
|---------------------|---------------|  
|Registrar instancias de servidores locales|[Registrar un servidor conectado &#40;SQL Server Management Studio&#41;](./register-a-connected-server-sql-server-management-studio.md)|  
|Registrar un servidor|[Crear un servidor registrado &#40;SQL Server Management Studio&#41;](./create-a-new-registered-server-sql-server-management-studio.md)|  
|Ver servidores registrados|[Ver los servidores registrados en SQL Server Management Studio](./view-registered-servers-in-sql-server-management-studio.md)|  
|Quitar un servidor registrado|[Quitar un servidor registrado &#40;SQL Server Management Studio&#41;](./remove-a-registered-server-sql-server-management-studio.md)|  
|Cambiar el registro de un servidor|[Cambiar el registro de un servidor &#40;SQL Server Management Studio&#41;](./change-a-server-s-registration-sql-server-management-studio.md)|  
|Conectarse a un servidor registrado|[Conectarse a un servidor registrado &#40;SQL Server Management Studio&#41;](./connect-to-a-registered-server-sql-server-management-studio.md)|  
|Desconectarse de un servidor registrado|[Desconectarse de un servidor registrado &#40;SQL Server Management Studio&#41;](./disconnect-from-a-registered-server-sql-server-management-studio.md)|  
|Mover un servidor o un grupo de servidores registrados|[Mover un servidor registrado o un grupo de servidores registrados &#40;SQL Server Management Studio&#41;](./move-a-registered-server-or-registered-server-group.md)|  
|Cambiar el nombre de un servidor o un grupo de servidores registrados|[Cambiar el nombre de un servidor registrado o un grupo de servidores registrados &#40;SQL Server Management Studio&#41;](./change-the-name-of-registered-server-or-registered-server-group.md)|  
|Crear o editar un grupo de servidores|[Crear o editar un grupo de servidores &#40;SQL Server Management Studio&#41;](./create-or-edit-a-server-group-sql-server-management-studio.md)|  
|Quitar un grupo de servidores|[Quitar un grupo de servidores &#40;SQL Server Management Studio&#41;](./remove-a-server-group-sql-server-management-studio.md)|  
|Exportar información de servidores registrados|[Exportar información de servidores registrados &#40;SQL Server Management Studio&#41;](./export-registered-server-information-sql-server-management-studio.md)|  
|Importar información de servidores registrados|[Importar información de servidores registrados &#40;SQL Server Management Studio&#41;](./import-registered-server-information-sql-server-management-studio.md)|  
|Crear un servidor de administración central y un grupo de servidores|[Crear un servidor de administración central y un grupo de servidores &#40;SQL Server Management Studio&#41;](./create-a-central-management-server-and-server-group.md)|  
|Ejecutar instrucciones en varios servidores simultáneamente|[Ejecutar instrucciones con varios servidores simultáneamente &#40;SQL Server Management Studio&#41;](./execute-statements-against-multiple-servers-simultaneously.md)|  
  
## <a name="see-also"></a>Vea también  
 [Servidores remotos](../../database-engine/configure-windows/remote-servers.md)  
  
