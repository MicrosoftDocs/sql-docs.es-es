---
title: Actualización de bases de datos con el Asistente para la optimización de consultas
description: Obtenga información sobre el Asistente para la optimización de consultas y cómo le guía por el flujo de trabajo recomendado para mantener la estabilidad del rendimiento durante las actualizaciones a versiones más recientes de SQL Server.
ms.custom: seo-dt-2019
ms.date: 02/13/2019
ms.prod: sql
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.swb.querytuning.f1
helpviewer_keywords:
- query statistics [SQL Server] live query stats
- live query statistics
- debugging [SQL Server], live query stats
- statistics [SQL Server], live query statistics
- query profiling
- lightweight query profiling
- lightweight profiling
ms.assetid: 07f8f594-75b4-4591-8c29-d63811e7753e
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 1032f8e1ac32a08c9a6878c54fac0ee5e5c213e5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015534"
---
# <a name="upgrading-databases-by-using-the-query-tuning-assistant"></a>Actualización de bases de datos mediante el Asistente para la optimización de consultas

[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

Al migrar desde una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] o posterior, y al [actualizar el nivel de compatibilidad de la base de datos](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) al más reciente disponible, una carga de trabajo podría quedar expuesta al riesgo de regresión del rendimiento. Esto también es posible en menor grado al actualizar entre [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y cualquier versión más reciente.

A partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], y con cada nueva versión, todos los cambios del optimizador de consultas se canalizan al nivel de compatibilidad de la base de datos más reciente, por lo que los planes de ejecución no se cambian en el momento de la actualización, sino cuando un usuario cambia la opción de base de datos `COMPATIBILITY_LEVEL` a la más reciente disponible. Para obtener más información sobre los cambios del optimizador de consultas incorporados en [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], vea [Estimación de cardinalidad](../../relational-databases/performance/cardinality-estimation-sql-server.md). Para obtener más información sobre los niveles de compatibilidad y cómo pueden afectar a las actualizaciones, vea [Actualizaciones del motor de base de datos y niveles de compatibilidad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades).

Esta capacidad de canalización proporcionada por el nivel de compatibilidad de la base de datos, en combinación con el Almacén de consultas, ofrece un gran nivel de control sobre el rendimiento de las consultas durante el proceso de actualización si la actualización sigue el flujo de trabajo recomendado que se indica a continuación. Para obtener más información sobre el flujo de trabajo recomendado para actualizar el nivel de compatibilidad, vea [Cambiar el nivel de compatibilidad de la base de datos y usar el almacén de consultas](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md). 

![Flujo de trabajo de actualización de base de datos recomendado mediante el almacén de consultas](../../relational-databases/performance/media/query-store-usage-5.png "Flujo de trabajo de actualización de base de datos recomendado mediante el almacén de consultas") 

Este control sobre las actualizaciones se ha mejorado aún más con [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], donde se ha incorporado el [ajuste automático](../../relational-databases/automatic-tuning/automatic-tuning.md) y se permite la automatización del último paso del flujo de trabajo recomendado anterior.

A partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v18, la nueva característica **Asistente para la optimización de consultas (QTA)** guía a los usuarios a través del flujo de trabajo recomendado para mantener la estabilidad del rendimiento durante las actualizaciones a las versiones más recientes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], como se documenta en la sección *Mantener la estabilidad del rendimiento al actualizar a una versión más reciente de SQL Server* de [Escenarios de uso del Almacén de consultas](../../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade). Sin embargo, QTA no puede revertir a un buen plan conocido previamente como se muestra en el último paso del flujo de trabajo recomendado. En su lugar, QTA realizará el seguimiento de las regresiones encontradas en la vista [Consultas devueltas de **Query Stores**](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md#Regressed) y recorrerá en iteración las permutaciones posibles de variaciones de modelos de optimizador aplicables para que se genere un plan mejor.

> [!IMPORTANT]
> QTA no genera carga de trabajo de usuario. Si ejecuta QTA en un entorno que las aplicaciones no usan, asegúrese de que todavía se puedan ejecutar cargas de trabajo de prueba representativas en el [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] de destino por otros medios.

## <a name="the-query-tuning-assistant-workflow"></a>Flujo de trabajo del Asistente para la optimización de consultas

Como punto de partida de QTA se da por supuesto que una base de datos de una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ha migrado (mediante [CREATE DATABASE ... FOR ATTACH](../..//relational-databases/databases/attach-a-database.md) o [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)) a una versión más reciente del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], y que el nivel de compatibilidad de la base de datos anterior a la actualización no se ha cambiado de inmediato. QTA le guía a través de los pasos siguientes:

1. Configure el Almacén de consultas conforme a la configuración recomendada para la duración de la carga de trabajo (en días) establecida por el usuario. Piense en la duración de la carga de trabajo que coincida con el ciclo comercial típico.
2. Solicite iniciar la carga de trabajo necesaria para que el Almacén de consultas pueda recopilar una línea de base de datos de carga de trabajo (si no hay ninguna disponible aún).
3. Actualice al nivel de compatibilidad de la base de datos de destino elegido por el usuario.
4. Solicite que se recopile una segunda pasada de datos de carga de trabajo para la comparación y la detección de regresiones.
5. Recorra en iteración las regresiones detectadas en función de la vista [**Consultas con regresión** del Almacén de consultas](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md#Regressed), experimente mediante la recopilación de estadísticas en tiempo de ejecución sobre las posibles permutaciones de variaciones de modelo del optimizador aplicables y mida el resultado. 
6. Informe sobre las mejoras medidas y, opcionalmente, permita que los cambios se conserven mediante [guías de plan](../../relational-databases/performance/plan-guides.md).

Para obtener más información sobre cómo asociar una base de datos, vea [Adjuntar y separar bases de datos](../../relational-databases/databases/database-detach-and-attach-sql-server.md#AttachDb).

Vea a continuación cómo QTA solo cambia los últimos pasos del flujo de trabajo recomendado para actualizar el nivel de compatibilidad mediante el Almacén de consultas visto arriba. En lugar de dar la opción de elegir entre el plan de ejecución actualmente ineficaz y el último plan de ejecución bueno conocido, QTA presenta opciones de optimización que son específicas de las consultas con regresión seleccionadas, con el fin de crear un nuevo estado mejorado con planes de ejecución optimizados.

![Flujo de trabajo de actualización de base de datos recomendado mediante QTA](../../relational-databases/performance/media/qta-usage.png "Flujo de trabajo de actualización de base de datos recomendado mediante QTA")

### <a name="qta-tuning-internal-search-space"></a>Espacio de búsqueda interno de optimización de QTA

QTA solo se ocupa de las consultas `SELECT` que se pueden ejecutar desde el Almacén de consultas. Las consultas parametrizadas son aptas si se conoce el parámetro compilado. Las consultas que dependen de construcciones en tiempo de ejecución, como tablas temporales o variables de tabla, no son aptas en este momento.

QTA tiene como destino posibles patrones conocidos de regresiones de consulta debidos a cambios en las versiones de [Estimación de cardinalidad](../../relational-databases/performance/cardinality-estimation-sql-server.md). Por ejemplo, al actualizar una base de datos desde [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y el nivel de compatibilidad de la base de datos 110 a [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] y el nivel de compatibilidad de la base de datos 140, algunas consultas pueden sufrir regresión porque se diseñaron específicamente para trabajar con la versión de estimación de cardinalidad existente en [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] (70). Esto no significa que la reversión de estimación de cardinalidad 140 a 70 sea la única opción. Si solo un cambio determinado en la versión más reciente incorpora la regresión, es posible sugerir que la consulta use solo la parte relevante de la versión de estimación de cardinalidad anterior que funcionaba mejor para esa consulta, sin dejar de aprovechar todas las demás mejoras de versiones más recientes de estimación de cardinalidad. Además permite que las demás consultas de la carga de trabajo que no han sufrido regresión se beneficien de las últimas mejoras de estimación de cardinalidad.

Los patrones de estimación de cardinalidad que busca QTA son los siguientes:

- **Independencia frente a correlación**: si la suposición de independencia proporciona mejores estimaciones para la consulta específica, la sugerencia de consulta `USE HINT ('ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES')` hace que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genere un plan de ejecución mediante la selectividad mínima al estimar predicados `AND` para que los filtros tengan en cuenta la correlación. Para obtener más información, vea [USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint) y [Versiones de la estimación de cardinalidad](../../relational-databases/performance/cardinality-estimation-sql-server.md#versions-of-the-ce).
- **Contención simple frente a contención base**: si otra contención de combinación proporciona mejores estimaciones para la consulta específica, la sugerencia de consulta `USE HINT ('ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS')` hace que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genere un plan de ejecución mediante la suposición de contención simple en lugar de la suposición de contención base predeterminada. Para obtener más información, vea [USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint) y [Versiones de la estimación de cardinalidad](../../relational-databases/performance/cardinality-estimation-sql-server.md#versions-of-the-ce).
- **Estimación de cardinalidad fija de función con valores de tabla de múltiples instrucciones (MSTVF)** de 100 filas frente a 1 fila: si la estimación fija predeterminada para TVF de 100 filas no da lugar a un plan más eficaz que el uso de la estimación fija para TVF de 1 fila (correspondiente al valor predeterminado en el modelo de estimación de cardinalidad del optimizador de consultas de [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] y versiones anteriores), se usa la sugerencia de consulta `QUERYTRACEON 9488` para generar un plan de ejecución. Para obtener más información sobre MSTVF, vea [Crear funciones definidas por el usuario &#40;motor de base de datos&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#TVF).

> [!NOTE]
> Como último recurso, si las sugerencias de ámbito estrecho no generan resultados lo suficientemente buenos para los patrones de consulta aptos, también se considera el uso completo de estimación de cardinalidad 70, mediante la sugerencia de consulta `USE HINT ('FORCE_LEGACY_CARDINALITY_ESTIMATION')` para generar un plan de ejecución.

> [!IMPORTANT]
> Cualquier sugerencia fuerza determinados comportamientos que podrían tratarse en futuras actualizaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se recomienda aplicar sugerencias únicamente cuando no exista ninguna otra opción, y planee revisar el código sugerido con cada nueva actualización. Al forzar comportamientos, es posible que evite que la carga de trabajo se beneficie de las mejoras incorporadas en las versiones más recientes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="starting-query-tuning-assistant-for-database-upgrades"></a>Inicio del Asistente para la optimización de consultas para actualizaciones de base de datos

QTA es una característica basada en sesión que almacena el estado de sesión en el esquema `msqta` de la base de datos de usuario donde se crea una sesión por primera vez. Se pueden crear varias sesiones de optimización en una sola base de datos con el tiempo, pero solo puede existir una sesión activa de cualquier base de datos determinada.

### <a name="creating-a-database-upgrade-session"></a>Creación de una sesión de actualización de base de datos

1. En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], abra el Explorador de objetos y conéctese al [!INCLUDE[ssDE](../../includes/ssde-md.md)].

2. En la base de datos cuyo nivel de compatibilidad de la base de datos intenta actualizar, haga clic con el botón derecho en el nombre de la base de datos, seleccione **Tareas**, seleccione **Actualización de base de datos** y seleccione **Nueva sesión de actualización de base de datos**.

3. En la ventana del asistente QTA, se requieren dos pasos para configurar una sesión:

    1. En la ventana **Setup** (Configuración), configure el Almacén de consultas para capturar el equivalente a un ciclo comercial completo de datos de carga de trabajo para analizar y optimizar.
        - Especifique la duración de la carga de trabajo esperada en días (el mínimo es 1 día). Se usa para proponer la configuración recomendada del Almacén de consultas para permitir que se recopile la línea de base completa. La captura de una buena línea de base es importante para garantizar que las consultas con regresión detectadas después de cambiar el nivel de compatibilidad de la base de datos se puedan analizar. 
        - Establezca el nivel de compatibilidad previsto de la base de datos de destino en el que debería estar la base de datos de usuario una vez completado el flujo de trabajo de QTA.
        Cuando haya terminado, seleccione **Siguiente**.

       ![Nueva ventana de configuración de sesión de actualización de base de datos](../../relational-databases/performance/media/qta-new-session-setup.png "Nueva ventana de configuración de actualización de base de datos")  
  
    2. En la ventana **Configuración**, dos columnas muestran el estado **Actual** del Almacén de consultas de la base de destino, así como la configuración **Recomendada**. 
        - La configuración recomendada se selecciona de forma predeterminada, pero al seleccionar el botón radio en la columna actual, se acepta la configuración actual y también se permite ajustar con precisión la configuración actual del Almacén de consultas.
        - El valor de *Umbral de consultas obsoletas* propuesto es dos veces la duración de la carga de trabajo esperada en días. Esto se debe a que el Almacén de consultas debe contener información sobre la carga de trabajo de línea de base y la carga de trabajo de actualización posterior a la base de datos.
        Cuando haya terminado, seleccione **Siguiente**.

       ![Nueva ventana de configuración de actualización de base de datos](../../relational-databases/performance/media/qta-new-session-settings.png "Nueva ventana de configuración de actualización de base de datos")

       > [!IMPORTANT]
       > El valor *Tamaño máximo* propuesto es un valor arbitrario que puede ser adecuado para una carga de trabajo de breve duración.
       > Pero tenga en cuenta que puede ser insuficiente para contener información sobre la línea de base y las cargas de trabajo de actualización posteriores a la base de datos de cargas de trabajo muy intensivas, concretamente cuando pueden generarse muchos planes diferentes.
       > Si anticipa que este pueda ser el caso, escriba un valor más alto que sea adecuado.

4. La ventana **Optimización** concluye la configuración de la sesión e indica los pasos siguientes para abrir la sesión y continuar con ella. Cuando haya finalizado, seleccione **Finalizar**.

    ![Nueva ventana de optimización de actualización de base de datos](../../relational-databases/performance/media/qta-new-session-tuning.png "Nueva ventana de optimización de actualización de base de datos")

> [!NOTE]
> Un posible escenario alternativo se inicia al restaurar la copia de seguridad de una base de datos desde el servidor de producción donde una base de datos ya ha pasado por el flujo de trabajo de actualización de compatibilidad de base de datos recomendado en un servidor de prueba.

### <a name="executing-the-database-upgrade-workflow"></a>Ejecución del flujo de trabajo de actualización de base de datos

1. En la base de datos cuyo nivel de compatibilidad de la base de datos intenta actualizar, haga clic con el botón derecho en el nombre de la base de datos, seleccione **Tareas**, seleccione **Actualización de base de datos** y seleccione **Supervisar sesiones**.

2. La página de **administración de sesiones** enumera las sesiones actuales y pasadas de la base de datos en cuestión. Seleccione la sesión deseada y, después, **Detalles**.

    > [!NOTE]
    > Si la sesión actual no aparece, seleccione el botón **Actualizar**.

    La lista contiene la información siguiente:
    - **Id. de sesión**
    - **Nombre de sesión**: nombre generado por el sistema compuesto por el nombre de la base de datos y la fecha y hora de creación de la sesión.
    - **Estado**: estado de la sesión (activa o cerrada).
    - **Descripción**: generada por el sistema; consta del nivel de compatibilidad de la base de datos de destino seleccionado por el usuario y el número de días de la carga de trabajo de ciclo comercial.
    - **Hora de inicio**: fecha y hora de creación de la sesión.

    ![Página de administración de sesiones de QTA](../../relational-databases/performance/media/qta-session-management.png "Página de administración de sesiones de QTA")

    > [!NOTE]
    > **Eliminar sesión** elimina todos los datos almacenados de la sesión seleccionada,
    > aunque la eliminación de una sesión cerrada **no** elimina guías de plan implementadas previamente.
    > Si elimina una sesión con guías de plan implementadas, no puede usar QTA para revertir.
    > En su lugar, busque guías de plan mediante la tabla del sistema [sys.plan_guides](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md) y elimine manualmente mediante [sp_control_plan_guide](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md).
  
3. El punto de entrada de una nueva sesión es el paso **Recopilación de datos**.

    > [!NOTE]
    > El botón **Sesiones** devuelve a la página de **administración de sesiones** y deja la sesión activa tal cual.

    Este paso tiene tres subpasos:

    1. **Recopilación de datos de línea de base** solicita al usuario que ejecute el ciclo de carga de trabajo representativo para que el Almacén de consultas pueda recopilar una línea de base. Una vez completada esa carga de trabajo, active **Ejecución de la carga de trabajo finalizada** y seleccione **Siguiente**.

        > [!NOTE]
        > La ventana de QTA se puede cerrar mientras la carga de trabajo se ejecuta. Al volver a la sesión que permanece en estado activo en un momento posterior, se reanuda desde el mismo paso donde se había quedado. 

        ![QTA, paso 2 subpaso 1](../../relational-databases/performance/media/qta-step2-substep1.png "QTA, paso 2 subpaso 1")

    2. **Actualizar base de datos** le pide permiso para actualizar el nivel de compatibilidad de la base de datos al destino deseado. Para continuar con el siguiente subpaso, seleccione **Sí**.

        ![QTA, paso 2 subpaso 2: Actualización del nivel de compatibilidad de la base de datos](../../relational-databases/performance/media/qta-step2-substep2-prompt.png "QTA, paso 2 subpaso 2: Actualización del nivel de compatibilidad de la base de datos")

        La siguiente página confirma que el nivel de compatibilidad de la base de datos se ha actualizado correctamente.

        ![QTA, paso 2 subpaso 2](../../relational-databases/performance/media/qta-step2-substep2.png "QTA, paso 2 subpaso 2")

    3. **Recopilación de datos observados** solicita al usuario que vuelva a ejecutar el ciclo de carga de trabajo representativo para que el Almacén de consultas pueda recopilar una línea de base comparativa que se use para buscar oportunidades de optimización. Mientras se ejecuta la carga de trabajo, use el botón **Actualizar** para seguir actualizando la lista de consultas con regresión, si se ha detectado alguna. Cambie el valor **Queries to show** (Consultas que se van a mostrar) para limitar el número de consultas que aparecen. El orden de la lista se ve afectado por **Métrica** (Duración o Tiempo de CPU) y **Agregación** (Promedio es el valor predeterminado). Seleccione también cuántas **consultas se van a mostrar**. Una vez completada esa carga de trabajo, active **Ejecución de la carga de trabajo finalizada** y seleccione **Siguiente**.

        ![QTA, paso 2 subpaso 3](../../relational-databases/performance/media/qta-step2-substep3.png "QTA, paso 2 subpaso 3")

        La lista contiene la información siguiente:
        - **Id. de consulta** 
        - **Texto de consulta**: instrucción de [!INCLUDE[tsql](../../includes/tsql-md.md)] que se puede expandir al seleccionar el botón **...** .
        - **Ejecuciones**: muestra el número de ejecuciones de esa consulta para la colección completa de cargas de trabajo.
        - **Métrica de línea de base**: métrica seleccionada (Duración o Tiempo de CPU) en milisegundos para la colección de datos de línea de base antes de la actualización de compatibilidad de base de datos.
        - **Métrica observada**: métrica seleccionada (Duración o Tiempo de CPU) en milisegundos para la colección de datos después de la actualización de compatibilidad de base de datos.
        - **% cambio**: cambio porcentual de la métrica seleccionada entre el antes y el después del estado de actualización de compatibilidad de base de datos. Un número negativo representa la cantidad de regresión medida de la consulta.
        - **Optimizable**: *True* o *False* en función de si la consulta es apta para la experimentación.

4. **Ver análisis** permite la selección de las consultas con las que se va a experimentar y buscar oportunidades de optimización. El valor de **consultas que se van a mostrar** se convierte en el ámbito de las consultas aptas con las que se va a experimentar. Una vez activadas las consultas deseadas, seleccione **Siguiente** para iniciar la experimentación.

    > [!NOTE]
    > Las consultas con Optimizable = Falso no se pueden seleccionar para experimentación.

    > [!IMPORTANT]
    > Un símbolo del sistema advierte de que una vez que QTA pasa a la fase de experimentación, no es posible volver a la página Ver análisis.   
    > Si no selecciona todas las consultas aptas antes de pasar a la fase de experimentación, debe crear una nueva sesión posteriormente y repetir el flujo de trabajo. Esto requiere el restablecimiento del nivel de compatibilidad de la base de datos al valor anterior.

    ![QTA, paso 3](../../relational-databases/performance/media/qta-step3.png "QTA, paso 3")

5. **Visualización de resultados** permite la selección de las consultas que van a implementar la optimización propuesta como guía de plan. 

    La lista contiene la información siguiente:
    - **Id. de consulta**
    - **Texto de consulta**: instrucción de [!INCLUDE[tsql](../../includes/tsql-md.md)] que se puede expandir al seleccionar el botón **...** .
    - **Estado**: muestra el estado de experimentación actual de la consulta.
    - **Métrica de línea de base**: métrica seleccionada (Duración o Tiempo de CPU) en milisegundos para la consulta tal como se ha ejecutado en el **Paso 2: subpaso 3**, que representa la consulta con regresión después de la actualización de compatibilidad de base de datos.
    - **Métrica observada**: métrica seleccionada (Duración o Tiempo de CPU) en milisegundos para la consulta después de la experimentación, para una optimización propuesta lo suficientemente buena.
    - **% cambio**: cambio porcentual de la métrica seleccionada entre el antes y el después del estado de experimentación, que representa la cantidad de mejora medida para la consulta con la optimización propuesta.
    - **Opción de consulta**: vínculo a la sugerencia propuesta que mejora la métrica de ejecución de consulta.
    - **Puede implementar**: *True* o *False* en función de si la optimización de consultas propuesta se puede implementar como guía de plan.

    ![QTA, paso 4](../../relational-databases/performance/media/qta-step4.png "QTA, paso 4")

6. **Comprobación** muestra el estado de implementación de consultas previamente seleccionadas de esta sesión. La lista de esta página difiere de la página anterior al cambiar la columna **Puede implementar** por **Puede revertir**. Esta columna puede ser *Verdadero* o *Falso* en función de si se puede revertir la optimización de consultas implementada y se puede quitar su guía de plan.

    ![QTA, paso 5](../../relational-databases/performance/media/qta-step5.png "QTA, paso 5")

    Si más adelante es necesario revertir en una optimización propuesta, seleccione la consulta correspondiente y, después, **Revertir**. La guía de plan de consulta se quita y la lista se actualiza para quitar la consulta revertida. Tenga en cuenta que en la siguiente imagen se ha quitado la consulta 8.

    ![QTA paso 5: Reversión](../../relational-databases/performance/media/qta-step5-rollback.png "QTA paso 5: Reversión")

    > [!NOTE]
    > La eliminación de una sesión cerrada **no** elimina guías de plan implementadas previamente.
    > Si elimina una sesión con guías de plan implementadas, no puede usar QTA para revertir.
    > En su lugar, busque guías de plan mediante la tabla del sistema [sys.plan_guides](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md) y elimine manualmente mediante [sp_control_plan_guide](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md).
  
## <a name="permissions"></a>Permisos

Requiere la pertenencia al rol **db_owner**.
  
## <a name="see-also"></a>Consulte también

- [Actualizaciones del motor de base de datos y niveles de compatibilidad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades)
- [Herramientas de optimización y supervisión del rendimiento](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)
- [Supervisar el rendimiento mediante el Almacén de consultas](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [Cambiar el modo de compatibilidad de la base de datos y usar el almacén de consultas](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md)
- [Marcas de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)
- [Sugerencias de consulta USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint)
- [Estimación de cardinalidad](../../relational-databases/performance/cardinality-estimation-sql-server.md)
- [Ajuste automático](../../relational-databases/automatic-tuning/automatic-tuning.md)   
- [Uso del Asistente para la optimización de consultas de SQL Server](/learn/modules/use-sql-server-query-tuning-assistant/)