---
description: Agregar dependencias a un recurso de SQL Server
title: Adición de dependencias a un recurso de FCI de SQL Server
descriptoin: Describes how to add dependencies to an Always On failover cluster instance (FCI) resource using the Failover Cluster Manager.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- resource dependencies [SQL Server]
- failover clustering [SQL Server], dependencies
- clusters [SQL Server], dependencies
- dependencies [SQL Server], clustering
ms.assetid: 25dbb751-139b-4c8e-ac62-3ec23110611f
author: cawrites
ms.author: chadam
ms.openlocfilehash: a091db3649ddaa2a0e873609e5e87f1f7e6e648e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354004"
---
# <a name="add-dependencies-to-a-sql-server-resource"></a>Agregar dependencias a un recurso de SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo se agregan dependencias a un recurso de instancia de clúster de conmutación por error (FCI) Always On con el complemento Administrador de clústeres de conmutación por error. El complemento Administrador de clústeres de conmutación por error es la aplicación de administración de clústeres del servicio de clústeres de conmutación por error de Windows Server (WSFC).  
  
-   **Antes de empezar:**  [Limitaciones y restricciones](#Restrictions), [Requisitos previos](#Prerequisites)  
  
-   **Para agregar una dependencia a un recurso de SQL Server con:** [Administrador de clústeres de conmutación por error de Windows](#WinClusManager)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
 Tenga en cuenta que si agrega otros recursos al grupo de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , dichos recursos deben tener siempre sus propios recursos de nombre de red SQL únicos y sus propios recursos de dirección IP de SQL.  
  
 No utilice los recursos de nombre de red SQL ni los recursos de dirección IP SQL existentes en ningún otro entorno que no sea [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si los recursos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se comparten con otros recursos pueden producirse los siguientes problemas:  
  
-   Interrupciones inesperadas.  
  
-   Es posible que la instalación de los Service Pack no se haya realizado correctamente.  
  
-   Puede que el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no se ejecute correctamente. Si ocurre este problema, no puede instalar instancias adicionales de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ni realizar un mantenimiento rutinario del rendimiento.  
  
 Tenga en cuenta también estos problemas adicionales:  
  
-   FTP con replicación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] : para las instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que utilicen FTP con replicación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , el servicio FTP debe usar uno de los mismos discos físicos configurados para que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilice el servicio FTP.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] : si agrega un recurso a un grupo de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y tiene una dependencia en el recurso de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para asegurarse de que esté disponible [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , [!INCLUDE[msCoName](../../../includes/msconame-md.md)] recomienda agregar una dependencia en el recurso del Agente [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . No agregue una dependencia en el recurso [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Para asegurarse de que el equipo en el que se ejecuta [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] permanece altamente disponible, configure el recurso del Agente [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de modo que no afecte al grupo de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] si se produce un error en el recurso del Agente [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Recursos compartidos de archivos e impresoras: cuando instale recursos de archivo o recursos de clústeres de impresora, no los coloque en los mismos recursos de disco físico que el equipo en el que se ejecuta [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si los coloca en los mismos recursos de disco físico, puede experimentar una degradación del rendimiento y la pérdida de servicio en el equipo donde se ejecuta [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   Consideraciones sobre MS DTC: Después de instalar el sistema operativo y configurar la FCI, debe configurar el coordinador de transacciones distribuidas de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] (MS DTC) para que funcione en un clúster mediante el complemento Administrador de clústeres de conmutación por error. Si no logra crear el clúster de MS DTC, no se bloqueará el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , pero la funcionalidad de la aplicación [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] puede verse afectada si MS DTC no se configura correctamente.  
  
     Si instala MS DTC en el grupo de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y tiene otros recursos que dependen de MS DTC, MS DTC no estará disponible si este grupo se encuentra en modo sin conexión ni durante una conmutación por error. [!INCLUDE[msCoName](../../../includes/msconame-md.md)] recomienda que, si es posible, sitúe MS DTC en su propio grupo con su propio recurso de disco físico.  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
 Si instala [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en un grupo de recursos de WSFC con varias unidades de disco y decide colocar los datos en una de las unidades, el recurso de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se configurará para que solo dependa de dicha unidad. Para colocar datos o registros en otro disco, primero debe agregar una dependencia al recurso [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para el disco adicional.  
  
##  <a name="using-the-failover-cluster-manager-snap-in"></a><a name="WinClusManager"></a> Usar el complemento Administrador de clústeres de conmutación por error  
 **Para agregar una dependencia a un recurso de SQL Server**  
  
-   Abra el complemento Administrador de clústeres de conmutación por error.  
  
-   Busque el grupo que contiene el recurso de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] aplicable que desearía convertir en dependiente.  
  
-   Si el recurso del disco ya está en este grupo, vaya al paso 4. De lo contrario, busque el grupo que contiene el disco. Si ese grupo y el grupo que contiene [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no son propiedad del mismo nodo, mueva el grupo que contiene el recurso del disco al nodo propietario del grupo de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Seleccione el recurso de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , abra el cuadro de diálogo **Propiedades** y utilice la pestaña **Dependencias** para agregar el disco al conjunto de dependencias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
  
