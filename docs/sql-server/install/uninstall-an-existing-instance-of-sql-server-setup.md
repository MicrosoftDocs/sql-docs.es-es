---
title: Desinstalación de una instancia existente
description: En este artículo se describe cómo desinstalar una instancia independiente de SQL Server, que también prepara el sistema para que pueda volver a instalar SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- removing instances of SQL Server
- uninstalling instances of SQL Server
- removing SQL Server
- instances of SQL Server, uninstalling
- uninstalling SQL Server
ms.assetid: 3c64b29d-61d7-4b86-961c-0de62261c6a1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6a5b77c8e67d6512e691f748265e7901319a5e4f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336082"
---
# <a name="uninstall-an-existing-instance-of-sql-server-setup"></a>Desinstalar una instancia existente de SQL Server (programa de instalación)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En este artículo se describe cómo desinstalar una instancia independiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Siguiendo los pasos de este tema también podrá preparar el sistema para reinstalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 > [!NOTE]
 > Para desinstalar un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , utilice la funcionalidad Eliminar nodo proporcionada por el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para quitar cada nodo individualmente. Para obtener más información, vea [Agregar o quitar nodos en un clúster de conmutación por error de SQL Server &#40;programa de instalación&#41;](../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  

## <a name="considerations"></a>Consideraciones

- Para desinstalar SQL Server, debe ser administrador local con permisos para iniciar sesión como servicio. 
- Si el equipo tiene la cantidad *mínima* necesaria de memoria física, aumente el tamaño del archivo de paginación al doble de la cantidad de memoria física. Una cantidad insuficiente de memoria virtual puede dar como resultado una eliminación incompleta de SQL Server. 
- En un sistema con varias instancias de SQL Server, el servicio SQL Server Browser se desinstala solo cuando se quita la última instancia de SQL Server. El servicio SQL Server Browser se puede quitar manualmente en el **Panel de control** de **Programas y características**. 
- Al desinstalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se eliminan los archivos de datos tempdb que se agregaron durante el proceso de instalación. Se eliminan los archivos con el patrón de nombre tempdb_mssql_*.ndf si existen en el directorio de base de datos del sistema. 
  

  
## <a name="prepare"></a>Preparación  
  
1.  **Haga una copia de seguridad de los datos.** Cree [copias de seguridad completas](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md) de todas las bases de datos, incluidas las bases de datos del sistema, o copie manualmente los archivos .mdf y .ldf en una ubicación independiente. La base de datos **maestra** contiene toda la información de nivel de sistema del servidor, como los inicios de sesión y los esquemas. La base de datos **msdb** contiene información del trabajo, como los trabajos del agente SQL Server, el historial de copia de seguridad y los planes de mantenimiento. Consulte [Bases de datos del sistema](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md) para obtener más información sobre este tipo de bases de datos. 
  
    Entre los archivos que debe guardar se incluyen los siguientes archivos de base de datos:  

    * master.mdf
    * msdbdata.mdf
    * Tempdb.mdf
    * mastlog.ldf
    * msdblog.ldf
    * Templog.ldf
    * model.mdf
    * Mssqlsystemresource.mdf
    * ReportServer[$InstanceName]
    * modellog.ldf
    * Mssqlsustemresource.ldf
    * ReportServer[$InstanceName]TempDB

    > [!NOTE]
    > Las bases de datos de ReportServer se incluyen con SQL Server Reporting Services.   

 
1.  **Detenga todos** los **servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].** Se recomienda detener todos los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antes de desinstalar los componentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Las conexiones activas pueden evitar que la desinstalación se realice correctamente.  
  
1.  **Utilice una cuenta que tenga los permisos adecuados.** Inicie sesión en el servidor mediante la cuenta de servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o con una cuenta que tenga permisos equivalentes. Por ejemplo, puede iniciar sesión en el servidor mediante una cuenta miembro del grupo de administradores locales.  
  
## <a name="uninstall"></a>Desinstalación 

# <a name="windows-10--2016-"></a>[Windows 10/2016 +](#tab/Windows10)

Para desinstalar SQL Server de Windows 10, Windows Server 2016, Windows Server 2019 y versiones posteriores, siga estos pasos: 

1. Para empezar el proceso de eliminación, vaya a **Configuración** en el menú Inicio y luego elija **Aplicaciones**. 
1. Busque `sql` en el cuadro de búsqueda. 
1. Seleccione **Microsoft SQL Server (versión) (bit)** . Por ejemplo, `Microsoft SQL Server 2017 (64-bit)`.
1. Seleccione **Desinstalar**.
 
    ![Desinstalar SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/uninstall-sql-server-windows-10.png)

1. Seleccione **Quitar** en la ventana emergente del cuadro de diálogo SQL Server para iniciar el Asistente para la instalación de Microsoft SQL Server. 

    ![Eliminación de SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/remove-sql-2017.png)
  
1.  En la página **Seleccionar instancia**, use el cuadro desplegable para especificar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que quiere quitar, o especifique la opción para quitar solo las características compartidas y las herramientas de administración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para continuar, seleccione **Siguiente**.  
  
1.  En la página **Seleccionar características**, especifique las características que quiere quitar de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] especificada.  
  
1.  En la página **Listo para eliminar** , revise la lista de componentes y características que se van a desinstalar. Haga clic en **Quitar** para iniciar la desinstalación.  
 
1. Actualice la ventana **Aplicaciones y características** para comprobar que la instancia de SQL Server se ha quitado correctamente y determinar qué componentes de SQL Server todavía existen, si los hay. Quite estos componentes también de esta ventana, si lo prefiere. 

# <a name="windows-2008---2012-r2"></a>[Windows 2008 - 2012 R2](#tab/windows2012)

Para desinstalar SQL Server de Windows Server 2008, Windows Server 2012 y Windows 2012 R2, siga estos pasos: 

1. Para empezar el proceso de eliminación, vaya al **Panel de control** y luego seleccione **Programas y características**.
1. Haga clic con el botón derecho en **Microsoft SQL Server (versión) (bit)**  y seleccione **Desinstalar**. Por ejemplo, `Microsoft SQL Server 2012 (64-bit)`.  
  
    ![Desinstalar SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/uninstall-sql-server-windows-2012.png)

1. Seleccione **Quitar** en la ventana emergente del cuadro de diálogo SQL Server para iniciar el Asistente para la instalación de Microsoft SQL Server. 

    ![Eliminación de SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/remove-sql-2012.png)
  
1.  En la página **Seleccionar instancia**, use el cuadro desplegable para especificar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que quiere quitar, o especifique la opción para quitar solo las características compartidas y las herramientas de administración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para continuar, seleccione **Siguiente**.  
  
1.  En la página **Seleccionar características**, especifique las características que quiere quitar de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] especificada.  
  
1.  En la página **Listo para eliminar** , revise la lista de componentes y características que se van a desinstalar. Haga clic en **Quitar** para iniciar la desinstalación.  
 
1. Actualice la ventana **Programas y características** para comprobar que la instancia de SQL Server se ha quitado correctamente y determinar qué componentes de SQL Server todavía existen, si los hay. Quite estos componentes también de esta ventana, si lo prefiere. 

---

  
## <a name="in-the-event-of-failure"></a>En caso de error  

Si se produce un error en el proceso de eliminación, revise los [archivos de registro de instalación de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md) para determinar la causa raíz. 

El artículo de KB [Cómo identificar incidencias de instalación de SQL Server en los archivos de registro de instalación](https://support.microsoft.com/kb/955396/en-us) puede ayudarle en la investigación. Aunque se ha escrito para SQL Server 2008, la metodología descrita es aplicable a todas las versiones de SQL Server. 

  
## <a name="see-also"></a>Consulte también  
 [Ver y leer los archivos de registro de instalación de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
  
