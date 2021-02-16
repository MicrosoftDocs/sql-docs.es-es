---
description: Reparar una instalación de SQL Server con errores
title: Reparar una instalación de SQL Server con errores | Microsoft Docs
deescription: This article describes the scenarios where you can try a repair operation to fix failed SQL Server installation.
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 90c11b28-892b-44d6-978e-0eee48c75b7d
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 4edcf1335066d29c3f9a471417862f7f3967a92e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351592"
---
# <a name="repair-a-failed-sql-server-installation"></a>Reparar una instalación de SQL Server con errores

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

La operación de reparación se puede utilizar en los escenarios siguientes:  
  
- Para reparar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se ha dañado una vez instalada correctamente. 
  
- Para reparar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si la operación de actualización se canceló o produjo un error una vez asignado el nombre a la instancia recién actualizada. 
  
    - Si ve el mensaje siguiente en el registro de resumen, puede reparar la instancia de la actualización con errores:  
  
         "Error en la actualización de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para continuar, investigue el motivo del error, corrija el problema, desinstale SQL Server y, después, repare la instalación."  
  
    - Si ve el mensaje siguiente en el registro de resumen, tiene que desinstalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]y volver a instalarlo. No puede reparar la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . 
  
         "Error en la actualización de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para continuar, investigue el motivo del error y corrija el problema."  
  
 Al reparar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
- Se reemplazan todos los archivos que se hayan perdido o dañado. 
  
- Se reemplazan todas las claves del Registro que se hayan perdido o dañado. 
  
- Todos los valores de configuración que se hayan perdido o que no sean válidos se establecen en los valores predeterminados. 
  
 Antes de continuar, revise la siguiente información importante relacionada con los clústeres de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
- La reparación se debe llevar a cabo en nodos de clúster individuales. 
  
- Para reparar un nodo de clúster de conmutación por error después una operación de preparación con errores, use **Eliminar nodo de un clúster de conmutación por error de SQL Server** y, a continuación, vuelva a realizar el paso de preparación. Para obtener más información, vea [Agregar o quitar nodos en un clúster de conmutación por error de SQL Server &#40;programa de instalación&#41;](../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md). 
  
## <a name="repair-a-failed-installation-of-ssnoversion-from-the-installation-center"></a>Reparar una instalación con errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desde el Centro de instalación 
  
1. Inicie el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (setup.exe) desde el disco de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . 
  
2. Después de comprobar los requisitos previos y el sistema, el programa de instalación mostrará la página Centro de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . 
  
3. Haga clic en **Mantenimiento** en el área de navegación de la izquierda y, después, haga clic en **Reparar** para iniciar la operación de reparación. 
  
   >[!TIP]  
   > Si el Centro de instalación se ha iniciado con el menú de inicio, tendrá que proporcionar la ubicación del medio de instalación en este momento. 
  
4. Configure las reglas auxiliares del programa de instalación y las rutinas de archivo para asegurarse de que el sistema tiene instalados los requisitos previos y de que el equipo pasa las reglas de validación del programa de instalación. Haga clic en **Aceptar** o en **Instalar** para continuar. 
  
5. En la página Seleccionar instancia , seleccione la instancia que desea reparar y, a continuación, haga clic en **Siguiente** para continuar. 
  
6. Las reglas de reparación se ejecutarán para validar la operación. Para continuar, haga clic en **Siguiente**. 
  
7. La página Listo para reparar indica que la operación está lista para continuar. Para continuar, haga clic en **Reparar**. 
  
8. La página Progreso de la reparación muestra el estado de la operación de reparación. La página Operación completada indica que la operación ha finalizado. 
  
### <a name="to-repair-a-failed-installation-of-ssnoversion-using-command-prompt"></a>Para reparar una instalación con errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizando el símbolo del sistema  
  
1. Ejecute el comando siguiente en el símbolo del sistema:  
  
    ```  
    Setup.exe /q /ACTION=Repair /INSTANCENAME=instancename  
    ```  
  
## <a name="see-also"></a>Consulte también  
 [Ver y leer los archivos de registro de instalación de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)   
 [Artículos de procedimientos de instalación](/previous-versions/sql/)  
  
