---
description: Tareas específicas de programación
title: Tareas específicas de programación
ms.custom: seo-dt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- Visual Basic [SMO]
- Visual C# [SMO]
- programming [SMO]
- SQL Server Management Objects, programming
- SQL Server Management Objects, tasks
- SMO [SQL Server], programming
- SMO [SQL Server], tasks
ms.assetid: a15949ef-88d9-4205-892e-0b66588b4fcc
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b8415ab941364efce2f6a1b273e3eb48a1751b30
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475466"
---
# <a name="programming-specific-tasks"></a>Tareas específicas de programación
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  La programación de tareas específicas con objetos SMO incluye temas complejos que solo son necesarios en programas con una función concreta, como copia de seguridad, supervisión de estadísticas, replicación, administración de objetos de instancia y establecimiento de opciones de configuración.  
  
|Tema|Descripción|  
|-----------|-----------------|  
|[Utilizar servidores vinculados en SMO](../../../relational-databases/server-management-objects-smo/tasks/using-linked-servers-in-smo.md)|Describe cómo SMO utiliza el objeto <xref:Microsoft.SqlServer.Management.Smo.LinkedServer> para vincular servidores OLE-DB.|  
|[Configurar SQL Server en SMO](../../../relational-databases/server-management-objects-smo/tasks/configuring-sql-server-in-smo.md)|Describe cómo ver y modificar la configuración de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en SMO.|  
|[Utilizar particiones de tabla e índice](../../../relational-databases/server-management-objects-smo/tasks/using-table-and-index-partitioning.md)|Describe cómo utilizar particiones de índice y tabla en SMO.|  
|[Utilizar grupos de archivos y archivos para almacenar datos](../../../relational-databases/server-management-objects-smo/tasks/using-filegroups-and-files-to-store-data.md)|Describe cómo utilizar grupos de archivos en SMO.|  
|[Administrar servicios y configuración de red utilizando el proveedor WMI](../../../relational-databases/server-management-objects-smo/tasks/managing-services-and-network-settings-by-using-wmi-provider.md)|Describe varias maneras de realizar el seguimiento de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] mediante el objeto <xref:Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer> que representa el proveedor WMI de administración de configuración.|  
|[Trabajar con objetos de bases de datos](../../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-database-objects.md)|Describe cómo crear clases de instancia que representan objetos de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[Administrar usuarios, roles e inicios de sesión](../../../relational-databases/server-management-objects-smo/tasks/managing-users-roles-and-logins.md)|Describe cómo utilizar los roles de seguridad en SMO.|  
|[Conceder, revocar y denegar permisos](../../../relational-databases/server-management-objects-smo/tasks/granting-revoking-and-denying-permissions.md)|Describe cómo utilizar SMO para conceder, revocar y denegar permisos a usuarios o miembros de un rol.|  
|[Uso del cifrado](../../../relational-databases/server-management-objects-smo/tasks/using-encryption.md)|Describe cómo proteger los datos mediante el cifrado en SMO.|  
|[Programar tareas administrativas automáticas en el Agente SQL Server](../../../relational-databases/server-management-objects-smo/tasks/scheduling-automatic-administrative-tasks-in-sql-server-agent.md)|Describe cómo utilizar el Agente de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para supervisar, generar informes y programar trabajos en SMO.|  
|[Realizar copias de seguridad y restaurar bases de datos y registros de transacciones](../../../relational-databases/server-management-objects-smo/tasks/backing-up-and-restoring-databases-and-transaction-logs.md)|Describe cómo realizar copias de seguridad y restaurar bases de datos y registros de transacciones en SMO.|  
|[Scripting](../../../relational-databases/server-management-objects-smo/tasks/scripting.md)|Describe cómo crear script de objetos y detectar dependencias entre objetos en SMO.|  
|[Transferir datos](../../../relational-databases/server-management-objects-smo/tasks/transferring-data.md)|Describe cómo transferir datos en SMO.|  
|[Utilizar el correo electrónico de base de datos](../../../relational-databases/server-management-objects-smo/tasks/using-database-mail.md)|Describe cómo SMO utiliza los servicios de correo electrónico.|  
|[Administrar Service Broker](../../../relational-databases/server-management-objects-smo/tasks/managing-service-broker.md)|Describe cómo configurar Service Broker con SMO.|  
|[Utilizar esquemas XML](../../../relational-databases/server-management-objects-smo/tasks/using-xml-schemas.md)|Describe cómo utilizar el tipo de datos XML en SMO.|  
|[Usar sinónimos](../../../relational-databases/server-management-objects-smo/tasks/using-synonyms.md)|Describe cómo crear sinónimos en SMO.|  
|[Usar mensajes](../../../relational-databases/server-management-objects-smo/tasks/using-messages.md)|Describe cómo utilizar los mensajes del sistema y cómo definir sus propios mensajes definidos por el usuario.|  
|[Implementar la búsqueda de texto completo](../../../relational-databases/server-management-objects-smo/tasks/implementing-full-text-search.md)|Describe cómo implementar los catálogos e índices de búsqueda de texto completo en SMO.|  
|[Implementar extremos](../../../relational-databases/server-management-objects-smo/tasks/implementing-endpoints.md)|Describe cómo crear puntos finales para controlar las cargas útiles de creación de reflejos de base de datos, solicitudes SOAP y Service Broker.|  
|[Crear y actualizar estadísticas](../../../relational-databases/server-management-objects-smo/tasks/creating-and-updating-statistics.md)|Describe cómo configurar y supervisar las estadísticas de una base de datos en SMO.|  
|[Seguimiento y reproducción de eventos](../../../relational-databases/server-management-objects-smo/tasks/tracing-and-replaying-events.md)|Describe cómo usar los objetos **Trace** y **Replay** de SMO para realizar el seguimiento y reproducir eventos.|  
  
  
