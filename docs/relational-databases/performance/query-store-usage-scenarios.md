---
title: Escenarios de uso del Almacén de consultas | Microsoft Docs
description: Obtenga información sobre cómo se puede usar el Almacén de consultas para realizar un seguimiento y garantizar un rendimiento predecible de la carga de trabajo. Considere varios ejemplos en SQL Server.
ms.custom: ''
ms.date: 11/29/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Query Store, usage scenarios
ms.assetid: f5309285-ce93-472c-944b-9014dc8f001d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c65196b3195b8963b6d7c0cf97720b2c4f0d3212
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347700"
---
# <a name="query-store-usage-scenarios"></a>Escenarios de uso del Almacén de consultas
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  El Almacén de consultas se puede usar en un gran número de escenarios en los que es fundamental controlar y procurar un rendimiento de carga de trabajo de predicción. Estos son algunos ejemplos que se pueden tener en cuenta:  
  
-   Localizar y solucionar consultas con regresiones de elección del plan  
-   Identificación y optimización de las consultas que consumen más recursos  
-   Pruebas A/B  
-   Mantener la estabilidad del rendimiento al actualizar a una versión más reciente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
-   Identificación y mejora de las cargas de trabajo puntuales  
  
## <a name="pinpoint-and-fix-queries-with-plan-choice-regressions"></a>Localizar y solucionar consultas con regresiones de elección del plan  
 Durante su ejecución de consultas normal, el optimizador de consultas puede decidir adoptar un plan diferente porque las entradas importantes ahora son distintas, ya sea porque la cardinalidad de los datos ha cambiado, porque se han creado, modificado o eliminado índices, porque se han actualizado estadísticas, etc. En gran parte, el nuevo plan será mejor o aproximadamente igual que el que se usó anteriormente. Sin embargo, hay casos en los que el nuevo plan es considerablemente peor. Esta situación es conocida como "regresión de cambio de elección de plan". Antes de que existiera el Almacén de consultas, este era un problema difícil de identificar y corregir, ya que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no proporcionaba un almacén de datos integrado donde los usuarios pudieran buscar los planes de ejecución usados a lo largo del tiempo.  
  
 Con el Almacén de consultas puede hacer lo siguiente rápidamente:  
  
-   Identificar todas las consultas cuyas métricas de ejecución se hayan degradado en el período especificado (última hora, día, semana, etc.). Use la opción **Consultas devueltas** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para acelerar el análisis.  
  
-   Entre las consultas devueltas, es fácil distinguir las que tenían varios planes y que se degradaron debido a una mala elección del plan. Use el panel **Resumen del plan** en **Consultas devueltas** para visualizar todos los planes de una consulta devuelta y su rendimiento de consulta en el tiempo.  
  
-   Forzar el plan anterior del historial, si está confirmado que es mejor. Use el botón **Forzar plan** de **Consultas con regresión** para aplicar el plan seleccionado para la consulta.  
  
 ![Captura de pantalla del Almacén de consultas en la que se muestra un resumen del plan.](../../relational-databases/performance/media/query-store-usage-1.png "query-store-usage-1")  
  
 Para obtener una descripción detallada del escenario, consulte el blog [Query Store: A flight data recorder for your database](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/) (Almacén de consultas: Una caja negra de datos para la base de datos)  
  
## <a name="identify-and-tune-top-resource-consuming-queries"></a>Identificación y optimización de las consultas que consumen más recursos  
 Aunque la carga de trabajo puede generar miles de consultas, normalmente solo unas pocas usan realmente la mayor parte de los recursos del sistema y, por tanto, requieren su atención. Entre las consultas que más recursos consumen suelen estar las consultas devueltas o aquellas que se pueden ajustar para mejorarlas.  
  
 La forma más sencilla de empezar a explorar es abrir **Principales consultas que consumen recursos** en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. La interfaz de usuario se divide en tres paneles: un histograma con las consultas que consumen más recurso (izquierda), un resumen del plan para la consulta seleccionada (derecha) y un plan de consulta visual para el plan seleccionado (abajo). Haga clic en el botón **Configurar** para controlar la cantidad de consultas que quiere analizar y el intervalo de tiempo que le interesa. También puede elegir entre diferentes dimensiones de consumo de recursos (duración, CPU, memoria, E/S, número de ejecuciones) y la base (promedio, mínima, máxima, total, desviación estándar).  
  
 ![Captura de pantalla del Almacén de consultas en la que se muestra que se pueden identificar y ajustar las consultas que consumen más recursos.](../../relational-databases/performance/media/query-store-usage-2.png "query-store-usage-2")  
  
 Eche un vistazo al resumen del plan a la derecha para analizar el historial de ejecuciones y conocer los distintos planes y sus correspondientes estadísticas en tiempo de ejecución. Use el panel inferior para examinar los distintos planes o compararlos visualmente poniéndolos uno al lado del otro (para ello, use el botón Comparar).  
  
Cuando identifique una consulta con un rendimiento deficiente, la acción dependerá de la naturaleza del problema:  
  
1.  Si la consulta se ejecutó con varios planes y el último plan es mucho peor que el anterior, puede usar el mecanismo de forzado de plan para procurar que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] use siempre el mejor plan posible en futuras ejecuciones.  
  
2.  Compruebe si el optimizador sugiere que faltan índices en el plan XML. Si así es, cree el índice que falta y use el Almacén de consultas para evaluar el rendimiento de la consulta después de haber creado ese índice.  
  
3.  Asegúrese de que las estadísticas están actualizadas para las tablas subyacentes utilizadas por la consulta.  
  
4.  Asegúrese de que los índices usados por la consulta están desfragmentados.  
  
5.  Considere si merece la pena volver a escribir una consulta costosa. Por ejemplo, puede aprovechar las ventajas de la parametrización de consultas y reducir el uso de SQL dinámico. Implemente una lógica óptima al leer los datos (aplique el filtrado de datos en la base de datos, no en la aplicación).  

## <a name="ab-testing"></a>Pruebas A/B  
 Use el Almacén de consultas para comparar el rendimiento de una carga de trabajo antes y después del cambio de aplicación que tiene previsto realizar. La siguiente lista contiene ejemplos en los que se puede usar el Almacén de consultas para evaluar el impacto del cambio de entorno o aplicación en el rendimiento de la carga de trabajo:  
  
-   Implementar una nueva versión de la aplicación.  
  
-   Agregar nuevo hardware al servidor.  
  
-   Crear índices que faltan en las tablas a las que hacen referencia las consultas costosas.  
  
-   Aplicar una directiva de filtrado para la seguridad de nivel de fila. Para obtener más información, vea [Optimizing Row Level Security with Query Store](/archive/blogs/sqlsecurity/optimizing-rls-performance-with-the-query-store) (Optimización de la seguridad de nivel de fila con el Almacén de consultas).  
  
-   Agregar versiones de sistema temporales a tablas que las aplicaciones de OLTP modifican con frecuencia.  
  
En cualquiera de estos escenarios es válido el siguiente flujo de trabajo:  
  
1.  Ejecute la carga de trabajo con el Almacén de consultas antes de efectuar el cambio previsto para generar una base de rendimiento.  
  
2.  Realice el cambio de aplicación en el momento controlado de su elección.  
  
3.  Siga ejecutando la carga de trabajo el tiempo suficiente como para generar una imagen de rendimiento del sistema tras el cambio.  
  
4.  Compare los resultados de los pasos 1 y 3.  
  
    1.  Abra **Consumo general de base de datos** para conocer el impacto en toda la base de datos.  
  
    2.  Abra **Consultas que más recursos consumen** (o ejecute su propio análisis con [!INCLUDE[tsql](../../includes/tsql-md.md)]) para analizar el impacto del cambio en las consultas más importantes.  
  
5.  Decida si mantener el cambio o revertirlo si el nuevo rendimiento es inaceptable.  
  
En la siguiente ilustración se muestra el análisis del Almacén de consultas (paso 4) cuando se crea un índice que falta. Abra el panel Resumen del plan en **Consultas que más recursos consumen** para obtener una vista de la consulta que debería verse afectada por la creación del índice:  
  
![Captura de pantalla en la que se muestra el análisis del Almacén de consultas (paso 4) en caso de que falte la creación de índices.](../../relational-databases/performance/media/query-store-usage-3.png "query-store-usage-3")  
  
También puede comparar los planes anterior y posterior a la creación del índice poniéndolos uno al lado del otro (con la opción de barra de herramientas "Comparar los planes de la consulta seleccionada en una ventana independiente" que está marcada con un cuadrado de color rojo en la barra de herramientas).  
  
![Captura de pantalla en la que se muestra el Almacén de consultas y la opción para comparar los planes de la consulta seleccionada en una ventana independiente.](../../relational-databases/performance/media/query-store-usage-4.png "query-store-usage-4")  
  
El plan anterior a la creación del índice (con el número 1, arriba) muestra una sugerencia de falta de índice y puede observar que Clustered Index Scan fue el operador más caro de la consulta (enmarcado en un rectángulo rojo).  
  
El plan posterior a la creación del índice (con el número 15, abajo) tiene ahora Index Seek (Nonclustered), lo que reduce el costo total de la consulta y mejora el rendimiento (enmarcado en un rectángulo verde).  
  
Según el análisis, probablemente lo más conveniente sea mantener el índice, ya que el rendimiento de la consulta ha mejorado.  
  
## <a name="keep-performance-stability-during-the-upgrade-to-newer-ssnoversion"></a><a name="CEUpgrade"></a> Mantener la estabilidad del rendimiento al actualizar a una versión más reciente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
Antes de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], los usuarios corrían el riesgo de sufrir una regresión del rendimiento al actualizar a la versión más reciente de la plataforma. Esto se debía a que la versión más reciente del optimizador de consultas se activaba inmediatamente después de que se instalaran los nuevos bits.  
  
A partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], todos los cambios del optimizador de consultas están vinculados al [nivel de compatibilidad de la base de datos](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) más reciente, por lo que los planes no se cambian en el momento de la actualización, sino cuando un usuario cambia `COMPATIBILITY_LEVEL` a la versión más reciente. Esta función, junto con el Almacén de consultas, confiere al usuario un enorme control sobre el rendimiento de las consultas en el proceso de actualización. En la siguiente imagen se muestra el flujo de trabajo de actualización recomendado:  
  
![Diagrama que muestra el flujo de trabajo de actualización recomendado.](../../relational-databases/performance/media/query-store-usage-5.png "query-store-usage-5")  
  
1.  Actualice [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sin cambiar el nivel de compatibilidad de la base de datos. No expone los últimos cambios del optimizador de consultas, pero sí las características más recientes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], incluido el Almacén de consultas.  
  
2.  Habilite el Almacén de consultas. Para más información sobre este tema, vea [Mantener el Almacén de consultas ajustado a la carga de trabajo](../../relational-databases/performance/best-practice-with-the-query-store.md#Configure).

3.  Permita al Almacén de consultas capturar consultas y planes y establezca una base de rendimiento con el nivel de compatibilidad de base de datos de origen o anterior. Continúe en este paso el tiempo suficiente para capturar todos los planes y obtener una base estable. Puede ser la duración de un ciclo comercial habitual de una carga de trabajo de producción.  
  
4.  Pase al nivel de compatibilidad de base de datos más reciente: exponga la carga de trabajo a los últimos cambios del optimizador de consultas y deje que cree posibles nuevos planes.  
  
5.  Use el Almacén de consultas para realizar correcciones de regresión y análisis, si bien la mayoría de las veces los nuevos cambios del optimizador de consultas deberían generar mejores planes. Con todo, el Almacén de consultas proporcionará una forma fácil de identificar las regresiones de elección del plan y de corregirlas mediante el mecanismo de forzado de plan. A partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], cuando se usa la característica [Automatic Plan Correction](../../relational-databases/automatic-tuning/automatic-tuning.md#automatic-plan-correction) (Corrección de planes automática), este paso es automático.  

    a.  Para los casos en los que hay regresiones, fuerce el plan anterior de eficacia demostrada en el Almacén de consultas.  
  
    b.  Si hay planes de consulta que no se hayan podido forzar o si el rendimiento sigue siendo insuficiente, considere la posibilidad de revertir el [nivel de compatibilidad de la base de datos](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) al valor anterior y, después, póngase en contacto con el servicio de atención al cliente de Microsoft.  
    
> [!TIP]
> Use la tarea *Actualizar base de datos* de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]para actualizar el [nivel de compatibilidad de la base de datos](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades) de la base de datos. Consulte [Actualización de bases de datos mediante el Asistente para la optimización de consultas](../../relational-databases/performance/upgrade-dbcompat-using-qta.md) para más información.
  
## <a name="identify-and-improve-ad-hoc-workloads"></a>Identificación y mejora de las cargas de trabajo puntuales  
Algunas cargas de trabajo no tienen consultas dominantes que se puedan optimizar para mejorar el rendimiento general de la aplicación. Estas cargas de trabajo se suelen caracterizar por un número relativamente grande de consultas diferentes, y todas ellas consumen parte de los recursos del sistema. Al ser únicas, estas consultas se ejecutan con poca frecuencia (normalmente una sola vez, de ahí la nomenclatura “ad hoc”), por lo que su consumo de tiempo de ejecución no es crítico. Por otro lado, dado que la aplicación genera nuevas consultas netas todo el tiempo, se dedica una parte significativa de los recursos del sistema a compilar consultas, lo que no resulta óptimo. Esta situación tampoco es ideal para el Almacén de consultas, dado que el elevado número de consultas y planes acapara el espacio que tiene reservado, lo que significa que el Almacén de consultas probablemente acabe en modo de solo lectura muy rápidamente. Si activó la **directiva de limpieza según el tamaño** ([muy recomendable](best-practice-with-the-query-store.md) para mantener el almacén de consultas siempre en funcionamiento), el proceso en segundo plano limpiará las estructuras del almacén de consultas prácticamente todo el tiempo, lo que también consume muchos recursos del sistema.  
  
 La vista **Consultas que más recursos consumen** ofrece una primera indicación de la naturaleza ad hoc de la carga de trabajo:  
  
![Captura de pantalla de la vista de consultas que consumen más recursos en la que se muestra que la mayoría de las consultas que consumen más recursos solo se ejecuta una vez.](../../relational-databases/performance/media/query-store-usage-6.png "query-store-usage-6")  
  
Use la métrica **Recuento de ejecuciones** para analizar si las consultas principales son ad hoc (para ello, debe ejecutar el almacén de consultas con `QUERY_CAPTURE_MODE = ALL`). En el diagrama anterior, puede ver que el 90 % de las **Consultas que más recursos consumen** se ejecuta solo una vez.  
  
Opcionalmente, puede ejecutar el script [!INCLUDE[tsql](../../includes/tsql-md.md)] para obtener el número total de textos de consulta, consultas y planes en el sistema, y conocer sus diferencias comparando los valores query_hash y plan_hash:  
  
```sql  
--Do cardinality analysis when suspect on ad hoc workloads
SELECT COUNT(*) AS CountQueryTextRows FROM sys.query_store_query_text;  
SELECT COUNT(*) AS CountQueryRows FROM sys.query_store_query;  
SELECT COUNT(DISTINCT query_hash) AS CountDifferentQueryRows FROM  sys.query_store_query;  
SELECT COUNT(*) AS CountPlanRows FROM sys.query_store_plan;  
SELECT COUNT(DISTINCT query_plan_hash) AS  CountDifferentPlanRows FROM  sys.query_store_plan;  
```  
  
Este es un posible resultado que puede obtener en el caso de cargas de trabajo con consultas ad hoc:  
  
![Captura de pantalla del posible resultado que puede obtener en el caso de cargas de trabajo con consultas ad-hoc.](../../relational-databases/performance/media/query-store-usage-7.png "query-store-usage-7")  
  
El resultado de la consulta muestra que, a pesar del gran número de planes y consultas en el Almacén de consultas, sus query_hash y query_plan_hash no son realmente diferentes. Una relación entre textos de consulta únicos y hashes de consulta únicos que esté muy por encima de 1 es indicativa de que esa carga de trabajo es una buena candidata para la parametrización, dado que la única diferencia entre las consultas es la constante literal (parámetro) que se proporciona como parte del texto de la consulta.  
  
Normalmente, esta situación puede ocurrir si la aplicación genera consultas (en lugar de llamar a procedimientos almacenados o a consultas con parámetros) o si depende de marcos de trabajo de asignación relativos a objetos que generan consultas de forma predeterminada.  
  
Si tiene el control del código de la aplicación, sopese la posibilidad de volver a escribir la capa de acceso a los datos para que se usen procedimientos almacenados o consultas parametrizadas. Pese a todo lo anterior, esta situación también se puede mejorar considerablemente sin cambios en la aplicación. Basta con forzar la parametrización de consultas de toda la base de datos (todas las consultas) o de las plantillas de consulta individuales con el mismo query_hash.  
  
El método de las plantillas de consulta individuales requiere crear una guía de plan:  
  
```sql  
--Apply plan guide for the selected query template 
DECLARE @stmt nvarchar(max);  
DECLARE @params nvarchar(max);  
EXEC sp_get_query_template   
    N'<your query text goes here>',  
    @stmt OUTPUT,   
    @params OUTPUT;  
  
EXEC sp_create_plan_guide   
    N'TemplateGuide1',   
    @stmt,   
    N'TEMPLATE',   
    NULL,   
    @params,   
    N'OPTION (PARAMETERIZATION FORCED)';  
```  
  
Una solución con guías de plan es más precisa, pero requiere más trabajo.  
  
Si todas las consultas (o la mayoría de ellas) son aptas para la parametrización automática, cambiar `FORCED PARAMETERIZATION` para toda la base de datos puede ser una mejor opción:  
  
```sql  
--Apply forced parameterization for entire database  
ALTER DATABASE <database name> SET PARAMETERIZATION FORCED;  
```  

> [!NOTE]
> Para obtener más información sobre este tema, consulte [Directrices para usar la parametrización forzada](../../relational-databases/query-processing-architecture-guide.md#ForcedParamGuide).

Después de aplicar cualquiera de estos pasos, **Principales consultas que consumen recursos** mostrará una imagen distinta de la carga de trabajo.  
  
![Captura de pantalla de la vista de consultas que consumen más recursos en la que se muestra una imagen diferente de la carga de trabajo.](../../relational-databases/performance/media/query-store-usage-8.png "query-store-usage-8")  
  
En algunos casos, la aplicación puede generar una gran cantidad de consultas diferentes que no son aptas para la parametrización automática. En ese caso, verá un gran número de consultas en el sistema, pero la relación entre las consultas únicas y los `query_hash` únicos estará bastante próxima a 1.  
  
En ese caso, es posible que quiera habilitar la opción del servidor [**Optimizar para cargas de trabajo ad hoc**](../../database-engine/configure-windows/optimize-for-ad-hoc-workloads-server-configuration-option.md) para no desperdiciar memoria caché con consultas que probablemente no vuelvan a ejecutarse. Para evitar que se capturen esas consultas en el Almacén de consultas, establezca `QUERY_CAPTURE_MODE` en `AUTO`.  
  
```sql  
EXEC sys.sp_configure N'show advanced options', N'1' RECONFIGURE WITH OVERRIDE
GO
EXEC sys.sp_configure N'optimize for ad hoc workloads', N'1'
GO
RECONFIGURE WITH OVERRIDE
GO 
  
ALTER DATABASE [QueryStoreTest] SET QUERY_STORE CLEAR;  
ALTER DATABASE [QueryStoreTest] SET QUERY_STORE = ON   
    (OPERATION_MODE = READ_WRITE, QUERY_CAPTURE_MODE = AUTO);  
```  
  
## <a name="see-also"></a>Consulte también  
 [Monitoring Performance By Using the Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Procedimiento recomendado con el Almacén de consultas](../../relational-databases/performance/best-practice-with-the-query-store.md)         
 [Actualización de bases de datos mediante el Asistente para la optimización de consultas](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)           
