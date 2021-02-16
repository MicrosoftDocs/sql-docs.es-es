---
title: Descarga de copias de seguridad a una réplica secundaria de un grupo de disponibilidad
description: Obtenga información sobre los diferentes tipos de copia de seguridad admitidos para la descarga de copias de seguridad en una réplica secundaria de un grupo de disponibilidad AlwaysOn.
ms.custom: seo-lt-2019
ms.date: 09/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- backup priority
- backup on secondary replicas
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], backup on secondary replicas
- active secondary replicas [SQL Server], backup on
- automated backup preference
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 82afe51b-71d1-4d5b-b20a-b57afc002405
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4cefec3a8ff6ed45c00b09a339d06c3d022bdc1c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339285"
---
# <a name="offload-supported-backups-to-secondary-replicas-of-an-availability-group"></a>Descarga de copias de seguridad admitidas en las réplicas secundarias de un grupo de disponibilidad
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Las funciones secundarias activas de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] incluyen compatibilidad para realizar copias de seguridad en las réplicas secundarias. Las operaciones de copia de seguridad pueden provocar una demanda significativa de E/S y CPU (con la compresión de copia de seguridad). La descarga de las copias de seguridad en una réplica secundaria sincronizada o en proceso de sincronización permite utilizar los recursos de la instancia del servidor que hospeda la réplica principal para las cargas de trabajo de nivel 1.  

> [!NOTE]  
>  Las instrucciones RESTORE no se permiten en las bases de datos principales o secundarias de un grupo de disponibilidad.  
  
 
##  <a name="backup-types-supported-on-secondary-replicas"></a><a name="SupportedBuTypes"></a> Tipos de copia de seguridad admitidos en réplicas secundarias  
  
-   **BACKUP DATABASE** solo admite copias de seguridad completas de solo copia de bases de datos, archivos o grupos de archivos cuando se ejecuta en réplicas secundarias. Las copias de seguridad de solo copia no afectan a la cadena de registros ni borran el mapa de bits diferencial.  
  
-   Las copias de seguridad diferenciales no se admiten en las réplicas secundarias.

-   Actualmente no se admiten copias de seguridad simultáneas, como la ejecución de una copia de seguridad del registro de transacciones en la réplica principal mientras se ejecuta una copia de seguridad de base de datos completa en la réplica secundaria. 
  
-   **BACKUP LOG** solo admite copias de seguridad de registros periódicas (la opción COPY_ONLY no se admite para las copias de seguridad de registros en réplicas secundarias).  
  
     Se garantiza una cadena de registro coherente entre las copias de seguridad de registros realizaron en cualquiera de las réplicas (principal o secundaria), con independencia de su modo de disponibilidad (confirmación sincrónica o asincrónica).  
  
-   Para realizar una copia de seguridad de una base de datos secundaria, una réplica secundaria debe poder comunicarse con la réplica principal y su estado debe ser **SYNCHRONIZED** o **SYNCHRONIZING**.  

En un grupo de disponibilidad distribuido se pueden realizar copias de seguridad en réplicas secundarias del mismo grupo de disponibilidad que la réplica principal activa o bien en la réplica principal de cualquier grupo de disponibilidad secundario. No se pueden realizar copias de seguridad en las réplicas secundarias de un grupo de disponibilidad secundario, dado que las réplicas secundarias solo se comunican con la réplica principal en su propio grupo de disponibilidad. Solo pueden realizar operaciones de copia de seguridad las réplicas que se comunican directamente con la réplica principal global.

##  <a name="configuring-where-backup-jobs-run"></a><a name="WhereBuJobsRun"></a> Configurar dónde se ejecutan los trabajos de copia de seguridad  
 La realización de copias de seguridad en una réplica secundaria para descargar la carga de trabajo de copias de seguridad del servidor de producción principal es un gran ventaja. Sin embargo, realizar copias de seguridad en réplicas secundarias agrega una gran complejidad al proceso de determinar dónde deben ejecutarse los trabajos de copia de seguridad. Para solucionar este problema, configure dónde se han de ejecutar los trabajos de copia de seguridad del modo siguiente:  
  
1.  Configure el grupo de disponibilidad para que se especifiquen las réplicas de disponibilidad donde preferiría que se realizasen las copias de seguridad. Para obtener más información, vea los parámetros *AUTOMATED_BACKUP_PREFERENCE* y *BACKUP_PRIORITY* en [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/create-availability-group-transact-sql.md) o [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md).  
  
2.  Cree los trabajos de copia de seguridad incluidos en script para cada base de datos de disponibilidad de cada instancia de servidor que hospeda una réplica de disponibilidad que es candidata para realizar copias de seguridad. Para obtener más información, vea la sección "Seguimiento: después de configurar la copia de seguridad en las réplicas secundarias" de [Configurar la copia de seguridad en réplicas de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
 **Para configurar la copia de seguridad en las réplicas secundarias**  
  
-   [Configurar la copia de seguridad en réplicas de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
 **Para determina sir la réplica actual es la réplica de copia de seguridad preferida**  
  
-   [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)  
  
 **Para crear un trabajo de copia de seguridad**  
  
-   [Usar el Asistente para planes de mantenimiento](../../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)  
  
-   [Implementar trabajos](../../../ssms/agent/implement-jobs.md)  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Copias de seguridad de solo copia &#40;SQL Server&#41;](../../../relational-databases/backup-restore/copy-only-backups-sql-server.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)  
  
  
