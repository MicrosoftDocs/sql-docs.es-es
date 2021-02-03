---
title: Administrar el tamaño del archivo de registro de transacciones | Microsoft Docs
description: Obtenga información sobre cómo supervisar el tamaño del registro de transacciones de SQL Server, reducir y ampliar un registro, optimizar la tasa de crecimiento del registro de tempdb y controlar el crecimiento del registro de transacciones.
ms.custom: ''
ms.date: 01/05/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- transaction logs [SQL Server], size management
- manage log size
- log size, manage
ms.assetid: 3a70e606-303f-47a8-96d4-2456a18d4297
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 699052eedf2a32faac9f3bf849f5f4aaa4c43b4c
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237698"
---
# <a name="manage-the-size-of-the-transaction-log-file"></a>Administrar el tamaño del archivo de registro de transacciones
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
En este tema se incluye información sobre cómo supervisar el tamaño de un registro de transacciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], reducir el registro de transacciones, agregar o ampliar un archivo de registro de transacciones, optimizar la tasa de crecimiento del registro de transacciones **tempdb** y controlar el crecimiento de un archivo de registro de transacciones.  

##  <a name="monitor-log-space-use"></a><a name="MonitorSpaceUse"></a>Supervisión del uso del espacio del registro  
Supervise el uso del espacio del registro mediante [sys.dm_db_log_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md). Este DMV devuelve información sobre la cantidad de espacio del registro actualmente en uso e indica cuándo es necesario el truncamiento del registro de transacciones. 

Para obtener información sobre el tamaño actual del archivo de registro, su tamaño máximo y la opción de crecimiento automático de este archivo, también puede usar las columnas **size**, **max_size** y **growth** de ese archivo de registro en [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md).  
  
> [!IMPORTANT]
> Evite la sobrecarga del disco del registro. Asegúrese de que el almacenamiento del registro puede soportar la [IOPS](https://wikipedia.org/wiki/IOPS) y los requisitos de latencia baja para la carga de transacciones. 
  
##  <a name="shrink-log-file-size"></a><a name="ShrinkSize"></a> Reducir el tamaño del archivo de registro  
 Para reducir el tamaño físico de un archivo de registro físico, debe reducir el archivo de registro. Esto es útil si sabe que un archivo de registro de transacciones contiene espacio que no se ha utilizado. Puede reducir un archivo de registro siempre que la base de datos esté en línea y haya al menos un [archivo de registro virtual (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) libre. En algunos casos, no será posible reducir el registro hasta el siguiente truncamiento del registro.  
  
> [!NOTE]
> Los factores que mantienen activos los [VLF](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) por un periodo prolongado de tiempo, como puede ser una transacción de ejecución prolongada, pueden restringir la reducción del registro o incluso impedirla completamente. Para obtener información, vea [Factores que pueden ralentizar el truncamiento del registro](../../relational-databases/logs/the-transaction-log-sql-server.md#FactorsThatDelayTruncation).  
  
Con la reducción de un archivo de registro se quitan uno o varios [VLF](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) que no contienen ninguna parte del registro lógico (es decir, los *VLF inactivos*). Cuando se reduce un archivo de registro de transacciones, se quitan VLF inactivos del final del archivo de registro para reducirlo aproximadamente al tamaño de destino. 

> [!IMPORTANT]
> Antes de reducir el registro de transacciones, tenga en cuenta los [Factores que pueden ralentizar el truncamiento del registro](../../relational-databases/logs/the-transaction-log-sql-server.md#FactorsThatDelayTruncation). Si se requiere el espacio de almacenamiento de nuevo después de reducir un registro, el registro de transacciones volverá a crecer y esto implicará una sobrecarga de rendimiento durante las operaciones de ampliación de registro. Para obtener más información, vea [Recomendaciones](#Recommendations) en este tema.
  
 **Reducir un archivo de registro (sin reducir los archivos de base de datos)**  
  
-   [DBCC SHRINKFILE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md)  
  
-   [Reducir un archivo](../../relational-databases/databases/shrink-a-file.md)  
  
 **Supervisar los eventos de reducción de un archivo de registro**  
  
-   [Log File Auto Shrink Event Class](../../relational-databases/event-classes/log-file-auto-shrink-event-class.md).  
  
 **Supervisar el espacio del registro**  
  
-   [sys.dm_db_log_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md)  
  
-   [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) (Vea las columnas **size**, **max_size** y **growth** de los archivos de registro).  
  
##  <a name="add-or-enlarge-a-log-file"></a><a name="AddOrEnlarge"></a> Agregar o ampliar un archivo de registro  
Puede obtener espacio al ampliar el archivo de registro existente (si el espacio en disco lo permite) o al agregar un archivo de registro a la base de datos, normalmente en otro disco. Un archivo de registro de transacciones es suficiente, a menos que se esté agotando el espacio del registro y que el espacio en disco también se esté agotando en el volumen que contiene el archivo de registro.   
  
-   Para agregar un archivo de registro a la base de datos, use la cláusula `ADD LOG FILE` de la instrucción `ALTER DATABASE`. El hecho de agregar un archivo de registro permite que crezca el existente.  
-   Para aumentar el archivo de registro, use la cláusula `MODIFY FILE` de la instrucción `ALTER DATABASE`, especificando la sintaxis de `SIZE` y `MAXSIZE`. Para obtener más información, vea [Opciones File y Filegroup de ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).  

Para obtener más información, vea [Recomendaciones](#Recommendations) en este tema.
    
##  <a name="optimize-tempdb-transaction-log-size"></a><a name="tempdbOptimize"></a> Optimizar el tamaño del registro de transacciones tempdb  
 Al reiniciar una instancia del servidor se devuelve el tamaño del registro de transacciones de la base de datos **tempdb** a su tamaño original, antes del crecimiento automático. Esto puede reducir el rendimiento del registro de transacciones de **tempdb** . 
 
 Para evitar esta sobrecarga, aumente el tamaño del registro de transacciones de **tempdb** después de iniciar o reiniciar la instancia de servidor. Para obtener más información, consulte [tempdb Database](../../relational-databases/databases/tempdb-database.md).  
  
##  <a name="control-transaction-log-file-growth"></a><a name="ControlGrowth"></a> Controlar el crecimiento de un archivo de registro de transacciones  
 Use la instrucción [Opciones File y Filegroup de ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md) para administrar el crecimiento de un archivo de registro de transacciones. Tenga en cuenta lo siguiente:  
  
-   Para cambiar el tamaño del archivo actual en unidades de KB, MB, GB y TB, use la opción `SIZE`.  
-   Para cambiar el incremento de crecimiento, use la opción `FILEGROWTH`. El valor 0 indica que el aumento automático se establece en OFF y no se permite ningún espacio adicional.  
-   Para controlar el máximo el tamaño de un archivo de registro en unidades de KB, MB, GB y TB, o establecer el crecimiento en UNLIMITED, use la opción `MAXSIZE`.  

Para obtener más información, vea [Recomendaciones](#Recommendations) en este tema.

## <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones
Estas son algunas recomendaciones generales referentes a los archivos de registro de transacciones:

-   El incremento de crecimiento automático del registro de transacciones, según lo establecido por la opción `FILEGROWTH`, debe ser lo suficientemente grande como para anticiparse a las necesidades de las transacciones de la carga de trabajo. El incremento del crecimiento de un archivo de registro debe ser lo suficientemente grande para evitar una expansión frecuente. Un buen punto de referencia para ajustar correctamente el tamaño de un registro de transacciones es supervisar la cantidad de registro ocupada durante:
    -  El tiempo necesario para ejecutar una copia de seguridad completa, porque no se pueden realizar copias de seguridad del registro hasta que termine.
    -  El tiempo necesario para las operaciones de mantenimiento de índice más grandes.
    -  El tiempo necesario para ejecutar el lote más grande de una base de datos.

-   Al establecer **crecimiento automático** para archivos de datos y de registro mediante la opción `FILEGROWTH`, es recomendable establecerlo en **tamaño** en lugar de en **porcentaje**. De esta forma, se permite un mejor control en la proporción de crecimiento, ya que los porcentajes son una unidad de medida sin límite de crecimiento.
    -  Tenga en cuenta que los registros de transacciones no pueden aprovechar la [inicialización instantánea de archivos](../../relational-databases/databases/database-instant-file-initialization.md), por lo que los tiempos de crecimiento de registro extendido son especialmente importantes. 
    -  Como práctica recomendada, no establezca el valor de la opción `FILEGROWTH` por encima de 1024 MB para registros de transacciones. Los valores predeterminados de la opción `FILEGROWTH` son los siguientes:  
  
      |Versión|Valores predeterminados|  
      |-------------|--------------------|  
      |A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|Datos: 64 MB. Archivos de registro: 64 MB.|  
      |A partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|Datos: 1 MB. Archivos de registro: 10 %.|  
      |Antes de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|Datos: 10 %. Archivos de registro: 10 %.|  

-   Un aumento de crecimiento pequeño puede generar demasiados [VLF](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) pequeños y puede reducir el rendimiento. Para determinar la distribución óptima de VLF para el tamaño de registro de transacciones actual de todas las bases de datos en una instancia determinada, así como los incrementos de tamaño necesarios para conseguir el tamaño requerido, consulte este [script](https://github.com/Microsoft/tigertoolbox/tree/master/Fixing-VLFs).

-   Un aumento de crecimiento grande puede generar demasiados [VLF](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) pequeños y grandes, y también puede afectar al rendimiento. Para determinar la distribución óptima de VLF para el tamaño de registro de transacciones actual de todas las bases de datos en una instancia determinada, así como los incrementos de tamaño necesarios para conseguir el tamaño requerido, consulte este [script](https://github.com/Microsoft/tigertoolbox/tree/master/Fixing-VLFs). 

-   Incluso con el crecimiento automático habilitado, puede recibir un mensaje de que el registro de transacciones está lleno, si no puede crecer lo suficientemente rápido para satisfacer las necesidades de la consulta. Para obtener más información sobre cómo cambiar el aumento del crecimiento, vea [Opciones File y Filegroup de ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).

-   Aunque tenga varios archivos de registro en una base de datos, el rendimiento no mejorará, ya que los archivos de registro de transacciones no usan el [relleno proporcional](../../relational-databases/pages-and-extents-architecture-guide.md#ProportionalFill), como los archivos de datos de un mismo grupo de archivos.  

-   Puede configurar los archivos de registro para que se reduzcan automáticamente. Esto **no se recomienda** y la propiedad de base de datos **auto_shrink** está establecida en FALSE de manera predeterminada. Si **auto_shrink** está establecida en TRUE, el proceso de reducción automática solo reduce el tamaño de un archivo cuando más del 25 % de su espacio está sin usar. 
    -   El tamaño del archivo se reduce hasta un tamaño en el que solo el 25% del archivo corresponde al espacio sin utilizar o hasta el tamaño original del archivo (el que sea mayor). 
    -   Para obtener información sobre cómo cambiar la configuración de la propiedad **auto_shrink**, vea [Ver o cambiar las propiedades de una base de datos](../../relational-databases/databases/view-or-change-the-properties-of-a-database.md) y [Opciones de ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md). 
  
## <a name="see-also"></a>Consulte también  
[BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
[Solucionar problemas de un registro de transacciones lleno &#40;Error 9002 de SQL Server&#41;](../../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md)    
[Copias de seguridad de registros de transacciones en Guía de arquitectura y administración de registros de transacciones de SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Backups)    
[Copias de seguridad del registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)    
[Opciones File y Filegroup de ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)
