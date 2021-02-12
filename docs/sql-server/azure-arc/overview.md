---
title: SQL Server habilitado para Azure Arc
titleSuffix: ''
description: Administre instancias de SQL Server con SQL Server habilitado para Azure Arc.
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 12/08/2020
ms.topic: conceptual
ms.prod: sql
ms.custom: references_regions
ms.openlocfilehash: 4acc04883d4e4fcc0933078b81dcb8be2a45be89
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023269"
---
# <a name="azure-arc-enabled-sql-server-preview"></a>SQL Server habilitado para Azure Arc (versión preliminar)

SQL Server habilitado para Azure Arc forma parte de Azure Arc para servidores. Extiende los servicios de Azure a instancias de SQL Server hospedadas fuera de Azure en el centro de datos del cliente, en el perímetro o en un entorno de varias nubes.

Para habilitar los servicios de Azure, se debe registrar una instancia de SQL Server en ejecución con Azure Arc mediante Azure Portal y un script de registro. Después del registro, la instancia se representará en Azure como un recurso __SQL Server – Azure Arc__. Las propiedades de este recurso reflejan un subconjunto de los valores de configuración de SQL Server.

SQL Server se puede instalar en una máquina física o virtual con Windows o Linux que esté conectada a Azure Arc a través del agente de Azure Connected Machine. De forma automática, se instala el agente y se registra la máquina como parte del registro de la instancia de SQL Server. El agente de Azure Connected Machine se comunica de forma segura con la salida de Azure Arc a través del puerto TCP 443. Si la máquina se conecta mediante un servidor proxy de HTTP o un firewall para comunicarse a través de Internet, consulte los [requisitos de configuración de red para el agente de Azure Connected Machine](/azure/azure-arc/servers/agent-overview#prerequisites).

La versión preliminar pública de SQL Server habilitado para Azure Arc admite un conjunto de soluciones que requieren la instalación de la extensión de servidor Microsoft Monitoring Agent (MMA) y su conexión a un área de trabajo de Azure Log Analytics para la recopilación de datos y la generación de informes. Estas soluciones incluyen Advanced Data Security con Azure Defender y Azure Sentinel, así como comprobaciones de estado del entorno de SQL con la característica SQL Assessment a petición.

En el siguiente diagrama se ilustra la arquitectura de SQL Server habilitado para Azure Arc.

![Arquitectura de la versión preliminar pública](media/overview/pubic-preview-architecture.png)

## <a name="prerequisites"></a>Requisitos previos

### <a name="supported-sql-versions-and-operating-systems"></a>Versiones de SQL y sistemas operativos compatibles

SQL Server habilitado para Azure Arc admite SQL Server 2012 o una versión posterior si se ejecuta en una de las siguientes versiones del sistema operativo Windows o Linux:

- Windows Server 2012 R2 y versiones posteriores
- Ubuntu 16.04 y 18.04 (x64)
- Red Hat Enterprise Linux (RHEL) 7 (x64) 
- SUSE Linux Enterprise Server (SLES) 15 (x64)

### <a name="required-permissions"></a>Permisos necesarios

Para conectar las instancias de SQL Server y el equipo de hospedaje a Azure Arc, debe tener una cuenta con privilegios a fin de realizar las acciones siguientes:
   * Microsoft.AzureArcData/sqlServerInstances/read
   * Microsoft.AzureArcData/sqlServerInstances/write
   * Microsoft.HybridCompute/machines/read
   * Microsoft.HybridCompute/machines/write
   * Microsoft.GuestConfiguration/guestConfigurationAssignments/read

Para lograr una seguridad óptima, se recomienda crear un rol personalizado en Azure que tenga los permisos mínimos enumerados. Para obtener información sobre cómo crear un rol personalizado en Azure con estos permisos, consulte la [información general sobre roles personalizados](/azure/active-directory/users-groups-roles/roles-custom-overview). Para agregar una asignación de roles, consulte [Incorporación o eliminación de asignaciones de roles de Azure mediante Azure Portal](/azure/role-based-access-control/role-assignments-portal) o [Incorporación o eliminación de asignaciones de roles de Azure mediante la CLI de Azure](/azure/role-based-access-control/role-assignments-cli).

### <a name="azure-subscription-and-service-limits"></a>Límites del servicio y la suscripción de Azure

Antes de configurar sus máquinas e instancias de SQL Server con Azure Arc, revise los [límites de suscripción](/azure/azure-resource-manager/management/azure-subscription-service-limits#subscription-limits) y los [límites del grupo de recursos](/azure/azure-resource-manager/management/azure-subscription-service-limits#resource-group-limits) de Azure Resource Manager para planear el número de máquinas que se van a conectar.

### <a name="networking-configuration-and-resource-providers"></a>Configuración de red y proveedores de recursos

Revise [la configuración de red, la seguridad de la capa de transporte y los proveedores de recursos](/azure/azure-arc/servers/agent-overview#prerequisites) necesarios para el agente de Azure Connected Machine.

El proveedor de recursos `Microsoft.AzureArcData` es necesario para conectar las instancias de SQL Server a Azure Arc. Vea las instrucciones de registro del proveedor de recursos en la sección [Requisitos previos](connect.md#prerequisites).

Si ya tiene instancias de SQL Server conectadas a Azure Arc, siga estos pasos para migrar los recursos de **SQL Server: Azure Arc** existentes al nuevo espacio de nombres.

### <a name="supported-azure-regions"></a>Regiones de Azure compatibles

La versión preliminar pública está disponible en las siguientes regiones:
- Este de EE. UU.
- Este de EE. UU. 2
- Oeste de EE. UU. 2
- Este de Australia
- Sudeste de Asia
- Norte de Europa
- Oeste de Europa
- Sur de Reino Unido

## <a name="next-steps"></a>Pasos siguientes

- [Conexión de la instancia de SQL Server a Azure Arc](connect.md)
- [Configuración de la instancia de SQL Server para la comprobación periódica del estado del entorno mediante SQL Assessment a petición](assess.md)
- [Configuración de Advanced Data Security para la instancia de SQL Server](configure-advanced-data-security.md)