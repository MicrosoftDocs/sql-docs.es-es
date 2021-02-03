  Cambia una configuración existente del grupo de cargas de trabajo de Resource Governor y, opcionalmente, la asigna a un grupo de recursos de Resource Governor.  
  
 ![Icono de vínculo de tema](../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER WORKLOAD GROUP { group_name | "default" }  
[ WITH  
    ([ IMPORTANCE = { LOW | MEDIUM | HIGH } ]  
      [ [ , ] REQUEST_MAX_MEMORY_GRANT_PERCENT = value ]  
      [ [ , ] REQUEST_MAX_CPU_TIME_SEC = value ]  
      [ [ , ] REQUEST_MEMORY_GRANT_TIMEOUT_SEC = value ]   
      [ [ , ] MAX_DOP = value ]  
      [ [ , ] GROUP_MAX_REQUESTS = value ] )  
 ]  
[ USING { pool_name | "default" } ]  
[ ; ]  
```  
  
## <a name="arguments"></a>Argumentos

 *group_name* | "**default**"       
 Es el nombre de un grupo de cargas de trabajo definido por un usuario ya existente o el grupo de cargas de trabajo predeterminado del regulador de recursos.  
  
> [!NOTE]  
> El regulador de recursos crea los grupos "default" e internos al instalarse [!INCLUDE[ssNoVersion](ssnoversion-md.md)].  
  
 La opción "default" debe estar incluida entre comillas ("") o corchetes ([]) si se utiliza con ALTER WORKLOAD GROUP para evitar el conflicto con DEFAULT, que es una palabra reservada del sistema. Para obtener más información, vea [Database Identifiers](../relational-databases/databases/database-identifiers.md).  
  
> [!NOTE]  
> Todos los grupos de cargas de trabajo y de recursos predefinidos usan nombres en minúsculas, como "predeterminado". Debe tenerse esto en cuenta en los servidores que usan una intercalación que distingue entre mayúsculas y minúsculas. En los servidores que usan una intercalación que no distingue entre mayúsculas y minúsculas, como SQL_Latin1_General_CP1_CI_AS, los nombres "predeterminado" y "Predeterminado" son equivalentes.  
  
 IMPORTANCE = { LOW | **MEDIUM** | HIGH }       
 Especifica la importancia relativa de una solicitud en el grupo de cargas de trabajo. IMPORTANCE puede ser es uno de los siguientes valores:  
  
-   LOW  
-   MEDIUM (predeterminado)  
-   HIGH  
  
> [!NOTE]  
> Internamente, cada valor de IMPORTANCE se almacena como un número que se usa para los cálculos.  
  
 IMPORTANCE es local para el grupo de recursos de servidor; los grupos de cargas de trabajo de importancia distinta dentro del mismo grupo de recursos de servidor se influyen entre sí, pero no influyen en los grupos de cargas de trabajo de otro grupo de recursos de servidor.  
  
 REQUEST_MAX_MEMORY_GRANT_PERCENT = *value*     
 Especifica la cantidad máxima de memoria que una única solicitud puede tomar del grupo. *valor* es un porcentaje relativo al tamaño del grupo de recursos de servidor especificado por MAX_MEMORY_PERCENT.  

El elemento *value* es un entero hasta [!INCLUDE[ssSQL17](sssql17-md.md)], y un elemento de float a partir de [!INCLUDE[sql-server-2019](sssql19-md.md)]. El valor predeterminado es de 25. El intervalo permitido para *value* es de 1 a 100.
  
> [!NOTE]  
> La cantidad especificada se refiere únicamente a la memoria concedida para la ejecución de la consulta.  
  
> [!IMPORTANT]
> Establecer *valor* en 0 evita la ejecución de consultas con operaciones SORT y HASH JOIN en grupos de cargas de trabajo definidos por el usuario.     
>
> No se recomienda establecer el elemento *value* en un valor superior a 70 porque es posible que el servidor no pueda reservar memoria suficiente si se están ejecutando otras consultas simultáneas. Esto puede dar lugar al final a un error de tiempo de espera de consulta 8645.      
  
> [!NOTE]  
> Si los requisitos de memoria de consulta superan el límite especificado por este parámetro, el servidor hace lo siguiente:  
>   
> -  En el caso de los grupos de cargas de trabajo definidos por el usuario, el servidor intenta reducir el grado de paralelismo de consulta hasta que el requisito de memoria cae por debajo del límite, o hasta que el grado de paralelismo sea igual a 1. Si el requisito de memoria de consulta sigue siendo mayor que el límite, se produce el error 8657.  
>   
> -  En el caso de los grupos de cargas de trabajo internos y predeterminados, el servidor permite que la consulta obtenga la memoria necesaria.  
>   
> Tenga en cuenta que ambos casos están sujetos a un error de tiempo de espera 8645 si el servidor no tiene suficiente memoria física.  
  
 REQUEST_MAX_CPU_TIME_SEC = *value*       
 Especifica la cantidad máxima de tiempo de CPU, en segundos, que puede usar una solicitud. *valor* debe ser 0 o un entero positivo. El valor predeterminado de *value* es 0, que indica una cantidad ilimitada.  
  
> [!NOTE]  
> De forma predeterminada, Resource Governor no evita que una solicitud continúe si se supera el tiempo máximo. Sin embargo, se generará un evento. Para obtener más información, vea [Clase de eventos Umbral de la CPU superado](../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md). 

> [!IMPORTANT]
> A partir de [!INCLUDE[sssql16-md](sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](sssql17-md.md)] CU3, y si se usa la [marca de seguimiento 2422](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md), Resource Governor anula una solicitud cuando se supera el tiempo máximo.
  
 REQUEST_MEMORY_GRANT_TIMEOUT_SEC =*value*  
 Especifica el tiempo máximo, en segundos, que una consulta puede esperar hasta que esté disponible la concesión de memoria (memoria de búfer de trabajo).  
  
> [!NOTE]  
> Una consulta no tiene por qué generar un error cuando se agota el tiempo de espera para la concesión de memoria. Solo se producirá un error si se ejecutan demasiadas consultas simultáneamente. De lo contrario, es posible que la consulta obtenga la concesión de memoria mínima, lo que reducirá su rendimiento.  
  
 *value* debe ser un entero positivo. El valor predeterminado de *valor*, 0, usa un cálculo interno basado en el costo de la consulta para determinar el tiempo máximo.  
  
 MAX_DOP =*value*       
 Especifica el grado máximo de paralelismo (DOP) para las solicitudes paralelas. *value* debe ser 0 o un entero positivo entre 1 y 255. Cuando *value* es 0, el servidor elige el grado máximo de paralelismo. Esta es la configuración predeterminada y recomendada.  
  
> [!NOTE]  
> El valor real que [!INCLUDE[ssDE](ssde-md.md)] establece para MAX_DOP puede ser menor que el valor especificado. El valor final se determinada mediante la fórmula min(255, *número de CPU)* .  
  
> [!CAUTION]  
> El cambio de MAX_DOP puede afectar negativamente al rendimiento de un servidor. Si debe cambiar MAX_DOP, se recomienda que se establezca en un valor menor o igual que el número máximo de programadores de hardware que hay en un solo nodo NUMA. Se recomienda no establecer MAX_DOP en un valor mayor que 8.  
  
 MAX_DOP se trata de la siguiente manera:  
  
-   MAX_DOP como sugerencia de consulta, se acepta con tal de que no supere el MAX_DOP del grupo de cargas de trabajo.  
  
-   MAX_DOP como sugerencia de consulta, siempre invalida el 'grado máximo de paralelismo' de sp_configure.  
  
-   MAX_DOP del grupo de cargas de trabajo invalida el 'grado máximo de paralelismo' de sp_configure.  
  
-   Si se marca la consulta en tiempo de compilación como serie (MAX_DOP = 1), no se podrá volver a establecer como paralela en tiempo de ejecución, independientemente del grupo de cargas de trabajo o del valor de sp_configure.  
  
 Una vez configurado DOP, solo se puede reducir ante la concesión de presión de memoria. La reconfiguración del grupo de cargas de trabajo no es visible mientras se espera en la cola de concesión de memoria.  
  
 GROUP_MAX_REQUESTS = *value*      
 Especifica el número máximo de solicitudes simultáneas que pueden ejecutarse en el grupo de cargas de trabajo. *valor* debe ser 0 o un entero positivo. El valor predeterminado de *valor*, 0, permite solicitudes ilimitadas. Cuando se alcanza el máximo de solicitudes simultáneas, un usuario de ese grupo puede iniciar sesión, pero se coloca en estado de espera hasta que las solicitudes simultáneas caigan por debajo del valor especificado.  
  
 USING { *pool_name* | "**default**" }      
 Asocia el grupo de cargas de trabajo al grupo de recursos definido por el usuario identificado por *pool_name*, lo que coloca el grupo de cargas de trabajo en el grupo de recursos. Si no se proporciona *pool_name* o si no se usa el argumento USING, el grupo de cargas de trabajo se coloca en el grupo predeterminado de Resource Governor que se haya definido previamente.  
  
 La opción "default" debe estar incluida entre comillas ("") o corchetes ([]) si se utiliza con ALTER WORKLOAD GROUP para evitar el conflicto con DEFAULT, que es una palabra reservada del sistema. Para obtener más información, vea [Database Identifiers](../relational-databases/databases/database-identifiers.md).  
  
> [!NOTE]  
> La opción "default" distingue entre mayúsculas y minúsculas.  
  
## <a name="remarks"></a>Observaciones  
 ALTER WORKLOAD GROUP está permitido en el grupo predeterminado.  
  
 Los cambios en la configuración del grupo de cargas de trabajo no surtirán efecto hasta que se ejecute ALTER RESOURCE GOVERNOR RECONFIGURE. Cuando se cambia un plan que afecta al valor, el nuevo valor solo se aplica a los planes previamente almacenados en caché después de ejecutar DBCC FREEPROCCACHE (*pool_name*), donde *pool_name* es el nombre de un grupo de recursos de Resource Governor con el que está asociado el grupo de cargas de trabajo.  
  
-   Si va a cambiar MAX_DOP a 1, la ejecución de DBCC FREEPROCCACHE no es necesaria porque los planes paralelos se pueden ejecutar en serie. Pero puede que no sea tan eficaz como un plan compilado como un plan en serie.  
  
-   Si va a cambiar MAX_DOP de 1 a 0 o a un valor mayor que 1, no es necesario ejecutar DBCC FREEPROCCACHE. Pero los planes en serie no se pueden ejecutar en paralelo, así que el borrado de la memoria caché respectiva permite compilar potencialmente nuevos planes mediante paralelismo.  
  
> [!CAUTION]  
> El borrado de los planes almacenados en caché de un grupo de recursos asociado a más de un grupo de cargas de trabajo afecta a todos los grupos de cargas de trabajo con el grupo de recursos definido por el usuario identificado por *pool_name*.  
  
 Si va a ejecutar instrucciones de DDL, se recomienda familiarizarse primero con los estados de Resource Governor. Para obtener más información, vea [Resource Governor](../relational-databases/resource-governor/resource-governor.md).  
  
 REQUEST_MEMORY_GRANT_PERCENT: En [!INCLUDE[ssVersion2005](ssversion2005-md.md)], se permite que la creación de índices use más memoria del área de trabajo que la concedida inicialmente para mejorar el rendimiento. El regulador de recursos admite este tratamiento especial en versiones posteriores; sin embargo, la concesión inicial y cualquier concesión de memoria adicional están limitadas por la configuración del grupo de cargas de trabajo y del grupo de recursos de servidor.  
  
 **Creación de índices en una tabla con particiones**  
  
 La memoria usada para la creación de índices en una tabla con particiones no alineada es proporcional al número de particiones involucradas.  Si la memoria total necesaria supera el límite por consulta (REQUEST_MAX_MEMORY_GRANT_PERCENT) impuesto por la configuración del grupo de cargas de trabajo del regulador de recursos, puede que esta creación de índices no se ejecute. Dado que el grupo de cargas de trabajo "predeterminado" permite que una consulta supere el límite por consulta con la memoria mínima necesaria para iniciar la compatibilidad con [!INCLUDE[ssVersion2005](ssversion2005-md.md)], es posible que el usuario pueda ejecutar la misma creación de índices en el grupo de cargas de trabajo "predeterminado" si el grupo de recursos de servidor "predeterminado" tiene configurada una memoria total suficiente para ejecutar dicha consulta.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso `CONTROL SERVER`.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se cambia la importancia de las solicitudes en el grupo predeterminado de `MEDIUM` a `LOW`.  
  
```sql  
ALTER WORKLOAD GROUP "default"  
WITH (IMPORTANCE = LOW);  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
 En el ejemplo siguiente se muestra cómo mover un grupo de cargas de trabajo del grupo de recursos actual al predeterminado.  
  
```sql  
ALTER WORKLOAD GROUP adHoc  
USING [default];  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Regulador de recursos](../relational-databases/resource-governor/resource-governor.md)   
 [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../t-sql/statements/create-workload-group-transact-sql.md)   
 [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../t-sql/statements/drop-workload-group-transact-sql.md)   
 [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/create-resource-pool-transact-sql.md)   
 [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/alter-resource-pool-transact-sql.md)   
 [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/drop-resource-pool-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../t-sql/statements/alter-resource-governor-transact-sql.md)  
