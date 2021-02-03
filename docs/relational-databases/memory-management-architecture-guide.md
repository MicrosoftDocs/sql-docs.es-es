---
title: Guía de arquitectura de administración de memoria | Microsoft Docs
description: Obtenga información sobre la arquitectura de administración de memoria en SQL Server, incluidos los cambios en la administración de la memoria en versiones anteriores.
ms.custom: ''
ms.date: 01/09/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- guide, memory management architecture
- memory management architecture guide
- PMO
- Partitioned Memory Objects
- cmemthread
- AWE
- SPA, Single Page Allocator
- MPA, Multi Page Allocator
- memory allocation, SQL Server
- memory pressure, SQL Server
- stack size, SQL Server
- buffer manager, SQL Server
- buffer pool, SQL Server
- resource monitor, SQL Server
ms.assetid: 7b0d0988-a3d8-4c25-a276-c1bdba80d6d5
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 52cfa79cfa88646fea2bdd2d58168cbfdd60e57d
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236599"
---
# <a name="memory-management-architecture-guide"></a>guía de arquitectura de administración de memoria

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

## <a name="windows-virtual-memory-manager"></a>Administrador de memoria virtual de Windows  
El Administrador de memoria virtual (VMM) de Windows asigna las regiones confirmadas de espacio de direcciones a la memoria física disponible.  
  
Para obtener más información sobre la cantidad de memoria física admitida por los distintos sistemas operativos, vea la documentación de Windows sobre [Límites de memoria para versiones de Windows](/windows/desktop/Memory/memory-limits-for-windows-releases).  
  
Los sistemas de memoria virtual permiten una mayor asignación de memoria física, de forma que la proporción de memoria virtual a memoria física puede ser superior a 1:1. Como resultado, los programas más grandes se pueden ejecutar en equipos con una diversidad de configuraciones de memoria física. No obstante, el uso de una cantidad de memoria virtual significativamente superior al promedio combinado de los espacios de trabajo de todos los procesos puede provocar un rendimiento bajo. 

## <a name="sql-server-memory-architecture"></a>Arquitectura de la memoria de SQL Server

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] adquiere y libera memoria de manera dinámica según sea preciso. Normalmente, no es necesario que un administrador especifique la cantidad de memoria que se debe asignar a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], aunque todavía existe esta opción y es necesaria en algunos entornos.

Uno de los principales objetivos de diseño de todo el software de base de datos es minimizar la E/S de disco porque las operaciones de lectura y escritura del disco realizan un uso muy intensivo de los recursos. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] crea un grupo de búferes en la memoria para contener las páginas leídas en la base de datos. Gran parte del código de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] está dedicado a minimizar el número de lecturas y escrituras físicas entre el disco y el grupo de búferes. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] intenta encontrar un equilibrio entre dos objetivos:

* Evitar que el grupo de búferes sea tan grande que todo el sistema se quede con poca memoria.
* Minimizar la E/S física a los archivos de base de datos al maximizar el tamaño del grupo de búferes.

> [!NOTE]
> En un sistema con mucha carga, algunas consultas grandes que necesitan una gran cantidad de memoria para ejecutarse no pueden obtener la cantidad mínima de memoria solicitada y reciben un error de tiempo de espera agotado mientras esperan los recursos de memoria. Para solucionarlo, aumente la [opción Espera de consulta](../database-engine/configure-windows/configure-the-query-wait-server-configuration-option.md). Para una consulta en paralelo, considere la posibilidad de reducir la [opción Grado máximo de paralelismo](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).
 
> [!NOTE]
> En un sistema con mucha carga y mucha presión de la memoria, las consultas con combinaciones de mezcla, orden y mapa de bits en el plan de consulta pueden quitar el mapa de bits si no obtienen la memoria mínima necesaria para dicho mapa de bits. Esto puede afectar al rendimiento de la consulta y, si el proceso de ordenación no cabe en la memoria, puede aumentar el uso de las tablas de trabajo en la base de datos tempdb, lo que hace que tempdb crezca. Para resolver este problema, agregue memoria física u optimice las consultas para que usen otro plan de consulta más rápido.
 
### <a name="providing-the-maximum-amount-of-memory-to-ssnoversion"></a>Proporcionar la cantidad máxima de memoria a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]

Mediante AWE y el privilegio Bloquear páginas en memoria, puede proporcionar las siguientes cantidades de memoria al Motor de base de datos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . 

> [!NOTE]
> En la siguiente tabla se incluye una columna para las versiones de 32 bits que ya no están disponibles.

|Directiva de memoria|32 bits <sup>1</sup> |64 bits|
|-------|-------|-------| 
|Memoria convencional |Todas las ediciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Hasta el límite de espacio de direcciones virtuales del proceso: <br>- 2 GB<br>- 3 GB con el parámetro de arranque /3gb <sup>2</sup> <br>- 4 GB en WOW64 <sup>3</sup> |Todas las ediciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Hasta el límite de espacio de direcciones virtuales del proceso: <br>- 7 TB con la arquitectura IA64 (IA64 no se admite en [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] y versiones superiores)<br>- Sistema operativo máximo con arquitectura x64 <sup>4</sup>
|Mecanismo AWE (permite a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] superar el límite del espacio de direcciones virtuales del proceso en plataformas de 32 bits). |[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ediciones Standard, Enterprise y Developer: el grupo de búferes es capaz de acceder hasta 64 GB de memoria.|No aplicable <sup>5</sup> |
|Privilegio del sistema operativo (OS) Bloquear páginas en la memoria (permite bloquear memoria física e impedir la paginación en el sistema operativo de la memoria bloqueada). <sup>6</sup> |[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ediciones Standard, Enterprise y Developer: necesario para que el proceso de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] use el mecanismo AWE. La memoria asignada a través del mecanismo AWE no se puede paginar. <br> Si se concede este privilegio sin habilitar AWE, no tiene efecto en el servidor. | Solo se debe usar cuando sea necesario, principalmente si hay algún indicio de que el proceso sqlservr se está transfiriendo al almacenamiento auxiliar. En este caso, se informará sobre el error 17890 en el registro de errores, similar al siguiente ejemplo: `A significant part of sql server process memory has been paged out. This may result in a performance degradation. Duration: #### seconds. Working set (KB): ####, committed (KB): ####, memory utilization: ##%.`|

<sup>1</sup> Las versiones de 32 bits no están disponibles a partir de [!INCLUDE[ssSQL14](../includes/sssql14-md.md)].  
<sup>2</sup> /3gb es un parámetro de arranque del sistema operativo.  
<sup>3</sup> WOW64 (Windows on Windows 64) es un modo en el que [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] de 32 bits se ejecuta en un sistema operativo de 64 bits.  
<sup>4</sup> [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Standard Edition admite hasta 128 GB. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise Edition admite el máximo del sistema operativo máximo.  
<sup>5</sup> Tenga en cuenta que la opción sp_configure awe enabled estaba presente en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]de 64 bits, pero se omite.    
<sup>6</sup> Si se concede el privilegio Bloquear páginas en la memoria (LPIM) (con compatibilidad de 32 bits para AWE o directamente en 64 bits), se recomienda establecer también la opción Memoria de servidor máxima. Para obtener más información sobre LPIM, consulte [Opciones de configuración de memoria del servidor](../database-engine/configure-windows/server-memory-server-configuration-options.md#lock-pages-in-memory-lpim).

> [!NOTE]
> Las versiones anteriores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pueden ejecutarse en un sistema operativo de 32 bits. Para acceder a más de 4 gigabytes (GB) de memoria en un sistema operativo de 32 bits, se requieren las extensiones de ventana de dirección (AWE) para administrar la memoria. Esto no es necesario cuando [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se ejecuta en sistemas operativos de 64 bits. Para más información acerca de AWE, consulte [Espacio de dirección del proceso](/previous-versions/sql/sql-server-2008-r2/ms189334(v=sql.105)) y [Administrar la memoria para bases de datos de gran tamaño](/previous-versions/sql/sql-server-2008-r2/ms191481(v=sql.105)) en la documentación de [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)].   

<a name="changes-to-memory-management-starting-2012-11x-gm"></a>

## <a name="changes-to-memory-management-starting-with-sssql11"></a>Cambios en la administración de memoria a partir de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]

En versiones anteriores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ([!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] y [!INCLUDE[ssKilimanjaro](../includes/ssKilimanjaro-md.md)]), la asignación de memoria se realizaba mediante cinco mecanismos diferentes:
-  **Asignador de página única (SPA)** , que incluye solo las asignaciones de memoria menores o iguales a 8 KB en el proceso [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Las opciones de configuración *memoria de servidor máxima (MB)* y *memoria de servidor mínima (MB)* determinaban los límites de la memoria física que podía consumir el SPA. El grupo de búferes era simultáneamente el mecanismo para SPA y el mayor consumidor de asignaciones de página única.
-  **Asignador de varias páginas (MPA)** , para las asignaciones de memoria que solicitan más de 8 KB.
-  **Asignador de CLR**, que incluye las pilas CLR de SQL y las asignaciones globales creadas durante la inicialización de CLR.
-  Asignaciones de memoria para **[pilas de subprocesos](../relational-databases/memory-management-architecture-guide.md#stacksizes)** en el proceso [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].
-  **Asignaciones de Windows directas**, para las solicitudes de asignación de memoria que se hacen directamente a Windows. Esto incluye las asignaciones virtuales directas y de uso de pilas de Windows realizadas por los módulos que se cargan en los procesos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Algunos ejemplos de estas solicitudes de asignaciones de memoria incluyen asignaciones de DLL de procedimiento almacenado extendido, objetos que se crean mediante procedimientos de Automation (llamadas sp_OA) y asignaciones para proveedores de servidores vinculados.

A partir de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], las asignaciones de página única, las asignaciones de varias páginas y las asignaciones de CLR están consolidadas en un **Asignador de páginas de cualquier tamaño**, y se incluye en los límites de memoria controlados por las opciones de configuración *memoria de servidor máxima (MB)* y *memoria de servidor mínima (MB)* . Este cambio proporciona una capacidad de ajuste de tamaño más precisa para todos los requisitos de memoria que pasan por el administrador de memoria de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. 

> [!IMPORTANT]
> Revise cuidadosamente las configuraciones actuales de *memoria de servidor máxima (MB)* y *memoria de servidor mínima (MB)* después de actualizar [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] a [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]. Esto se debe a que, a partir de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], estas configuraciones ahora incluyen y representan más asignaciones de memoria en comparación con versiones anteriores. Estos cambios se aplican tanto a las versiones de 32 como de 64 bits de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] y [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], así como a las versiones de 64 bits de [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] hasta [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)].

En la tabla siguiente se indica si un tipo de asignación de memoria específico está bajo el control de las opciones de configuración *memoria de servidor máxima (MB)* y *memoria de servidor mínima (MB)* :

|Tipo de asignación de memoria| [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] y [!INCLUDE[ssKilimanjaro](../includes/ssKilimanjaro-md.md)]| A partir de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]|
|-------|-------|-------|
|Asignaciones de página única|Sí|Sí, consolidadas bajo las asignaciones de páginas de cualquier tamaño|
|Asignaciones de varias páginas|No|Sí, consolidadas bajo las asignaciones de páginas de cualquier tamaño|
|Asignaciones de CLR|No|Sí|
|Memoria de pilas de subprocesos|No|No|
|Asignaciones directas de Windows|No|No|

A partir de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] podría asignar más memoria que el valor especificado en el valor de memoria de servidor máxima. Esto puede ocurrir cuando el valor de **_Memoria total del servidor (KB_**) ya ha alcanzado la configuración de **_Memoria total del servidor (KB)_** (tal y como se especifica en la memoria de servidor máxima). Si no hay memoria libre contigua suficiente para atender a la demanda de solicitudes de memoria de varias páginas (más de 8 KB) debido a la fragmentación de memoria, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] puede realizar compromisos por encima de lo indicado en vez de rechazar las solicitudes de memoria. 

En cuanto se realiza esta asignación, la tarea en segundo plano *Monitor de recursos* empieza a indicar a todos los consumidores de memoria que liberen la memoria asignada e intenta llevar el valor de *Memoria total del servidor (KB)* por debajo de la especificación de *Memoria total del servidor (KB)* . Por lo tanto, el uso de memoria de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] podría superar brevemente la configuración establecida por memoria de servidor máxima. En esta situación, la lectura del contador de rendimiento de *Memoria total del servidor (KB)* superará el valor de memoria de servidor máxima y de *Memoria total del servidor (KB)* .

Este comportamiento se observa normalmente durante las siguientes operaciones: 
-  Consultas del índice de almacén de columnas grandes.
-  Construcciones o reconstrucciones del índice de almacén de columnas, que usan grandes volúmenes de memoria para realizar las operaciones Hash y Sort.
-  Operaciones de copia de seguridad que requieren grandes búferes de memoria.
-  Operaciones de seguimiento que tienen que almacenar parámetros de entrada grandes.

<a name="#changes-to-memory-management-starting-with-includesssql11includessssql11-mdmd"></a>
## <a name="changes-to-memory_to_reserve-starting-with-sssql11"></a>Cambios en "memory_to_reserve" a partir de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]
En versiones anteriores de SQL Server ([!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] y [!INCLUDE[ssKilimanjaro](../includes/ssKilimanjaro-md.md)]), el administrador de memoria de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] reservaba una parte del espacio de direcciones virtuales (VAS) del proceso para que la usasen el **asignador de varias páginas (MPA)** , el **asignador de CLR**, las asignaciones de memoria para **pilas de subprocesos** en el proceso de SQL Server, y las **asignaciones de Windows directas (DWA)** . Esta parte del espacio de direcciones virtuales también se conoce como región "Mem-To-Leave" o "grupo sin búferes".

El espacio de direcciones virtuales que está reservado para las asignaciones viene determinado por la opción de configuración _**memory\_to\_reserve**_. El valor predeterminado que usa [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] es 256 MB. Para invalidar el valor predeterminado, use el parámetro de inicio *-g* de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Consulte la página de documentación sobre [Opciones de inicio del servicio de motor de base de datos](../database-engine/configure-windows/database-engine-service-startup-options.md) para obtener información sobre el parámetro de inicio *-g*.

Dado que a partir de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] el nuevo asignador de páginas de cualquier tamaño también controla asignaciones superiores a 8 KB, el valor *memory_to_reserve* no incluye las asignaciones de varias páginas. A excepción de este cambio, todo lo demás sigue siendo igual con respecto a esta opción de configuración.

En la tabla siguiente se indica si un tipo específico de la asignación de memoria entra en la región *memory_to_reserve* del espacio de direcciones virtual para el proceso [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]:

|Tipo de asignación de memoria| [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] y [!INCLUDE[ssKilimanjaro](../includes/ssKilimanjaro-md.md)]| A partir de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]|
|-------|-------|-------|
|Asignaciones de página única|No|No, consolidadas bajo las asignaciones de páginas de cualquier tamaño|
|Asignaciones de varias páginas|Sí|No, consolidadas bajo las asignaciones de páginas de cualquier tamaño|
|Asignaciones de CLR|Sí|Sí|
|Memoria de pilas de subprocesos|Sí|Sí|
|Asignaciones directas de Windows|Sí|Sí|

## <a name="dynamic-memory-management"></a><a name="dynamic-memory-management"></a> Administración dinámica de memoria
El comportamiento predeterminado de administración de memoria del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] es adquirir toda la memoria que necesita sin provocar una escasez de memoria en el sistema. El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] lo consigue mediante las API de notificación de memoria de Microsoft Windows.

Cuando [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utiliza la memoria de manera dinámica, realiza una consulta periódica en el sistema para determinar la cantidad de memoria libre. El mantenimiento de esta memoria libre evita la paginación en el sistema operativo (SO). Si hay menos memoria libre, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] libera memoria para el sistema operativo. Si hay más memoria libre, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] puede asignar más memoria. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] agrega memoria solo cuando su carga de trabajo así lo requiere; un servidor inactivo no aumenta el tamaño de su espacio de direcciones virtual.  
  
La **[Memoria de servidor máxima](../database-engine/configure-windows/server-memory-server-configuration-options.md)** controla la asignación de memoria de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], todas las cachés (incluido el grupo de búferes), las [concesiones de memoria de ejecución de consultas](#effects-of-min-memory-per-query), [la memoria del administrador de bloqueos](#memory-used-by-sql-server-objects-specifications) y la memoria de CLR <sup>1</sup> (básicamente, cualquier distribuidor de memoria que se encuentre en **[sys.dm_os_memory_clerks](../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)** ). 

<sup>1</sup> La memoria CLR se administra en asignaciones de max_server_memory a partir de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)].

La consulta siguiente devuelve información sobre la memoria asignada actual:  
  
```sql  
SELECT 
  physical_memory_in_use_kb/1024 AS sql_physical_memory_in_use_MB, 
    large_page_allocations_kb/1024 AS sql_large_page_allocations_MB, 
    locked_page_allocations_kb/1024 AS sql_locked_page_allocations_MB,
    virtual_address_space_reserved_kb/1024 AS sql_VAS_reserved_MB, 
    virtual_address_space_committed_kb/1024 AS sql_VAS_committed_MB, 
    virtual_address_space_available_kb/1024 AS sql_VAS_available_MB,
    page_fault_count AS sql_page_fault_count,
    memory_utilization_percentage AS sql_memory_utilization_percentage, 
    process_physical_memory_low AS sql_process_physical_memory_low, 
    process_virtual_memory_low AS sql_process_virtual_memory_low
FROM sys.dm_os_process_memory;  
```  
 
La <a name="stacksizes"></a>memoria para pilas de subprocesos <sup>1</sup>, CLR <sup>2</sup>, los archivos .dll de procedimientos extendidos, los proveedores de OLE DB a los que se hacen referencias en consultas distribuidas, los objetos de automatización a los que se hacen referencia en instrucciones [!INCLUDE[tsql](../includes/tsql-md.md)] y cualquier otra memoria asignada por un DLL que no es de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]**no** están controladas por la memoria de servidor máxima.

<sup>1</sup> Consulte la página de documentación sobre cómo [Establecer la opción de configuración del servidor Máximo de subprocesos de trabajo](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md) para obtener información sobre los subprocesos de trabajo predeterminados calculados para un determinado número de CPU con afinidad en el host actual. Los tamaños de pila de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] son los siguientes:

|Arquitectura de SQL Server|Arquitectura del SO|Tamaño de la pila|  
|--------------------|----------------------|----------------------|
|x86 (32 bits)|x86 (32 bits)|512 KB|
|x86 (32 bits)|x64 (64 bits)|768 KB| 
|x64 (64 bits)|x64 (64 bits)|2048 KB|
|IA64 (Itanium)|IA64 (Itanium)|4096 KB|

<sup>2</sup> La memoria CLR se administra en asignaciones de max_server_memory a partir de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)].

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usa la API de notificación de memoria **QueryMemoryResourceNotification** para determinar el momento en que el Administrador de memoria de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] puede asignar y liberar memoria.  

Cuando [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se inicia, calcula el tamaño del espacio de direcciones virtuales del grupo de búferes basándose en un número de parámetros, como la cantidad de memoria física en el sistema, el número de subprocesos de servidor y varios parámetros de inicio. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] reserva la cantidad calculada de su espacio de direcciones virtuales del proceso para el grupo de búferes, pero solo adquiere (confirma) la cantidad necesaria de memoria física para la carga actual.

A continuación, la instancia sigue adquiriendo la memoria que necesita para la carga de trabajo. A medida que se conectan más usuarios y se ejecutan consultas, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] adquiere la memoria física adicional según la demanda. Una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sigue adquiriendo memoria física hasta que alcanza su asignación de memoria de servidor máxima o hasta que el sistema operativo indica que ya no existe más memoria libre; libera memoria cuando se supera el valor de memoria de servidor mínima y el sistema operativo indica que hay escasez de memoria libre. 

Cuando se inician otras aplicaciones en un equipo que ejecuta una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], consumen memoria y la cantidad de memoria física disponible cae por debajo del destino de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . La instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ajusta su consumo de memoria. Si se detiene otra aplicación y aumenta la cantidad de memoria disponible, la instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] aumenta el tamaño de su asignación de memoria. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] puede liberar y adquirir varios megabytes de memoria cada segundo, lo que le permite ajustarse rápidamente a los cambios de asignación de memoria.

## <a name="effects-of-min-and-max-server-memory"></a>Efectos de las opciones min y max server memory
Las opciones de configuración *min server memory* y *max server memory* establecen los límites superior e inferior de la cantidad de memoria que usa el grupo de búferes y otras memorias caché del motor de base de datos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. El grupo de búferes no adquiere inmediatamente la cantidad de memoria especificada en Memoria de servidor mínima. El grupo de búferes comienza con la memoria precisa para el inicio. Según aumenta la carga de trabajo del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], se sigue adquiriendo la memoria necesaria para permitir la carga de trabajo. El grupo de búferes no libera nada de la memoria adquirida hasta que alcanza la cantidad especificada en Memoria de servidor mínima. Una vez alcanzado el valor de Memoria de servidor mínima, el grupo de búferes utiliza el algoritmo estándar para adquirir y liberar memoria según sea preciso. La única diferencia es que el grupo de búferes nunca deja que su asignación de memoria baje del nivel especificado en Memoria de servidor mínima y adquiera más memoria del nivel especificado en Memoria de servidor máxima.

> [!NOTE]
> [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] adquiere, como un proceso, más memoria de la especificada en la opción max server memory. Los componentes tanto internos como externos pueden asignar memoria fuera del grupo de búferes, lo cual consume memoria adicional, pero la memoria asignada en el grupo de búferes todavía representa normalmente la cantidad más grande de memoria que consume [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

La cantidad de memoria que adquiere el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] es totalmente dependiente de la carga de trabajo colocada en la instancia. Una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] que no procesa muchas solicitudes nunca podrá alcanzar el nivel de min server memory.

Si se especifica el mismo valor para memoria de servidor mínima y memoria de servidor máxima, una vez que la memoria asignada a [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] alcanza ese valor, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] detiene dinámicamente la adquisición y liberación de la memoria para el grupo de búferes.

Si una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se está ejecutando en un equipo donde se inician o detienen otras aplicaciones con frecuencia, la asignación y cancelación de asignación de memoria por parte de la instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] puede ralentizar el inicio de otras aplicaciones. Además, si [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] es una de las diversas aplicaciones de servidor que se ejecutan en un único equipo, los administradores del sistema pueden necesitar controlar la cantidad de memoria asignada a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. En estos casos, puede utilizar las opciones Memoria de servidor mínima y Memoria de servidor máxima para controlar cuánta memoria puede usar [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Las opciones **memoria de servidor mínima** y **memoria de servidor máxima** se expresan en megabytes. Para obtener más información, vea [Opciones de configuración de memoria del servidor](../database-engine/configure-windows/server-memory-server-configuration-options.md).

## <a name="memory-used-by-sql-server-objects-specifications"></a>Memoria que usan las especificaciones de objetos de SQL Server
La siguiente lista muestra la cantidad de memoria aproximada que usan diferentes objetos en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Las cantidades mostradas son estimaciones y pueden variar según el entorno y cómo se crean los objetos:

* Bloqueo (tal y como lo mantiene el Administrador de bloqueos): 64 bytes + 32 bytes por propietario   
* Conexión de usuario: aproximadamente (3 \* tamaño_del_paquete_de_red + 94 kb)    

El **tamaño del paquete de red** es el tamaño de los paquetes del esquema de datos tabulares (TDS) que se utilizan para la comunicación entre las aplicaciones y el motor de base de datos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. El tamaño del paquete predeterminado es 4 KB y se controla mediante la opción de configuración Tamaño de paquete de red.

Cuando los conjuntos de resultados activos múltiples están habilitados, la conexión de usuario es aproximadamente (3 + 3 \*num_logical_connections)\* network_packet_size + 94 KB

## <a name="effects-of-min-memory-per-query"></a>Efectos de memoria mínima por consulta
La opción *min memory per query* establece la cantidad mínima de memoria (en kilobytes) que se va a asignar para la ejecución de una consulta. Esto también se conoce como concesión de memoria mínima. Todas las consultas deben esperar hasta que se pueda proteger la memoria mínima solicitada, antes de que se pueda iniciar la ejecución, o bien hasta que se supera el valor especificado en la opción de configuración del servidor Espera de consulta. El tipo de espera que se acumula en este escenario es RESOURCE_SEMAPHORE.

> [!IMPORTANT]
> No establezca la opción de configuración del servidor Memoria mínima por consulta en un valor demasiado alto, especialmente en sistemas muy ocupados, ya que si lo hace, podría provocar:         
> - El aumento de la competición por los recursos de memoria.         
> - La reducción de la simultaneidad al aumentar la cantidad de memoria para cada consulta única, incluso si la memoria necesaria en tiempo de ejecución es menor que esta configuración.    
>    
> Para obtener recomendaciones sobre el uso de esta configuración, vea [Configurar la opción de configuración del servidor Memoria mínima por consulta](../database-engine/configure-windows/configure-the-min-memory-per-query-server-configuration-option.md#Recommendations).

### <a name="memory-grant-considerations"></a><a name="memory-grant-considerations"></a>Consideraciones de concesión de memoria
Para la **ejecución del modo de fila**, no se puede superar la concesión de memoria inicial bajo ninguna condición. Si se necesita más memoria que la concesión inicial para ejecutar operaciones de **hash** u **orden**, estas se desbordarán al disco. Una operación de hash que se desborda es compatible con un archivo de trabajo en TempDB, mientras que una operación de orden que se desborda es compatible con una [tabla de trabajo](../relational-databases/query-processing-architecture-guide.md#worktables).   

Un desbordamiento que se produzca durante una operación de orden se conoce como una [advertencia antes de ordenar](../relational-databases/event-classes/sort-warnings-event-class.md). Las advertencias antes de ordenar indican que las operaciones de orden no caben en la memoria. Esto no incluye las operaciones de orden que implican la creación de índices, solo las operaciones de orden dentro de una consulta (como las de una cláusula `ORDER BY` en una instrucción `SELECT`).

Un desbordamiento que se produzca durante una operación de hash se conoce como una [advertencia hash](../relational-databases/event-classes/hash-warning-event-class.md). Tienen lugar cuándo se ha producido una recursividad hash o un cese de hash (salida hash) durante una operación de hash.
-  La recursividad hash se produce cuando no hay suficiente memoria para la entrada generada, lo que causa que ésta se divida en varias particiones que se procesan por separado. Si alguna de estas particiones sigue sin caber en la memoria disponible, se vuelve a dividir en subparticiones, que también se procesan por separado. Este proceso de división continúa hasta que cada partición quepa en la memoria disponible o hasta que se alcance el nivel máximo de recursividad.
-  La salida hash se produce cuando una operación de hash alcanza el nivel máximo de repetición y vuelve a un plan alternativo para procesar el resto de datos con particiones. Estos eventos pueden producir un rendimiento reducido en el servidor.

Para la **ejecución del modo por lotes**, la concesión de memoria inicial puede aumentar de forma dinámica hasta un umbral interno concreto de forma predeterminada. Este mecanismo de concesión de memoria dinámico está diseñado para permitir la ejecución residente en memoria de las operaciones **hash** y **sort** que se ejecutan en el modo de procesamiento por lotes. Si estas operaciones siguen sin caber en la memoria, se desbordarán al disco.

Para obtener más información sobre los modos de ejecución, vea [Guía de arquitectura de procesamiento de consultas](../relational-databases/query-processing-architecture-guide.md#execution-modes).

## <a name="buffer-management"></a>Administración de búfer
El propósito principal de una base de datos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] es almacenar y recuperar datos, por lo que una E/S de disco intensiva es una de las características principales del Motor de base de datos. Debido a que las operaciones de E/S de disco pueden consumir muchos recursos y tardar bastante tiempo en completarse, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se centra en hacer la E/S muy eficaz. La administración de búfer es un componente clave para lograr esta eficacia. El componente de administración de búfer consta de dos mecanismos: el **administrador de búfer** para obtener acceso a las páginas de bases de datos y actualizarlas y la **memoria caché del búfer** (también conocida como **grupo de búferes**) para reducir la E/S de archivos de base de datos. 

### <a name="how-buffer-management-works"></a>Cómo funciona la administración de búfer
Un búfer es una página de 8 KB en memoria (el mismo tamaño que una página de índice o de datos). Por tanto, la memoria caché del búfer está dividida en páginas de 8 KB. El administrador de búfer administra las funciones para la lectura de páginas de índice o de datos de los archivos de disco de base de datos en la caché del búfer y para la escritura de páginas modificadas nuevamente en el disco. Una página permanece en la memoria caché del búfer hasta que el administrador de búfer necesita el área del búfer para leer en ella más datos. Los datos solo vuelven a escribirse en el disco si se han modificado. Los datos de la memoria caché del búfer se pueden modificar varias veces antes de que se vuelvan a escribir en el disco. Para más información, consulte [Leer páginas](../relational-databases/reading-pages.md) y [Escribir páginas](../relational-databases/writing-pages.md).

Cuando [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se inicia, calcula el tamaño del espacio de direcciones virtuales de la caché de búferes basándose en un número de parámetros, como la cantidad de memoria física en el sistema, el número configurado de subprocesos máximos de servidor y varios parámetros de inicio. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] reserva la cantidad calculada de su espacio de direcciones virtuales del proceso (llamado destino de memoria) para la caché de búferes, pero solo adquiere (confirma) la cantidad necesaria de memoria física para la carga actual. Puede realizar una consulta en las columnas **bpool_commit_target** y **bpool_committed** en la vista de catálogo [sys.dm_os_sys_info](../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) para devolver el número de páginas reservadas como memoria objetivo y el número de páginas actualmente confirmadas en la caché del búfer, respectivamente.

El intervalo entre el inicio de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y el momento en que la caché del búfer obtiene su memoria objetivo se llama arranque. Durante este período, las solicitudes de lectura llenan los búferes según sea necesario. Por ejemplo, una solicitud de lectura de una página de 8 KB llena una única página de búfer. Esto significa que el arranque depende del número y el tipo de solicitudes del cliente. El arranque se agiliza mediante la transformación de solicitudes de lectura de una página en solicitudes de ocho páginas alineadas (creando una extensión). Esto permite que el arranque finalice mucho más rápido, especialmente en equipos con mucha memoria. Para obtener más información acerca de las páginas y las extensiones, vea [Guía de arquitectura de páginas y extensiones](../relational-databases/pages-and-extents-architecture-guide.md#pages-and-extents).

Debido a que el administrador de búfer utiliza la mayor parte de la memoria en el proceso de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , coopera con el administrador de memoria para permitir que otros componentes utilicen sus búferes. El administrador de búfer interactúa principalmente con los siguientes componentes:

* Administrador de recursos, para controlar la utilización de memoria general y, en plataformas de 32 bits, para controlar el uso del espacio de direcciones.  
* Administrador de bases de datos y [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Operating System (SQLOS), para operaciones de E/S de archivos de bajo nivel.  
* Administrador de registros, para registros de escritura anticipada.  

### <a name="supported-features"></a>Características admitidas
El administrador de búfer admite las características siguientes:

* El administrador de búfer está preparado para el **acceso no uniforme a memoria (NUMA, Non-Uniform Memory Access)** . Las páginas de la caché del búfer se distribuyen por los nodos NUMA de hardware, que permiten que un subproceso tenga acceso a una página de búfer que esté asignada en el nodo NUMA local y no desde una memoria externa. 
* El administrador de búfer admite la función de **Agregar memoria sin interrupción**, que permite a los usuarios agregar memoria física sin reiniciar el servidor. 
* El administrador de búfer admite **páginas grandes** en plataformas de 64 bits. El tamaño de página es específico de la versión de Windows.

  > [!NOTE]
  > En versiones anteriores a [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], es necesaria una [marca de seguimiento 834](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) para habilitar las páginas grandes en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  

* El administrador de búfer proporciona diagnósticos adicionales que se muestran mediante vistas de administración dinámica. Puede utilizar estas vistas para supervisar diversos recursos del sistema operativo específicos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Por ejemplo, puede usar la vista [sys.dm_os_buffer_descriptors](../relational-databases/system-dynamic-management-views/sys-dm-os-buffer-descriptors-transact-sql.md) para supervisar las páginas de la caché del búfer.   

### <a name="disk-io"></a>E/S de disco
El administrador de búfer solo realiza tareas de lectura y escritura en la base de datos. Las otras operaciones con archivos, como la apertura, el cierre, la extensión y la reducción, las realizan el administrador de base de datos y los componentes del administrador de archivos. 

Las operaciones de E/S de disco que realiza el administrador de búfer tienen las siguientes características:

* Todas las operaciones de E/S se realizan de forma asincrónica, lo que permite que el subproceso de llamada siga con el procesamiento mientras la operación de E/S se realiza en segundo plano.
* Todas las operaciones de E/S se emiten en los subprocesos de llamada a menos que la opción affinity I/O (E/S de afinidad) esté en uso. La opción de máscara de afinidad de E/S enlaza la E/S del disco de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a un subconjunto específico de unidades CPU. En entornos de procesamiento de transacciones en línea (OLTP) de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] de grandes prestaciones, esta extensión puede mejorar el rendimiento de los subprocesos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] que emiten E/S.
* Las operaciones de E/S de múltiples páginas se logran con E/S por dispersión y recopilación, que permite transferir datos a áreas no contiguas de memoria, o desde ellas. Esto significa que [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] puede llenar o vaciar rápidamente la caché del búfer y, a la vez, evitar múltiples solicitudes de E/S física. 

#### <a name="long-io-requests"></a>Solicitudes de E/S largas  
El administrador de búfer informa sobre cualquier solicitud de E/S que haya quedado pendiente durante al menos 15 segundos. Esto ayuda al administrador del sistema a distinguir entre problemas de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y problemas del subsistema de E/S. El mensaje de error 833 se notifica y aparece en el registro de errores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] de la siguiente forma:

`SQL Server has encountered ## occurrence(s) of I/O requests taking longer than 15 seconds to complete on file [##] in database [##] (#). The OS file handle is 0x00000. The offset of the latest long I/O is: 0x00000.` 

Una E/S larga puede ser de lectura o de escritura, pero esto no se indica actualmente en el mensaje. Los mensajes de E/S larga son advertencias, no errores. No indican problemas con [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], sino con el sistema de E/S subyacente. Los mensajes se notifican para ayudar a los administradores del sistema a encontrar la causa de los tiempos de respuesta largos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con mayor rapidez y a distinguir problemas que estén fuera del control de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Por eso, no es necesario tomar ninguna acción, pero el administrador del sistema debe investigar por qué la solicitud de E/S tardó tanto tiempo y si ese tiempo es justificable.

#### <a name="causes-of-long-io-requests"></a>Causas de solicitudes de E/S largas  
Un mensaje de E/S larga puede indicar que una E/S está permanente bloqueada y que nunca se completará (lo que también se conoce como E/S perdida) o que simplemente no se completó aún. No es posible saber con los datos del mensaje de qué caso se trata, aunque una E/S perdida casi siempre llevará a un tiempo de espera de bloqueo temporal.

Las E/S largas suelen indicar una carga de trabajo de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] demasiado intensa para el subsistema de disco. Se puede indicar un subsistema de disco inadecuado cuando:

* Aparecen múltiples mensajes de E/S largas en el registro de errores durante una carga de trabajo pesada de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .
* Los contadores de rendimiento muestran latencias de disco prolongadas, colas de disco largas o no muestran el tiempo de inactividad de disco.  

Otra posible causa de las E/S largas es que un componente de la ruta de acceso de E/S (por ejemplo, un controlador o el firmware) posponga de forma continua el servicio para una solicitud de E/S antigua en favor de dar servicio a solicitudes nuevas que están más cerca de la posición actual del cabezal del disco. La técnica corriente de procesar solicitudes según su prioridad sobre la base de las que están más cerca de la posición actual del cabezal de lectura/escritura se conoce como "búsqueda de elevador". Esto puede resultar difícil de corroborar con la herramienta Monitor del sistema de Windows (PERFMON.EXE) porque a la mayor parte de las E/S se las da servicio inmediatamente. Las solicitudes de E/S largas pueden agravarse con cargas de trabajo que realicen un gran número de operaciones de E/S secuenciales, como copias de seguridad y restauración, recorridos de tabla, ordenaciones, creación de índices, cargas masivas y puestas a cero de archivos.

Las E/S largas aisladas que no están relacionadas con ninguna de las situaciones anteriores pueden estar causadas por un problema con el hardware o el controlador. El registro de eventos del sistema puede contener un evento relacionado que ayude a diagnosticar el problema.

### <a name="memory-pressure-detection"></a>Detección de la presión de memoria
La presión de memoria es una condición resultante de la escasez de memoria y puede dar lugar a:
- Operaciones de E/S adicionales (por ejemplo, un subproceso en segundo plano de escritura diferida muy activo).
- Mayor proporción de recompilación.
- Consultas en ejecución más prolongadas (si existen esperas de concesión de memoria).
- Ciclos de CPU adicionales.

Esta situación se puede desencadenar debido a causas externas o internas. Las causas externas incluyen:
- La memoria física (RAM) disponible es baja. Esto hace que el sistema recorte los espacios de trabajo de los procesos actualmente en ejecución, lo que puede provocar una ralentización general. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] puede reducir el destino de confirmación del grupo de búferes e iniciar el recorte de las memorias caché internas con más frecuencia. 
- La memoria del sistema disponible general (que incluye el archivo de paginación del sistema) es baja. Esto puede provocar un error del sistema en las asignaciones de memoria, ya que no puede paginar la memoria asignada actualmente.
Las causas internas incluyen:
- Responder a la presión de memoria externa, cuando el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] establece límites de uso de memoria más bajos.
- La configuración de memoria se redujo manualmente reduciendo la configuración *memoria de servidor máxima*. 
- Cambios en la distribución de memoria de los componentes internos entre varias cachés.

El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] implementa un marco de trabajo dedicado para detectar y controlar la presión de memoria, como parte de su administración de memoria dinámica. Este marco de trabajo incluye la tarea en segundo plano llamada **Monitor de recursos**. La tarea Monitor de recursos supervisa el estado de los indicadores de memoria externa e interna. Cuando uno de estos indicadores cambia de estado, calcula la notificación correspondiente y la difunde. Estas notificaciones son mensajes internos desde cada uno de los componentes del motor y se almacenan en búferes en anillo. 

Dos búferes en anillo contienen información relevante para la administración dinámica de memoria: 
- El búfer en anillo Monitor de recursos, que realiza el seguimiento de la actividad del Monitor de recursos, como si la presión de memoria se ha señalado o no. Este búfer en anillo tiene información de estado en función de la condición actual de *RESOURCE_MEMPHYSICAL_HIGH*, *RESOURCE_MEMPHYSICAL_LOW*, *RESOURCE_MEMPHYSICAL_STEADY* o *RESOURCE_MEMVIRTUAL_LOW*.
- El búfer en anillo Agente de memoria, que contiene los registros de las notificaciones de memoria para cada grupo de recursos de Resource Governor. Cuando se detecta la presión de memoria interna, se activa la notificación de memoria insuficiente para los componentes que asignan memoria, para desencadenar acciones diseñadas para equilibrar la memoria entre las cachés. 

Los agentes de memoria supervisan la demanda de consumo de memoria de cada componente y, después, en función de la información recopilada, calculan el valor óptimo de memoria para cada uno de estos componentes. Hay un conjunto de agentes para cada grupo de recursos de Resource Governor. Después, esta información se difunde a cada uno de los componentes, que aumentan o reducen su uso según sea necesario.
Para obtener más información sobre los agentes de memoria, vea [sys.dm_os_memory_brokers](../relational-databases/system-dynamic-management-views/sys-dm-os-memory-brokers-transact-sql.md). 

### <a name="error-detection"></a>Detección de errores  
Las páginas de bases de datos pueden utilizar uno de los dos mecanismos opcionales que ayudan a garantizar la integridad de la página desde el momento en que se escribe en el disco hasta que se vuelve a leer: protección contra página rasgada y protección de suma de comprobación. Estos mecanismos permiten emplear un método independiente para comprobar la corrección, no solo del almacenamiento de datos, sino también de los componentes de hardware, como controladores, cables e incluso el sistema operativo. La protección se agrega a la página justo antes de escribirla en el disco y se comprueba después de que se lee desde el disco.

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] vuelve a intentar cualquier lectura que genere un error con una suma de comprobación, una página rasgada u otros errores de E/S, en cuatro ocasiones. Si la lectura se desarrolla correctamente en uno de los reintentos, se escribe un mensaje en el registro de errores y el comando que ha desencadenado la lectura continúa. Si los reintentos no se realizan correctamente, el comando genera el mensaje de error 824. 

El tipo de protección de página que se utilice es un atributo de la base de datos que contiene la página. La protección de suma de comprobación es la protección predeterminada para bases de datos creadas en [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] y en versiones posteriores. El mecanismo de protección de páginas se especifica al crear la base de datos y se puede modificar con ALTER DATABASE SET. La configuración de protección de página actual se puede determinar consultando la columna *page_verify_option* de la vista de catálogo [sys.databases](../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la propiedad *IsTornPageDetectionEnabled* de la función [DATABASEPROPERTYEX](../t-sql/functions/databasepropertyex-transact-sql.md). 

> [!NOTE]
> Si se modifica la configuración de protección de página, la nueva configuración no afecta a toda la base de datos de forma inmediata. En cambio, las páginas adoptan el nivel de protección actual de la base de datos cuando se vuelven a escribir. Esto significa que la base de datos puede estar compuesta de páginas con distintos tipos de protección. 

#### <a name="torn-page-protection"></a>Protección contra página rasgada  
La protección contra página rasgada, que se introdujo en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2000, es básicamente una forma de detectar errores en las páginas a causa de problemas con el suministro eléctrico. Por ejemplo, es posible que por un problema con el suministro eléctrico solo se escriba una parte de la página en el disco. Cuando se usa la protección de páginas rasgadas, se almacena un patrón de firma de 2 bits específico por cada sector de 512 bytes de la página de base de datos de 8 kilobytes (KB) en el encabezado de página de la base de datos cuando la página se escribe en disco. Si la página se lee desde el disco, los bits rasgados almacenados en el encabezado de página se comparan con la información del sector de la página real. El patrón de firma alterna entre los binarios 01 y 10 en cada escritura, por lo que siempre es posible detectar las ocasiones en que solo una parte de los sectores las hicieron en el disco: si hay un bit con el estado incorrecto cuando la página se lee posteriormente, la página se escribió de forma incorrecta y se detecta una página rasgada. La detección de página rasgada utiliza un mínimo de recursos; sin embargo, no detecta todos los errores causados por errores del hardware de disco. Para obtener información acerca de cómo configurar la detección de páginas rasgadas, vea [ALTER DATABASE SET Options &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-set-options.md#page_verify) (Opciones de ALTER DATABASE SET (Transact-SQL).

#### <a name="checksum-protection"></a>Protección de suma de comprobación  
La protección de suma de comprobación, característica implementada en [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], proporciona una comprobación de integridad de datos más sólida. Se calcula una suma de comprobación para los datos de cada página que se escribe y se almacena en el encabezado de la página. Cada vez que se lee desde disco una página con una suma de comprobación almacenada, el motor de base de datos vuelve a calcular la suma de comprobación para los datos de la página y muestra el error 824 cuando la nueva suma de comprobación no coincide con la suma almacenada. La protección de suma de comprobación puede detectar más errores que la protección contra página rasgada porque tiene en cuenta cada byte de la página; sin embargo, consume una cantidad moderada de recursos. Cuando la suma de comprobación está habilitada, pueden detectarse los errores debidos a cualquier problema con el suministro eléctrico o a hardware o firmware defectuosos cada vez que el administrador de búfer lea una página del disco. Para obtener información acerca de cómo configurar la suma de comprobación, vea [ALTER DATABASE SET Options &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-set-options.md#page_verify) (Opciones de ALTER DATABASE SET (Transact-SQL)).

> [!IMPORTANT]
> Si una base de datos de usuario o del sistema se actualiza a [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] o una versión posterior, se conserva el valor de [PAGE_VERIFY](../t-sql/statements/alter-database-transact-sql-set-options.md#page_verify) (NONE o TORN_PAGE_DETECTION). Se recomienda utilizar CHECKSUM.
> Es posible que TORN_PAGE_DETECTION utilice menos recursos, pero proporciona en cambio un subconjunto mínimo de la protección de CHECKSUM.

## <a name="understanding-non-uniform-memory-access"></a>Descripción del acceso no uniforme a memoria
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]  está preparado para el acceso no uniforme a memoria (NUMA) y realiza un buen rendimiento en hardware NUMA sin necesidad de establecer ninguna configuración especial. A medida que aumentan la velocidad del reloj y el número de procesadores, resulta cada vez más difícil reducir la latencia de la memoria necesaria para utilizar esta potencia de procesamiento adicional. Para evitarlo, los proveedores de hardware proporcionan cachés L3 grandes, pero esto es solo una solución limitada. La arquitectura NUMA proporciona una solución escalable para este problema. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se ha diseñado para aprovechar los equipos basados en NUMA sin necesidad de realizar cambios en las aplicaciones. Para más información, vea: [Cómo: Configurar SQL Server para que use Soft-NUMA](../database-engine/configure-windows/soft-numa-sql-server.md).

## <a name="dynamic-partition-of-memory-objects"></a>Partición dinámica de objetos de memoria
Los asignadores de montón, llamados objetos de memoria en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], permiten a [!INCLUDE[ssde_md](../includes/ssde_md.md)] asignar memoria del montón. Se puede realizar un seguimiento de ellos mediante la DMV [sys.dm_os_memory_objects](../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md). CMemThread es un tipo de objeto de memoria seguro para subprocesos que permite asignaciones de memoria simultáneas desde varios subprocesos. Para realizar un seguimiento correcto, los objetos CMemThread se basan en construcciones de sincronización (una exclusión mutua) para garantizar que un solo subproceso actualiza datos críticos a la vez. 

> [!NOTE]
> El tipo de objeto CMemThread se utiliza en la base de código de [!INCLUDE[ssde_md](../includes/ssde_md.md)] para muchas asignaciones diferentes y se puede particionar globalmente, por nodo o por CPU.   

Sin embargo, el uso de exclusiones mutuas puede llevar a contención si muchos subprocesos asignan desde el mismo objeto de memoria de un modo muy simultáneo. Por lo tanto, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tiene el concepto de objetos de memoria con particiones (PMO) y cada partición se representa mediante un único objeto CMemThread. La creación de particiones de un objeto de memoria se define estáticamente y no se puede cambiar después de la creación. Dado que los patrones de asignación de memoria varían considerablemente en función de aspectos como el uso de hardware y memoria, es imposible conseguir el patrón de particionamiento perfecto de antemano. En la gran mayoría de los casos, el uso de una sola partición será suficiente, pero en algunos escenarios esto puede provocar contención, lo que solo se puede evitar con un objeto de memoria muy particionado. No es aconsejable particionar cada objeto de memoria, ya que más particiones pueden afectar a la eficiencia de otras formas y aumentar la fragmentación de la memoria.

> [!NOTE]
> Antes de [!INCLUDE[sssql15-md](../includes/sssql16-md.md)], la marca de seguimiento 8048 se podría usar para forzar que un PMO basada en nodo se convierta en un PMO basada en CPU. A partir de [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] SP2 y [!INCLUDE[sssql15-md](../includes/sssql16-md.md)], este comportamiento es dinámico y lo controla el motor.

A partir de [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] SP2 y [!INCLUDE[sssql15-md](../includes/sssql16-md.md)], [!INCLUDE[ssde_md](../includes/ssde_md.md)] puede detectar dinámicamente la contención en un objeto CMemThread específico y promover el objeto a una implementación por nodo o por CPU.  Una vez promocionado, el PMO sigue promocionado hasta que se reinicie el proceso [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. La contención de CMemThread se puede detectar por la presencia de esperas CMEMTHREAD altas en la DMV [sys.dm_os_wait_stats](../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md) y mediante la observación de las siguientes columnas de la DMV [sys.dm_os_memory_objects](../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md): *contention_factor*, *partition_type*, *exclusive_allocations_count* y *waiting_tasks_count*.

## <a name="see-also"></a>Consulte también
[Opciones de configuración de memoria del servidor](../database-engine/configure-windows/server-memory-server-configuration-options.md)   
[Leer páginas](../relational-databases/reading-pages.md)   
[Escribir páginas](../relational-databases/writing-pages.md)   
[Cómo: Configurar SQL Server para que use Soft-NUMA](../database-engine/configure-windows/soft-numa-sql-server.md)   
[Requisitos para utilizar las tablas con optimización para memoria](../relational-databases/in-memory-oltp/requirements-for-using-memory-optimized-tables.md)   
[Resolver problemas de memoria insuficiente](../relational-databases/in-memory-oltp/resolve-out-of-memory-issues.md)