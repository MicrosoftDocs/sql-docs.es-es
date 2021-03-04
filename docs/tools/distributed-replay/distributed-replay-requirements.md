---
description: Distributed Replay Requirements
title: Requisitos de Distributed Replay
titleSuffix: SQL Server Distributed Replay
ms.prod: sql
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/18/2018
ms.openlocfilehash: 237a2ca3b7488688162b38ea3e7f916e2a4d3c6d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836892"
---
# <a name="distributed-replay-requirements"></a>Distributed Replay Requirements

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Antes de utilizar la característica Distributed Replay de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , tenga en cuenta los requisitos de productos que se describen en este tema.  

## <a name="input-trace-requirements"></a>Requisitos del seguimiento de entrada  

Para reproducir correctamente los datos de seguimiento, deben cumplir los requisitos de versión y el formato, y contener las columnas y eventos necesarios.  

### <a name="input-trace-versions"></a>Versiones de seguimiento de entrada  

Distributed Replay admite los datos de seguimiento de entrada que se recopilan en las siguientes versiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  

- [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]   Actualización acumulada 1 y posteriores. Consulte [Actualizaciones acumulativas de SQL Server 2017](https://aka.ms/sql2017cu).
- [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]   
- [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
- [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
- [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]    
- [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]  
  
### <a name="input-trace-formats"></a>Formatos de seguimiento de entrada

Los datos de seguimiento de entrada pueden estar en cualquiera de los siguientes formatos:  

- Un archivo de seguimiento con la extensión `.trc` .  

- Un conjunto de archivos de seguimiento de sustitución que cumplen la convención de nomenclatura de sustitución incremental de archivos, por ejemplo: `<TraceFile>.trc`, `<TraceFile>_1.trc`, `<TraceFile>_2.trc`, `<TraceFile>_3.trc`… `<TraceFile>_n.trc`.

### <a name="input-trace-events-and-columns"></a>Columnas y eventos de seguimiento de entrada

Los datos de seguimiento de entrada deben contener columnas y eventos específicos que se reproduzcan mediante Distributed Replay. La plantilla **TSQL_Replay** de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] contiene todas las columnas y eventos necesarios, además de información adicional. Para obtener más información acerca de esa plantilla, vea [Replay Requirements](../../tools/sql-server-profiler/replay-requirements.md).  
  
> [!WARNING]
> Si no usa la plantilla **TSQL_Replay** para capturar los datos de seguimiento de entrada, o si los requisitos de seguimiento de entrada no se cumplen, puede obtener resultados de la reproducción inesperados.  

También puede crear una plantilla de seguimiento personalizada y utilizarla para reproducir los eventos mediante Distributed Replay, siempre que contenga los eventos siguientes:
  
- Audit Login  
  
- Audit Logout  
  
- ExistingConnection  
  
- RPC Output Parameter  
  
- RPC:Completed  
  
- RPC:Starting  
  
- SQL:BatchCompleted  
  
- SQL:BatchStarting  
  
 Si reproduce los cursores de servidor, los siguientes eventos también se requieren:  
  
- CursorClose  
  
- CursorExecute  
  
- CursorOpen  
  
- CursorPrepare  
  
- CursorUnprepare  
  
 Si está reproduciendo instrucciones de SQL preparadas en el servidor, los siguientes eventos también se requieren:  
  
- Exec Prepared SQL  
  
- Prepare SQL  
  
 Todos los datos de seguimiento de entrada deben contener las columnas siguientes:  
  
- Clase de eventos  
  
- EventSequence  
  
- TextData  
  
- Nombre de la aplicación  
  
- LoginName  
  
- DatabaseName  
  
- Identificador de base de datos  
  
- HostName  
  
- Binary Data  
  
- SPID  
  
- Hora de inicio  
  
- EndTime  
  
- IsSystem  
  
## <a name="supported-input-trace-and-target-server-combinations"></a>Combinaciones admitidas de servidor de destino y seguimiento de entrada  

En la tabla siguiente se enumeran las versiones admitidas de los datos de seguimiento y, para cada una, las versiones admitidas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en las que los datos se pueden reproducir.  
  
|Versión de los datos de seguimiento de entrada|Versiones admitidas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para la instancia del servidor de destino|  
|---------------------------------|------------------------------------------------------------------------------------|  
|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)],[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
  
## <a name="operating-system-requirements"></a>Requisitos del sistema operativo

Los sistemas operativos admitidos para ejecutar la herramienta de administración y los servicios del controlador y el cliente son los mismos que para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener más información sobre qué sistemas operativos son compatibles con la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vea [Requisitos de hardware y software para instalar SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).  

Las características de Distributed Replay se admiten en sistemas operativos basados en x86 y en x64. Para los sistemas operativos basados en x64, solo se admite el modo Windows sobre Windows (WOW).  

## <a name="installation-limitations"></a>Limitaciones de la instalación

Un equipo solo puede tener instalada una única instancia de cada característica Distributed Replay. La siguiente tabla muestra cuántas instalaciones de cada característica se permiten en un único entorno de Distributed Replay.  
  
|Característica Distributed Replay|Instalaciones máximas por cada entorno de reproducción|  
|--------------------------------|--------------------------------------------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay|1|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay|16 (equipos físicos o virtuales)|  
|Herramienta de administración|Sin límite|  
  
> [!NOTE]  
>  Aunque solo se puede instalar una instancia de la herramienta de administración en un solo equipo, puede iniciar varias instancias de la herramienta de administración. Los comandos emitidos desde varias herramientas de administración se resuelven en el mismo orden en el que se reciben.  
  
## <a name="data-access-provider"></a>Proveedor de acceso a datos

 Distributed Replay solamente admite el proveedor de acceso a datos ODBC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  

## <a name="target-server-preparation-requirements"></a>Requisitos de preparación del servidor de destino  

Se recomienda que el servidor de destino se encuentre en un entorno de prueba. Para reproducir los datos de seguimiento en otra instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] diferente a donde se registró originalmente, asegúrese de que se ha realizado lo siguiente en el servidor de destino:  

- Todos los inicios de sesión y los usuarios que se encuentran en los datos de seguimiento deben estar presentes en la misma base de datos del servidor de destino.  

- Todos los inicios de sesión y los usuarios en el servidor destino deben tener los mismos permisos que tenían en el servidor original.  

- Los Id. de base de datos del destino deben ser los mismos que los del origen. Sin embargo, si no son los mismos, se puede realizar la coincidencia basándose en **DatabaseName** , si está presente en el seguimiento.

- La base de datos predeterminada de cada inicio de sesión contenido en los datos de seguimiento debe establecerse (en el servidor de destino) en la base de datos de destino respectiva del inicio de sesión. Por ejemplo, los datos de seguimiento que se van a reproducir contienen la actividad del inicio de sesión, **Fred**, en la base de datos **Fred_Db** de la instancia original de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Por tanto, en el servidor de destino, la base de datos predeterminada del inicio de sesión, **Fred**, debe establecerse en la base de datos que coincida con **Fred_Db** (aunque el nombre de la base de datos sea diferente). Para establecer la base de datos predeterminada del inicio de sesión, utilice el procedimiento almacenado del sistema `sp_defaultdb` .  
  
 La reproducción de eventos asociados a inicios de sesión que faltan o que son incorrectos tendrá como resultado errores de reproducción, pero la operación de reproducción continuará.  
  
## <a name="see-also"></a>Consulte también

- [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)
- [Seguridad de Distributed Replay](../../tools/distributed-replay/distributed-replay-security.md)
- [Install Distributed Replay - Overview (Instalar Distributed Replay: información general)](../../tools/distributed-replay/install-distributed-replay-overview.md)