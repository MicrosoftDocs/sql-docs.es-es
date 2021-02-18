---
title: Instalación de una instancia de clúster de conmutación por error
description: Aprenda a instalar un clúster de conmutación por error de SQL Server. Cree y configure una instancia de clúster de conmutación por error mediante la ejecución del programa de instalación de SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
ms.assetid: c0e75a7c-85c5-423c-a218-77247bf071aa
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4e46540d75518b4da2a11337e35dfe22a335af60
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346643"
---
# <a name="sql-server-failover-cluster-installation"></a>Instalación de clúster de conmutación por error de SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Para instalar un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , debe crear y configurar una instancia en clúster de conmutación por error mediante la ejecución del programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
## <a name="installing-a-failover-cluster"></a>Instalar un clúster de conmutación por error  
 Para instalar un clúster de conmutación por error, debe utilizar una cuenta de dominio con permisos de administrador local para iniciar sesión como servicio y para actuar como parte del sistema operativo en todos los nodos del clúster de conmutación por error. Para instalar un clúster de conmutación por error mediante el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , siga estos pasos:  
  
1.  Para instalar, configurar y mantener un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , utilice el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
    -   Identifique la información que necesita para crear la instancia en clúster de conmutación por error (por ejemplo, recurso de disco de clúster, direcciones IP y nombre de red) y los nodos disponibles para conmutación por error. Para obtener más información:  
  
        -   [Antes de instalar los clústeres de conmutación por error](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md)  
  
        -   [Consideraciones de seguridad para una instalación de SQL Server](../../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
  
    -   Los pasos de configuración deben realizarse antes de ejecutar el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . A tal efecto, utilice el Administrador de clústeres de Windows. Debe disponer de un grupo WSFC para cada instancia en clúster de conmutación por error que desee configurar.  
  
    -   Debe asegurarse de que su sistema cumple los requisitos mínimos. Para obtener más información sobre los requisitos concretos de un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , vea [Antes de instalar los clústeres de conmutación por error](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md).  
  
2.  Agregar o quitar nodos de una configuración de clúster de conmutación por error sin influir en los demás nodos de clúster. Para obtener más información, vea [Agregar o quitar nodos en un clúster de conmutación por error de SQL Server &#40;programa de instalación&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
    -   Todos los nodos de un clúster de conmutación por error deben ser de la misma plataforma, de 32 bits o de 64 bits, y deben ejecutar la misma edición y versión del sistema operativo. Además, las versiones de 64 bits de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] deben instalarse en hardware de 64 bits en el que se ejecuten versiones de 64 bits de los sistemas operativos de Windows. No hay compatibilidad con WOW64 para los clústeres de conmutación por error en esta versión.  
  
3.  Especificar varias direcciones IP para cada instancia en clúster de conmutación por error. Puede especificar varias direcciones IP para cada subred. Si las diversas direcciones IP están en la misma subred, el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] establece la dependencia en AND. Si está agrupando en clústeres los nodos en varias subredes, el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] establece la dependencia en OR.  

4.  Instancia de clúster de conmutación por error de SQL Server (FCI) requiere que los nodos de clúster estén unidos a un dominio. **No se admiten** las configuraciones siguientes:
    - FCI de SQL en clústeres de grupo de trabajo. 
    - FCI de SQL en el clúster con múltiples dominios.   
    - FCI de SQL en clústeres de dominio + grupo de trabajo. 

## <a name="ssnoversion-failover-cluster-installation-options"></a>[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Opciones de instalación de clústeres de conmutación por error  
  
##### <a name="option-1-integrated-installation-with-add-node"></a>Opción 1: instalación integrada con Agregar nodo  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] integrada consta de dos pasos:  
  
1.  Cree y configure una instancia en clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de un único nodo. Cuando termine de configurar correctamente el nodo, dispondrá de una instancia del clúster de conmutación por error totalmente funcional. En ese momento no dispondrá de alta disponibilidad porque solamente hay un nodo en el clúster de conmutación por error.  
  
2.  En cada nodo que se va a agregar al clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , ejecute el programa de instalación con la función Agregar nodo para agregar ese nodo.  
  
##### <a name="option-2-advancedenterprise-installation"></a>Opción 2: Instalación de Advanced/Enterprise  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] La instalación de clústeres de conmutación por error Advanced/Enterprise consta de dos pasos:  
  
1.  En cada nodo que va a formar parte del clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , ejecute el programa de instalación con la función Preparar clúster de conmutación por error. En este paso se preparan los nodos para su agrupación en clústeres, pero al final de este paso no hay ninguna instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] operativa.  
  
2.  Una vez preparados los nodos para su agrupación en clústeres, ejecute el programa de instalación en el nodo propietario del disco compartido con la función Completar clúster de conmutación por error. En este paso se configura y se completa la instancia del clúster de conmutación por error. Al final de este paso, dispondrá de una instancia del clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] operativa.  
  
    > [!NOTE]  
    >  Ambas opciones de instalación permiten la instalación de un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de varios nodos. La función Agregar nodo se puede utilizar para agregar otros nodos en ambas opciones una vez creado un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
    > [!IMPORTANT]  
    >  La letra de unidad del sistema operativo de las ubicaciones de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] debe coincidir en todos los nodos agregados al clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
#### <a name="ip-address-configuration-during-setup"></a>Configuración de las direcciones IP durante la instalación  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] permite establecer o cambiar los valores de la dependencia de recursos IP durante las siguientes acciones:  
  
-   Instalación integrada: [Crear un nuevo clúster de conmutación por error de SQL Server &#40;programa de instalación&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)  
  
-   CompleteFailoverCluster (instalación avanzada): [Crear un nuevo clúster de conmutación por error de SQL Server &#40;programa de instalación&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)  
  
-   Agregar nodo: [Agregar o quitar nodos en un clúster de conmutación por error de SQL Server &#40;programa de instalación&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)  
  
-   Quitar nodo: [Agregar o quitar nodos en un clúster de conmutación por error de SQL Server &#40;programa de instalación&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)  
  
 **Nota** : se admiten las direcciones IP de IPV6.  Si configura tanto IPV4 como IPV6, se tratan como subredes diferentes y se espera que IPV6 se ponga primero en conexión.  
  
##### <a name="ssnoversion-multi-subnet-failover-cluster"></a>[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Clúster de conmutación por error de varias subredes  
 Puede establecer dependencias OR cuando los nodos en el clúster están en subredes diferentes. Sin embargo, cada nodo del clúster de conmutación por error de varias redes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] debe ser un posible propietario de al menos una de las direcciones IP especificadas.  
  
## <a name="see-also"></a>Consulte también  
 [Antes de instalar los clústeres de conmutación por error](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md)   
 [Crear un nuevo clúster de conmutación por error de SQL Server &#40;programa de instalación&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)   
 [Instalar SQL Server 2016 desde el símbolo del sistema](../../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)   
 [Actualización de una instancia del clúster de conmutación por error de SQL Server](../../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md).  
  
