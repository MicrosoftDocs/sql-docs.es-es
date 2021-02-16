---
description: MSSQLSERVER_17890
title: MSSQLSERVER_17890
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17890 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 121ec18d62a79a10df015d26c0e2c2885c8cd0b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340763"
---
# <a name="mssqlserver_17890"></a>MSSQLSERVER_17890
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|17890|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|SRV_WS_TRIMMED|
|Texto del mensaje|Se ha transferido al archivo de paginación una parte significativa de la memoria de proceso de SQL Server. Esto puede dar lugar a una degradación del rendimiento. Duración: %d segundos. Espacio de trabajo (KB): %I64d, confirmado (KB): %I64d, uso de memoria: %d%%.|
||

## <a name="explanation"></a>Explicación

Es posible que encuentre el mensaje de error siguiente en el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o en el registro de eventos de aplicación Windows.

> Se ha transferido al archivo de paginación una parte significativa de la memoria de proceso de SQL Server. Esto puede dar lugar a una degradación del rendimiento. Duración: 0 segundos. Espacio de trabajo (KB): 3383250, confirmado (KB): 9112480, uso de memoria: 37 %.

También puede observar una degradación repentina del rendimiento con la ejecución de la consulta y todas las demás operaciones en SQL Server.

## <a name="cause"></a>Causa

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supervisa la información relacionada con las distintas memorias sobre el proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En este caso, ha detectado que el espacio de trabajo del proceso es inferior al 50 % de la memoria de proceso confirmada. Como resultado, se imprime esta advertencia. Las causas normales de esta advertencia son las siguientes:

- El sistema operativo pagina las partes grandes de la memoria confirmada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asignada al archivo de paginación.
- Esto se puede deber a un aumento repentino de la memoria de otras aplicaciones o de las necesidades del sistema operativo.
- Esto también puede ocurrir cuando determinados controladores de dispositivos solicitan asignaciones de memoria contiguas para sus necesidades.

## <a name="user-action"></a>Acción del usuario

Puede impedir que el sistema operativo Windows pagine la memoria del grupo de búferes del proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante el bloqueo de la memoria asignada para el grupo de búferes en la memoria física. Para bloquear la memoria, asigne el permiso del usuario Bloquear páginas en la memoria a la cuenta de usuario que se usa como la cuenta de inicio del servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sin embargo, antes de implementar esta solución, revise las secciones [Causas de la paginación de la memoria de SQL Server](#what-causes-sql-server-memory-to-be-paged-out) y Consideraciones importantes antes de asignar el permiso del usuario "Bloquear páginas en la memoria" para una instancia de SQL Server.

> [!NOTE]
> El uso de Bloquear páginas en la memoria garantiza que no se pagine la memoria administrada por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sin embargo, el SO puede seguir paginando las pilas de subprocesos, las imágenes EXE y DLL, la memoria en montón y la memoria CLR.
>
> A partir de la actualización acumulativa 2 de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008 SP1, las ediciones Standard y Enterprise de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pueden usar el permiso del usuario Bloquear páginas en la memoria. Para más información sobre la compatibilidad con las páginas bloqueadas, consulte [KB970070: Compatibilidad con páginas bloqueadas en sistemas SQL Server Standard Edition (64 bits)](https://support.microsoft.com/help/970070).

Siga estos pasos para asignar el permiso del usuario Bloquear páginas en la memoria:

1. Haga clic en **Inicio** y en **Ejecutar**, escriba *gpedit.msc* y, luego, haga clic en **Aceptar**.
1. Observe que aparecerá el cuadro de diálogo Directiva de grupo.
1. Expanda **Configuración del equipo** y, luego, expanda **Configuración de Windows**.
1. Expanda **Configuración de seguridad** y, a continuación, expanda **Directivas locales**.
1. Haga clic en Asignación de permisos del usuario y, luego, haga doble clic en **Bloquear páginas** en la memoria.
1. En el cuadro de diálogo **Configuración de la directiva de seguridad local**, haga clic en **Agregar usuario** o **Grupo**.
1. En el cuadro de diálogo **Seleccionar usuarios** o **Grupos**, agregue la cuenta que tiene permiso para ejecutar el archivo Sqlservr.exe y, luego, haga clic en **Aceptar**.
1. Cierre el cuadro de diálogo **Directiva de grupo**.
1. Reinicie el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Después de asignar el permiso del usuario Bloquear páginas en la memoria y de reiniciar el servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el sistema operativo Windows ya no pagina la memoria del grupo de búferes dentro del proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sin embargo, el sistema operativo Windows todavía puede paginar la memoria del grupo que no es de búferes dentro del proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Para validar que la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa el permiso del usuario, asegúrese de que el mensaje siguiente se escribe en el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el inicio: "Utilizando páginas bloqueadas para grupo de búferes"

Este mensaje solo se aplica a SQL Server. Para más información sobre este mensaje en el registro de errores, consulte el artículo siguiente: [¿Es necesario asignar el privilegio Bloquear páginas para la memoria en el sistema local?](https://techcommunity.microsoft.com/t5/sql-server-support/do-i-have-to-assign-the-lock-pages-in-memory-privilege-for-local/ba-p/315426)

Cuando el sistema operativo Windows pagina la memoria del grupo que no es de búferes, es posible que el usuario siga encontrando problemas de rendimiento. Sin embargo, los mensajes de error que se mencionan en la sección "Explicación" no se registran en el registros de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="what-causes-sql-server-memory-to-be-paged-out"></a>Causas de la paginación de la memoria de SQL Server

Hay tres categorías amplias de problemas que pueden provocar este problema:

- Problemas relacionados con la aplicación: en conjunto, todas las aplicaciones han agotado la memoria física disponible y el SO debe liberar memoria para las nuevas solicitudes de recursos de parte de las aplicaciones. Por lo general, el enfoque aquí es encontrar las aplicaciones que agotan la memoria y tomar las medidas necesarias para equilibrar la memoria entre ellas sin agotar la RAM.
- Problemas de los controladores de dispositivos: los controladores de dispositivos pueden paginar el espacio de trabajo de todos los procesos si el controlador llama de manera incorrecta a una función de asignación de memoria.
- Problemas del sistema operativo

A continuación, puede encontrar información sobre cada una de estas categorías

- **Problemas relacionados con la aplicación**: en conjunto, es posible que las aplicaciones consuman el total de la RAM del sistema. Si se hacen nuevas solicitudes de memoria, el SO intenta cumplirlas y, si no hay memoria disponible, recortará el espacio de trabajo de las aplicaciones en ejecución para satisfacer las solicitudes de memoria. En tales casos, puede observar que el espacio de trabajo de la mayoría o de la totalidad de las aplicaciones no disminuye de manera considerable. Para controlar este comportamiento, puede supervisar el contador de Monitor de rendimiento siguiente para todas las aplicaciones del sistema:

  - Objeto de rendimiento: Proceso
  - Contador: Espacio de trabajo
  
  Además, supervise el contador siguiente para correlacionar la cantidad de memoria física disponible en el sistema.
  
  - Objeto de rendimiento: Memoria
  - Contador: Memoria disponible (MB)
  
  El comportamiento típico que se puede observar es la reducción de la memoria disponible cerca de 0 MB al mismo tiempo que disminuyen repentinamente los contadores de espacio de trabajo para la mayoría de (todos) los procesos del sistema. Si observa este comportamiento, puede que tenga que tomar medidas para reducir el uso de la memoria en el sistema, las que incluyen, por ejemplo, la reducción de la memoria máxima del servidor para SQL Server.
  
    También es posible que las aplicaciones usen demasiado la caché del sistema, lo que puede generar un gran crecimiento de esta caché. Como respuesta al crecimiento de la caché del sistema, el sistema pagina el espacio de trabajo del proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o de otras aplicaciones. Si experimenta este problema, puede usar algunas funciones de administración de memoria en la aplicación. Estas funciones controlan el espacio de la caché del sistema que las operaciones de E/S de archivos pueden usar en la aplicación. Por ejemplo, puede usar la función SetSystemFileCacheSize y la función GetSystemFileCacheSize para controlar el espacio de la caché del sistema que pueden usar las operaciones de E/S de archivos.
  
    Puede usar el objeto Rendimiento de memoria para ver los valores de distintos contadores en este objeto para determinar si el espacio de trabajo de la caché del sistema usa demasiada memoria. Por ejemplo, puede ver los contadores Bytes de caché y Bytes residentes de caché del sistema. Para más información sobre este tema, consulte:
  
    - [¿Demasiada caché?](/archive/blogs/ntdebugging/too-much-cache)
    - [Servicio de caché dinámica de Microsoft Windows](/archive/blogs/ntdebugging/microsoft-windows-dynamic-cache-service)
    - [Experimenta problemas de rendimiento en aplicaciones y servicios cuando la caché de archivos de sistema consume la mayor parte de la RAM física](https://support.microsoft.com/help/976618)
  
    Puede descargar e implementar el "Servicio de caché dinámica de Microsoft Windows" para controlar la memoria que consume la caché del sistema.

- **Problemas de los controladores de dispositivos**: si un controlador de dispositivo usa la función `MmAllocateContiguousMemory`, y si establece el valor del parámetro HighestAcceptableAddress en menos de 4 gigabytes (GB), el sistema operativo Windows puede paginar el espacio de trabajo de los procesos del sistema, incluido el proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para resolver este problema, póngase en contacto con el proveedor del controlador del dispositivo para obtener las actualizaciones del controlador.

    Cuando un controlador de dispositivo intenta asignar memoria, el sistema operativo Windows puede paginar el espacio de trabajo de otras aplicaciones. Esta revisión de Windows le permite usar el seguimiento de eventos para buscar el controlador de dispositivo que causa el problema. Para más información sobre el controlador específico que provoca el comportamiento de recorte del espacio de trabajo, consulte el artículo sobre la [identificación de los controladores que asignan la memoria contigua](/previous-versions/windows/desktop/xperf/identifying-drivers-that-allocate-contiguous-memory).

- **Problemas de sistema operativo**: para resolver los problemas conocidos que hacen que el sistema operativo Windows pagine el espacio de trabajo del proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], aplique las revisiones que se describen en los artículos siguientes de Microsoft Knowledge Base.

  > [!NOTE]
  > Las revisiones son acumulativas. Una versión posterior de una revisión contiene las versiones anteriores de dicha revisión.

  - El espacio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se puede recortar cuando el sistema usa algunas características avanzadas de TCP. Para más información, consulte el artículo sobre cómo [solucionar problemas con características avanzadas del rendimiento de la red, como RSS y NetDMA](/troubleshoot/windows-server/networking/troubleshoot-network-performance-features-rss-netdma).

  - Si ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en Windows Server 2008, debe aplicar revisiones para los problemas conocidos que pueden hacer que sea necesario recortar el espacio de trabajo o el consumo excesivo de memoria por parte de otros componentes del sistema operativo. Para más información, revise el artículo que indica que [el proceso de generación de informes puede dejar de responder cuando se ejecuta Perfmon.exe con la plantilla de diagnósticos de Active Directory para generar un informe sobre un controlador de dominio basado en Windows Server 2008](/troubleshoot/windows-server/performance/report-generation-process-stops-responding).

  - Si ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en Windows Server 2008 R2, debe aplicar revisiones para los problemas conocidos que pueden hacer que sea necesario recortar el espacio de trabajo. Para más información, revise los artículos siguientes:

    - [Un equipo que ejecuta Windows 7 o Windows Server 2008 R2 deja de responder cuando ejecuta una aplicación grande](https://support.microsoft.com/help/979149)
    - [Rendimiento deficiente de un equipo que tiene procesadores basados en NUMA y que ejecuta Windows Server 2008 R2 o Windows 7 si un subproceso solicita mucha memoria que está dentro de los primeros 4 GB de la memoria](https://support.microsoft.com/help/2155311)
    - [El equipo funciona de manera intermitente o deja de responder cuando se usa el controlador Storport en Windows Server 2008 R2](https://support.microsoft.com/help/2468345)

## <a name="important-considerations-before-you-assign-the-lock-pages-in-memory-user-right"></a>Consideraciones importantes de asignar el permiso del usuario "Bloquear páginas en la memoria"

Debe tener en cuenta ciertas consideraciones adicionales antes de asignar el permiso del usuario Bloquear páginas en la memoria. Si asigna este permiso del usuario en sistemas que no están configurados correctamente, el sistema puede volverse inestable o experimentar una disminución del rendimiento de todo el sistema. Además, se puede registrar el id. de evento 333 en el registro de eventos.

Si se pone en contacto con el servicio de soporte al cliente (CSS) de Microsoft para recibir ayuda sobre estos problemas, es probable que los ingenieros del CSS le pidan revocar este permiso del usuario para la cuenta de usuario que se usa como cuenta de inicio del servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Este paso puede ser necesario para recopilar datos de rendimiento importantes que los ingenieros del CSS pueden usar para la configuración necesaria de las diversas opciones para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y para otras aplicaciones que se ejecutan en el sistema. Una vez que los ingenieros del CSS recopilen los datos de rendimiento, puede asignar el permiso del usuario Bloquear páginas en la memoria a la cuenta de inicio del servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Antes de asignar el permiso del usuario Bloquear páginas en la memoria, asegúrese de capturar un registro del Monitor de rendimiento para determinar los requisitos de memoria de distintas aplicaciones y servicios instalados en el sistema. Estas aplicaciones también incluyen SQL Server. Para determinar los requisitos de memoria, recopile la información de línea de base siguiente:

- Asegúrese de establecer correctamente las opciones Memoria máxima del servidor y Memoria mínima del servidor. Estas opciones reflejan solo los requisitos de memoria del grupo de búferes del proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Estas opciones no incluyen la memoria asignada para otros componentes del proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Estos componentes incluyen los siguientes:

  - Los subprocesos de trabajo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  - Componentes y archivos DLL que el proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] carga en el espacio de direcciones del proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  - Operaciones de copia de seguridad y restauración.

- Los componentes y los archivos DLL incluyen varios proveedores OLE DB, procedimientos almacenados extendidos, objetos COM de Microsoft que se usa para el procedimiento almacenado sp_OACreate, servidores vinculados y CLR de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La memoria que se asigna para estos componentes se encuentra en la región del grupo que no es de búferes del espacio de direcciones del proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para determinar la cantidad máxima de memoria que puede usar todo el proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], debe restar la memoria que se asigna para los componentes que no usan el grupo de búferes de la memoria total que quiere que el proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] use. Después, puede usar el valor restante para establecer la opción de memoria máxima del servidor. Antes de establecer las opciones de memoria máxima del servidor y de memoria mínima del servidor, debe revisar cuidadosamente el tema "Establecimiento manual de las opciones de memoria" en Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

- Determine el requisito de memoria de otras aplicaciones y de los componentes del sistema operativo Windows. Las aplicaciones pueden incluir otros componentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], por ejemplo, Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], Agentes de replicación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Reporting Services, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Analysis Services, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Integration Services y Búsqueda de texto completo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Las aplicaciones que realizan operaciones de copia de seguridad y operaciones de copia de archivos pueden grandes cantidades de memoria. Considere operaciones como la copia masiva y el Agente de instantáneas que generan E/S de archivos. Debe tener en cuenta los requisitos de memoria de todas estas aplicaciones al determinar el valor de las opciones Memoria máxima del servidor y Memoria mínima del servidor. Puede usar el contador Bytes privados y el contador Espacio de trabajo en el objeto Proceso de cada proceso con el fin de determinar los requisitos de memoria de un proceso específico.

- De manera predeterminada, el permiso del usuario Bloquear páginas en la memoria ya se ha asignado a la cuenta integrada del sistema local. Para más información, visite el sitio web de Microsoft: [¿Es necesario asignar el privilegio Bloquear páginas en la memoria para el sistema local?](https://techcommunity.microsoft.com/t5/sql-server-support/do-i-have-to-assign-the-lock-pages-in-memory-privilege-for-local/ba-p/315426?advanced=false&collapse_discussion=true&search_type=thread)

- Si usa una cuenta de usuario de Windows de manera global para todos los procesos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de un dominio, determine los permisos del usuario que se asignan mediante el uso de una configuración de directiva de grupo. Un proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de 32 bits puede usar esta cuenta como la cuenta de inicio. Sin embargo, esta cuenta requiere el permiso del usuario Bloquear páginas en la memoria para habilitar la característica `Address Windowing Extensions` (AWE). Para más información, consulte el tema "Proporcionar la cantidad máxima de memoria a SQL Server" en Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

- Antes de configurar las opciones Memoria máxima del servidor y Memoria mínima del servidor para varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], tenga en cuenta los requisitos de memoria del grupo que no es de búferes para cada instancia de SQL Server. A continuación, configure estas opciones para cada instancia de SQL Server.

Lo ideal es recopilar esta información de línea de base durante las cargas máximas. Por lo tanto, puede determinar los requisitos de memoria para diferentes aplicaciones y componentes a fin de admitir la carga máxima. Los requisitos de memoria varían de un sistema a otro, en función de las actividades y las aplicaciones que se ejecutan en el sistema. Puede consultar la información que se proporciona en la vista de administración dinámica sys.dm_os_process_memory para comprender si el sistema encuentra condiciones de memoria insuficiente. Para más información, consulte [sys.dm_os_process_memory (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md).

## <a name="improvements-added-in-windows-server-2008-and-r2-version"></a>Mejoras agregadas en Windows Server 2008 y R2

Windows Server 2008 y Windows Server 2008 R2 mejoran el mecanismo de asignación de la memoria contigua. Esta mejora permite a Windows Server 2008 y a Windows Server 2008 R2 reducir hasta cierto punto los efectos de la paginación del espacio de trabajo de las aplicaciones cuando llegan solicitudes de memoria nuevas.

A continuación se explican las mejoras de las notas del producto de Microsoft "Avances en la administración de memoria de Windows":

*En Windows Server 2008, la asignación de memoria contigua física mejoró considerablemente. Es mucho más probable que las solicitudes de asignación de memoria contigua se realicen correctamente, ya que ahora el administrador de memoria reemplaza dinámicamente las páginas, por lo general sin recortar el espacio de trabajo ni realizar operaciones de E/S. Además, muchos más tipos de páginas, como las pilas de kernel y las páginas de metadatos del sistema de archivos, entre otras, ahora son candidatas para el reemplazo. Por consiguiente, hay más memoria contigua disponible con carácter general en un momento determinado. Además, se reduce en gran medida el costo de obtener tales asignaciones.*

Para más información, consulte el artículo sobre los [problemas con el recorte del espacio de trabajo de SQL Server](https://techcommunity.microsoft.com/t5/sql-server-support/sql-server-working-set-trim-problems-consider/ba-p/315462?advanced=false&collapse_discussion=true&search_type=thread).

Los productos de terceros que se tratan en este artículo están fabricados por compañías independientes de Microsoft. Microsoft no otorga ninguna garantía, implícita o de otro tipo, sobre el rendimiento o la confiabilidad de estos productos.