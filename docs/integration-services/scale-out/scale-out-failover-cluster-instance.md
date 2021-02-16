---
title: Compatibilidad con la escalabilidad horizontal para una alta disponibilidad mediante la instancia de clúster de conmutación por error de SQL Server | Microsoft Docs
description: En este artículo se describe cómo configurar Escalabilidad horizontal de SSIS para lograr alta disponibilidad con la instancia de clúster de conmutación por error de SQL Server.
ms.custom: performance
ms.date: 04/10/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 444226f7e0f28a6f587a5f01b1e512f2392e9916
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351961"
---
# <a name="scale-out-support-for-high-availability-via-sql-server-failover-cluster-instance"></a>Scale Out support for high availability via SQL Server failover cluster instance (Compatibilidad con la escalabilidad horizontal para una alta disponibilidad mediante Instancia de clústeres de conmutación por error de SQL Server).

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Para configurar la alta disponibilidad en el lado del servicio principal de escalabilidad horizontal con Instancia de clúster de conmutación por error de SQL Server, realice lo siguiente:

## <a name="1-prerequisites"></a>1. Prerrequisitos
Configure un clúster de conmutación por error de Windows. Vea la entrada de blog [Installing the Failover Cluster Feature and Tools for Windows Server 2012](https://techcommunity.microsoft.com/t5/failover-clustering/installing-the-failover-cluster-feature-and-tools-in-windows/ba-p/371733) (Instalación de las herramientas y la característica de clúster de conmutación por error para Windows Server 2012) para obtener instrucciones. Instale la característica y las herramientas en todos los nodos del clúster.

## <a name="2-install-sql-server-failover-cluster"></a>2. Instalar un clúster de conmutación por error de SQL Server
Instale un clúster de conmutación por error de SQL Server. Vea [Instalación de un clúster de conmutación por error de SQL Server](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md) para obtener instrucciones. Durante la instalación, seleccione Servicios de Motor de base de datos en la página Selección de características. Registre el nombre de red de SQL Server para la futura configuración.

![Nombre de red de SQL](media/sql-network-name.PNG)

Agregue un nodo secundario al clúster de conmutación por error de SQL Server.

## <a name="3-install-scale-out-master-on-the-primary-node"></a>3. Instalación del Servicio principal de escalabilidad horizontal en el nodo principal
Instale Integration Services y el Servicio principal de escalabilidad horizontal en el nodo principal con el asistente para la instalación de instalaciones no agrupadas. 

Durante la instalación, incluya el nombre de red de SQL Server en los nombres comunes del certificado del Servicio principal de escalabilidad horizontal.

> [!NOTE]
> Si quiere conmutar por error SSISDB y el Servicio principal de escalabilidad horizontal por separado, siga [2. Instalación del Servicio principal de escalabilidad horizontal en el nodo principal](scale-out-support-for-high-availability.md#2-install-scale-out-master-on-the-primary-node) para la configuración del Servicio principal de escalabilidad horizontal.

## <a name="4-install-scale-out-master-on-the-secondary-node"></a>4. Instalación del Servicio principal de escalabilidad horizontal en el nodo secundario
Siga [3. Instalación del Servicio principal de escalabilidad horizontal en el nodo secundario](scale-out-support-for-high-availability.md#3-install-scale-out-master-on-the-secondary-node)

## <a name="5-update-the-scale-out-master-service-configuration-file"></a>5. Actualizar el archivo de configuración del servicio de patrón de escalabilidad horizontal
Actualice el archivo de configuración del servicio del patrón de escalabilidad horizontal, \<drive\>:\Archivos de programa\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config, en los nodos principal y secundario. Actualice **SqlServerName** a [nombre de red de SQL Server]//[nombre de instancia] o [nombre de red de SQL Server] para la instancia predeterminada.

## <a name="6-add-scale-out-master-service-to-sql-server-role-in-windows-failover-cluster"></a>6. Agregar el Servicio principal de escalabilidad horizontal al rol de SQL Service del clúster de conmutación por error de Windows
En el Administrador de clústeres de conmutación por error, conéctese al clúster de escalabilidad horizontal. Seleccione Roles en el explorador, haga clic con el botón derecho en el rol de SQL Server y seleccione Agregar recurso, Servicio genérico. 

![Servicio genérico](media/generic-service.PNG)

En el Asistente para nuevo recurso, seleccione el Servicio principal de escalabilidad horizontal y finalice el asistente. 

Conectar el Servicio principal de escalabilidad horizontal.

![Conectar](media/bring-online.PNG)

> [!NOTE]
> Si quiere conmutar por error SSISDB y el Servicio principal de escalabilidad horizontal por separado, siga [7. Configuración del rol del Servicio principal de escalabilidad horizontal del clúster de conmutación por error de Windows](scale-out-support-for-high-availability.md#7-configure-the-scale-out-master-service-role-of-the-windows-server-failover-cluster)

## <a name="7-install-scale-out-workers"></a>7. Instalar los trabajadores de escalabilidad horizontal
Instalar los trabajadores de escalabilidad horizontal en los nodos de trabajo. Durante la instalación, especifique https://[nombre de red de SQL Server]:[puerto maestro] para el punto de conexión principal. 

> [!NOTE]
> Si quiere conmutar por error SSISDB y el Servicio principal de escalabilidad horizontal por separado, especifique el nombre de host DNS del Servicio principal de escalabilidad principal en lugar del nombre de red de SQL Server.

## <a name="8-install-scale-out-worker-client-certificate"></a>8. Instalar el certificado de cliente del trabajador de escalabilidad horizontal
Instale el certificado de cliente del trabajador en todos los nodos del clúster de conmutación por error de SQL Server. Vea [Instalar el certificado de cliente del trabajador de escalado horizontal](walkthrough-set-up-integration-services-scale-out.md#InstallCert).

> [!NOTE]
> El Administrador de escalabilidad horizontal no es compatible con el clúster de conmutación por error de SQL Server. Si usa el Administrador de escalabilidad horizontal para agregar el trabajador de escalabilidad horizontal, deberá instalar manualmente el certificado del trabajador en todos los nodos principales.

## <a name="next-steps"></a>Pasos siguientes
Para más información, consulte los siguientes artículos:
-   [Servicio principal de escalabilidad horizontal de Integration Services (SSIS)](integration-services-ssis-scale-out-master.md)
-   [Trabajo de escalabilidad horizontal de Integration Services (SSIS)](integration-services-ssis-scale-out-worker.md)
