---
description: Crear un entorno multiservidor
title: Crear un entorno multiservidor
ms.custom: seo-lt-2019
ms.date: 01/30/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, multiserver environments
- master servers [SQL Server], about master servers
- target servers [SQL Server], about target servers
- multiserver environments [SQL Server]
ms.assetid: edc2b60d-15da-40a1-8ba3-f1d473366ee6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 66a3faf1f419bb92aee91ac1621734560fd16a4c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466526"
---
# <a name="create-a-multiserver-environment"></a>Crear un entorno multiservidor
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

La administración multiservidor requiere que se configure un servidor maestro (MSX) y uno o más servidores de destino (TSX). Los trabajos que se van a procesar en todos los servidores de destino se definen primero en el servidor maestro y luego se descargan en los servidores de destino.  
  
De forma predeterminada, el cifrado y la validación de certificados completos mediante la Seguridad de la capa de transporte (TLS), anteriormente conocida como Capa de sockets seguros (SSL), se habilitan para las conexiones entre los servidores maestros y los servidores de destino. Para más información, [Establecer opciones de cifrado en servidores de destino](../../ssms/agent/set-encryption-options-on-target-servers.md).  
  
Si tiene un muchos servidores de destino, no defina el servidor maestro en un servidor de producción con requisitos de rendimiento elevados de otras funciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ya que el tráfico del servidor de destino puede afectar negativamente al rendimiento del servidor de producción. Si también reenvía eventos a este servidor maestro dedicado, puede centralizar la administración en un servidor. Para más información, consulte [Administrar eventos](../../ssms/agent/manage-events.md).  
  
> [!NOTE]  
> Para usar el procesamiento de trabajos multiservidor, la cuenta de servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe ser miembro del rol **TargetServersRole** de la base de datos **msdb** del servidor maestro. El Asistente para servidor maestro agrega automáticamente la cuenta de servicio a este rol como parte del proceso de alta.  
  
## <a name="considerations-for-multiserver-environments"></a>Consideraciones para entornos multiservidor  
  
Considere lo siguiente cuando cree un entorno multiservidor:  
  
-   Utilice la versión más reciente como servidor maestro. Se admiten dos las versiones anteriores y la actual.

-   Cada servidor de destino notifica únicamente a un servidor maestro. Para dar de alta un servidor de destino en otro servidor maestro, primero debe darlo de baja en el servidor maestro actual.  
  
-   Si desea cambiar el nombre de un servidor de destino, debe darlo de baja antes de cambiar el nombre y volver a darlo de alta después del cambio.  
  
-   Si desea anular una configuración multiservidor, debe dar de baja todos los servidores de destino del servidor maestro.  
  
-   SQL Server Integration Services solo admite servidores de destino que tengan la misma versión que el servidor maestro o una superior.  
  
## <a name="related-tasks"></a>Related Tasks  
En los temas siguientes se presentan las tareas habituales para crear un entorno multiservidor.  
  
|Descripción|Tema|  
|---------------|---------|  
|Describe cómo crear un servidor maestro.|[Establecer un servidor maestro](../../ssms/agent/make-a-master-server.md)|  
|Describe cómo crear un servidor de destino.|[Establecer un servidor de destino](../../ssms/agent/make-a-target-server.md)|  
|Describe cómo dar de alta un servidor de destino en un servidor maestro.|[Dar de alta un servidor de destino en un servidor maestro](../../ssms/agent/enlist-a-target-server-to-a-master-server.md)|  
|Describe cómo dar de baja un servidor de destino en un servidor maestro.|[Dar de baja un servidor de destino desde un servidor maestro](../../ssms/agent/defect-a-target-server-from-a-master-server.md)|  
|Describe cómo dar de baja varios servidores de destino en un servidor maestro.|[Dar de baja varios servidores de destino desde un servidor maestro](../../ssms/agent/defect-multiple-target-servers-from-a-master-server.md)|  
|Describe cómo comprobar el estado de un servidor de destino.|[sp_help_targetserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-targetserver-transact-sql.md)<br /><br />[sp_help_targetservergroup (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-targetservergroup-transact-sql.md)|  
  
## <a name="see-also"></a>Consulte también  
[Solucionar problemas de trabajos multiservidor que usan servidores proxy](../../ssms/agent/troubleshoot-multiserver-jobs-that-use-proxies.md)  
