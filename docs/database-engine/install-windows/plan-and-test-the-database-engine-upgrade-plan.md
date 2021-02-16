---
title: Planear y probar el plan de actualización del Motor de base de datos | Microsoft Docs
description: En este artículo se describe el planeamiento necesario para comenzar la actualización de SQL Server, que incluye una lista de comprobación de planeamiento y el desarrollo y las pruebas de un plan de actualización.
ms.custom: ''
ms.date: 05/18/2018
ms.prod: sql
ms.technology: install
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 19c5b725-7400-4881-af8f-fd232ca28234
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 7a6311f79aa87d93ee980cf4d0c8d00352b14687
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353057"
---
# <a name="plan-and-test-the-database-engine-upgrade-plan"></a>Planear y probar el plan de actualización del Motor de base de datos

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
 Para realizar una actualización a [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] correcta, se precisa una planeación adecuada, con independencia del enfoque.  
  
## <a name="release-notes-and-known-upgrade-issues"></a>Notas de la versión y problemas conocidos de la actualización  
 Antes de actualizar el [!INCLUDE[ssDE](../../includes/ssde-md.md)], revise lo siguiente:

- [Notas de la versión de SQL Server 2019](../../sql-server/sql-server-version-15-release-notes.md)
- [Notas de la versión de SQL Server 2017](../../sql-server/sql-server-2017-release-notes.md) 
- [Notas de la versión de SQL Server 2016](../../sql-server/sql-server-2016-release-notes.md)
- Artículo [Compatibilidad con versiones anteriores del motor de base de datos de SQL Server](../discontinued-database-engine-functionality-in-sql-server.md).  
  
## <a name="pre-upgrade-planning-checklist"></a>Lista de comprobación previa a la planificación de la actualización  
 Antes de actualizar [!INCLUDE[ssDE](../../includes/ssde-md.md)], revise esta lista de comprobación y los artículos relacionados. Estos artículos se aplican a todas las actualizaciones, independientemente del método de actualización, y con ellos puede determinar cuál es la actualización más adecuada: local, gradual o una nueva instalación. Por ejemplo, es posible que no pueda realizar una actualización local o gradual si va a actualizar el sistema operativo, o bien si actualiza desde SQL Server 2005 o desde una versión de 32 bits de SQL Server. Para ver un árbol de decisión, consulte [Choose a Database Engine Upgrade Method](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md).  
  
-   **Requisitos de hardware y software:** revise los requisitos de hardware y software para instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Podrá encontrarlos en: [Requisitos de hardware y software para instalar SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md). Una parte de cualquier plan de ciclo de actualización es plantearse la actualización del hardware (el hardware más reciente es más rápido y necesita menos licencias porque tiene menos procesadores o debido a la consolidación de los servidores y las bases de datos) y del sistema operativo. Estos tipos de cambios de hardware y software repercuten en el tipo de método de actualización que elija.  
  
-   **Entorno actual:** investigue su entorno actual para descubrir los componentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se utilizan y los clientes que se conectan a él.  
  
    -   **Proveedores de cliente:** aunque la actualización no requiere actualizar el proveedor de cada uno de los clientes, puede optar por hacerlo. Si va a actualizar desde [!INCLUDE[sql14](../../includes/sssql14-md.md)] o desde una versión anterior, las siguientes características de [!INCLUDE[sql15](../../includes/sssql16-md.md)] requieren un proveedor actualizado para cada cliente, o bien una actualización del proveedor para ofrecer una funcionalidad adicional:  
  
       -   [Always Encrypted &#40;motor de base de datos&#41;](../../relational-databases/security/encryption/always-encrypted-database-engine.md)  
  
       -   [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
       -   [Agentes de escucha de grupo de disponibilidad, conectividad de cliente y conmutación por error de una aplicación &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)  
  
       -   Actualización de seguridad de TLS  

   >[!NOTE]
   >La lista anterior es válida también para [!INCLUDE[sscurrent](../../includes/sscurrent-md.md)].
  
-   **Componentes de terceros:** determine la compatibilidad de los componentes de terceros, como la copia de seguridad integrada.  
  
-   **Entorno de destino:** compruebe que el entorno de destino cumple los requisitos de hardware y software y que puede admitir los requisitos del sistema original. Por ejemplo, la actualización puede conllevar la consolidación de varias instancias de SQL Server en una única instancia de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] nueva o la virtualización de su entorno de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en una nube privada o pública.  
  
-   **Edición:** determine la edición adecuada de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] para su actualización e identifique las rutas de actualización válidas para esta. Para obtener información detallada, vea [Supported Version and Edition Upgrades](../../database-engine/install-windows/supported-version-and-edition-upgrades.md). Antes de actualizar de una edición de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a otra, compruebe que las funciones que actualmente utiliza son compatibles con la edición a la que desea actualizar.  
  
    > [!NOTE]  
    >  Al actualizar [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] desde una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] edición Enterprise, elija entre Enterprise Edition: Licencia basada en Core y Enterprise Edition. Estas ediciones Enterprise solo se diferencian en los modos de licencia. Para obtener más información, consulte [Compute Capacity Limits by Edition of SQL Server](../../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).  
  
-   **Compatibilidad con versiones anteriores:** lea el artículo sobre la compatibilidad con versiones anteriores del motor de base de datos de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] para ver los cambios en el comportamiento entre [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] y la versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desde la que vaya a actualizar. Consulte [SQL Server Database Engine Backward Compatibility](../discontinued-database-engine-functionality-in-sql-server.md).  
  
-   **Data Migration Assistant:** ejecute Data Migration Assistant para ayudar a diagnosticar problemas que podrían bloquear el proceso de actualización o precisar la modificación de scripts o aplicaciones existentes debido a un cambio importante.
    Puede descargar Data Migration Assistant [aquí](https://aka.ms/get-dma).  
  
-   **Comprobador de configuración del sistema:** ejecute el Comprobador de configuración del sistema (SCC) de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] para determinar si el programa de instalación de SQL Server detecta cualquier problema de bloqueo antes de programar la actualización. Para obtener más información, vea [Check Parameters for the System Configuration Checker](../../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md).  
  
-   **Actualizar las tablas optimizadas para memoria:** al actualizar una instancia de base de datos de SQL Server 2014 que contenga tablas optimizadas para memoria a SQL Server 2016, el proceso requiere un tiempo adicional para convertir dichas tablas en el nuevo formato en disco (y la base de datos está sin conexión mientras se producen estos pasos).   La cantidad de tiempo depende del tamaño de las tablas optimizadas para memoria y de la velocidad del subsistema de E/S. Se requieren tres tamaños de operaciones de datos en el caso de las actualizaciones locales y de nueva instalación (el paso 1 no es obligatorio en el caso de las actualizaciones graduales, pero los pasos 2 y 3, sí):  
  
    1.  Ejecute la recuperación de bases de datos con el formato en disco antiguo (incluida la carga de todos los datos de tablas optimizadas para memoria en la memoria desde el disco).  
  
    2.  Serialice los datos en el disco en el nuevo formato en disco.  
  
    3.  Ejecute la recuperación de bases de datos con el nuevo formato (incluida la carga de todos los datos de tablas optimizadas para memoria en la memoria desde el disco).  
  
     Además, si no hay suficiente espacio en disco durante este proceso, se producirá un error de recuperación. Asegúrese de que haya suficiente espacio en disco para almacenar la base de datos existente y un almacenamiento adicional equivalente al tamaño actual de los contenedores en el grupo de archivos MEMORY_OPTIMIZED_DATA de la base de datos para llevar a cabo una actualización local o a la hora de adjuntar una base de datos de SQL Server 2014 a una instancia de SQL Server 2016. Use la siguiente consulta para determinar el espacio en disco que se necesita para el grupo de archivos MEMORY_OPTIMIZED_DATA y, por consiguiente, también la cantidad de espacio libre en disco necesario para que la actualización se lleve a cabo correctamente:  
  
    ```  
    select cast(sum(size) as float)*8/1024/1024 'size in GB'   
    from sys.database_files  
    where data_space_id in (select data_space_id from sys.filegroups where type=N'FX')  
    ```  
  
## <a name="develop-and-test-the-upgrade-plan"></a>Desarrollo y prueba del plan de actualización  
 El mejor método consiste en tratar la actualización como cualquier otro proyecto de TI. Organice un equipo de actualización con conocimientos sobre la administración de bases de datos; redes; extracción, transformación y carga (ETL); y otras destrezas necesarias para llevar a cabo la actualización. El equipo debe realizar lo siguiente:  
  
-   **Elegir el método de actualización:** vea [Elegir un método de actualización del motor de base de datos](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md).  
  
-   **Desarrollar un plan de reversión:** la ejecución de este plan le permite restaurar el entorno original si necesita llevar a cabo una reversión.  
  
-   **Determinar los criterios de aceptación:** compruebe que la actualización se ha llevado a cabo correctamente antes de migrar a los usuarios al entorno actualizado.  
  
-   **Probar el plan de actualización:** para probar el rendimiento mediante la carga de trabajo real, use la utilidad Microsoft SQL Server Distributed Replay. Esta herramienta puede usar varios equipos para reproducir los datos de seguimiento, con lo que se simula una carga de trabajo crítica. Si realiza una reproducción en un servidor de prueba antes y después de una actualización de SQL Server, podrá medir las diferencias de rendimiento y buscar cualquier incompatibilidad que su aplicación pueda tener con la actualización. Para obtener más información, vea [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md) y [Opciones de línea de comandos de la herramienta de administración &#40;utilidad Distributed Replay&#41;](../../tools/distributed-replay/administration-tool-command-line-options-distributed-replay-utility.md).  
  
## <a name="next-steps"></a>Pasos siguientes  
[Actualizar el motor de base de datos](../../database-engine/install-windows/upgrade-database-engine.md) 
  
## <a name="additional-resources"></a>Recursos adicionales 
[Guía sobre Database Migration](https://aka.ms/datamigration)