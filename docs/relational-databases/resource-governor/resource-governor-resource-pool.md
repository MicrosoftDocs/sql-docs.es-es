---
title: Grupo de recursos de servidor del regulador de recursos
description: Resource Governor de SQL Server especifica los límites en cuanto a la cantidad de CPU, E/S física y memoria del grupo de recursos de servidor que pueden usar las solicitudes entrantes procedentes de las aplicaciones.
ms.custom: ''
ms.date: 10/20/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Resource Governor, resource pool
- resource pool [SQL Server], overview
- resource pool [SQL Server]
ms.assetid: 306b6278-e54f-42e6-b746-95a9315e0cbe
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 065cf98e5ada04d4ac192c50f4c5b5a3d1790bf6
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237363"
---
# <a name="resource-governor-resource-pool"></a>Grupo de recursos de servidor del regulador de recursos
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  En el regulador de recursos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , un grupo de recursos de servidor representa un subconjunto de los recursos físicos de una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)]. El regulador de recursos permite especificar los límites en cuanto a la cantidad de CPU, E/S física y memoria del grupo de recursos de servidor que pueden usar las solicitudes entrantes procedentes de las aplicaciones. Cada grupo de recursos de servidor puede contener uno o más grupos de cargas de trabajo. Cuando se inicia una sesión, el clasificador del regulador de recursos asigna la sesión a un grupo de cargas de trabajo concreto y la sesión se debe ejecutar utilizando los recursos asignados al grupo de cargas de trabajo.  
  
## <a name="resource-pool-concepts"></a>Conceptos de los grupos de recursos de servidor  
 Un grupo de recursos de servidor, o grupo, representa los recursos físicos del servidor. Puede pensar en un grupo como en una instancia virtual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dentro de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Un grupo tiene dos partes. Una parte no se superpone con otros grupos, lo que permite una reserva mínima de recursos. La otra parte se comparte con otros grupos, lo que permite consumir el número máximo de recursos. Los recursos del grupo se definen especificando uno o varios de los siguientes valores para cada recurso (CPU, memoria y E/S física):  
  
-   **MIN_CPU_PERCENT y MAX_CPU_PERCENT**  
  
     Estos son los valores mínimo y máximo para el ancho de banda de la CPU promedio garantizado para todas las solicitudes del grupo de recursos de servidor cuando hay contención de CPU. Puede utilizar estos valores para establecer un uso de recursos de la CPU predecible para varias cargas de trabajo que se basa en las necesidades de cada carga de trabajo. Por ejemplo, suponga que los departamentos de ventas y marketing de una empresa comparten la misma base de datos. El departamento de ventas tiene una carga de trabajo intensiva de CPU con consultas de alta prioridad. El departamento de marketing también tiene una carga de trabajo intensiva de CPU, pero tiene consultas de baja prioridad. Si crea un grupo de recursos de servidor independiente para cada departamento, puede asignar un porcentaje de CPU *mínimo* de 70 para el grupo de recursos de servidor de ventas y un porcentaje *máximo* de CPU de 30 para el grupo de recursos de servidor de marketing. Así se asegurará de que la carga de trabajo de ventas recibe los recursos de CPU que necesita y la carga de trabajo de marketing está aislada de la demanda de la carga de trabajo de ventas. Tenga en cuenta que el porcentaje de CPU máximo es un máximo oportunista. Si hay capacidad de CPU disponible, la carga de trabajo utiliza hasta el 100 por cien. El valor máximo solo se aplica cuando hay contención de recursos de CPU. En este ejemplo, si la carga de trabajo de ventas se desactiva, la carga de trabajo de marketing puede utilizar el 100 por cien de la CPU si es necesario.  
  
-   **CAP_CPU_PERCENT**  
  
     El valor CAP_CPU_PERCENT constituye un límite máximo en cuanto al ancho banda de la CPU para todas las solicitudes del grupo de recursos. Las cargas de trabajo asociadas al grupo pueden utilizar la capacidad de CPU existente por encima del valor de MAX_CPU_PERCENT si está disponible, pero sin sobrepasar el valor de CAP_CPU_PERCENT. Utilizando el ejemplo anterior, supongamos que se cobra al departamento de marketing por el uso de recursos. Desean una facturación predecible y no desean pagar más del 30 por ciento de la CPU. Esto se logra estableciendo CAP_CPU_PERCENT en 30 para el grupo de recursos de servidor de marketing.  
  
-   **MIN_MEMORY_PERCENT y MAX_MEMORY_PERCENT**  
  
     Estos valores representan la cantidad mínima y máxima de memoria reservada para el grupo de recursos que no se puede compartir con otros grupos de recursos. Para bases de datos sin tablas optimizada para memoria, la memoria especificada aquí se refiere a la memoria concedida para la ejecución de consultas, no la memoria del grupo de búferes (por ejemplo, las páginas de datos y de índices). Establecer un valor mínimo de memoria para un grupo significa asegurarse de que el porcentaje de memoria especificado estará disponible para cualquier solicitud que pueda ejecutarse en este grupo de recursos de servidor. Existe una diferencia importante en comparación con MIN_CPU_PERCENT ya que, en este caso, la memoria puede permanecer en el grupo de recursos de servidor incluso si este no tiene ninguna solicitud procedente de los grupos de cargas de trabajo que pertenecen a este grupo de recursos. Por consiguiente, es fundamental que tenga mucha precaución al utilizar este valor, porque esta memoria no estará disponible para su uso en ningún otro grupo, aunque no haya solicitudes activas. Establecer un valor máximo de memoria para un grupo significa que cuando las solicitudes se ejecuten en este grupo nunca obtendrán más que este porcentaje de la memoria total.

     Con el fin de controlar la memoria de las tablas optimizadas para memoria con Resource Governor, debe enlazar la base de datos a un grupo de recursos independiente. Para más información, consulte [Enlace de una base de datos con tablas optimizadas para memoria a un grupo de recursos](../in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md).
  
-   **AFFINITY**  
  
     Este valor permite establecer afinidad entre un grupo de recursos de servidor y uno o varios programadores o nodos NUMA para conseguir un mayor aislamiento de los recursos de CPU. Siguiendo con el escenario de ventas y marketing anterior, supongamos que el departamento de ventas necesita un entorno más aislado y desea disponer en todo momento del 100 por cien de un núcleo de CPU. Mediante la opción AFFINITY, las cargas de trabajo de ventas y marketing se pueden programar para que se ejecuten en CPU distintas. Suponiendo que sigue vigente CAP_CPU_PERCENT para el grupo de marketing, la carga de trabajo de marketing continúa utilizando un máximo del 30 por ciento de un núcleo, mientras que la carga de trabajo de ventas utiliza el 100 por cien del otro núcleo. En lo que se refiere a las cargas de trabajo de ventas y de marketing, estas se ejecutan en dos equipos aislados.  
  
-   **MIN_IOPS_PER_VOLUME y MAX_IOPS_PER_VOLUME**  
  
     Estos valores representan el número mínimo y máximo de operaciones de E/S físicas por segundo (IOPS) por cada volumen de disco para un grupo de recursos de servidor. Puede utilizar estos valores para controlar las E/S físicas emitidas para los subprocesos de usuario para un grupo de recursos de servidor determinado. Por ejemplo, el departamento de ventas genera varios informes de fin de mes en lotes grandes. Las consultas de estos lotes pueden generar operaciones de E/S que podrían saturar el volumen de disco y afectar al rendimiento de otras cargas de trabajo de mayor prioridad en la base de datos. Para aislar esta carga de trabajo, MIN_IOPS_PER_VOLUME se establece en 20 y MAX_IOPS_PER_VOLUME se establece en 100 para el grupo de recursos del departamento de ventas, lo que controla el nivel de operaciones de E/S que se pueden emitir para la carga de trabajo.  
  
Cuando se configura la CPU o la memoria, la suma de los valores MIN de todos los grupos no puede superar el 100 por cien de los recursos del servidor. Además, al configurar la CPU o la memoria, los valores MAX y CAP se pueden establecer en cualquier punto del intervalo entre el valor MIN y el 100 por cien inclusive.  
  
Si un grupo tiene definido un valor MIN distinto de cero, el valor MAX efectivo de otros grupos se reajustará. El mínimo del valor MAX configurado de un grupo y la suma de los valores MIN de otros grupos se resta del 100 %.  
  
En la tabla siguiente se muestran algunos de los conceptos anteriores. La tabla muestra los valores para el grupo interno, el grupo predeterminado y para dos grupos definidos por el usuario. 
  
|Nombre del grupo|Valor de % MIN|Valor de % MAX|% MAX efectivo calculado|% compartido calculado|Comentario|  
|---------------|-------------------|-------------------|--------------------------------|-------------------------|-------------|  
|interno|0|100|100|0|Los valores de % MAX efectivo y % compartido no son aplicables al grupo interno.|  
|default|0|100|30|30|El valor MAX efectivo se calcula como: min (100,100 - (20+50)) = 30. El % compartido calculado es el MAX efectivo - MIN = 30.|  
|Grupo 1|20|100|50|30|El valor MAX efectivo se calcula como: min(100,100-50) = 50. El % compartido calculado es el MAX efectivo - MIN = 30.|  
|Grupo 2|50|70|70|20|El valor MAX efectivo se calcula como: min(70,100-20) = 70. El % compartido calculado es el MAX efectivo - MIN = 20.|  

Las fórmulas siguientes se usan para calcular el porcentaje máximo efectivo y el porcentaje compartido en la tabla anterior:  
  
-   Min(X,Y) se refiere al valor mínimo de X e Y.  
  
-   Sum(X) se refiere a la suma del valor X a lo largo de todos los grupos.  
  
-   % total compartido = 100 - sum(% MIN).  
  
-   % MAX efectivo = min(X,Y).  
  
-   % compartido = % MAX efectivo - % MIN.  

Si tomamos la tabla anterior como ejemplo, podemos mostrar más detalladamente los ajustes que tienen lugar cuando se crea otro grupo. Este grupo es el Grupo 3 y tiene un valor de % MIN de 5.  
  
|Nombre del grupo|Valor de % MIN|Valor de % MAX|% MAX efectivo calculado|% compartido calculado|Comentario|  
|---------------|-------------------|-------------------|--------------------------------|-------------------------|-------------|  
|interno|0|100|100|0|Los valores de % MAX efectivo y % compartido no son aplicables al grupo interno.|  
|default|0|100|25|25|El valor MAX efectivo se calcula como: min(100,100-(20+50+5)) = 25. El % compartido calculado es el MAX efectivo - MIN = 25.|  
|Grupo 1|20|100|45|25|El valor MAX efectivo se calcula como: min (100,100-55) = 45. El % compartido calculado es el MAX efectivo - MIN = 25.|  
|Grupo 2|50|70|70|20|El valor MAX efectivo se calcula como: min(70,100-25) = 70. El % compartido calculado es el MAX efectivo - MIN = 20.|  
|Grupo 3|5|100|30|25|El valor MAX efectivo se calcula como: min(100,100-70) = 30. El % compartido calculado es el MAX efectivo - MIN = 25.|  
  
 La parte compartida del grupo se utiliza para indicar dónde pueden ir los recursos en caso de que estén disponibles. Sin embargo, cuando se utilizan los recursos, estos van al grupo especificado y no se comparten. Esto puede mejorar la utilización de los recursos en aquellos casos en los que no existen solicitudes en un grupo dado y donde es posible que los recursos configurados para el grupo se liberen para otros grupos.  
  
 Algunos casos extremos de configuración del grupo son:  
  
-   Todos los grupos definen mínimos que en total representan el 100 por cien de los recursos del servidor. En este caso, los máximos efectivos son iguales que los mínimos. Esto equivale a dividir los recursos del servidor en partes no superpuestas independientemente de que los recursos se utilicen dentro de un grupo determinado.  
  
-   Todos los grupos tienen mínimos cero. Todos los grupos compiten por los recursos disponibles y sus tamaños finales están basados en el consumo de cada grupo. Otros factores, como pueden ser las directivas, desempeñan un rol importante a la hora de determinar el tamaño final del grupo.  
  
El regulador de recursos predefine dos grupos de recursos de servidor, el grupo interno y el grupo predeterminado. Puede agregar grupos adicionales.  
  
**Grupo interno**  
  
El grupo interno representa los recursos utilizados por el propio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Este grupo siempre contiene el grupo interno únicamente y el grupo no se puede alterar de ninguna forma. El consumo de recursos por parte del grupo interno no está restringido. Cualquier carga de trabajo del grupo se considera crítica para la función del servidor y el regulador de recursos permite al grupo interno presionar a otros grupos, incluso aunque esto signifique infringir los límites establecidos para los demás grupos.  
  
> [!NOTE]  
>  El grupo interno y el uso de recursos del grupo interno no se restan del uso de recursos totales. Los porcentajes se calculan a partir de los recursos totales disponibles.  
  
**Grupo predeterminado**  
  
El grupo predeterminado es el primer grupo de usuario predefinido. Antes de realizar cualquier configuración, el grupo de recursos de servidor predeterminado solo contiene el grupo predeterminado. El grupo de recursos de servidor predeterminado no se puede crear o quitar, pero se puede modificar. El grupo de recursos de servidor predeterminado puede contener grupos definidos por el usuario además del grupo predeterminado. A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hay un grupo de recursos predeterminado para las operaciones rutinarias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y un grupo de recursos externo predeterminado para los procesos externos, como la ejecución de scripts de R.  
  
> [!NOTE]  
>  El grupo predeterminado se puede modificar, pero no puede moverse fuera del grupo de recursos de servidor predeterminado.  
  
**Grupo externo**  
  
Los usuarios pueden definir un grupo externo para definir los recursos para los procesos externos. Para los servicios de R, esto rige específicamente `rterm.exe`, `BxlServer.exe` y otros procesos generados por ellos.  
  
**Grupos de recursos de servidor definidos por el usuario**  
  
Los grupos de recursos de servidor definidos por el usuario son los que se crean para las cargas de trabajo concretas de un entorno. El regulador de recursos proporciona instrucciones DDL para crear, modificar y quitar grupos de recursos de servidor.  
  
## <a name="resource-pool-tasks"></a>Tareas de los grupos de recursos de servidor  
  
|Descripción de la tarea|Artículo|  
|----------------------|-----------|  
|Describe cómo crear un grupo de recursos de servidor.|[Crear un grupo de recursos de servidor](../../relational-databases/resource-governor/create-a-resource-pool.md)|  
|Describe cómo cambiar la configuración del grupo de recursos de servidor.|[Cambiar la configuración del grupo de recursos de servidor](../../relational-databases/resource-governor/change-resource-pool-settings.md)|  
|Describe cómo eliminar un grupo de recursos de servidor.|[Eliminar un grupo de recursos de servidor](../../relational-databases/resource-governor/delete-a-resource-pool.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Regulador de recursos](../../relational-databases/resource-governor/resource-governor.md)   
 [Grupos de cargas de trabajo del regulador de recursos](../../relational-databases/resource-governor/resource-governor-workload-group.md)   
 [Función clasificadora del regulador de recursos](../../relational-databases/resource-governor/resource-governor-classifier-function.md)   
 [Configurar el regulador de recursos utilizando una plantilla](../../relational-databases/resource-governor/configure-resource-governor-using-a-template.md)   
 [Ver las propiedades del regulador de recursos](../../relational-databases/resource-governor/view-resource-governor-properties.md)  
  
  
