---
title: Establecer la opción de configuración del servidor Grado máximo de paralelismo | Microsoft Docs
description: Obtenga información sobre la opción de grado máximo de paralelismo (MAXDOP). Vea cómo usarla para limitar el número de procesadores que usa SQL Server en la ejecución de planes paralelos.
ms.date: 02/12/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- parallel queries [SQL Server]
- processors [SQL Server], parallel queries
- number of processors for parallel queries
- max degree of parallelism option
- MaxDop
ms.assetid: 86b65bf1-a6a1-4670-afc0-cdfad1558032
author: markingmyname
ms.author: maghan
ms.custom: contperf-fy20q4
ms.openlocfilehash: 4006ad1707d30d0a9147056ddfff8b71ae7643e1
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236972"
---
# <a name="configure-the-max-degree-of-parallelism-server-configuration-option"></a>Establecer la opción de configuración del servidor Grado máximo de paralelismo
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo establecer la opción de configuración del servidor **grado máximo de paralelismo (MAXDOP)** en SQL Server mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Cuando una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecuta en un equipo que tiene más de un microprocesador o CPU, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] detecta si se puede usar el paralelismo. El grado de paralelismo establece el número de procesadores que se usan para ejecutar una sola instrucción, para cada ejecución de plan paralelo. Puede utilizar la opción **max degree of parallelism** (grado máximo de paralelismo) para limitar el número de procesadores que debe utilizarse en la ejecución de planes en paralelo. Para obtener más información sobre el límite establecido por el **grado de paralelismo máximo (MAXDOP)** , vea la sección [Consideraciones](#Considerations) de esta página. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considera los planes de ejecución en paralelo para las consultas, las operaciones de lenguaje de definición de datos (DDL) de índice, las inserciones en paralelo, la modificación de columna en línea, la colección de estadísticas en paralelo y el rellenado de cursor estático y controlado por conjuntos de claves.

> [!NOTE]
> [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)] presenta recomendaciones automáticas para establecer la opción de configuración de servidor MAXDOP durante el proceso de instalación basadas en el número de procesadores disponibles. La interfaz de usuario del programa de instalación permite aceptar la configuración recomendada o introducir su propio valor. Para obtener más información, vea la [página Configuración del Motor de base de datos: MaxDOP](../../sql-server/install/instance-configuration.md#maxdop).

##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="considerations"></a><a name="Considerations"></a> Consideraciones  
-   Esta opción es avanzada y solo debe cambiarla un administrador de base de datos con experiencia o un profesional certificado de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

-   Si el valor de la opción affinity mask no es el predeterminado, es posible que se limite el número de procesadores disponibles para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en sistemas de multiproceso simétrico (SMP). 
  
-   Si se establece el grado máximo de paralelismo (MAXDOP) en 0, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede usar todos los procesadores disponibles hasta un máximo de 64. Pero este no es el valor recomendado para la mayoría de los casos. Para obtener más información sobre los valores recomendados para el grado máximo de paralelismo, vea la sección [Recomendaciones](#Recommendations) en esta página.

-   Para suprimir la generación de planes paralelos, establezca la opción **max degree of parallelism** en 1. Establezca el valor en un número de 1 a 32 767 para especificar el número máximo de núcleos de procesador que puede usar durante una única ejecución de consulta. Si se especifica un valor superior al número de procesadores disponibles, se utilizará el número real de procesadores disponibles. Si el equipo tiene solo un procesador, el valor de **grado máximo de paralelismo** se pasará por alto.  

-   El límite del grado máximo de paralelismo se establece por [tarea](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md). No es un límite por [solicitud](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md) ni por consulta. Esto significa que, durante una ejecución de consulta en paralelo, una solicitud única puede generar varias tareas hasta alcanzar el límite MAXDOP, y que cada tarea usará un trabajo y un programador. Para obtener más información, vea la sección *Programar tareas en paralelo* de la [Guía de arquitectura de subprocesos y tareas](../../relational-databases/thread-and-task-architecture-guide.md). 
  
-   Puede invalidar el valor de configuración del servidor de grado máximo de paralelismo:
    -   En el nivel de consulta, mediante la [sugerencia de consulta](../../t-sql/queries/hints-transact-sql-query.md) **MAXDOP**.     
    -   En el nivel de base de datos, con la [configuración con ámbito de base de datos](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) **MAXDOP**.
    -   En el nivel de carga de trabajo, con la [opción de configuración del grupo de cargas de trabajo de Resource Governor](../../t-sql/statements/create-workload-group-transact-sql.md) **MAX_DOP**.

-   Las operaciones de índice que crean o vuelven a generar un índice, o que eliminan un índice clúster, pueden consumir recursos de forma intensiva. Puede omitir el valor de la opción max degree of parallelism para operaciones de índice especificando la opción de índice MAXDOP en la instrucción del índice. El valor de MAXDOP se aplica a la instrucción en tiempo de ejecución y no se almacena en los metadatos del índice. Para obtener más información, vea [Configurar operaciones de índice en paralelo](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
-   Además de las operaciones de consultas e índices, esta opción también controla el paralelismo de DBCC CHECKTABLE, DBCC CHECKDB y DBCC CHECKFILEGROUP. Puede deshabilitar los planes de ejecución en paralelo de estas instrucciones mediante el uso de la marca de seguimiento 2528. Para obtener más información, vea [Marcas de seguimiento &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

###  <a name="recommendations"></a><a name="Recommendations"></a> <a name="Guidelines"></a> Recomendaciones  
A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], durante el inicio del servicio, si [!INCLUDE[ssde_md](../../includes/ssde_md.md)] detecta más de ocho núcleos físicos por nodo NUMA o socket en el inicio, se crean nodos soft-NUMA de forma automática y predeterminada. [!INCLUDE[ssde_md](../../includes/ssde_md.md)] coloca los procesadores lógicos del mismo núcleo físico en nodos soft-NUMA diferentes. Las recomendaciones de la tabla siguiente están pensadas para mantener todos los subprocesos de trabajo de una consulta en paralelo en el mismo nodo soft-NUMA. Esto mejorará el rendimiento de las consultas y la distribución de los subprocesos de trabajo entre los nodos NUMA para la carga de trabajo. Para obtener más información, vea [Soft-NUMA](../../database-engine/configure-windows/soft-numa-sql-server.md).

A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], use las siguientes directrices al configurar el valor de configuración del servidor de **grado máximo de paralelismo**:

|Configuración del servidor|Número de procesadores|Guía|
|----------------|-----------------|-----------------|
|Servidor con un solo nodo NUMA|8 procesadores lógicos como mínimo|Mantener MAXDOP en ese número de procesadores lógicos o por debajo de este|
|Servidor con un solo nodo NUMA|Más de 8 procesadores lógicos|Mantener MAXDOP en 8|
|Servidor con varios nodos NUMA|16 procesadores lógicos como mínimo por nodo NUMA|Mantener MAXDOP en ese número de procesadores lógicos por nodo NUMA o por debajo de este|
|Servidor con varios nodos NUMA|Más de 16 procesadores lógicos por nodo NUMA|Mantener MAXDOP a la mitad del número de procesadores lógicos por nodo de NUMA con un valor máximo de 16|
  
> [!NOTE]
> El nodo NUMA de la tabla anterior hace referencia a los nodos NUMA de software creados automáticamente mediante [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, o a los nodos NUMA basados en hardware en caso de que el nodo NUMA de software esté deshabilitado.   
>  Utilice estas mismas instrucciones al establecer la opción de grado máximo de paralelismo de los grupos de cargas de trabajo de Resource Governor. Para obtener más información, vea [CREATE WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/create-workload-group-transact-sql.md).
  
De [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] a [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], use las siguientes directrices al configurar el valor de configuración del servidor de **grado máximo de paralelismo**:

|Configuración del servidor|Número de procesadores|Guía|
|----------------|-----------------|-----------------|
|Servidor con un solo nodo NUMA|8 procesadores lógicos como mínimo|Mantener MAXDOP en ese número de procesadores lógicos o por debajo de este|
|Servidor con un solo nodo NUMA|Más de 8 procesadores lógicos|Mantener MAXDOP en 8|
|Servidor con varios nodos NUMA|8 procesadores lógicos como mínimo por nodo NUMA|Mantener MAXDOP en ese número de procesadores lógicos por nodo NUMA o por debajo de este|
|Servidor con varios nodos NUMA|Más de 8 procesadores lógicos por nodo NUMA|Mantener MAXDOP en 8|
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 De forma predeterminada, todos los usuarios tienen permisos de ejecución en **sp_configure** sin ningún parámetro o solo con el primero. Para ejecutar **sp_configure** con ambos parámetros y cambiar una opción de configuración, o para ejecutar la instrucción RECONFIGURE, un usuario debe tener el permiso ALTER SETTINGS en el servidor. Los roles fijos de servidor **sysadmin** y **serveradmin** tienen el permiso ALTER SETTINGS de forma implícita.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-configure-the-max-degree-of-parallelism-option"></a>Para configurar la opción de grado máximo de paralelismo  
  
1.  En el **Explorador de objetos**, haga clic con el botón derecho en un servidor y seleccione **Propiedades**.  
  
2.  Haga clic en el nodo **Avanzado** .  
  
3.  En el cuadro **Grado máximo de paralelismo** , seleccione el número máximo de procesadores que se usarán en la ejecución de planes paralelos.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-configure-the-max-degree-of-parallelism-option"></a>Para configurar la opción de grado máximo de paralelismo  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se muestra cómo usar [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) para configurar la opción `max degree of parallelism` en `16`.  
  
```sql  
USE AdventureWorks2012 ;  
GO   
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE WITH OVERRIDE;  
GO  
EXEC sp_configure 'max degree of parallelism', 16;  
GO  
RECONFIGURE WITH OVERRIDE;  
GO  
```  
  
 Para obtener más información, vea [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-max-degree-of-parallelism-option"></a><a name="FollowUp"></a> Seguimiento: Después de configurar la opción de grado máximo de paralelismo  
 La configuración surte efecto inmediatamente, sin necesidad de reiniciar el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)        
 [affinity mask (opción de configuración del servidor)](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md)      
 [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Query Processing Architecture Guide](../../relational-databases/query-processing-architecture-guide.md#DOP)      (Guía de arquitectura de procesamiento de consultas)  
 [Guía de arquitectura de subprocesos y tareas](../../relational-databases/thread-and-task-architecture-guide.md)    
 [Configurar operaciones de índice en paralelo](../../relational-databases/indexes/configure-parallel-index-operations.md)    
 [Sugerencias de consulta &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md)     
 [Establecer opciones de índice](../../relational-databases/indexes/set-index-options.md)     

## <a name="next-steps"></a>Pasos siguientes

[RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)
[Supervisión y optimización del rendimiento](../../relational-databases/performance/monitor-and-tune-for-performance.md)
