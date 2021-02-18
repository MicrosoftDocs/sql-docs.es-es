---
description: sys.fn_xe_file_target_read_file (Transact-SQL)
title: sys.fn_xe_file_target_read_file (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_xe_file_target_read_file_TSQL
- fn_xe_file_target_read_file
- sys.fn_xe_file_target_read_file_TSQL
- sys.fn_xe_file_target_read_file
dev_langs:
- TSQL
helpviewer_keywords:
- extended events [SQL Server], functions
- fn_xe_file_target_read_file function
- sys.fn_xe_file_target_read_file function
ms.assetid: cc0351ae-4882-4b67-b0d8-bd235d20c901
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: daa5c2bb81ea9f89ebf415e242e881732ad08389
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100338101"
---
# <a name="sysfn_xe_file_target_read_file-transact-sql"></a>sys.fn_xe_file_target_read_file (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Lee los archivos creados en el destino de archivo asincrónico de Eventos extendidos. Se devuelve un evento, en formato XML, por cada fila.  
  
> [!WARNING]  
>  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] aceptan los resultados de seguimiento generados en formato Xel y Xem. [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Los eventos extendidos solo admiten resultados de seguimiento en formato XEL. Es recomendable que use SQL Server Management Studio para leer los resultados de seguimiento en formato XEL.    
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.fn_xe_file_target_read_file ( path, mdpath, initial_file_name, initial_offset )  
```  
  
## <a name="arguments"></a>Argumentos  
 *path*  
 Ruta de acceso a los archivos que se van a leer. la *ruta de acceso* puede contener caracteres comodín e incluir el nombre de un archivo. *path* es **nvarchar (260)**. No existe ningún valor predeterminado. En el contexto de Azure SQL Database, este valor es una dirección URL HTTP a un archivo en Azure Storage.
  
 *mdpath*  
 Ruta de acceso al archivo de metadatos correspondiente al archivo o archivos especificados por el argumento *path* . *mdpath* es **nvarchar (260)**. No existe ningún valor predeterminado. A partir de SQL Server 2016, este parámetro se puede proporcionar como null.
  
> [!NOTE]  
>  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] no requiere el parámetro *mdpath* . Sin embargo, se mantiene para la compatibilidad con versiones anteriores para los archivos de registro generados en versiones anteriores de SQL Server.  
  
 *initial_file_name*  
 Primer archivo que se va a leer de *path*. *initial_file_name* es **nvarchar (260)**. No existe ningún valor predeterminado. Si se especifica **null** como argumento, se leen todos los archivos encontrados en la *ruta de acceso* .  
  
> [!NOTE]  
>  *initial_file_name* y *initial_offset* son argumentos emparejados. Si especifica un valor para cualquiera de ellos, debe especificar un valor para el otro.  
  
 *initial_offset*  
 Se usa para especificar el último desplazamiento leído previamente y omite todos los eventos hasta el desplazamiento (incluido). La enumeración de eventos comienza después del desplazamiento especificado. *initial_offset* es **BIGINT**. Si se especifica **null** como argumento, se leerá el archivo completo.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|module_guid|**uniqueidentifier**|GUID del módulo de eventos. No admite valores NULL.|  
|package_guid|**uniqueidentifier**|GUID del paquete de eventos. No admite valores NULL.|  
|object_name|**nvarchar(256)**|Nombre del evento. No admite valores NULL.|  
|event_data|**nvarchar(max)**|Contenido del evento, en formato XML. No admite valores NULL.|  
|file_name|**nvarchar(260)**|Nombre del archivo que contiene el evento. No admite valores NULL.|  
|file_offset|**bigint**|Desplazamiento del bloque en el archivo que contiene el evento. No admite valores NULL.|  
|timestamp_utc|**datetime2**|**Se aplica a**: [!INCLUDE[sssql17](../../includes/sssql17-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]<br /><br />Fecha y hora (zona horaria UTC) del evento. No admite valores NULL.|  

  
## <a name="remarks"></a>Observaciones  
 La lectura de grandes conjuntos de resultados ejecutando **Sys.fn_xe_file_target_read_file** en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] puede producir un error. Use el modo **resultados a archivo** (**Ctrl + Mayús + F**) para exportar grandes conjuntos de resultados a un archivo y, en su lugar, leer el archivo con otra herramienta.  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-retrieving-data-from-file-targets"></a>A. Recuperar datos de los destinos de archivo  
 En el ejemplo siguiente se obtienen todas las filas de todos los archivos. En este ejemplo, los destinos de archivo y metarchivos se encuentran en la carpeta de seguimientos en la unidad C:\.  
  
```  
SELECT * FROM sys.fn_xe_file_target_read_file('C:\traces\*.xel', 'C:\traces\metafile.xem', null, null);  
```  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de administración dinámica de eventos extendidos](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)   
 [Vistas de catálogo de eventos extendidos &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [Eventos extendidos](../../relational-databases/extended-events/extended-events.md)  
  
  
