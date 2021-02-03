---
description: MSSQLSERVER_833
title: MSSQLSERVER_833 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 833 (Database Engine error)
ms.assetid: 14129cc4-be80-4772-9e3f-0e5da4d0696b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d562e3ffcde77c706d0cf56a92f188b895eeb457
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197876"
---
# <a name="mssqlserver_833"></a>MSSQLSERVER_833
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sql-asdbmi.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|833|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|BUF_LONG_IO|  
|Texto del mensaje|SQL Server ha detectado %d instancias de solicitudes de E/S que están tardando más de %d segundos en completarse en el archivo [%ls] de la base de datos `[%ls] (%d)`.  El identificador de archivo del SO es 0x%p.  El desplazamiento de la operación de E/S más reciente y más larga es: %#016I64x.|  
  
## <a name="explanation"></a>Explicación  
Este mensaje indica que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha emitido una solicitud de lectura o escritura desde el disco y que la solicitud ha tardado más de 15 segundos en volver. Este error ha sido notificado por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e indica un problema con el subsistema de E/S. También podría observar otros síntomas asociados a este mensaje: tiempos de espera elevados para esperas PAGEIOLATCH, advertencias o errores en el registro de eventos del sistema, indicaciones de problemas de latencia de disco en los contadores del monitor de sistema. Supervise [sys.dm_io_virtual_file_stats](../system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql.md) y elija la capa de almacenamiento y las IOPS adecuadas para el rendimiento de almacenamiento. 
  
### <a name="possible-causes"></a>Causas posibles  
Este problema puede producirse debido a problemas de rendimiento del sistema operativo, errores de hardware y de firmware, problemas de los controladores de dispositivos o intervención de los controladores de filtro en el proceso de E/S o la ruta de acceso de almacenamiento de los archivos de base de datos. SQL Server registra la hora en que se inició una solicitud de E/S y la hora en que se completó la E/S. Si esa diferencia es de 15 segundos o más, se detecta esta condición. Esto también significa que SQL Server no es la causa de una condición de E/S diferida que este informe describe e informa. Esta condición se conoce como "E/S detenida". La mayoría de las solicitudes de disco se producen dentro de la velocidad típica del disco. Esta velocidad de disco típica se conoce con frecuencia como "tiempo de búsqueda en el disco". El tiempo de búsqueda en disco para la mayoría de los discos estándar se produce en 10 milisegundos o menos. Por lo tanto, 15 segundos es mucho tiempo para que la ruta de acceso de E/S del sistema vuelva a SQL Server. 
  
## <a name="user-action"></a>Acción del usuario  
Solucione este error examinando el registro de eventos del sistema para localizar mensajes de error relacionados con el hardware. Examine también registros específicos de hardware si están disponibles. Debe utilizar los métodos y las técnicas necesarios para determinar la causa del retraso en el sistema operativo, con los controladores o con el hardware de E/S. La resolución de este problema podría implicar la actualización de todos los controladores de dispositivos y firmware o la realización de otros diagnósticos asociados con el sistema de disco. 
  
Use el Monitor de rendimiento para examinar los siguientes contadores:  
  
-   **Average Disk Sec/Transfer**  
  
-   **Average Disk Queue Length**  
  
-   **Current Disk Queue Length**  
  
Por ejemplo, el tiempo de **Average Disk Sec/Transfer** en un equipo que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suele ser inferior a 15 milisegundos. Si el valor de la **Average Disk Sec/Transfer** aumenta, esto indica que el subsistema de E/S no se mantiene al nivel de forma óptima de la demanda de E/S.

También puede usar funciones como [el registro de ETW de Storport](/archive/blogs/ntdebugging/storport-etw-logging-to-measure-requests-made-to-a-disk-unit) para medir la latencia de las solicitudes que se realizan en una unidad de disco. Hay otro kit de solución de problemas de E/S de disco similar disponible como perfil integrado del [Grabador de rendimiento de Windows](/windows-hardware/test/wpt/introduction-to-wpr).
  
> [!NOTE]  
> El acceso al disco puede ralentizarse debido a un programa antivirus. Para aumentar la velocidad de acceso, excluya los archivos de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se especifican en el mensaje de error de las búsquedas de virus activos. Puede usar la [utilidad de línea de comandos fltmc.exe](/windows-hardware/drivers/ifs/development-and-testing-tools#fltmcexe-control-program) para consultar todos los controladores de filtro instalados en el sistema y entender las funciones que realiza en la ruta de acceso de almacenamiento a los archivos de base de datos. 
  
Para obtener más información sobre de los errores de E/S, vea el [capítulo 2 del documento sobre elementos fundamentales de E/S de Microsoft SQL Server ](/previous-versions/sql/sql-server-2005/administrator/cc917726(v=technet.10)) y el artículo de Knowledge Base en [https://support.microsoft.com/kb/897284/en-us](https://support.microsoft.com/kb/897284/en-us).  
