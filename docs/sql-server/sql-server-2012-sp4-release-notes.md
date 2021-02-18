---
title: Notas de la versión de SQL Server 2012 Service Pack | Microsoft Docs
description: Este artículo contiene las notas de la versión agregadas de los cuatro Service Pack de SQL Server 2012. Cada Service Pack es una acumulación de Service Pack anteriores.
ms.prod: sql
ms.technology: release-landing
ms.custom: ''
ms.date: 07/22/2020
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 67cb8b3e-3d82-47f4-840d-0f12a3bff565
author: rothja
ms.author: jroth
monikerRange: = sql-server-2016
ms.openlocfilehash: 83a7828c93d6863e99d88d3cce45654e8f73abbd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341120"
---
# <a name="sql-server-2012-service-pack-release-notes"></a>Notas de la versión de SQL Server 2012 Service Pack
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]
Este tema contiene las notas de la versión agregadas de los cuatro Service Pack para SQL Server 2012. Cada Service Pack es una acumulación de Service Pack anteriores.

Los Service Pack solo están disponibles en línea, no en los soportes de instalación, y se pueden descargar como se indica a continuación:
- [SQL Server 2012 SP4](https://go.microsoft.com/fwlink/?linkid=846937)
- [SQL Server 2012 SP3](https://support.microsoft.com/help/3072779/sql-server-2012-service-pack-3-release-information)
- [SQL Server 2012 SP2](https://support.microsoft.com/KB/2958429)
- [SQL Server 2012 SP1](https://go.microsoft.com/fwlink/p/?LinkID=268158)

## <a name="service-pack-4-release-notes"></a>Notas de la versión del Service Pack 4

### <a name="download-pages"></a>Páginas de descarga

- [SQL Server 2012 SP4 Feature Pack](https://www.microsoft.com/download/details.aspx?id=56041)
- [Instalación de la revisión SQL Server 2012 SP4](https://www.microsoft.com/download/details.aspx?id=56040)
- [SQL Server 2012 SP4 Express](https://www.microsoft.com/download/details.aspx?id=56042)


### <a name="performance-and-scale-improvements"></a>Mejoras de rendimiento y escalado
- **Procedimiento de limpieza del agente de distribución mejorado**: una base de datos de distribución demasiado grande ha provocado la situación de bloqueo e interbloqueo. Un procedimiento de limpieza mejorado aspira a eliminar algunos de estos escenarios de bloqueo o interbloqueo. 
- **Escalado de objetos de memoria dinámico**: partición dinámica de objetos de memoria según el número de nodos y núcleos para escalar en hardware moderno. El objetivo de la promoción dinámica es evitar posibles cuellos de botella y realizar automáticamente la partición de un objeto de memoria seguro para subprocesos. Los objetos de memoria no particionados se pueden promocionar de forma dinámica para particionarse por nodo. El número de particiones es igual al número de nodos NUMA. Los objetos de memoria particionados por nodo se pueden promocionar aún más para particionarse por CPU, donde el número de particiones es igual al número de CPU.
- **Habilitar > 8TB para grupo de búferes**: habilita el espacio de direcciones virtual de 128 TB para uso por parte del grupo de búferes
- **Limpieza de Change Tracking**: rendimiento y eficacia mejorados de limpieza del seguimiento de cambios para tablas de Change Tracking. 

### <a name="supportability-and-diagnostics-improvements"></a>Mejoras de compatibilidad y diagnóstico
- **Compatibilidad de volcados de memoria completos con los agentes de replicación**: en este momento si los agentes de replicación detectan una excepción no controlada, el comportamiento predeterminado es crear un minivolcado de los síntomas de la excepción. El comportamiento predeterminado exige complejos pasos de solución de problemas para las excepciones no controladas. SP4 presenta una nueva clave del Registro que admite la creación de un volcado completo para los agentes de replicación.
- **Diagnósticos mejorados en el plan de presentación XML**: el plan de presentación XML se ha mejorado para exponer información sobre las marcas de seguimiento habilitadas, las fracciones de memoria para la combinación de bucle anidado optimizada, el tiempo de CPU y el tiempo transcurrido. 
- **Mejor correlación entre DMV y XE de diagnóstico**: los campos query_hash y query_plan_hash se usan para identificar una consulta de forma exclusiva. DMV los define como varbinary (8), mientras que XEvent los define como UINT64. Puesto que SQL Server no tiene "bigint sin signo", la conversión no siempre funciona. Esta mejora presenta nuevas columnas de filtro o acción XEvent equivalentes a query_hash y query_plan_hash, salvo que se definen como INT64, lo que puede ayudar a correlacionar consultas entre XE y DMV. 
- **Mejores diagnósticos de concesión o uso de memoria**: nuevo XEvent query_memory_grant_usage (actualización retroactiva desde Server 2016 SP1)
- **Adición de seguimiento de protocolos a los pasos de negociación SSL**: agrega información de seguimiento de bits de negociación correcta o errónea, incluido el protocolo, etc. Puede ser útil a la hora de solucionar problemas de escenarios de conectividad mientras, por ejemplo, se implementa TLS 1.2
- **Establecimiento del nivel de compatibilidad correcto para la base de datos de distribución**: después de la instalación del Service Pack, cambia el nivel de compatibilidad de la base de datos de distribución a 90. El cambio de nivel se debe a un problema en el procedimiento almacenado sp_vupgrade_replication. Ahora el SP se ha modificado para establecer el nivel de compatibilidad correcto para la base de datos de distribución. 
- **Nuevo comando DBCC para clonar una base de datos**: Base de datos clonada es un nuevo comando DBCC agregado que permite a los usuarios avanzados, como CSS, solucionar problemas de bases de datos de producción existentes mediante la clonación del esquema y los metadatos, sin los datos. La llamada se realiza con clonedatabase de DBCC ("source_database_name", "clone_database_name"). Las bases de datos clonadas no se deben usar en entornos de producción. Para ver si una base de datos se ha generado a partir de una llamada a la base de datos clonada, puede usar el comando siguiente, select DATABASEPROPERTYEX('clonedb', 'isClone'). El valor devuelto de 1 es true y 0 es false. 
- **Información del archivo TempDB y del tamaño de archivo en el registro de errores de SQL**: si el tamaño y el crecimiento automático es diferente para los archivos de datos de TempDB durante el inicio, imprime el número de archivos y desencadena una advertencia.
- **IFI admite mensajes en el registro de errores de SQL Server**: indica en el registro de errores que la inicialización instantánea de archivos de base de datos está habilitada o deshabilitada
- **Nueva DMF para reemplazar a DBCC INPUTBUFFER**: se ha presentado una nueva función de administración dinámica sys.dm_input_buffer que toma session_id como parámetro para reemplazar a DBCC INPUTBUFFER
- **Mejora de XEvents para el error de enrutamiento de lectura de un grupo de disponibilidad**: actualmente el XEvent read_only_rout_fail solo se desencadena si hay una lista de enrutamiento, pero ninguno de los servidores de la lista de enrutamiento está disponible para las conexiones. Esta mejora incluye información adicional para ayudar a solucionar el problema y además se expande en los puntos de código donde se desencadena el XEvent. 
- **Mejora del control de Service Broker con conmutación por error de grupo de disponibilidad**: actualmente, cuando Service Broker está habilitado en bases de datos de un grupo de disponibilidad, durante una conmutación por error del grupo de disponibilidad se dejan abiertas todas las conexiones de Service Broker originadas en la réplica principal. La mejora cierra todas estas conexiones abiertas durante una conmutación por error del grupo de disponibilidad.
- **Creación de particiones de soft-NUMA automática**: con SQL 2014 SP2, se presenta la creación de particiones de [soft-NUMA](../database-engine/configure-windows/soft-numa-sql-server.md) automática cuando la marca de seguimiento 8079 está habilitada en el nivel de servidor. Cuando la marca de seguimiento 8079 está habilitada durante el inicio, SQL Server 2014 SP2 interroga al diseño de hardware y configura automáticamente soft-NUMA en los sistemas que notifican ocho o más CPU por nodo NUMA. El comportamiento de soft-NUMA automática reconoce el hiperproceso (procesador HT/lógico). La creación de particiones y la creación de nodos adicionales escala el procesamiento en segundo plano al aumentar el número de agentes de escucha, escalado y capacidades de red y cifrado. Se recomienda probar primero el rendimiento de la carga de trabajo con soft-NUMA automática antes de activarla en producción.

## <a name="service-pack-3-release-notes"></a>Notas de la versión del Service Pack 3

### <a name="download-pages"></a>Páginas de descarga
- [SQL Server 2012 SP3 Feature Pack](https://go.microsoft.com/fwlink/?linkid=615935)
- [SQL Server 2012 SP3 Express](https://go.microsoft.com/fwlink/?linkid=692144)

Para obtener más información para identificar la ubicación y el nombre del archivo que quiere descargar en función de la versión instalada actualmente, vea la sección "Seleccione el archivo correcto para descargar e instalar" en [Información de la versión de SQL Server 2012 Service Pack 3](https://support.microsoft.com/help/3072779/sql-server-2012-service-pack-3-release-information).

## <a name="service-pack-2-release-notes"></a>Notas de la versión del Service Pack 2
  
### <a name="download-pages"></a>Páginas de descarga 
- [SQL Server 2012 SP2 Feature Pack](https://www.microsoft.com/download/details.aspx?id=43340)
- [SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)

Use la tabla siguiente para determinar la ubicación y el nombre del archivo que debe descargar en función de la versión que tenga instalada actualmente. Las páginas de descarga contienen los requisitos del sistema e instrucciones básicas para la instalación.  

|Si la versión que tiene instalada actualmente es...|Y desea...|Descargue e instale...|  
|---|---|---|   
|Instalaciones de 32 bits:|||  
|Una versión de 32 bits de cualquier edición de SQL Server 2012|Actualizar a la versión de 32 bits de SQL Server 2012 SP2|**SQLServer2012SP2-KB2958429-** <arch> **-** <lang id> **.exe** desde la página de descarga de [SQL Server 2012 SP2](https://go.microsoft.com/fwlink/?LinkID=401006)|  
|Una versión de 32 bits de SQL Server 2012 RTM Express|Actualizar a la versión de 32 bits de SQL Server 2012 Express SP2|**SQLEXPR_** <arch> **_** <lang> **.msi** desde la [página de descarga de SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Una versión de 32 bits únicamente las herramientas de cliente y de administración para SQL Server 2012 (incluido SQL Server 2012 Management Studio)|Actualizar las herramientas de cliente y de administración a la versión de 32 bits de SQL Server 2012 SP2|**SQLEXPRWT_** <arch> **_** <lang> **.msi** desde la [página de descarga de SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Una versión de 32 bits de SQL Server 2012 Management Studio Express|Actualizar a la versión de 32 bits de SQL Server 2012 SP2 Management Studio Express|**SQLManagementStudio_** <arch> **_** <lang> **.msi** desde la [página de descarga de SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Una versión de 32 bits de cualquier edición de SQL Server 2012 y una versión de 32 bits de las herramientas de cliente y de administración (incluido SQL Server 2012 RTM Management Studio)|Actualizar todos los productos a la versión de 32 bits de SQL Server 2012 SP2|**SQLEXPRADV_** <arch> **_** <lang> **.msi** desde la [página de descarga de SQL Server 2012 SP2 Express.](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Una versión de 32 bits de una o más herramientas del [Feature Pack de Microsoft SQL Server 2012 RTM](https://www.microsoft.com/download/details.aspx?id=56041) o el [Feature Pack de Microsoft SQL Server 2012 SP1](https://www.microsoft.com/download/details.aspx?id=35575)|Actualizar las herramientas a la versión de 32 bits del Feature Pack de Microsoft SQL Server 2012 S2|Una o varias herramientas de [la página de descarga del Feature Pack de SQL Server 2012 SP2 de Microsoft](https://www.microsoft.com/download/details.aspx?id=43340)|  
|Instalaciones de 64 bits:|||  
|Una versión de 64 bits de cualquier edición de SQL Server 2012|Actualizar a la versión de 64 bits de SQL Server 2012 SP2|SQLServer2012SP2-KB2958429-<arch>-<langid>.exe desde la [página de descarga de SQL Server 2012 SP2](https://go.microsoft.com/fwlink/?LinkID=401006)|  
|Una versión de 64 bits de SQL Server 2012 RTM Express|Actualizar a la versión de 64 bits de SQL Server 2012 SP2|**SQLEXPR_** <arch> **_** <lang> **.msi** desde la [página de descarga de SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Una versión de 64 bits únicamente las herramientas de cliente y de administración para SQL Server 2012 (incluido SQL Server 2012 Management Studio)|Actualizar las herramientas de cliente y de administración a la versión de 64 bits de SQL Server 2012 SP2|**SQLEXPRWT_** <arch> **_** <lang> **.msi** desde la [página de descarga de SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Una versión de 64 bits de SQL Server 2012 Management Studio Express|Actualizar a la versión de 64 bits de SQL Server 2012 SP2 Management Studio Express|**SQLManagementStudio_** <arch> **_** <lang> **.msi** desde la [página de descarga de SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Una versión de 64 bits de una o más herramientas del [Feature Pack de Microsoft SQL Server 2012 RTM](https://www.microsoft.com/download/details.aspx?id=56041) o el [Feature Pack de Microsoft SQL Server 2012 SP1](https://www.microsoft.com/download/details.aspx?id=35575)|Actualizar las herramientas a la versión de 64 bits del Feature Pack de Microsoft SQL Server 2012 S2|Una o varias herramientas de [la página de descarga del Feature Pack de SQL Server 2012 SP2 de Microsoft](https://www.microsoft.com/download/details.aspx?id=43340)|   


## <a name="service-pack-1-release-notes"></a>Notas de la versión del Service Pack 1

### <a name="download-pages"></a>Páginas de descarga  
- [SQL Server 2012 SP1 Feature Pack](https://go.microsoft.com/fwlink/p/?LinkID=268158)
- [SQL Server 2012 SP1 Express](https://www.microsoft.com/download/details.aspx?id=35579)


Use la tabla siguiente para determinar qué archivo va a descargar e instalar. Compruebe que cumple los requisitos del sistema correctos antes de instalar el Service Pack. Los requisitos del sistema se proporcionan en las páginas de descarga cuyos vínculos aparecen en la tabla.  

|Si la versión que tiene instalada actualmente es...|Y desea...|Descargue e instale...|  
|---|---|---|  
|**Instalaciones de 32-bits:**|||  
|Una versión de 32 bits de cualquier edición de SQL Server 2012|Actualizar a la versión de 32 bits de SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x86-ENU.exe desde [aquí](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Una versión de 32 bits de SQL Server 2012 RTM Express|Actualizar a la versión de 32 bits de SQL Server 2012 Express SP1|SQLServer2012SP1-KB2674319-x86-ENU.exe desde [aquí](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Una versión de 32 bits únicamente las herramientas de cliente y de administración para SQL Server 2012 (incluido SQL Server 2012 Management Studio)|Actualizar las herramientas de cliente y de administración a la versión de 32 bits de SQL Server 2012 SP1|SQLManagementStudio_x86_ENU.exe desde [aquí](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Una versión de 32 bits de SQL Server 2012 Management Studio Express|Actualizar a la versión de 32 bits de SQL Server 2012 SP1 Management Studio Express|SQLManagementStudio_x86_ENU.exe desde [aquí](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Una versión de 32 bits de cualquier edición de SQL Server 2012 **y** una versión de 32 bits de las herramientas de cliente y de administración (incluido SQL Server 2012 RTM Management Studio)|Actualizar todos los productos a la versión de 32 bits de SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x86-ENU.exe desde [aquí](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Una versión de 32 bits de una o más herramientas del [Microsoft SQL Server 2012 RTM Feature Pack](https://www.microsoft.com/download/details.aspx?id=44272)|Actualizar las herramientas a la versión de 32 bits del Microsoft SQL Server 2012 SP1 Feature Pack|Uno o más archivos del [Microsoft SQL Server 2012 SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575)|  
|No instalar la versión de 32 bits de SQL Server 2012|Instalar una versión de 32 bits de Server 2012 incluido SP1 (nueva instancia con SP1 preinstalado)|SQLServer2012SP1-FullSlipstream-x86-ENU.exe **y** SQLServer2012SP1-FullSlipstream-x86-ENU.box desde [aquí](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|No instalar la versión de 32 bits de SQL Server 2012 Management Studio|Instalar la versión de 32 bits de SQL Server 2012 Management Studio junto con el SP1|SQLManagementStudio_x86_ENU.exe desde [aquí](https://go.microsoft.com/fwlink/p/?LinkId=267905)|  
|Sin versión de 32 bits de SQL Server 2012 RTM Express|Instalar versión de 32 bits de SQL Server 2012 Express incluido el SP1|SQLEXPR32_x86_ENU.exe desde [aquí](https://go.microsoft.com/fwlink/p/?LinkId=267905)|  
|Instalación de una versión de 32 bits de **SQL Server 2008** o **SQL Server 2008 R2**|**Actualización en contexto** a la versión de 32 bits de SQL Server 2012 incluido el SP1|SQLServer2012SP1-FullSlipstream-x86-ENU.exe **y** SQLServer2012SP1-FullSlipstream-x86-ENU.box desde [aquí](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|**Instalaciones de 64-bits:**|||  
|Una versión de 64 bits de cualquier edición de SQL Server 2012|Actualizar a la versión de 64 bits de SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x64-ENU.exe desde [aquí](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Una versión de 64 bits de SQL Server 2012 RTM Express|Actualizar a la versión de 64 bits de SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x64-ENU.exe desde [aquí](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Una versión de 64 bits únicamente las herramientas de cliente y de administración para SQL Server 2012 (incluido SQL Server 2012 Management Studio)|Actualizar las herramientas de cliente y de administración a la versión de 64 bits de SQL Server 2012 SP1|SQLManagementStudio_x64_ENU.exe desde [aquí](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Una versión de 64 bits de SQL Server 2012 Management Studio Express|Actualizar a la versión de 64 bits de SQL Server 2012 SP1 Management Studio Express|SQLManagementStudio_x64_ENU.exe desde [aquí](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Una versión de 64 bits de cualquier edición de SQL Server 2012 **y** una versión de 64 bits de las herramientas de cliente y de administración (incluido SQL Server 2012 RTM Management Studio)|Actualizar todos los productos a la versión de 64 bits de SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x64-ENU.exe desde [aquí](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Una versión de 64 bits de una o más herramientas del [Microsoft SQL Server 2012 RTM Feature Pack](https://www.microsoft.com/download/details.aspx?id=44272)|Actualizar las herramientas a la versión de 64 bits del Microsoft SQL Server 2012 SP1 Feature Pack|Uno o más archivos del [Microsoft SQL Server 2012 SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575)|  
|No instalar la versión de 64 bits de SQL Server 2012|Instalar una versión de 64 bits de Server 2012 incluido SP1 (nueva instancia con SP1 preinstalado)|SQLServer2012SP1-FullSlipstream-x64-ENU.exe **y** SQLServer2012SP1-FullSlipstream-x64-ENU.box desde [aquí](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|No instalar la versión de 64 bits de SQL Server 2012 Management Studio|Instalar la versión de 64 bits de SQL Server 2012 Management Studio junto con el SP1|SQLManagementStudio_x64_ENU.exe desde [aquí](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Sin versión de 64 bits de SQL Server 2012 RTM Express|Instalar versión de 64 bits de SQL Server 2012 Express incluido el SP1|SQLEXPR_x64_ENU.exe desde [aquí](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Instalación de una versión de 64 bits de **SQL Server 2008** o **SQL Server 2008 R2**|**Actualización en contexto** a la versión de 64 bits de SQL Server 2012 incluido el SP1|SQLServer2012SP1-FullSlipstream-x64-ENU.exe **y** SQLServer2012SP1-FullSlipstream-x64-ENU.box desde [aquí](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  

### <a name="known-issues-fixed-in-this-service-pack"></a>Problemas conocidos corregidos en este Service Pack  
Para obtener una lista completa de errores y de problemas conocidos corregidos en este Service Pack, vea [este artículo de KB](https://support.microsoft.com/kb/2674319).   

### <a name="reinstalling--instances-of-sql-server-failover-cluster-fails-if-you-use-the-same-ip-address"></a>La reinstalación de instancias de un clúster de conmutación por error de SQL Server produce un error si usa la misma dirección IP  
**Problema:** Si especifica una dirección IP incorrecta durante la instalación de una instancia de clúster de conmutación por error de SQL Server, la instalación produce errores. Después de desinstalar la instancia con errores, y si intenta reinstalar la instancia de clúster de conmutación por error de SQL Server con el mismo nombre de instancia, y la dirección IP correcta, la instalación produce errores. El error se debe al grupo de recursos duplicados que deja atrás la instalación anterior.  
  
**Solución alternativa:** para resolver este problema, use otro nombre de instancia durante la reinstalación, o bien elimine manualmente el grupo de recursos antes de reinstalar. Para obtener más información, vea [Agregar o quitar nodos en un clúster de conmutación por error de SQL Server](failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md). 
  
### <a name="analysis-services-and-powerpivot"></a>Analysis Services y PowerPivot  
  
##### <a name="powerpivot-configuration-tool-does-not-create-the-powerpivot-gallery"></a>La herramienta de configuración de PowerPivot no crea la Galería de PowerPivot  
**Problema:** La herramienta de configuración de PowerPivot aprovisiona un sitio de grupo y, por tanto, la Galería de PowerPivot no se crea.  
  
**Solución alternativa:** Cree una nueva aplicación (biblioteca).  
  
1.  Compruebe si la característica **Integración de características de PowerPivot para colecciones de sitios** está activa.  
  
2.  En la página **Contenido del sitio** de un sitio existente, haga clic en **Agregar aplicación**.  
  
3.  Haga clic en **Galería de PowerPivot**.  
  
#### <a name="to-use-powerpivot-for-excel-with-excel-2013-you-must-use-the-add-in-that-is-installed-with-excel"></a>Para usar PowerPivot para Excel con Excel 2013, debe usar el complemento que se instala con Excel  
**Problema:** con Office 2010, PowerPivot para Excel es un complemento independiente que se puede descargar desde [https://www.microsoft.com/bi/powerpivot.aspx](https://www.microsoft.com/bi/powerpivot.aspx). También se puede descargar desde el [Centro de descarga Microsoft](https://www.microsoft.com/download/details.aspx?id=29074). Tenga en cuenta que hay dos versiones del complemento PowerPivot como una descarga: Uno que se incluye con SQL Server 2008 R2 y otro que se incluye con SQL Server 2012. Sin embargo, en el caso de Office 2013, PowerPivot para Excel se incluye con Office y se instala al instalar Excel. Si bien las versiones de SQL Server 2008 R2 y SQL Server 2012 de PowerPivot para Excel 2010 no son compatibles con Excel 2013, puede instalar PowerPivot para Excel 2010 en el equipo cliente si desea ejecutar Excel 2010 en paralelo con Excel 2013. Es decir, las dos versiones de Excel pueden coexistir, así como sus complementos PowerPivot correspondientes.  
  
**Solución alternativa:** Para usar PowerPivot para Excel 2013 debe habilitar el complemento COM. En Excel 2013, seleccione **Archivo** | **Opciones** | **Complementos**. En el cuadro desplegable **Administrar** , seleccione **Complementos COM** y haga clic en **Ir**. En **Complementos COM**, seleccione **Microsoft Office PowerPivot para Excel 2013** y haga clic en **Aceptar**.  
  
### <a name="reporting-services"></a>Reporting Services  
  
#### <a name="install-and-configure-sharepoint-server-2013-prior-to-installing-reporting-services"></a>Instalar y configurar SharePoint Server 2013 antes de instalar Reporting Services  
**Problema:** complete los requisitos siguientes **antes** de instalar SQL Server Reporting Services (SSRS).  
  
1.  Ejecutar la herramienta de preparación de Productos de SharePoint 2013.  
  
2.  Instalar SharePoint Server 2013.  
  
3.  Ejecutar el Asistente para configuración de Productos de SharePoint 2013 o completar un conjunto equivalente de pasos de configuración para configurar la granja de SharePoint.  
  
**Solución alternativa:**  Si instaló el modo SharePoint de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] antes de configurar la granja de SharePoint, la solución alternativa necesaria depende de los otros componentes que están instalados.  
  
#### <a name="power-view-in-sharepoint-server-2013-requires-microsoftanalysisservicesspclientdll"></a>Power View en SharePoint Server 2013 requiere Microsoft.AnalysisServices.SPClient.dll  
**Problema:** [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] no instala un componente requerido, **Microsoft.AnalysisServices.SPClient.dll**. Si instala Vista previa de SharePoint Server 2013 y [!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)][!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] en modo SharePoint, pero no descarga e instala el paquete del instalador de PowerPivot para SharePoint 2013, **spPowerPivot.msi** , Power View no funcionará y Power View presentará los siguientes síntomas.  
  
**Síntomas:** Cuando intenta crear un informe de Power View, ve un mensaje de error similar al siguiente:  
  
-   "No se puede crear una conexión al origen de datos...''.  
  
Los detalles del error interno contendrán un mensaje similar al siguiente:  
  
-   "El valor 'SharePoint Principal' no es compatible con la propiedad de la cadena de conexión 'User Identity'."  
  
**Solución alternativa:** instale el paquete del instalador de PowerPivot para SharePoint 2013 (**spPowerPivot.msi**) en SharePoint Server 2013. El paquete del instalador está disponible como parte de [!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)] Feature Pack. Feature Pack se puede descargar desde el centro de descargas de [!INCLUDE[msCoName](../includes/msconame-md.md)] en [SQL Server 2012 SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575)  
  
#### <a name="power-view-sheets-in-a-powerpivot-workbook-are-deleted-after-a-scheduled-data-refresh"></a>Las hojas de Power View en un libro PowerPivot se borran tras una actualización de datos programada  
**Problema:** en el complemento PowerPivot para SharePoint, al usar **Actualización de datos programada** en un libro con Power View, se eliminan todas las hojas de Power View.  
  
**Solución alternativa**: para usar la **Actualización de datos programada** con libros Power View, cree un libro PowerPivot que sea el modelo de datos. Cree otro libro con las hojas de Excel y Power View que se vincule al libro PowerPivot con el modelo de datos. Para la actualización de datos, solo se debe programar el libro PowerPivot con el modelo de datos.  
  
### <a name="data-quality-services"></a>Data Quality Services  
  
#### <a name="dqs-available-in-the-incorrect-edition-of-sql-server-2012"></a>DQS disponible en la edición incorrecta de SQL Server 2012  
**Problema:** en la versión [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] RTM, la característica Data Quality Services (DQS) está disponible en ediciones de SQL Server distintas de Enterprise, Business Intelligence y Developer. Después de instalar SQL Server 2012 SP1, DQS no estará disponible en todas las ediciones salvo en Enterprise, Business Intelligence y Developer.  
  
**Solución alternativa**: si usa DQS en una edición no admitida, actualice a una edición admitida o desinstale la dependencia de esta característica de sus aplicaciones.  
  
### <a name="sql-server-express"></a>SQL Server Express  
  
#### <a name="full-version-of-sql-server-management-studio-available-in-sql-server-2012-express-sp1"></a>La versión completa de SQL Server Management Studio está disponible en SQL Server 2012 Express SP1  
SQL Server 2012 Express Service Pack 1 (SP1) incluye la versión completa de SQL Server 2012 Management Studio (que antes solo estaba disponible en el DVD de SQL Server 2012) en lugar de SQL Server 2012 Management Studio Express. Para descargar e instalar SQL Server 2012 Express SP1, vea [SQL Server 2012 Express Service Pack 1](https://go.microsoft.com/fwlink/p/?linkid=267905).  
  
### <a name="change-data-capture-service-and-designer-for-oracle-by-attunity"></a>Servicio y diseñador de captura de datos modificados para Oracle de Attunity  
  
#### <a name="upgrading-the-cdc-service-and-designer"></a>Actualizar CDC Service y CDC Designer  
**Problema:** Si Change Data Capture Designer para Oracle y Change Data Capture Service para Oracle de Attunity están instalados en el equipo cuando se instala SQL Server 2012 SP1, estos componentes no se actualizarán con la instalación de SP1.  
  
**Solución alternativa:** Para actualizar los componentes de CDC a la versión más reciente:  
  
1.  Descargue los archivos .msi para Change Data Capture Service para Oracle de Attunity desde la [Página de descarga de Feature Pack de SQL Server 2012 SP1](https://www.microsoft.com/download/details.aspx?id=35575).  
  
2.  Ejecute el archivo .msi.  
  
### <a name="sql-server-data-tier-application-framework-dacfx"></a>SQL Server Data-Tier Application Framework (DACFx)  
**Compatibilidad con la actualización en contexto**  
  
Esta versión del Marco de trabajo de la aplicación de capa de datos (DACFx) es compatible con la actualización en contexto de versiones anteriores por lo que no es necesario quitar instalaciones anteriores de DACFx antes de la actualización a esta versión. Puede encontrar próximas versiones de DACFx [aquí](/previous-versions/sql/sql-server-2016/dn702988(v=sql.130)).  
  
**Compatibilidad con índice XML selectivo**  
  
SQL Server 2012 SP1 es compatible con [Índice XML selectivo (SXI)](../relational-databases/xml/selective-xml-indexes-sxi.md), una nueva característica de SQL Server que proporciona una nueva manera de indizar datos de columnas XML y mejorar así el rendimiento y la eficacia.  
  
DACFx ahora es compatible con índices SXI en todos los escenarios DAC y herramientas cliente. SXI solo es compatible con la versión más reciente de SSDT. La versión RTM y la versión de septiembre de 2012 de SSDT no son compatibles con SXI.  
  
**Compatibilidad con el formato de datos BCP nativo**  
  
Anteriormente, el formato de datos utilizado para almacenar datos de tabla en los paquetes DACPAC y BACPAC era JSON. Con esta actualización, el formato de datos BCP nativo es ahora el formato de persistencia de datos. Este cambio proporciona una fidelidad a DACFx mejorada de los tipos de datos de SQL Server, incluida la compatibilidad para tipos SQL_Variant y una implementación y rendimiento de datos mejorados para bases de datos a gran escala.  
  
**Conservación del estado de la restricción CHECK en la creación e implementación de paquetes**  
  
Anteriormente, DACFx no conservaba el estado (WITH CHECK/NOCHECK) de las restricciones CHECK definidas en tablas en el esquema de base de datos ni almacenaba esta información dentro de los DACPAC. Este comportamiento podía crear posibles problemas en la implementación de paquetes cuando existieran datos de tabla que incumplieran las restricciones CHECK. Con esta actualización, DACFx ahora almacena el estado actual de las restricciones CHECK dentro del DACPAC cuando se extrae de una base de datos y restaura adecuadamente este estado durante la implementación del paquete.  
  
**Actualizaciones de SqlPackage.exe (herramienta de línea de comandos DACFx)**  
  
-   Extraer DACPAC con datos: crea un archivo de instantánea de base de datos (.dacpac) a partir de un servidor SQL Server o una instancia de Azure SQL Database en vivo que contiene datos de tablas de usuario además del esquema de la base de datos. Estos paquetes se pueden publicar en una instancia nueva o existente de SQL Server o Azure SQL Database con la acción Publicar de SqlPackage.exe. Los datos contenidos en el paquete reemplazan a los datos existentes en la base de datos de destino.  
  
-   Exportar BACPAC: crea un archivo de copia de seguridad lógica (.bacpac) a partir de una instancia de SQL Server o Azure SQL Database que contiene el esquema de base de datos y los datos de usuario que se pueden usar para migrar una base de datos desde una instancia local de SQL Server a Azure SQL Database. Las bases de datos compatibles con Azure se pueden exportar y a continuación importar entre versiones compatibles de SQL Server.  
  
-   Importar BACPAC: importa un archivo .bacpac para crear una instancia nueva de SQL Server o Azure SQL Database, o bien para rellenar una vacía.  
  
La documentación completa de SqlPackage.exe en MSDN se puede encontrar [aquí](../tools/sqlpackage/sqlpackage.md).  
  
**Compatibilidad de los paquetes**  
  
Esta versión presenta varios escenario de compatibilidad con versiones posteriores para los paquetes DAC.  
  
-   Los paquetes DAC creados por esta versión que no contengan elementos SXI o datos de tabla se pueden utilizar en versiones anteriores de DACFx (SQL Server 2012 RTM, SQL Server 2012 CU1 y DACFx de septiembre de 2012).  
  
-   Todos los paquetes DAC creados por versiones anteriores de DACFx se pueden utilizar en esta versión.  
  
## <a name="see-also"></a>Consulte también
- [Instalar actualizaciones de servicio de SQL Server 2012](/previous-versions/sql/sql-server-2012/hh479746(v=sql.110))
- [Identificar la versión y edición de SQL Server](https://support.microsoft.com/help/321185)
- [Instalar actualizaciones de servicio de SQL Server 2012](/previous-versions/sql/sql-server-2012/hh479746(v=sql.110))
- [Identificar la versión y edición de SQL Server](https://support.microsoft.com/help/321185) 
- [Cómo determinar la versión y la edición de SQL Server](https://support.microsoft.com/kb/321185)  
- [Características admitidas por las ediciones de SQL Server 2014](./editions-and-components-of-sql-server-2016.md)  

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
