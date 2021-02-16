---
title: Elegir un método de actualización del motor de base de datos | Microsoft Docs
description: En este artículo se describen las rutas de actualización del Motor de base de datos en SQL Server, incluida la actualización local, la migración a una nueva instalación y una actualización gradual.
ms.custom: ''
ms.date: 07/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 5e57a427-2e88-4ef6-b142-4ccad97bcecc
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 185e718e1b131398f81fe782c4d5fcb843489c3a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347847"
---
# <a name="choose-a-database-engine-upgrade-method"></a>Elegir un método de actualización del motor de base de datos
[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Existen varios métodos que se deben considerar a la hora de planear la actualización del [!INCLUDE[ssDE](../../includes/ssde-md.md)] de una versión previa de SQL Server si se pretende reducir al mínimo el tiempo de inactividad y los riesgos. Puede realizar una actualización local, migrar a una nueva instalación o efectuar una actualización gradual. El siguiente diagrama le ayudará a elegir uno de estos enfoques. Además, más adelante en este artículo se describen todos los enfoques presentes en el diagrama. Si quiere obtener ayuda para tomar las decisiones que se le presentan en el diagrama, consulte también [Planear y probar el plan de actualización del Motor de base de datos](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md).  
  
 ![Árbol de decisión del método de actualización del motor de base de datos](../../database-engine/install-windows/media/database-engine-upgrade-method-decision-tree.png "Árbol de decisión del método de actualización del motor de base de datos")  
  
 **Descargar**  
  
-   Para descargar [!INCLUDE[SSnoversion](../../includes/ssnoversion-md.md)], vaya al  **[Centro de evaluación](https://www.microsoft.com/evalcenter/evaluate-sql-server)** .  
  
-   ¿Tiene una cuenta de Azure?  Si es así, haga clic **[aquí](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2017-ws2019?tab=overview)** para poner en marcha una máquina virtual con [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Developer Edition ya instalado.  
  
> [!NOTE]  
>  También puede plantearse actualizar la base de datos SQL de Azure o virtualizar su entorno de SQL Server como parte de su plan de actualización. Estos artículos están fuera del ámbito de este artículo, pero aquí incluimos algunos vínculos:
>   - [Información general sobre SQL Server en Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/#overview)
>   - [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) 
>   - [Selección de una opción de SQL Server en Azure](/azure/azure-sql/azure-sql-iaas-vs-paas-what-is-overview)  
  
## <a name="upgrade-in-place"></a>Actualización local  
 Con este método, el programa de instalación de SQL Server actualiza la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] existente reemplazando los bits de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] existentes por los nuevos bits de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] y, después, actualiza cada una de las bases de datos de usuario y del sistema.  El enfoque de actualización local es el más sencillo, conlleva la menor cantidad de tiempo de inactividad, tarda más en tiempo en revertirse (si esto fuera necesario) y no se admite en todos los casos. Para más información sobre los escenarios de actualización local que se admiten y los que no, vea [Actualizaciones de ediciones y versiones admitidas](../../database-engine/install-windows/supported-version-and-edition-upgrades-version-15.md).  
  
 Este enfoque se suele usar en los escenarios siguientes:  
  
-   Un entorno de desarrollo sin una configuración de alta disponibilidad.  
  
-   Un entorno de producción no esencial que pueda tolerar el tiempo de inactividad y que ejecute hardware y software recientes. La cantidad de tiempo de inactividad depende del tamaño de la base de datos y la velocidad de su subsistema de E/S. Se necesitará un poco más de tiempo a la hora de actualizar SQL Server 2014 cuando se estén usando tablas optimizadas para memoria. Para obtener más información, consulte [Planeación y prueba del plan de actualización del motor de base de datos](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md).  

> [!NOTE]
> Para ver un ejemplo del modo en que Microsoft ha usado esta estrategia, consulte el siguiente blog: [To upgrade or not to upgrade](https://azure.microsoft.com/blog/azure-sql-server-to-upgrade-or-not-to-upgrade-that-is-the-question/) (Actualizar o no actualizar). 


> [!WARNING]  
>  Cuando se ejecuta el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se detiene y reinicia como parte de la ejecución de las comprobaciones previas a la actualización.  
  
> [!CAUTION]  
>  Cuando actualice a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la instancia anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se sobrescribirá y ya no estará en el equipo. Antes de actualizar, realice una copia de seguridad de las bases de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de otros objetos asociados con la instancia anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 En el siguiente diagrama se ofrece una descripción general de los pasos necesarios para llevar a cabo una actualización local del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 ![Actualización local no de alta disponibilidad del motor de base de datos](../../database-engine/install-windows/media/database-engine-upgrade-non-ha-in-place-upgrade.png "Actualización local no de alta disponibilidad del motor de base de datos")  
  
 Para más información detallada, vea [Actualización a SQL Server mediante el Asistente para instalación &#40;programa de instalación&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md).  
  
## <a name="migrate-to-a-new-installation"></a>Migración a una nueva instalación  
 Con este método, conserva el entorno actual a la vez que crea un entorno de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], con frecuencia en nuevo hardware y con una nueva versión del sistema operativo. Después de instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el nuevo entorno, debe realizar una serie de pasos a fin de prepararlo para poder migrar las bases de datos de usuario existentes desde el entorno antiguo al nuevo y minimizar el tiempo de inactividad. En estos pasos se incluye la migración de los siguientes elementos:  
  
-   **Objetos del sistema:** Algunas aplicaciones dependen de información, entidades u objetos que se encuentran fuera del ámbito de una base de datos de usuario único. Normalmente, una aplicación depende de las bases de datos maestra y msdb, así como la de usuario. Cualquier elemento almacenado fuera de la base de datos de usuario que sea necesario para el funcionamiento correcto de dicha base de datos debe estar disponible en la instancia de servidor de destino. Por ejemplo, los inicios de sesión de una aplicación se almacenan como metadatos en la base de datos maestra y se deben volver a crear en el servidor de destino. Si una aplicación o un plan de mantenimiento de bases de datos dependen de trabajos del Agente SQL Server, cuyos metadatos estén almacenados en la base de datos msdb, dichos trabajos se deben volver a crear en la instancia de servidor de destino. De forma similar, los metadatos de un desencadenador de servidor se almacenan en la base de datos maestra.  
 
   Si mueve la base de datos de una aplicación a otra instancia de servidor, debe volver a crear todos los metadatos de las entidades y los objetos dependientes de las bases de datos maestra y msdb en la instancia de servidor de destino. Por ejemplo, si una aplicación de la base de datos usa desencadenadores de servidor, no basta con adjuntar o restaurar la base de datos en el nuevo sistema. La base de datos no funcionará según lo previsto a menos que se vuelvan a crear manualmente los metadatos para dichos desencadenadores en la base de datos maestra. Para más información detallada, vea [Administrar los metadatos cuando una base de datos pasa a estar disponible en otra instancia del servidor &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
-   **Paquetes de Integration Services almacenados en MSDB:** si almacena paquetes en MSDB, debe incluirlos en un script mediante la [utilidad dtutil](../../integration-services/dtutil-utility.md) o volver a implementarlos en el nuevo servidor. Antes de usar los paquetes en el nuevo servidor, debe actualizarlos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, consulte [Upgrade Integration Services Packages](../../integration-services/install-windows/upgrade-integration-services-packages.md).  
  
-   **Claves de cifrado de Reporting Services:** Una parte importante de la configuración del servidor de informes es la creación de una copia de seguridad de la clave simétrica utilizada para cifrar información confidencial. La copia de seguridad de la clave se necesita para varias operaciones rutinarias y, además, permite volver a utilizar una base de datos del servidor de informes existente en una nueva instalación. Para obtener más información, vea [Hacer copia de seguridad y restaurar claves de cifrado de Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md) y [Upgrade y Migrate Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)  
  
 Cuando el nuevo entorno de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuente con los mismos objetos de sistema que el antiguo, deberá migrar las bases de datos de usuario desde el sistema existente a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con un método que reduzca al mínimo el tiempo de inactividad en dicho sistema. Podrá realizar la migración de la base de datos por medio de una copia de seguridad y una restauración, o bien redirigiendo LUN si se encuentra en un entorno de SAN. Los pasos de ambos métodos se escriben en los diagramas siguientes.  
  
> [!CAUTION]  
>  La cantidad de tiempo de inactividad depende del tamaño de la base de datos y la velocidad de su subsistema de E/S. Se necesitará un poco más de tiempo a la hora de actualizar SQL Server 2014 cuando se estén usando tablas optimizadas para memoria. Para obtener más información, consulte [Planeación y prueba del plan de actualización del motor de base de datos](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md).  
  
 Después de migrar las bases de datos de usuario, dirija a los nuevos usuarios a la nueva instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante uno de los diversos métodos disponibles (por ejemplo, cambiar el nombre del servidor, usar una entrada DNS, modificar las cadenas de conexión, etc.).  El método de nueva instalación reduce los riesgos y el tiempo de inactividad (en comparación con una actualización local). Además, facilita que se actualice el hardware y el sistema operativo con la actualización a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Si ya dispone de una solución de alta disponibilidad o de algún otro entorno con varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vaya a [Actualización gradual](#rolling-upgrade). Si no cuenta con una solución de alta disponibilidad, puede plantearse configurar temporalmente la [creación de reflejo de la base de datos](../database-mirroring/setting-up-database-mirroring-sql-server.md) a fin de minimizar el tiempo de inactividad y facilitar la actualización, o bien aprovechar esta oportunidad para configurar un [grupo de disponibilidad AlwaysOn](../availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server.md) como solución de alta disponibilidad permanente.  
  
 Por ejemplo, puede utilizar este enfoque para actualizar los siguientes elementos:  
  
-   Una instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un sistema operativo no compatible.    
-   Una instalación x86 de SQL Server, ya que ni [!INCLUDE[ss2016](../../includes/sssql16-md.md)] ni las versiones posteriores admiten este tipo de instalaciones.   
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en hardware nuevo o una nueva versión del sistema operativo.    
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] junto con una consolidación de servidores.   
-   SQL Server 2005, ya que ni [!INCLUDE[ss2016](../../includes/sssql16-md.md)] ni las versiones posteriores admiten la actualización local de SQL Server 2005. Para más información, vea [¿Desea actualizar desde SQL Server 2005?](../../sql-server/end-of-support/sql-server-end-of-life-overview.md).

  
Los pasos necesarios realizar una actualización mediante una nueva instalación varían ligeramente en función de si usa almacenamiento conectado o SAN.  
  
-   **Entorno de almacenamiento conectado:** si tiene un entorno de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el que se usa almacenamiento conectado, el diagrama siguiente y los vínculos que incluye le servirán de guía para seguir los pasos necesarios para realizar una actualización de [!INCLUDE[ssDE](../../includes/ssde-md.md)] a través de una nueva instalación.  
  
     ![Nuevo método de actualización de instalación con copias de seguridad y restauración para el almacenamiento conectado](../../database-engine/install-windows/media/new-installation-upgrade-method-using-backup-and-restore-for-attached-storage.png "Nuevo método de actualización de instalación con copias de seguridad y restauración para el almacenamiento conectado")  
  
-   **Entorno de almacenamiento SAN:**  si tiene un entorno de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el que se usa almacenamiento SAN, el diagrama siguiente y los vínculos que incluye le servirán de guía para seguir los pasos necesarios para realizar una actualización de [!INCLUDE[ssDE](../../includes/ssde-md.md)] a través de una nueva instalación.  
  
     ![Nuevo método de actualización de instalación mediante Separar y Adjuntar para almacenamiento SAN](../../database-engine/install-windows/media/new-installation-upgrade-method-using-detach-and-attach-for-san-storage.png "Nuevo método de actualización de instalación mediante Separar y Adjuntar para almacenamiento SAN")  
  
## <a name="rolling-upgrade"></a>Actualización gradual  
 Se requiere una actualización gradual en entornos de soluciones de SQL Server donde se deba actualizar varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un orden determinado para maximizar el tiempo de actividad, minimizar los riesgos y conservar determinada funcionalidad. Una actualización gradual consiste básicamente en la actualización de varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un orden determinado, ya sea mediante una actualización local en cada instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] existente o efectuando una actualización con una nueva instalación a fin de facilitar la actualización del hardware o el sistema operativo como parte del proyecto de actualización. Hay una serie de escenarios en los que deberá poner en práctica el enfoque de actualización gradual. Estos se documentan en los siguientes artículos:  
  
-   Grupos de disponibilidad Always On: para obtener instrucciones detalladas sobre cómo realizar una actualización gradual en un entorno de este tipo, vea [Actualización de instancias de la réplica del grupo de disponibilidad Always On](../../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md).    
-   Instancias de clúster de conmutación por error: para obtener instrucciones detalladas sobre cómo realizar una actualización gradual en un entorno de este tipo, vea [Actualización de una instancia de clúster de conmutación por error de SQL Server](../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md).    
-   Instancias reflejadas: para obtener instrucciones detalladas sobre cómo realizar una actualización gradual en un entorno de este tipo, vea [Actualización de instancias reflejadas](../../database-engine/database-mirroring/upgrading-mirrored-instances.md).    
-   Instancias de trasvase de registros: para obtener instrucciones detalladas sobre cómo realizar una actualización gradual en un entorno de este tipo, vea [Actualización del trasvase de registros a SQL Server &#40;Transact-SQL&#41;](../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md).    
-   Un entorno de replicación: para obtener instrucciones detalladas sobre cómo realizar una actualización gradual en este entorno, vea [Actualizar bases de datos replicadas](../../database-engine/install-windows/upgrade-replicated-databases.md).  
-   Un entorno de escalabilidad horizontal de SQL Server Reporting Services: para obtener instrucciones detalladas sobre cómo realizar una actualización gradual en este entorno, vea [Actualizar y migrar Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md).  
  
## <a name="next-steps"></a>Pasos siguientes
 [Planeación y prueba del plan de actualización del motor de base de datos](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md)   
 [Completar la actualización motor de base de datos](../../database-engine/install-windows/complete-the-database-engine-upgrade.md)