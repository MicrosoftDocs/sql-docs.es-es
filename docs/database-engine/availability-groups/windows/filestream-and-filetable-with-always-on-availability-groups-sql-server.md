---
title: Uso de FILESTREAM y FileTable con grupos de disponibilidad
description: Pasos para usar FILESTREAM o FileTable con las bases de datos que participan en un grupo de disponibilidad Always On.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- FileTables [SQL Server], Availability Groups
- FILESTREAM [SQL Server], Availability Groups
- Availability Groups [SQL Server], interoperability
ms.assetid: fdceda9a-a9db-4d1d-8745-345992164a98
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 7019f87f5b6476c273a20008fb6d081f816a4e2e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353877"
---
# <a name="use-filestream-and-filetable-with-always-on-availability-groups"></a>Uso de FILESTREAM y FileTable con grupos de disponibilidad Always On

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  Este tema contiene información sobre cómo usar las características FILESTREAM y FileTable con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
 Se admite toda la funcionalidad de FILESTREAM. Después de una conmutación por error, los datos de FILESTREAM son accesibles tanto en las réplicas secundarias legibles como en la nueva réplica principal.  
  
 Se admite parcialmente la funcionalidad de FileTable. Después de una conmutación por error, es posible tener acceso a los datos de FileTable en la réplica principal, pero no en las réplicas secundarias legibles.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   Para poder agregar una base de datos que utiliza FILESTREAM, con o sin FileTable, a un grupo de disponibilidad, asegúrese de que FILESTREAM está habilitada en cada instancia de servidor que hospeda una replicación de disponibilidad para el grupo de disponibilidad. Para obtener más información, consulte [Enable and Configure FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md).  
  
##  <a name="using-virtual-network-names-vnns-for-filestream-and-filetable-access"></a><a name="vnn"></a> Usar nombres de red virtual (VNN) para el acceso de FILESTREAM y FileTable  
 Al habilitar FILESTREAM en una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], se crea un recurso compartido de nivel de instancia para proporcionar acceso a los datos de FILESTREAM. Puede obtener acceso a este recurso compartido mediante el nombre de equipo en el siguiente formato:  
  
 `\\<computer_name>\<filestream_share_name>`  
  
 Pero, en un grupo de disponibilidad AlwaysOn, el nombre del equipo está virtualizado con un nombre de red virtual (VNN). Si el equipo es la replicación primaria de un grupo de disponibilidad y las bases de datos del grupo de disponibilidad contienen datos de FILESTREAM, también se crea un recurso compartido de ámbito de VNN para proporcionar acceso a los datos de FILESTREAM. Esto no afecta al acceso de Transact-SQL a los datos de FILESTREAM. Sin embargo, las aplicaciones que usan las API del sistema de archivos tienen que utilizar el recurso compartido de ámbito de VNN, que tiene una ruta de acceso con el siguiente formato:  
  
 `\\<VNN>\<filestream_share_name>`  
  
 Se crea este recurso compartido de ámbito de VNN cuando tiene lugar uno de los siguientes eventos.  
  
-   Agrega una base de datos que contiene datos de FILESTREAM a un grupo de disponibilidad AlwaysOn de la replicación principal. En este caso, el recurso compartido `\\<computer_name>\<filestream_share_name>` ya existe. Se crea el recurso compartido `\\<VNN>\<filestream_share_name>` .  
  
-   Habilita FILESTREAM para el acceso de transmisión por secuencias de E/S de archivo en una replicación primaria que incluye grupos de disponibilidad. Se crean los siguientes recursos compartidos:  
  
    1.  `\\<computer_name>\<filestream_share_name>`  
  
    2.  `\\<VNN1>\<filestream_share_name>` para el grupo de disponibilidad 1.  
  
    3.  `\\<VNN2>\<filestream_share_name>` para el grupo de disponibilidad 2.  
  
 Estos recursos compartidos de ámbito de VNN también se propagan a todas las réplicas secundarias.  
  
 Si la base de datos que contiene datos de FILESTREAM o FileTable pertenece a un grupo de disponibilidad AlwaysOn:  
  
-   Las funciones FILESTREAM y FileTable aceptan o devuelven nombres de red virtual (VNN) en lugar de nombres de equipo. Para obtener más información sobre estas funciones, vea [Funciones FILESTREAM y FileTable &#40;Transact-SQL&#41;](../../../relational-databases/system-functions/filestream-and-filetable-functions-transact-sql.md).  
  
-   Todo acceso a los datos de FILESTREAM o FileTable a través de las API del sistema de archivos debe utilizar VNN en lugar de nombres de equipo.  
  
 Si la aplicación intenta tener acceso al recurso compartido mediante el nombre del equipo en formato `\\<computer_name>\<filestream_share_name>` cuando la base de datos forma parte de un grupo de disponibilidad, se genera un error.  
  
 Si la aplicación intenta tener acceso al recurso compartido con una ruta de acceso de ámbito de VNN cuando la base de datos no forma parte de un grupo de disponibilidad, la solicitud puede realizarse correctamente. En este caso, el nombre de red virtual se resuelve en el nombre de equipo. Sin embargo, este uso no se recomienda en absoluto, ya que la ruta de acceso de ámbito de VNN dejará de funcionar si se quita el grupo de disponibilidad.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Enable and Configure FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md)  
  
-   [Habilitar los requisitos previos de FileTables](../../../relational-databases/blob/enable-the-prerequisites-for-filetable.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado  
 Ninguno.  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
