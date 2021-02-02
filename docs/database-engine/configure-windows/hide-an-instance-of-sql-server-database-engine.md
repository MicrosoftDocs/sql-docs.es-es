---
title: Ocultar una instancia del Motor de base de datos de SQL Server | Microsoft Docs
description: Aprenda a ocultar una instancia del motor de base de datos de SQL Server. Los equipos cliente no pueden usar el servicio SQL Server Browser para buscar instancias ocultas.
ms.custom: ''
ms.date: 08/19/2015
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- Database Engine [SQL Server], hiding instances
- hiding instances of Database Engine
ms.assetid: 392de21a-57fa-4a69-8237-ced8ca86ed1d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f0e4c13745057b1276f06f3bc77ace266921a43d
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076277"
---
# <a name="hide-an-instance-of-sql-server-database-engine"></a>Ocultar una instancia del motor de base de datos de SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo ocultar una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizando el Administrador de configuración de SQL Server. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser para enumerar las instancias del [!INCLUDE[ssDE](../../includes/ssde-md.md)] instaladas en el equipo. De esta manera, las aplicaciones cliente pueden buscar un servidor y los clientes pueden distinguir las distintas instancias del [!INCLUDE[ssDE](../../includes/ssde-md.md)] que están instaladas en el mismo equipo. Puede usar el procedimiento siguiente para evitar que el servicio SQL Server Browser exponga una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] a los equipos cliente que intenten buscarla mediante el botón **Examinar** .  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Usar el Administrador de configuración de SQL Server  
  
#### <a name="to-hide-an-instance-of-the-sql-server-database-engine"></a>Para ocultar una instancia del motor de base de datos de SQL Server  
  
1.  En el **Administrador de configuración de SQL Server**, expanda **Configuración de red de SQL Server**, haga clic con el botón derecho en **Protocolos de** *\<server instance>* y, después, seleccione **Propiedades**.  
  
2.  En la pestaña **Marcas** , en el cuadro **HideInstance** , seleccione **Sí** y, a continuación, haga clic en **Aceptar** para cerrar el cuadro de diálogo. El cambio se aplica de forma inmediata para las conexiones nuevas.  
  
## <a name="remarks"></a>Observaciones  
 Si oculta una instancia con nombre, deberá proporcionar el número de puerto en la cadena de conexión para conectarse a la instancia oculta, aunque se esté ejecutando el servicio de explorador. Se recomienda utilizar un puerto estático en lugar de un puerto dinámico para la instancia con nombre oculta.  
  Para obtener más información, vea [Configurar un servidor para que escuche en un puerto TCP específico &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md).  
  
### <a name="clustering"></a>Agrupación en clústeres  
 Si oculta el nombre de una instancia en clúster o de un grupo de disponibilidad, es posible que el servicio de clúster no pueda conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esto provocará el error de la comprobación **IsAlive** de la instancia en clúster y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se desconectará. 
 
Para evitarlo, cree un alias en todos los nodos de la instancia en clúster o de todas las instancias que hospedan réplicas del grupo de disponibilidad con el fin de reflejar el puerto estático que ha configurado para la instancia.  Por ejemplo, en un grupo de disponibilidad con dos réplicas, en node-one, cree un alias para la instancia node-two, como `node-two\instancename`. En node-two, cree un alias denominado `node-one\instancename`. Los alias son necesarios para que la conmutación por error sea correcta. 
 
 Para obtener más información, vea [Crear o eliminar un alias de servidor para que lo utilice un cliente &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/create-or-delete-a-server-alias-for-use-by-a-client.md).  
  
 Si oculta una instancia con nombre agrupada, es posible que el servicio de clúster no pueda conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si la clave del Registro **LastConnect** (**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer\Client\SNI11.0\LastConnect**) tiene un puerto distinto del puerto en el que escucha [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si el servicio de clúster no puede establecer la conexión con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], podría aparecer un error similar al siguiente:  
**Identificador del evento: 1001: nombre del evento: interbloqueo de recurso de clústeres de conmutación por error.**  
  
## <a name="see-also"></a>Consulte también  
 [Configuración de red del servidor](../../database-engine/configure-windows/server-network-configuration.md)   
 [Descripción de las conexiones de cliente del servidor virtual de SQL](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/273673)   
 [Cómo asignar un puerto estático a una instancia con nombre de SQL Server y evitar un problema común](https://deep.data.blog/2012/09/08/how-to-assign-a-static-port-to-a-sql-server-named-instance-and-avoid-a-common-pitfall/)  
  
  
