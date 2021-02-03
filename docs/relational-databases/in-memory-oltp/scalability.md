---
title: Escalabilidad | Microsoft Docs
description: Obtenga información sobre las mejoras de escalabilidad en el almacenamiento en disco de las tablas optimizadas para memoria en SQL Server como, por ejemplo, el uso de varios subprocesos para conservar las tablas.
ms.custom: ''
ms.date: 08/27/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: a4891c57-56bb-49f4-9bb5-f11b745279e5
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2b3f4141afeb26c7be406a3c810f2403978f016f
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236124"
---
# <a name="scalability"></a>Escalabilidad
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] contiene mejoras de escalabilidad en el almacenamiento en disco para las tablas optimizadas para memoria. 

## <a name="multiple-threads-to-persist-memory-optimized-tables"></a>Varios subprocesos para conservar las tablas optimizadas para memoria  
  
[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] tenía un subproceso de punto de control único sin conexión que examinaba los cambios del registro de transacciones de las tablas optimizadas para memoria y los conservaba en archivos de punto de control (como archivos delta y de datos). En equipos con mayor número de núcleos, el subproceso de punto de control único sin conexión podría retrasarse.  
  
A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], hay varios subprocesos simultáneos responsables de conservar los cambios en tablas optimizadas para memoria.  
  
## <a name="multi-threaded-recovery"></a>Recuperación multiproceso
En la versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la aplicación del registro como parte de la operación de recuperación era de un solo subproceso. A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], la aplicación del registro es multiproceso.  
  
## <a name="merge-operation"></a>Operación de COMBINACIÓN  
La operación de COMBINACIÓN ahora es multiproceso.  
   
> [!NOTE]
> Se ha deshabilitado la Combinación manual como combinación multiproceso con la intención de soportar la carga. 

## <a name="dynamic-management-views"></a>Vistas de administración dinámica  
Las DMV [sys.dm_db_xtp_checkpoint_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-stats-transact-sql.md) y [sys.dm_db_xtp_checkpoint_files &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-files-transact-sql.md) han cambiado de forma significativa.  

## <a name="storage-management"></a>Administración del almacenamiento
El motor OLTP en memoria continúa utilizando el grupo de archivos optimizados para memoria basado en el grupo de archivos FILESTREAM, pero los archivos individuales se desacoplan de FILESTREAM. Estos archivos están totalmente administrados (como para creación, anulación y recolección de elementos no utilizados) por el motor OLTP en memoria. 

> [!NOTE]
> [DBCC SHRINKFILE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md) no se admite.  
  
## <a name="see-also"></a>Consulte también   
[Creación y administración del almacenamiento de objetos con optimización para memoria](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)     
[Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md)    
[Opciones File y Filegroup de ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)    
