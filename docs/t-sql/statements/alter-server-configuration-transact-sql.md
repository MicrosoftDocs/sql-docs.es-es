---
description: ALTER SERVER CONFIGURATION (Transact-SQL)
title: ALTER SERVER CONFIGURATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER SERVER CONFIGURATION
- ALTER_SERVER_CONFIGURATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SERVER CONFIGURATION, ALTER
- process affinity, setting
- ALTER SERVER CONFIGURATION statement
- setting process affinity
ms.assetid: f3059e42-5f6f-4a64-903c-86dca212a4b4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7a61ed11e141376c0b0c769f961fad4e9520c271
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237052"
---
# <a name="alter-server-configuration-transact-sql"></a>ALTER SERVER CONFIGURATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Modifica las opciones de configuración global del servidor actual en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  

```syntaxsql
ALTER SERVER CONFIGURATION  
SET <optionspec>   
[;]  
  
<optionspec> ::=  
{  
     <process_affinity>  
   | <diagnostic_log>  
   | <failover_cluster_property>  
   | <hadr_cluster_context>  
   | <buffer_pool_extension>  
   | <soft_numa>  
   | <memory_optimized>
}  
  
<process_affinity> ::=   
   PROCESS AFFINITY   
   {  
     CPU = { AUTO | <CPU_range_spec> }   
   | NUMANODE = <NUMA_node_range_spec>   
   }  
   <CPU_range_spec> ::=   
      { CPU_ID | CPU_ID  TO CPU_ID } [ ,...n ]   
  
   <NUMA_node_range_spec> ::=   
      { NUMA_node_ID | NUMA_node_ID TO NUMA_node_ID } [ ,...n ]  
  
<diagnostic_log> ::=   
   DIAGNOSTICS LOG   
   {   
     ON    
   | OFF    
   | PATH = { 'os_file_path' | DEFAULT }    
   | MAX_SIZE = { 'log_max_size' MB | DEFAULT }    
   | MAX_FILES = { 'max_file_count' | DEFAULT }    
   }  
  
<failover_cluster_property> ::=   
   FAILOVER CLUSTER PROPERTY <resource_property>  
   <resource_property> ::=  
      {  
        VerboseLogging = { 'logging_detail' | DEFAULT }    
      | SqlDumperDumpFlags = { 'dump_file_type' | DEFAULT }  
      | SqlDumperDumpPath = { 'os_file_path'| DEFAULT }  
      | SqlDumperDumpTimeOut = { 'dump_time-out' | DEFAULT }  
      | FailureConditionLevel = { 'failure_condition_level' | DEFAULT }  
      | HealthCheckTimeout = { 'health_check_time-out' | DEFAULT }  
      }  
  
<hadr_cluster_context> ::=  
   HADR CLUSTER CONTEXT = { 'remote_windows_cluster' | LOCAL }  
  
<buffer_pool_extension>::=  
    BUFFER POOL EXTENSION   
    { ON ( FILENAME = 'os_file_path_and_name' , SIZE = <size_spec> )   
    | OFF }  
  
    <size_spec> ::=  
        { size [ KB | MB | GB ] }  
  
<soft_numa> ::=  
    SOFTNUMA  
    { ON | OFF }  

<memory-optimized> ::=   
   MEMORY_OPTIMIZED   
   {   
     ON 
   | OFF
   | [ TEMPDB_METADATA = { ON [(RESOURCE_POOL='resource_pool_name')] | OFF }
   | [ HYBRID_BUFFER_POOL = { ON | OFF }
   }  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
**\<process_affinity> ::=**  
  
PROCESS AFFINITY  
Permite asociar los subprocesos de hardware a las CPU.  
  
CPU = { AUTO | \<CPU_range_spec> }  
Distribuye los subprocesos de trabajo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cada CPU dentro del rango especificado. Las CPU que no pertenezcan al rango especificado no tendrán subprocesos asignados.  
  
AUTO  
Especifica que los subprocesos no se asignan a ninguna CPU. El sistema operativo puede mover los subprocesos libremente entre las CPU según la carga de trabajo del servidor. Esta es la configuración predeterminada y recomendada.  
  
\<CPU_range_spec> ::=  
Especifica la CPU o el rango de las CPU a las que se asignan los subprocesos.  
  
{ CPU_ID | CPU_ID  TO  CPU_ID } [ ,...n ]  
Es la lista de una o varias CPU. Los identificadores de CPU comienzan en 0 y son valores **integer**.  
  
NUMANODE = \<NUMA_node_range_spec>  
Asigna subprocesos a todas las CPU que pertenecen al nodo o rango de nodos NUMA especificado.  
  
\<NUMA_node_range_spec> ::=  
Especifica el nodo o rango de nodos NUMA.  
  
{ NUMA_node_ID | NUMA_node_ID  TO NUMA_node_ID } [ ,...n ]  
Es la lista de uno o varios nodos NUMA. Los identificadores del nodo NUMA comienzan por 0 y son valores **integer**.  
  
**\<diagnostic_log> ::=**  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).  

  
DIAGNOSTICS LOG  
Inicia o detiene el registro de datos de diagnóstico que captura el procedimiento sp_server_diagnostics. Este argumento también establece los parámetros de configuración del registro SQLDIAG como el recuento de sustitución incremental de archivos de registro, el tamaño del archivo de registro y la ubicación del archivo. Para obtener más información, vea [Ver y leer el registro de diagnósticos de la instancia de clúster de conmutación por error](../../sql-server/failover-clusters/windows/view-and-read-failover-cluster-instance-diagnostics-log.md).  
  
ACTIVAR  
Inicia el registro de datos de diagnóstico de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] en la ubicación especificada en la opción de archivo PATH. Este es el argumento predeterminado.  
  
Apagado  
Detiene el registro de datos de diagnóstico.  
  
PATH = { 'os_file_path' | DEFAULT }  
Ruta de acceso que indica la ubicación de los registros de diagnóstico. La ubicación predeterminada es \<\MSSQL\Log> en la carpeta de instalación de la instancia del clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
MAX_SIZE = { 'log_max_size' MB | DEFAULT }  
Tamaño máximo en megabytes que cada registro de diagnóstico puede alcanzar. El valor predeterminado es 100 MB.  
  
MAX_FILES = { 'max_file_count' | DEFAULT }  
Número máximo de archivos de registro de diagnóstico que pueden almacenarse en el equipo antes de que se reciclen para nuevos registros de diagnóstico.  
  
**\<failover_cluster_property> ::=**  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).    
  
FAILOVER CLUSTER PROPERTY  
Modifica las propiedades privadas de clúster de conmutación por error de recursos de SQL Server.  
  
VERBOSE LOGGING = { 'logging_detail' | DEFAULT }  
Establece el nivel de registro para clústeres de conmutación por error de SQL Server. Se puede activar para proporcionar detalles adicionales en los registros de errores para solucionar problemas.  
  
-   0: el registro está desactivado (valor predeterminado)  
  
-   1: solo errores  
  
-   2: errores y advertencias  
  
En escenarios de conmutación por error de recursos, la DLL de recursos de SQL Server puede obtener un archivo de volcado antes de que se produzca una conmutación por error. Esto se aplica a las tecnologías FCI y de grupo de disponibilidad. Cuando el archivo DLL de recurso de SQL Server determina que se ha producido un error en un recurso de SQL Server, la DLL usa la utilidad Sqldumper.exe para obtener un archivo de volcado de memoria del proceso de SQL Server. Para asegurarse de que la utilidad Sqldumper.exe genera correctamente el archivo de volcado de memoria tras la conmutación por error del recurso, debe establecer las tres propiedades siguientes como requisitos previos: SqlDumperDumpTimeOut, SqlDumperDumpPath, SqlDumperDumpFlags.

SQLDUMPEREDUMPFLAGS  
Determina el tipo de archivos de volcado generados por la utilidad SQLDumper de SQL Server. El valor predeterminado es 0. Para esta configuración se usan valores decimales, en lugar de hexadecimales. Para el minivolcado use 288, para el minivolcado con memoria indirecta 296 y, para el volcado filtrado, 33024. Para obtener más información, vea el artículo de Knowledgebase sobre la [utilidad SQLDumper de SQL Server](/troubleshoot/sql/tools/use-sqldumper-generate-dump-file).  
  
SQLDUMPERDUMPPATH = { 'os_file_path' | DEFAULT }  
Ubicación donde la utilidad SQLDumper almacena los archivos de volcado. Para obtener más información, vea el artículo de Knowledgebase sobre la [utilidad SQLDumper de SQL Server](/troubleshoot/sql/tools/use-sqldumper-generate-dump-file).  
  
SQLDUMPERDUMPTIMEOUT = { 'dump_time-out' | DEFAULT }  
Valor de tiempo de espera en milisegundos para que la utilidad SQLDumper genere un volcado en caso de un error de SQL Server. El valor predeterminado es 0, lo que significa que no hay ningún límite de tiempo para completar el volcado. Para obtener más información, vea el artículo de Knowledgebase sobre la [utilidad SQLDumper de SQL Server](/troubleshoot/sql/tools/use-sqldumper-generate-dump-file).  
  
 FAILURECONDITIONLEVEL = { 'failure_condition_level' | DEFAULT }  
 Condiciones bajo las que la instancia de clúster de conmutación por error de SQL Server debe realizar la conmutación por error o reiniciarse. El valor predeterminado es 3, lo que significa que el recurso de SQL Server producirá la conmutación por error o se reiniciará en errores de servidor críticos. Para obtener más información sobre cómo configurar esta propiedad, vea [Configurar los valores de la propiedad FailureConditionLevel](../../sql-server/failover-clusters/windows/configure-failureconditionlevel-property-settings.md).  
  
HEALTHCHECKTIMEOUT = { 'health_check_time-out' | DEFAULT }  
Valor de tiempo de espera que establece cuánto tiempo debe la DLL del recurso del motor de base de datos de SQL Server esperar la información de estado del servidor antes de considerar que la instancia de SQL Server no responde. El valor de tiempo de espera se expresa en milisegundos. El valor predeterminado es 60 000 milisegundos (o 60 segundos).  
  
**\<hadr_cluster_context> ::=**  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).   
  
HADR CLUSTER CONTEXT **=** { **'** _remote\_windows\_cluster_ **'** | LOCAL }  
Cambia el contexto de clúster de HADR de la instancia de servidor al clúster de conmutación por error de Windows Server (WSFC) especificado. El *contexto de clúster de HADR* determina qué clúster de conmutación por error de Windows Server administra los metadatos para las réplicas de disponibilidad hospedadas por la instancia de servidor. Use la opción SET HADR CLUSTER CONTEXT solo durante una migración entre clústeres de [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] a una instancia de [!INCLUDE[ssSQL11SP1](../../includes/sssql11sp1-md.md)] o una versión posterior en un nuevo WSFC.  
  
Solo puede cambiar el contexto de clúster de HADR desde el WSFC local a uno remoto, y viceversa. El contexto de clúster de HADR se puede cambiar a un clúster remoto solo cuando la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no hospeda ninguna réplica de disponibilidad.  
  
Un contexto de clúster de HADR remoto se puede volver a cambiar al clúster local en cualquier momento. Sin embargo, el contexto no se puede cambiar de nuevo si la instancia de servidor hospeda réplicas de disponibilidad. 
  
Para identificar el clúster de destino, especifique uno de los valores siguientes:  
  
*windows_cluster*  
El nombre de red de un WSFC. Puede especificar el nombre corto o el nombre de dominio completo. Para buscar la dirección IP de destino de un nombre corto, ALTER SERVER CONFIGURATION usa la resolución de DNS. En algunos casos, un nombre corto puede producir confusiones y DNS podría devolver la dirección IP errónea. Se recomienda especificar el nombre de dominio completo.  
  
> [!NOTE] 
> Ya no se admite una migración entre clústeres utilizando esta configuración. Para realizar una migración entre clústeres, utilice un grupo de disponibilidad distribuida o algún otro método como el trasvase de registros. 
  
LOCAL  
WSFC local.  
  
Para obtener más información, consulte [Cambiar el contexto de clúster de HADR de la instancia de servidor &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md).  
  
**\<buffer_pool_extension>::=**  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]).    
  
ACTIVAR  
Habilita la opción de extensión del grupo de búferes. Esta opción extiende el tamaño del grupo de búferes con almacenamiento permanente. El almacenamiento permanente, como las unidades de estado sólido (SSD), conservan las páginas de datos limpias en el grupo. Para obtener más información sobre esta característica, vea [Buffer Pool Extension](../../database-engine/configure-windows/buffer-pool-extension.md). La extensión del grupo de búferes no está disponible en todas las ediciones de SQL Server. Para obtener más información, consulte [Ediciones y características admitidas de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
FILENAME = 'os_file_path_and_name'  
Define la ruta de acceso al directorio y el nombre del archivo de la memoria caché de la extensión del grupo de búferes. La extensión de archivo se debe especificar como .BPE. Desactive BUFFER POOL EXTENSION para poder modificar FILENAME.  
  
SIZE = *size* [ **KB** | MB | GB ]  
Define el tamaño de la memoria caché. La especificación de tamaño predeterminado es KB. El tamaño mínimo es el de la Memoria de servidor máxima. El límite máximo es 32 veces el tamaño de Memoria de servidor máxima. Para obtener más información sobre la memoria de servidor máxima, vea [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
Desactive BUFFER POOL EXTENSION para poder modificar el tamaño del archivo. Para especificar un tamaño menor que el actual, la instancia de SQL Server debe reiniciarse para reclamar memoria. De lo contrario, el tamaño especificado debe ser igual o mayor que el actual.  
  
Apagado  
Deshabilita la opción de extensión del grupo de búferes. Deshabilite la opción de extensión del grupo de búferes antes de modificar los parámetros asociados como el tamaño o el nombre de archivo. Cuando esta opción está deshabilitada, toda la información de configuración relacionada se quita del Registro.  
  
> [!WARNING]  
>  Deshabilitar la extensión del grupo de búferes puede tener un impacto negativo en el rendimiento del servidor porque el tamaño del grupo de búferes se reduce significativamente.  
  
**\<soft_numa>**  

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]).  
  
ACTIVAR  
Permite la creación automática de particiones para dividir grandes nodos de hardware NUMA en nodos NUMA más pequeños. Para cambiar el valor de ejecución, es necesario reiniciar el motor de base de datos.  
  
Apagado  
Deshabilita la creación automática de particiones de software de grandes nodos de hardware NUMA en nodos NUMA más pequeños. Para cambiar el valor de ejecución, es necesario reiniciar el motor de base de datos.  

> [!WARNING]
> Existen problemas conocidos en el comportamiento de la instrucción ALTER SERVER CONFIGURATION con la opción SOFT NUMA y el Agente SQL Server.  La secuencia recomendada de operaciones es la siguiente:  
> 1) Detenga la instancia del Agente SQL Server.  
> 2) Ejecute la opción ALTER SERVER CONFIGURATION  SOFT NUMA.  
> 3) Reinicie la instancia de SQL Server.  
> 4) Inicie la instancia del Agente SQL Server.  
  
**Más información:** Si ejecuta ALTER SERVER CONFIGURATION con el comando SET SOFTNUMA antes de reiniciar el servicio de SQL Server, cuando el servicio del Agente SQL Server se detiene, ejecuta un comando RECONFIGURE de T-SQL que restablece la configuración de SOFTNUMA a su estado original anterior a ALTER SERVER CONFIGURATION. 

**\<memory_optimized> ::=**

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]).

ACTIVAR <br>
Habilita todas las características en el nivel de instancia que forman parte de la familia de características de la [base de datos en memoria](../../relational-databases/in-memory-database.md). Esto incluye los [metadatos tempdb optimizados para memoria](../../relational-databases/databases/tempdb-database.md#memory-optimized-tempdb-metadata) y el [grupo de búferes híbrido](../../database-engine/configure-windows/hybrid-buffer-pool.md). Es necesario llevar a cabo un reinicio para que surta efecto.

Apagado <br>
Deshabilita todas las características en el nivel de instancia que forman parte de la familia de características de la base de datos en memoria. Es necesario llevar a cabo un reinicio para que surta efecto.

TEMPDB_METADATA = ON | OFF <br>
Habilita o deshabilita solamente los metadatos tempdb optimizados para memoria. Es necesario llevar a cabo un reinicio para que surta efecto. 

RESOURCE_POOL='resource_pool_name' <br>
Cuando se combina con TEMPDB_METADATA = ON, se especifica el grupo de recursos definido por el usuario que debe usarse para tempdb. Si no se especifica, tempdb usará el grupo predeterminado. El grupo ya debe existir. Si el grupo no está disponible cuando se reinicia el servicio, tempdb usará el grupo predeterminado.


HYBRID_BUFFER_POOL = ON | OFF <br>
Habilita o deshabilita el grupo de búferes híbrido en el nivel de la instancia. Es necesario llevar a cabo un reinicio para que surta efecto.


## <a name="general-remarks"></a>Notas generales  
Esta instrucción no requiere un reinicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], a menos que se indique explícitamente lo contrario. En el caso de una instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], no se requiere un reinicio del recurso de clúster de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
Esta instrucción no admite desencadenadores DDL.  
  
## <a name="permissions"></a>Permisos  
Requiere:
- Permisos de `ALTER SETTINGS` para la opción de afinidad de proceso.
- Permisos de `ALTER SETTINGS` y `VIEW SERVER STATE` para las opciones de propiedad de clúster de conmutación por error y registro de diagnóstico.
- Permiso de `CONTROL SERVER` para la opción de contexto de clúster de HADR.  
- Permiso de `ALTER SERVER STATE` para la opción de extensión del grupo de búferes.  
  
La DLL de recursos del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de [!INCLUDE[ssDE](../../includes/ssde-md.md)] se ejecuta en la cuenta de sistema local. Por tanto, la cuenta de sistema local debe tener acceso de lectura y escritura a la ruta de acceso especificada en la opción de registro de diagnóstico.  
  
## <a name="examples"></a>Ejemplos  
  
|Category|Elementos de sintaxis ofrecidos|  
|--------------|------------------------------|  
|[Configuración de una afinidad de proceso](#Affinity)|CPU • NUMANODE • AUTO|  
|[Configuración de opciones de registro de diagnóstico](#Diagnostic)|ON • OFF • PATH • MAX_SIZE|  
|[Configuración de propiedades de clúster de conmutación por error](#Failover)|HealthCheckTimeout|  
|[Cambio del contexto de clúster de una réplica de disponibilidad](#ChangeClusterContextExample)|**'** *windows_cluster* **'**|  
|[Configuración de la extensión del grupo de búferes](#BufferPoolExtension)|BUFFER POOL EXTENSION| 
|[Configurar las opciones de base de datos en memoria](#MemoryOptimized)|MEMORY_OPTIMIZED|

  
###  <a name="setting-process-affinity"></a><a name="Affinity"></a> Configuración de la afinidad de proceso  
En los ejemplos de esta sección se muestra cómo establecer la afinidad de proceso en las CPU y nodos NUMA. En los ejemplos se supone que el servidor contiene 256 CPU organizadas en cuatro grupos de 16 nodos NUMA cada una. Los subprocesos no se asignan a ningún nodo NUMA ni a ninguna CPU.  
  
-   Grupo 0: nodos NUMA del 0 al 3, CPU de la 0 a la 63  
-   Grupo 1: nodos NUMA del 4 al 7, CPU de la 64 a la 127  
-   Grupo 2: nodos NUMA del 8 al 12, CPU de la 128 a la 191  
-   Grupo 3: nodos NUMA del 13 al 16, CPU de la 192 a la 255  
  
#### <a name="a-setting-affinity-to-all-cpus-in-groups-0-and-2"></a>A. Establecer la afinidad en todas las CPU de los grupos 0 y 2  
En el ejemplo siguiente se establece la afinidad en todas las CPU de los grupos 0 y 2.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY CPU=0 TO 63, 128 TO 191;  
```  
  
#### <a name="b-setting-affinity-to-all-cpus-in-numa-nodes-0-and-7"></a>B. Establecer la afinidad en todas las CPU de los nodos NUMA 0 y 7  
En el ejemplo siguiente únicamente se establece la afinidad de las CPU con los nodos `0` y `7`.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY NUMANODE=0, 7;  
```  
  
#### <a name="c-setting-affinity-to-cpus-60-through-200"></a>C. Establecer la afinidad en las CPU comprendidas entre la 60 y la 200  
En el ejemplo siguiente se establece la afinidad de las CPU comprendidas entre la 60 y la 200.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY CPU=60 TO 200;  
```  
  
#### <a name="d-setting-affinity-to-cpu-0-on-a-system-that-has-two-cpus"></a>D. Establecer la afinidad en la CPU 0 para un sistema que tiene dos CPU  
En el ejemplo siguiente se establece la afinidad en `CPU=0` para un equipo que tiene dos CPU. Antes de que se ejecute la instrucción siguiente, la máscara de bits de afinidad interna es 00.  
  
```sql  
ALTER SERVER CONFIGURATION SET PROCESS AFFINITY CPU=0;  
```  
  
#### <a name="e-setting-affinity-to-auto"></a>E. Establecer la afinidad en AUTO  
En el siguiente ejemplo se establece la afinidad en `AUTO`.  
  
```sql  
ALTER SERVER CONFIGURATION  
SET PROCESS AFFINITY CPU=AUTO;  
```  
  
###  <a name="setting-diagnostic-log-options"></a><a name="Diagnostic"></a> Setting diagnostic log options  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).    
  
En los ejemplos de esta sección se muestra cómo establecer los valores para la opción de registro de diagnóstico.  
  
#### <a name="a-starting-diagnostic-logging"></a>A. Iniciar el registro de diagnóstico  
En el ejemplo siguiente se inicia el registro de los datos de diagnóstico.  
  
```sql  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG ON;  
```  
  
#### <a name="b-stopping-diagnostic-logging"></a>B. Detener el registro de diagnóstico  
En el ejemplo siguiente se detiene el registro de los datos de diagnóstico.  
  
```sql  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG OFF;  
```  
  
#### <a name="c-specifying-the-location-of-the-diagnostic-logs"></a>C. Especificar la ubicación de los registros de diagnóstico  
En el ejemplo siguiente se establece la ubicación de los registros de diagnóstico en la ruta de acceso de archivo especificada.  
  
```sql  
ALTER SERVER CONFIGURATION  
SET DIAGNOSTICS LOG PATH = 'C:\logs';  
```  
  
#### <a name="d-specifying-the-maximum-size-of-each-diagnostic-log"></a>D. Especificar el tamaño máximo de cada registro de diagnóstico  
En el ejemplo siguiente se establece el tamaño máximo de cada registro de diagnóstico en 10 megabytes.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET DIAGNOSTICS LOG MAX_SIZE = 10 MB;  
```  
  
###  <a name="setting-failover-cluster-properties"></a><a name="Failover"></a> Configuración de las propiedades de clúster de conmutación por error  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).   
  
En el ejemplo siguiente se muestra la forma de establecer los valores de las propiedades de recurso de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
#### <a name="a-specifying-the-value-for-the-healthchecktimeout-property"></a>A. Especificar el valor de la propiedad HealthCheckTimeout  
En el ejemplo siguiente se establece la opción `HealthCheckTimeout` en 15.000 milisegundos (15 segundos).  
  
```sql  
ALTER SERVER CONFIGURATION   
SET FAILOVER CLUSTER PROPERTY HealthCheckTimeout = 15000;  
```  
  
###  <a name="b-changing-the-cluster-context-of-an-availability-replica"></a><a name="ChangeClusterContextExample"></a> B. Cambiar el contexto de clúster de una réplica de disponibilidad  
En el ejemplo siguiente se cambia el contexto de clúster de HADR de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para especificar el clúster de destino de WSFC, `clus01`, el ejemplo especifica el nombre de objeto completo del clúster, `clus01.xyz.com`.  
  
```sql  
ALTER SERVER CONFIGURATION SET HADR CLUSTER CONTEXT = 'clus01.xyz.com';  
```  
  
### <a name="setting-buffer-pool-extension-options"></a>Establecer las opciones de extensión del grupo de búferes  
  
####  <a name="a-setting-the-buffer-pool-extension-option"></a><a name="BufferPoolExtension"></a> A. Establecer la opción de extensión del grupo de búferes  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]).    
  
En el siguiente ejemplo se habilita la opción de extensión del grupo de búferes y se especifica un nombre de archivo y un tamaño.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET BUFFER POOL EXTENSION ON  
    (FILENAME = 'F:\SSDCACHE\Example.BPE', SIZE = 50 GB);  
```  
  
#### <a name="b-modifying-buffer-pool-extension-parameters"></a>B. Modificar parámetros de extensión del grupo de búferes  
En el ejemplo siguiente se modifica el tamaño de un archivo de extensión del grupo de búferes. La opción de extensión del grupo de búferes debe deshabilitarse antes de que se modifique alguno de los parámetros.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET BUFFER POOL EXTENSION OFF;  
GO  
EXEC sp_configure 'max server memory (MB)', 12000;  
GO  
RECONFIGURE;  
GO  
ALTER SERVER CONFIGURATION  
SET BUFFER POOL EXTENSION ON  
    (FILENAME = 'F:\SSDCACHE\Example.BPE', SIZE = 60 GB);  
GO   
```  

### <a name="setting-in-memory-database-options"></a><a name="MemoryOptimized"></a> Configurar las opciones de base de datos en memoria

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]).

#### <a name="a-enable-all-in-memory-database-features-with-default-options"></a>A. Habilitar todas las características de base de datos en memoria con las opciones predeterminadas

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED ON;
GO
```

#### <a name="b-enable-memory-optimized-tempdb-metadata-using-the-default-resource-pool"></a>B. Habilitar los metadatos tempdb optimizados para memoria con el grupo de recursos predeterminado

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON;
GO
```

#### <a name="c-enable-memory-optimized-tempdb-metadata-with-a-user-defined-resource-pool"></a>C. Habilitar los metadatos tempdb optimizados para memoria con un grupo de recursos definido por el usuario

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON (RESOURCE_POOL = 'pool_name');
GO
```

#### <a name="d-enable-hybrid-buffer-pool"></a>D. Habilitar el grupo de búferes híbrido

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED HYBRID_BUFFER_POOL = ON;
GO
```

## <a name="see-also"></a>Consulte también  
[Soft-NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md)   
[Cambiar el contexto de clúster de HADR de la instancia de servidor &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md)   
[sys.dm_os_schedulers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md)   
[sys.dm_os_memory_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-nodes-transact-sql.md)   
[sys.dm_os_buffer_pool_extension_configuration &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-buffer-pool-extension-configuration-transact-sql.md)   
[Extensión del grupo de búferes](../../database-engine/configure-windows/buffer-pool-extension.md)  
