---
title: 'Creación y prueba de la función clasificadora de regulación de recursos definida por el usuario: Resource Governor'
description: Obtenga información sobre cómo crear y probar una función clasificadora definida por el usuario que implique ejecutar instrucciones Transact-SQL en el editor de consultas de SQL Server Management Studio.
ms.custom: seo-dt-2019
ms.date: 07/11/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Resource Governor, classifier function create
- classifier function [SQL Server], test
- classifier function [SQL Server], create
- Resource Governor, classifier function test
ms.assetid: 7866b3c9-385b-40c6-aca5-32d3337032be
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d08ddba59acdf2692e8493a10bac58048458ae23
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506607"
---
# <a name="create-and-test-a-classifier-user-defined-function"></a>Crear y probar una función clasificadora definida por el usuario
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  En este tema se muestra cómo crear y probar una función clasificadora definida por el usuario (UDF). Los pasos implican ejecutar instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)] en el Editor de consultas de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
 El ejemplo mostrado en el procedimiento siguiente ilustra las posibilidades para crear una función clasificadora definida por el usuario bastante compleja.  
  
 En el ejemplo:  
  
-   Se crean un grupo de recursos de servidor (pProductionProcessing) y un grupo de cargas de trabajo (gProductionProcessing) para el procesamiento de producción durante un intervalo de tiempo especificado.  
  
-   Se crean un grupo de recursos de servidor (pOffHoursProcessing) y un grupo de cargas de trabajo (gOffHoursProcessing) para administrar las conexiones que no cumplen los requisitos del procesamiento de producción.  
  
-   Se crea una tabla (TblClassificationTimeTable) en la base de datos maestra para contener la hora de inicio y de finalización que se puede evaluar con una hora de inicio de sesión. Esto se debe crear en la base de datos maestra porque el regulador de recursos utiliza el enlace de esquemas para las funciones clasificadoras.  
  
    > [!NOTE]  
    >  Como práctica recomendada, no debería almacenar en la base de datos maestra las tablas grandes que se actualicen con frecuencia.  
  
 La función clasificadora extiende el tiempo de inicio de sesión. Una función demasiado compleja puede hacer que los inicios de sesión agoten el tiempo de espera o ralenticen las conexiones rápidas.  
  
## <a name="to-create-the-classifier-user-defined-function"></a>Para crear la función clasificadora definida por el usuario  
  
1.  Cree y configure los grupos de recursos y grupos de cargas de trabajo nuevos. Asigne cada grupo de cargas de trabajo al grupo de recursos de servidor adecuado.  
  
    ```sql  
    --- Create a resource pool for production processing  
    --- and set limits.  
    USE master;  
    GO  
    CREATE RESOURCE POOL pProductionProcessing  
    WITH  
    (  
         MAX_CPU_PERCENT = 100,  
         MIN_CPU_PERCENT = 50  
    );  
    GO  
    
    --- Create a workload group for production processing  
    --- and configure the relative importance.  
    CREATE WORKLOAD GROUP gProductionProcessing  
    WITH  
    (  
         IMPORTANCE = MEDIUM  
    );  
    
    --- Assign the workload group to the production processing  
    --- resource pool.  
    USING pProductionProcessing  
    GO  
    
    --- Create a resource pool for off-hours processing  
    --- and set limits.  
    CREATE RESOURCE POOL pOffHoursProcessing  
    WITH  
    (  
         MAX_CPU_PERCENT = 50,  
         MIN_CPU_PERCENT = 0  
    );  
    GO  
    
    --- Create a workload group for off-hours processing  
    --- and configure the relative importance.  
    CREATE WORKLOAD GROUP gOffHoursProcessing  
    WITH  
    (  
         IMPORTANCE = LOW  
    )  
    --- Assign the workload group to the off-hours processing  
    --- resource pool.  
    USING pOffHoursProcessing;  
    GO  
    ```  
  
2.  Actualice la configuración en la memoria.  
  
    ```sql  
    ALTER RESOURCE GOVERNOR RECONFIGURE;  
    GO  
    ```  
  
3.  Cree una tabla y defina las horas de finalización e inicio para el intervalo de tiempo de proceso de producción.  
  
    ```sql  
    USE master;  
    GO  
    CREATE TABLE tblClassificationTimeTable  
    (  
         strGroupName     sysname          not null,  
         tStartTime       time              not null,  
         tEndTime         time              not null  
    );  
    GO  
    --- Add time values that the classifier will use to  
    --- determine the workload group for a session.  
    INSERT into tblClassificationTimeTable VALUES('gProductionProcessing', '6:35 AM', '6:15 PM');  
    GO  
    ```  
  
4.  Cree la función clasificadora que utiliza las funciones de hora y los valores que se pueden evaluar con las horas en la tabla de búsqueda. Para obtener información sobre cómo usar tablas de búsqueda en una función clasificadora, vea "Procedimientos recomendados para usar tablas de búsqueda en una función clasificadora" en este tema.  
  
    > [!NOTE]  
    >  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] incluía un conjunto expandido de funciones y tipos de datos de fecha y hora. Para obtener más información, vea [Tipos de datos y funciones de fecha y hora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
    ```sql  
    CREATE FUNCTION fnTimeClassifier()  
    RETURNS sysname  
    WITH SCHEMABINDING  
    AS  
    BEGIN  
    /* We recommend running the classifier function code under 
    snapshot isolation level OR using NOLOCK hint to avoid blocking on 
    lookup table. In this example, we are using NOLOCK hint. */
         DECLARE @strGroup sysname  
         DECLARE @loginTime time  
         SET @loginTime = CONVERT(time,GETDATE())  
         SELECT TOP 1 @strGroup = strGroupName  
              FROM dbo.tblClassificationTimeTable WITH(NOLOCK)
              WHERE tStartTime <= @loginTime and tEndTime >= @loginTime  
         IF(@strGroup is not null)  
         BEGIN  
              RETURN @strGroup  
         END  
    --- Use the default workload group if there is no match  
    --- on the lookup.  
         RETURN N'gOffHoursProcessing'  
    END;  
    GO  
    ```  
  
5.  Registre la función clasificadora y actualice la configuración en memoria.  
  
    ```sql  
    ALTER RESOURCE GOVERNOR with (CLASSIFIER_FUNCTION = dbo.fnTimeClassifier);  
    ALTER RESOURCE GOVERNOR RECONFIGURE;  
    GO  
    ```  
  
## <a name="to-verify-the-resource-pools-workload-groups-and-the-classifier-user-defined-function"></a>Para comprobar los grupos de recursos, los grupos de cargas de trabajo y la función clasificadora definida por el usuario  
  
1.  Obtenga la configuración del grupo de cargas de trabajo y del grupo de recursos de servidor con la consulta siguiente.  
  
    ```sql  
    USE master;  
    SELECT * FROM sys.resource_governor_resource_pools;  
    SELECT * FROM sys.resource_governor_workload_groups;  
    GO  
    ```  
  
2.  Compruebe que la función clasificadora existe y está habilitada con las consultas siguientes.  
  
    ```sql  
    --- Get the classifier function Id and state (enabled).  
    SELECT * FROM sys.resource_governor_configuration;  
    GO  
    --- Get the classifer function name and the name of the schema  
    --- that it is bound to.  
    SELECT   
          object_schema_name(classifier_function_id) AS [schema_name],  
          object_name(classifier_function_id) AS [function_name]  
    FROM sys.dm_resource_governor_configuration;  
    ```  
  
3.  Obtenga los datos de tiempo de ejecución actuales para los grupos de recursos y de cargas de trabajo con la consulta siguiente.  
  
    ```sql  
    SELECT * FROM sys.dm_resource_governor_resource_pools;  
    SELECT * FROM sys.dm_resource_governor_workload_groups;  
    GO  
    ```  
  
4.  Averigüe qué sesiones están en cada grupo con la consulta siguiente.  
  
    ```sql  
    SELECT s.group_id, CAST(g.name as nvarchar(20)), s.session_id, s.login_time, 
        CAST(s.host_name as nvarchar(20)), CAST(s.program_name AS nvarchar(20))  
    FROM sys.dm_exec_sessions AS s  
    INNER JOIN sys.dm_resource_governor_workload_groups AS g  
        ON g.group_id = s.group_id  
    ORDER BY g.name;  
    GO  
    ```  
  
5.  Averigüe qué solicitudes están en cada grupo con la consulta siguiente.  
  
    ```sql  
    SELECT r.group_id, g.name, r.status, r.session_id, r.request_id, 
        r.start_time, r.command, r.sql_handle, t.text   
    FROM sys.dm_exec_requests AS r  
    INNER JOIN sys.dm_resource_governor_workload_groups AS g  
        ON g.group_id = r.group_id  
    CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) AS t  
    ORDER BY g.name;  
    GO  
    ```  
  
6.  Averigüe qué solicitudes está ejecutándose en la función clasificadora con la consulta siguiente.  
  
    ```sql  
    SELECT s.group_id, g.name, s.session_id, s.login_time, s.host_name, s.program_name   
    FROM sys.dm_exec_sessions AS s  
    INNER JOIN sys.dm_resource_governor_workload_groups AS g  
        ON g.group_id = s.group_id  
           AND 'preconnect' = s.status  
    ORDER BY g.name;  
    GO  
  
    SELECT r.group_id, g.name, r.status, r.session_id, r.request_id, r.start_time, 
        r.command, r.sql_handle, t.text   
    FROM sys.dm_exec_requests AS r  
    INNER JOIN sys.dm_resource_governor_workload_groups AS g  
        ON g.group_id = r.group_id  
           AND 'preconnect' = r.status  
     CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) AS t  
    ORDER BY g.name;  
    GO  
    ```  
  
## <a name="best-practices-for-using-lookup-tables-in-a-classifier-function"></a>Prácticas recomendadas para utilizar tablas de búsqueda en una función clasificadora  
  
1.  No utilice una tabla de búsqueda a menos que sea absolutamente necesario. Si necesita usar una tabla de búsqueda, esta se puede codificar de forma rígida en la propia función, pero es necesario que esté en equilibrio con la complejidad y los cambios dinámicos de la función clasificadora.  
  
2.  Limite la E/S realizada para las tablas de búsqueda.  
  
    1.  Use `TOP 1` para que solo se devuelva una fila.  
  
    2.  Minimice el número de filas de la tabla.  
  
    3.  Haga que todas las filas de la tabla estén en una sola página o en un número reducido de páginas.  
  
    4.  Confirme que las filas encontradas utilizando las operaciones de Index Seek usan tantas columnas de búsqueda como sea posible.  
  
    5.  Desnormalice a una sola tabla si está pensando en utilizar varias tablas con combinaciones.  
  
3.  Evite el bloqueo en la tabla de búsqueda.  
  
    1.  Use la sugerencia `NOLOCK` para evitar el bloqueo o use `SET LOCK_TIMEOUT` en la función con un valor máximo de 1000 milisegundos.  
  
    2.  Debe haber tablas en la base de datos maestra. (La base de datos maestra es la única base de datos cuya recuperación está asegurada cuando los equipos clientes intentan la conexión).  
  
    3.  Use siempre el nombre completo de la tabla con el esquema. El nombre de la base de datos no es necesario porque tiene que ser la base de datos maestra.  
  
    4.  No debe haber desencadenadores en la tabla.  
  
    5.  Si está actualizando el contenido de la tabla, asegúrese de utilizar una transacción de nivel de aislamiento de instantáneas en la función clasificadora para que los escritores no bloqueen los lectores. Observe que el uso de la sugerencia `NOLOCK` también debe mitigar este riesgo.  
  
    6.  Si es posible, deshabilite la función clasificadora al cambiar el contenido de la tabla.  
  
        > [!WARNING]  
        >  Se aconseja encarecidamente seguir estas prácticas recomendadas. Si hay problemas que impiden seguir las prácticas recomendadas, conviene ponerse en contacto con el servicio de soporte técnico de Microsoft para poder evitar de forma proactiva cualquier problema futuro.  
  
## <a name="see-also"></a>Consulte también  
 [Regulador de recursos](../../relational-databases/resource-governor/resource-governor.md)   
 [Habilitar el regulador de recursos](../../relational-databases/resource-governor/enable-resource-governor.md)   
 [Grupo de recursos de servidor del regulador de recursos](../../relational-databases/resource-governor/resource-governor-resource-pool.md)   
 [Grupos de cargas de trabajo del regulador de recursos](../../relational-databases/resource-governor/resource-governor-workload-group.md)   
 [Configurar el regulador de recursos utilizando una plantilla](../../relational-databases/resource-governor/configure-resource-governor-using-a-template.md)   
 [Ver las propiedades del regulador de recursos](../../relational-databases/resource-governor/view-resource-governor-properties.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)   
 [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-resource-pool-transact-sql.md)   
 [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)   
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  
