---
title: Cambio de nombre de una instancia de clúster de conmutación por error
description: En este artículo se describe cómo cambiar el nombre de una instancia de SQL Server que forma parte de un clúster de conmutación por error, que difiere de cambiar el nombre de una instancia independiente.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- clusters [SQL Server], virtual servers
- renaming virtual servers
- virtual servers [SQL Server], failover clustering
- failover clustering [SQL Server], virtual servers
ms.assetid: 2a49d417-25fb-4760-8ae5-5871bfb1e6f3
author: cawrites
ms.author: chadam
ms.openlocfilehash: 42fd6dbb5053993d5ad6c2859460e164f476bac1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354026"
---
# <a name="rename-a-sql-server-failover-cluster-instance"></a>Cambiar el nombre de una instancia de clúster de conmutación por error de SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cuando una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] forma parte de un clúster de conmutación por error, el proceso para cambiar el nombre del servidor virtual no es el mismo que para cambiar el nombre de una instancia independiente. Para obtener más información, vea [Cambiar el nombre de un equipo que hospeda una instancia independiente de SQL Server](../../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md).  
  
 El nombre del servidor virtual es siempre el nombre de red de SQL (el nombre de red del servidor virtual SQL Server). Aunque se puede cambiar el nombre del servidor virtual, no se puede cambiar el nombre de la instancia. Por ejemplo, es posible cambiar el nombre del servidor virtual VS1\instancia1 por cualquier otro nombre, como SQL35\instancia1, pero la parte del nombre que corresponde a la instancia (instancia1) no varía.  
  
 Antes de comenzar el proceso de cambio de nombre, revise los siguientes puntos.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no admite el cambio de nombre de los servidores que participan en la replicación, excepto si se utiliza el trasvase de registros con la replicación. Se puede cambiar el nombre del servidor secundario del trasvase de registros si se pierde el servidor principal de manera permanente. Para obtener más información, vea [Trasvase de registros y replicación &#40;SQL Server&#41;](../../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md).  
  
-   Cuando se cambia el nombre de un servidor virtual configurado para utilizar la creación de reflejo de la base de datos, debe desactivarse la creación de reflejo de la base de datos antes de realizar la operación de cambio de nombre y, después, volver a establecer la creación de reflejo de la base de datos con el nombre de servidor virtual nuevo. Los metadatos para la creación de reflejo de la base de datos no se actualizan automáticamente con el nuevo nombre del servidor virtual.  
  
### <a name="to-rename-a-virtual-server"></a>Para cambiar el nombre de un servidor virtual  
  
1.  En el Administrador de clústeres, cambie el nombre de red de SQL Server por el nombre nuevo.  
  
2.  Ponga el recurso de nombre de red en modo sin conexión. Esta operación pone también el recurso de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y otros recursos dependientes en modo sin conexión.  
  
3.  Vuelva a poner en línea el recurso de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
## <a name="verify-the-renaming-operation"></a>Comprobar la operación de cambio de nombre  
 Después de cambiar el nombre de un servidor virtual, las conexiones que utilizaban el nombre anterior ahora deben realizarse con el nuevo nombre.  
  
 Para comprobar que la operación de cambio de nombre ha finalizado, seleccione información de **@@servername** o **sys.servers**. La función **@@servername** devolverá el nuevo nombre del servidor virtual y la tabla **sys.servers** mostrará también este nombre. Para comprobar que el proceso de conmutación por error funciona correctamente con el nuevo nombre, el usuario también debe intentar que el recurso de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] conmute por error a los otros nodos.  
  
 Para las conexiones desde cualquier nodo del clúster se puede utilizar el nombre nuevo casi inmediatamente. Sin embargo, no se puede utilizar para las conexiones con el servidor desde un equipo cliente hasta que no esté visible para este último. El tiempo necesario para que el nuevo nombre se propague a través de la red pueden ser segundos o de 3 a 5 minutos, dependiendo de la configuración de la red; puede que se necesite más tiempo para que el nombre de servidor virtual anterior deje de estar visible en la red.  
  
 Para reducir al mínimo el tiempo necesario para propagar en la red el cambio de nombre de un servidor virtual, realice los pasos siguientes:  
  
#### <a name="to-minimize-network-propagation-delay"></a>Para reducir al mínimo el tiempo necesario de propagación por la red  
  
1.  Ejecute los comandos siguientes desde un símbolo del sistema del nodo del servidor:  
  
    ```  
    ipconfig /flushdns  
    ipconfig /registerdns  
    nbtstat -RR  
    ```  
  
## <a name="additional-considerations-after-the-renaming-operation"></a>Consideraciones adicionales después de la operación de cambio de nombre  
 Después de cambiar el nombre de red en clúster del clúster de conmutación por error, hay que comprobar y seguir las siguientes instrucciones para conseguir que todos los escenarios del Agente [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]funcionen.  
  
 **[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Servicio del Agente:** compruebe y realice las acciones adicionales siguientes para el Servicio Agente [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]:  
  
-   Corrija la configuración del Registro si el Agente SQL se configura para el reenvío de eventos. Para obtener más información, vea [Designar un servidor de reenvío de eventos &#40;SQL Server Management Studio&#41;](../../../ssms/agent/designate-an-events-forwarding-server-sql-server-management-studio.md).  
  
-   Corregir los nombres de instancia de los servidores de destino (TSX) y el servidor maestro (MSX) cuando se cambie el nombre de red en clúster o equipos. Para obtener más información, vea los temas siguientes:  
  
    -   [Dar de baja varios servidores de destino desde un servidor maestro](../../../ssms/agent/defect-multiple-target-servers-from-a-master-server.md)  
  
    -   [Crear un entorno multiservidor](../../../ssms/agent/create-a-multiserver-environment.md)  
  
-   Volver a configurar el trasvase de registros de modo que se use el nombre de servidor actualizado para realizar la copia de seguridad de los registros y para restaurarlos. Para obtener más información, vea los temas siguientes:  
  
    -   [Configurar el trasvase de registros &#40;SQL Server&#41;](../../../database-engine/log-shipping/configure-log-shipping-sql-server.md)  
  
    -   [Quitar el trasvase de registros &#40;SQL Server&#41;](../../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
-   Actualizar los pasos de trabajo que dependen del nombre de servidor. Para más información, consulte [Manage Job Steps](../../../ssms/agent/manage-job-steps.md).  
  
## <a name="see-also"></a>Consulte también  
 [Cambiar el nombre de un equipo que hospeda una instancia independiente de SQL Server](../../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md)  
  
