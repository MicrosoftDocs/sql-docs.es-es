---
title: Eliminación de una instancia de clúster de conmutación por error
description: Use este procedimiento para desinstalar una instancia en clúster de conmutación por error Always On. En este artículo se incluyen consideraciones importantes antes de continuar.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- clusters [SQL Server], removing failover cluster instance
- failover clustering [SQL Server], removing failover cluster instance
- uninstalling failover cluster instances
- removing failover cluster instances
ms.assetid: bf63353b-69cf-4c5c-98ea-7b151e36537f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2a095fdf70e8190073079944fbbcfa57e8e0ecac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354077"
---
# <a name="remove-a-failover-cluster-instance-setup"></a>Eliminación de una instancia de clúster de conmutación por error (programa de instalación)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Use este procedimiento para desinstalar una instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Always On.  
  
> [!IMPORTANT]  
>  Para actualizar o quitar un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] es preciso ser administrador local con derecho de iniciar sesión como servicio en todos los nodos del clúster de conmutación por error de Windows Server.  
  
 **Antes de empezar**  
  
 Tenga en cuenta los siguiente puntos importantes antes de desinstalar una instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]:  
  
-   Si se desinstala [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client por accidente, se generará un error al iniciar los recursos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Para volver a instalar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, ejecute el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para instalar los requisitos previos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Si desinstala un clúster de conmutación por error que tenga más de un recurso de clúster IP de SQL, debe quitar los recursos IP de SQL adicionales mediante el administrador de clústeres de conmutación por error o PowerShell.  
  
 Para obtener más información sobre la sintaxis de la línea de comandos, vea [Instalar SQL Server 2016 desde el símbolo del sistema](../../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md).  
  
### <a name="to-uninstall-a-ssnoversion-failover-cluster-instance"></a>Para desinstalar una instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]
  
1.  Para desinstalar un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , utilice la funcionalidad Eliminar nodo proporcionada por el programa de instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para quitar cada nodo individualmente. Para obtener más información, vea [Agregar o quitar nodos en un clúster de conmutación por error de SQL Server (programa de instalación)](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
## <a name="see-also"></a>Consulte también  
 [Ver y leer los archivos de registro de instalación de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
