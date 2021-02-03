---
description: Administración de la retención de datos históricos en las tablas temporales con versiones del sistema
title: Administración de los datos históricos en las tablas temporales con versiones del sistema
ms.custom: seo-lt-2019
ms.date: 05/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 7925ebef-cdb1-4cfe-b660-a8604b9d2153
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2e37f8234a1b8ee2ab72c76a423ea72ac9d3f14a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235824"
---
# <a name="manage-retention-of-historical-data-in-system-versioned-temporal-tables"></a>Administración de la retención de datos históricos en las tablas temporales con versiones del sistema


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Con las tablas temporales con versiones del sistema, la tabla de historial puede aumentar el tamaño de la base de datos más que las tablas normales, especialmente en las siguientes condiciones:

- Retención de datos históricos durante un largo período
- Disponibilidad de una actualización o eliminación del modelo de modificación de gran cantidad de datos

Una tabla de historial de gran tamaño y creciente puede ser un problema debido a los costos de almacenamiento puro y a la imposición de un impuesto de rendimiento sobre las consultas temporales. Por lo tanto, al desarrollar una directiva de retención de datos para administrar datos en la tabla de historial es un aspecto importante de la planeación y la administración del ciclo de vida de cada tabla temporal.

## <a name="data-retention-management-for-history-table"></a>Administración de la retención de datos para la tabla de historial

La administración de la retención de datos de la tabla temporal empieza por determinar el período de retención requerido para cada tabla temporal. La directiva de retención, en la mayoría de los casos, debe considerarse parte de la lógica de negocios de la aplicación mediante las tablas temporales. Por ejemplo, las aplicaciones de datos de auditoría y escenarios de viaje en el tiempo tienen requisitos firmes en términos de cuánto tiempo deben estar disponibles los datos históricos para la consulta en línea.

Una vez que determine el período de retención de datos, el siguiente paso es desarrollar un plan para administrar los datos históricos, cómo y dónde almacenar los datos históricos y cómo eliminar los datos históricos que son anteriores a los requisitos de retención. Los cuatro enfoques siguientes están disponibles para administrar los datos históricos en la tabla temporal de historial:

- [Stretch Database](#using-stretch-database-approach)
- [Partición de tabla](#using-table-partitioning-approach)
- [Script de limpieza personalizado](#using-custom-cleanup-script-approach)
- [Directiva de retención](#using-temporal-history-retention-policy-approach)

 Con cada uno de estos enfoques, la lógica para la migración o limpieza de datos del historial se basa en la columna que se corresponde con el final del período en la tabla actual. El final del valor del período para cada fila determina el momento en el que la versión de fila se "cierra", es decir, cuando llega a la tabla de historial. Por ejemplo, la condición `SysEndTime < DATEADD (DAYS, -30, SYSUTCDATETIME ())` especifica que esos datos históricos anteriores a un mes tienen quitarse o extraerse de la tabla de historial.

> [!NOTE]
> Los ejemplos de este tema usan este [ejemplo de tabla temporal](creating-a-system-versioned-temporal-table.md).

## <a name="using-stretch-database-approach"></a>Uso del enfoque de Stretch Database

> [!NOTE]
> El uso del enfoque de Stretch Database solo se aplica a [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] y no se aplica a [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

[Stretch Database](../../sql-server/stretch-database/stretch-database.md) en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] migra los datos históricos de forma transparente a Azure. Para obtener seguridad adicional, puede cifrar los datos en movimiento con la característica [Always Encrypted](../security/encryption/always-encrypted-database-engine.md) de SQL Server. Además, puede usar [Seguridad de nivel de fila](../../relational-databases/security/row-level-security.md) y otras características de seguridad avanzadas de SQL Server con Temporal y Stretch Database para proteger los datos.

Con el enfoque de Stretch Database, puede ajustar algunas o todas las tablas de historial temporales en Azure y SQL Server moverá de forma silenciosa los datos históricos a Azure. La habilitación del ajuste de una tabla de historial no cambia la forma en la que interactúa con la tabla temporal en términos de modificación de datos y consultas temporales.

- **Ajuste de la tabla de historial completa:** configure Stretch Database para la tabla de historial completa si el escenario principal es la auditoría de datos en un entorno con cambios frecuentes de datos y una consulta relativamente poco frecuente sobre los datos históricos. En otras palabras, puede utilizar este enfoque si el rendimiento de las consultas temporales no es importante. En este caso, la rentabilidad proporcionada por Azure puede resultar atractiva. Cuando ajuste la tabla de historial completa, puede usar el Asistente de Stretch o Transact-SQL. A continuación aparecen ejemplos de ambos.

- **Ajuste de una parte de la tabla de historial:** configure Stretch Database para una sola parte de la tabla de historial para mejorar el rendimiento si el escenario principal implica principalmente consultar datos históricos recientes, pero quiere conservar la opción de consultar datos históricos anteriores cuando sea necesario mientras se almacenan esos datos de forma remota a un menor costo. Con Transact-SQL puede hacerlo si especifica una función de predicado para seleccionar las filas que se van a migrar de la tabla de historial en lugar de migrar todas las filas. Cuando se trabaja con las tablas temporales, normalmente tiene sentido mover los datos en función de la condición de tiempo (es decir, según la edad de la versión de fila en la tabla de historial).

  Utilice una función de predicado determinista para mantener una parte del historial en la misma base de datos con los datos actuales, mientras el resto se migra a Azure. Para ver ejemplos y limitaciones, consulte [Selección de las filas que se van a migrar mediante una función de filtro (Stretch Database)](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md). Puesto que las funciones no determinista no son válidas, si desea transferir datos de historial al estilo de ventana deslizante, necesitaría alterar regularmente la definición de las funciones de predicado en línea de manera que la ventana de filas que mantenga localmente sea constante en términos de edad. La ventana deslizante le permite mover constantemente datos históricos con una antigüedad superior a un mes a Azure. A continuación, aparece un ejemplo de este enfoque.

> [!NOTE]
> Stretch Database migra los datos a Azure. Por lo tanto, necesita una cuenta de Azure y una suscripción para la facturación. Para obtener una cuenta de Azure de evaluación gratuita, haga clic en [Evaluación gratuita de un mes](https://azure.microsoft.com/pricing/free-trial/).

Puede configurar una tabla de historial temporal para Stretch con el Asistente de Stretch o Transact-SQL, y puede habilitar el ajuste para una tabla de historial temporal mientras la versión del sistema se establece en **ON**. El ajuste de la tabla actual no está permitido porque no tiene sentido aplicarlo.

### <a name="using-the-stretch-wizard-to-stretch-the-entire-history-table"></a>Uso del Asistente de Stretch para ajustar la tabla de historial completo

El método más sencillo para los principiantes es usar el Asistente de Stretch para habilitar el ajuste para la base de datos completa y, después, seleccionar la tabla de historial temporal en el Asistente de Stretch (en este ejemplo se supone que ha configurado la tabla de departamento como una tabla temporal con versiones del sistema en una base de datos vacía). En [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], no puede hacer clic con el botón derecho en la propia tabla de historial temporal y hacer clic en Stretch.

1. Haga clic con el botón derecho en la base de datos y seleccione **Tareas**, seleccione **Stretch** y, después, haga clic en **Habilitar** para iniciar el asistente.
2. En la ventana **Seleccionar tablas** , seleccione la casilla de la tabla de historial temporal y haga clic en Siguiente.

    ![Selección de la tabla de historial en la página Seleccionar tablas](../../relational-databases/tables/media/stretch-wizard-2-for-temporal.png "Selección de la tabla de historial en la página Seleccionar tablas")
3. En la ventana **Configuración de Azure** , proporcione las credenciales de inicio de sesión. Inicie sesión en Microsoft Azure o regístrese para obtener una cuenta. Seleccione la suscripción que va a usar y la región de Azure. Después, cree un nuevo servidor o seleccione un servidor existente. Haga clic en **Next**.

    ![Creación de un nuevo Asistente para Stretch Database de servidor de Azure](../../relational-databases/tables/media/stretch-wizard-4.png "Creación de un nuevo Asistente para Stretch Server de servidor de Azure")
4. En la ventana **Credenciales de seguridad** , proporcione una contraseña para la clave maestra de base de datos para proteger sus credenciales de base de datos de SQL Server de origen y haga clic en Siguiente.

    ![Página de credenciales seguras del Asistente para Stretch Database](../../relational-databases/tables/media/stretch-wizard-6.png "Página de credenciales seguras del Asistente para Stretch Database")
5. En la ventana **Seleccionar dirección IP**, proporcione el intervalo de direcciones IP para SQL Server para que el servidor de Azure se comunique con SQL Server (si selecciona un servidor existente para el que ya existe una regla de firewall, simplemente haga clic en Siguiente aquí para usar la regla de firewall existente). Haga clic en **Siguiente** y, después, en **Finalizar** para habilitar Stretch Database y ajustar la tabla de historial temporal.

    ![Página Seleccionar dirección IP del Asistente para Stretch Database](../../relational-databases/tables/media/stretch-wizard-7.png "Página Seleccionar dirección IP del Asistente para Stretch Database")
6. Cuando se complete el asistente, compruebe que se haya habilitado el ajuste correctamente para la base de datos. Vea los iconos del Explorador de objetos que indican que se ha ajustado la base de datos.

> [!NOTE]
> Si se produce un error en Habilitar base de datos para Stretch, revise el registro de errores. Un error común consiste en configurar incorrectamente la regla de firewall.

Consulte también:

- [Habilitación de Stretch Database para una base de datos](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md)
- [Ejecución del asistente para habilitar la base de datos para Stretch](../../sql-server/stretch-database/get-started-by-running-the-enable-database-for-stretch-wizard.md)
- [Habilitación de Stretch Database para una tabla](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)

### <a name="using-transact-sql-to-stretch-the-entire-history-table"></a>Uso de Transact-SQL para ajustar la tabla de historial completo

También puede usar Transact-SQL para habilitar Stretch en el servidor local y [Habilitación de Stretch Database para una base de datos](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md). Después, puede [usar Transact-SQL para habilitar Stretch Database en una tabla](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md). Con una base de datos habilitada previamente para Stretch Database, ejecute el siguiente script de Transact-SQL para ajustar una tabla de historial temporal con versiones del sistema existente:

```sql
ALTER TABLE <history table name>
SET (REMOTE_DATA_ARCHIVE = ON (MIGRATION_STATE = OUTBOUND));
```

### <a name="using-transact-sql-to-stretch-a-portion-of-the-history-table"></a>Uso de Transact-SQL para ajustar una parte de la tabla de historial

Para ajustar solo una parte de la tabla de historial primero debe crear una [función de predicado en línea](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md). En este ejemplo, supongamos que ha configurado la función de predicado en línea por primera vez el 1 de diciembre de 2015 y quiere ajustar en Azure todas las fechas de historial anteriores al 1 de noviembre de 2015. Para lograr esto, empiece por crear la siguiente función:

```sql
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20151101(@systemEndTime datetime2)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN SELECT 1 AS is_eligible
  WHERE @systemEndTime < CONVERT(datetime2, '2015-11-01T00:00:00', 101) ;
```

a continuación, use el siguiente script para agregar el predicado de filtro a la tabla de historial y establecer el estado de la migración en OUTBOUND para permitir la migración de datos basado en predicado para la tabla de historial.

```sql
ALTER TABLE <history table name>
SET (
      REMOTE_DATA_ARCHIVE = ON
        (
          FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20151101 (SysEndTime)
            , MIGRATION_STATE = OUTBOUND
        )
    )
;
```

Para mantener una ventana deslizante, necesita que la función de predicado sea precisa cada día (es decir, cambiar la condición de fila de filtrado cada día en un día). El siguiente script es el script que necesitaría ejecutar el 2 de diciembre de 2015:

```sql
BEGIN TRAN
/*(1) Create new predicate function definition */
  CREATE FUNCTION dbo.fn_StretchBySystemEndTime20151102(@systemEndTime datetime2)
   RETURNS TABLE
    WITH SCHEMABINDING
      AS
        RETURN SELECT 1 AS is_eligible
          WHERE @systemEndTime < CONVERT(datetime2,'2015-11-02T00:00:00', 101)
  GO
 
/*(2) Set the new function as filter predicate */
  ALTER TABLE <history table name>
    SET
      (
        REMOTE_DATA_ARCHIVE = ON
          (
            FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20151102(SysEndTime),
              MIGRATION_STATE = OUTBOUND
          )
      )
COMMIT ;
```

utilice el Agente SQL Server o algún otro mecanismo de programación para asegurarse de que la definición de la función de predicado es válida todo el tiempo.

## <a name="using-table-partitioning-approach"></a>Uso del enfoque de la partición de tabla

La[partición de tabla](../partitions/create-partitioned-tables-and-indexes.md) puede hacer que las tablas grandes sean más escalables y fáciles de administrar. Con el enfoque de partición de tabla, puede usar particiones de tabla de historial para implementar la limpieza de datos personalizada o el archivado sin conexión según una condición de tiempo. La partición de tabla también le proporcionará ventajas de rendimiento cuando se realicen consultas de tablas temporales en un subconjunto de historial de datos mediante la eliminación de una partición.

Con la partición de tabla, puede implementar un enfoque de ventana deslizante para extraer la parte más antigua de los datos históricos de la tabla de historial y mantener el tamaño de la parte retenida constante en términos de edad: manteniendo los datos en la tabla de historial igual que en el período de retención requerido. Se admite la operación de conmutación de datos fuera de la tabla de historial cuando SYSTEM_VERSIONING está activado, lo que significa que puede limpiar una parte de los datos del historial sin introducir una ventana de mantenimiento o bloquear las cargas de trabajo normales.

> [!NOTE]
> Para realizar la conmutación de particiones, el índice agrupado en la tabla de historial debe alinearse con el esquema de partición (debe contener SysEndTime). La tabla de historial predeterminada creada por el sistema contiene un índice agrupado que incluye las columnas SysEndTime y SysStartTime, que es óptimo para la creación de particiones, la inserción de nuevos datos de historial y la típica consulta temporal. Para obtener más información, consulte [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

Un enfoque de ventana deslizante tiene dos conjuntos de tareas que tiene que realizar:

- Una tarea de configuración de partición
- Tareas periódicas de mantenimiento de partición

En la ilustración, supongamos que se quieren mantener datos históricos durante seis meses y mantener todos los meses de datos en una partición independiente. Además, supongamos que se ha activado el control de versiones del sistema en septiembre de 2015.

Una tarea de configuración de particiones crea la configuración inicial de partición de la tabla de historial. En este ejemplo, crearíamos las mismas particiones de número que el tamaño de la ventana deslizante, en meses, más una partición vacía adicional preparada previamente (se explica a continuación). Esta configuración garantiza que el sistema podrá almacenar correctamente los datos nuevos cuando iniciemos la tarea de mantenimiento periódico de la partición la primera vez y garantiza que nunca dividiremos las particiones con datos para evitar movimientos de datos valiosos. Debe realizar esta tarea mediante Transact-SQL con el siguiente script de ejemplo.

En la siguiente imagen se muestra la configuración inicial de la creación de particiones para mantener 6 meses de datos.

![Diagrama que muestra la configuración inicial de la creación de particiones de particiones para mantener seis meses de datos.](../../relational-databases/tables/media/partitioning.png "Creación de particiones")

> [!NOTE]
> Vea las consideraciones de rendimiento con las particiones de tabla siguientes para las implicaciones de rendimiento de uso de la opción RANGE LEFT frente a la opción RANGE RIGHT al configurar la creación de particiones.

La primera y última partición están "abiertas" en los límites inferior y superior respectivamente para asegurarse de que cada fila nueva tiene la partición de destino con independencia del valor de la columna de partición. A medida que pasa el tiempo, las nuevas filas de la tabla del historial se dirigirán a particiones superiores. Cuando se llene la partición 6ª, se habrá alcanzado el período de retención de destino. Este es el momento en el que se debe iniciar la tarea de mantenimiento periódico de la partición por primera vez (debe programarse para ejecutarse periódicamente; una vez al mes en este ejemplo).

La imagen siguiente muestra las tareas de mantenimiento periódico de la partición (vea los pasos detallados a continuación).

![Diagrama que muestra las tareas periódicas de mantenimiento de partición.](../../relational-databases/tables/media/partitioning2.png "Partitioning2")

Los pasos detallados para las tareas de mantenimiento periódico de la partición son:

1. SWITCH OUT: permite crear una tabla de almacenamiento provisional y, después, cambiar una partición entre la tabla de historial y la tabla de almacenamiento provisional mediante la instrucción [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) con el argumento SWITCH PARTITION (vea el ejemplo C. Cambio de particiones entre tablas).

    ```sql
    ALTER TABLE <history table> SWITCH PARTITION 1 TO <staging table>
    ```

    Después del cambio de partición, puede archivar opcionalmente los datos de la tabla de almacenamiento provisional y, a continuación, quitar o truncar la tabla de almacenamiento provisional para que estén listos para la próxima vez que necesite realizar esta tarea de mantenimiento periódico de la partición.
2. MERGE RANGE: permite combinar la partición 1 vacía con la partición 2 mediante [ALTER PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-function-transact-sql.md) con la opción MERGE RANGE (vea el ejemplo B). Al quitar el límite inferior con esta función, se combina eficazmente la partición vacía 1 con la partición anterior 2 para formar una nueva partición 1. Las demás particiones también cambian de forma efectiva sus ordinales.
3. SPLIT RANGE: permite crear una partición 7 vacía mediante [ALTER PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-function-transact-sql.md) con la opción SPLIT RANGE (vea el ejemplo A). Al agregar un nuevo límite superior mediante esta función, crea eficazmente una partición independiente para el próximo mes.

### <a name="use-transact-sql-to-create-partitions-on-history-table"></a>Uso de Transact-SQL para crear particiones en la tabla de historial

Utilice el script de Transact-SQL en la ventana de código siguiente para crear la función de partición y el esquema de partición, y volver a crear el índice agrupado para que la partición se alinee con las particiones o el esquema de partición. En este ejemplo, crearemos un enfoque de ventana deslizante de seis meses con particiones mensuales a partir de septiembre de 2015.

```sql
BEGIN TRANSACTION
/*Create partition function*/
    CREATE PARTITION FUNCTION [fn_Partition_DepartmentHistory_By_SysEndTime] (datetime2(7))
        AS RANGE LEFT FOR VALUES
          (
            N'2015-09-30T23:59:59.999'
          , N'2015-10-31T23:59:59.999'
          , N'2015-11-30T23:59:59.999'
          , N'2015-12-31T23:59:59.999'
          , N'2016-01-31T23:59:59.999'
          , N'2016-02-29T23:59:59.999'
          )
/*Create partition scheme*/
    CREATE PARTITION SCHEME [sch_Partition_DepartmentHistory_By_SysEndTime]
        AS PARTITION [fn_Partition_DepartmentHistory_By_SysEndTime]
            TO ([PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY])
/*Re-create index to be partition-aligned with the partitioning schema*/
    CREATE CLUSTERED INDEX [ix_DepartmentHistory] ON [dbo].[DepartmentHistory]
        (
            [SysEndTime] ASC
          , [SysStartTime] ASC
        )  
    WITH
        (
            PAD_INDEX = OFF
          , STATISTICS_NORECOMPUTE = OFF
          , SORT_IN_TEMPDB = OFF
          , DROP_EXISTING = ON
          , ONLINE = OFF
          , ALLOW_ROW_LOCKS = ON
          , ALLOW_PAGE_LOCKS = ON
          , DATA_COMPRESSION = PAGE
        )
    ON [sch_Partition_DepartmentHistory_By_SysEndTime] ([SysEndTime])

COMMIT TRANSACTION;
```

### <a name="using-transact-sql-to-maintain-partitions-in-sliding-window-scenario"></a>Uso de Transact-SQL para mantener particiones en el escenario de ventana deslizante

Utilice el script de Transact-SQL en la siguiente ventana de código siguiente para mantener las particiones en el escenario de ventana deslizante. En este ejemplo, se conmutará la partición de septiembre de 2015 con la opción MERGE RANGE y, a continuación, se agregará una nueva partición de marzo de 2016 con la opción SPLIT RANGE.

```sql
BEGIN TRANSACTION
/*(1) Create staging table */
    CREATE TABLE [dbo].[staging_DepartmentHistory_September_2015]
        (
            [DeptID] [int] NOT NULL
          , [DeptName] [varchar](50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL
          , [ManagerID] [int] NULL
          , [ParentDeptID] [int] NULL
          , [SysStartTime] [datetime2](7) NOT NULL
          , [SysEndTime] [datetime2](7) NOT NULL
        ) ON [PRIMARY]
    WITH
        (
            DATA_COMPRESSION = PAGE
        )
/*(2) Create index on the same filegroups as the partition that will be switched out*/
    CREATE CLUSTERED INDEX [ix_staging_DepartmentHistory_September_2015]
        ON [dbo].[staging_DepartmentHistory_September_2015]
        (
            [SysEndTime] ASC
          , [SysStartTime] ASC
        )
    WITH
        (
            PAD_INDEX = OFF
          , SORT_IN_TEMPDB = OFF
          , DROP_EXISTING = OFF
          , ONLINE = OFF
          , ALLOW_ROW_LOCKS = ON
          , ALLOW_PAGE_LOCKS = ON
        )
    ON [PRIMARY]
/*(3) Create constraints matching the partition that will be switched out*/
    ALTER TABLE [dbo].[staging_DepartmentHistory_September_2015] WITH CHECK
        ADD CONSTRAINT [chk_staging_DepartmentHistory_September_2015_partition_1]
            CHECK ([SysEndTime]<=N'2015-09-30T23:59:59.999')
    ALTER TABLE [dbo].[staging_DepartmentHistory_September_2015]
        CHECK CONSTRAINT [chk_staging_DepartmentHistory_September_2015_partition_1]
/*(4) Switch partition to staging table*/
    ALTER TABLE [dbo].[DepartmentHistory]
        SWITCH PARTITION 1 TO [dbo].[staging_DepartmentHistory_September_2015]
        WITH (WAIT_AT_LOW_PRIORITY (MAX_DURATION = 0 MINUTES, ABORT_AFTER_WAIT = NONE))
/*(5) [Commented out] Optionally archive the data and drop staging table
      INSERT INTO [ArchiveDB].[dbo].[DepartmentHistory]
      SELECT * FROM [dbo].[staging_DepartmentHistory_September_2015];
      DROP TABLE [dbo].[staging_DepartmentHIstory_September_2015];
*/
/*(6) merge range to move lower boundary one month ahead*/
    ALTER PARTITION FUNCTION [fn_Partition_DepartmentHistory_By_SysEndTime]()
        MERGE RANGE(N'2015-09-30T23:59:59.999')
/*(7) Create new empty partition for "April and after" by creating new boundary point and specifying NEXT USED file group*/
    ALTER PARTITION SCHEME [sch_Partition_DepartmentHistory_By_SysEndTime] NEXT USED [PRIMARY]
        ALTER PARTITION FUNCTION [fn_Partition_DepartmentHistory_By_SysEndTime]() SPLIT RANGE(N'2016-03-31T23:59:59.999')
COMMIT TRANSACTION
```

Puede modificar ligeramente el script anterior y usarlo en el proceso normal de mantenimiento mensual:

1. En el paso (1), cree una tabla de almacenamiento provisional para el mes que quiera quitar (octubre sería el siguiente en nuestro ejemplo).
2. En el paso (3), cree la restricción y compruebe que coincide con el mes de datos que quiere quitar: `[SysEndTime]<=N'2015-10-31T23:59:59.999'` para la partición de octubre.
3. En el paso (4), cambie la partición 1 a la tabla de almacenamiento provisional recién creada.
4. En el paso (6), modifique la función de partición mediante la combinación del límite inferior: `MERGE RANGE(N'2015-10-31T23:59:59.999'` después de extraer los datos de octubre.
5. En el paso (7), divida la función de partición mediante la creación del límite superior: `SPLIT RANGE (N'2016-04-30T23:59:59.999'` después de extraer los datos de octubre.

Sin embargo, la mejor solución sería ejecutar regularmente un script de Transact-SQL genérico que fuese capaz de llevar a cabo la acción apropiada cada mes sin modificar el script. Es posible generalizar el script anterior para que actúe sobre los parámetros proporcionados (límite inferior que debe combinarse y límite nuevo que se creará con la división de particiones). Para evitar la creación de una tabla de almacenamiento provisional cada mes, puede crear una con antelación y volver a usarla cambiando la restricción de comprobación para que coincida con la partición que se conmutará. Eche un vistazo a las páginas siguientes para obtener ideas sobre [cómo la ventana deslizante puede automatizarse al completo](/previous-versions/sql/sql-server-2005/administrator/aa964122(v=sql.90)) mediante un script de Transact-SQL.

### <a name="performance-considerations-with-table-partitioning"></a>Consideraciones de rendimiento con las particiones de tabla

Es importante realizar las operaciones MERGE y SPLIT RANGE para evitar cualquier movimiento de datos, ya que este puede provocar una sobrecarga considerable del rendimiento. Para obtener más información, vea [Modificar una función de partición](../../relational-databases/partitions/modify-a-partition-function.md). Conseguirá esto usando la opción RANGE LEFT en lugar de la opción RANGE RIGHT cuando aplique [CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md).

Primero se va a explicar visualmente el significado de las opciones RANGE LEFT y RANGE RIGHT:

![Diagrama que muestra las opciones RANGE LEFT y RANGE RIGHT.](../../relational-databases/tables/media/partitioning3.png "Partitioning3")

Si se define una función de partición como RANGE LEFT, los valores especificados son los límites superiores de las particiones. Cuando utilice la opción RANGE RIGHT, los valores especificados son los límites inferiores de las particiones. Cuando utilice la operación MERGE RANGE para quitar un límite de la definición de la función de partición, la implementación subyacente también quita la partición que contiene el límite. Si esa partición no está vacía, los datos se moverán a la partición que resulta de la operación MERGE RANGE.

En el escenario de ventana deslizante, siempre quitamos el límite inferior de la partición.

- Caso RANGE LEFT: en el caso RANGE LEFT, el límite inferior de la partición pertenece a la partición 1, que está vacía (después de conmutar la partición), por lo que MERGE RANGE no causará ningún movimiento de datos.
- Caso RANGE RIGHT: en el caso RANGE RIGHT, el límite inferior de la partición pertenece a la partición 2, que no está vacía ya que se ha supuesto que la partición 1 se había vaciado mediante la conmutación. En este caso, la opción MERGE RANGE provocará un movimiento de datos (los datos de la partición 2 se moverán a la partición 1). Para evitar esto, la opción RANGE RIGHT del escenario de ventana deslizante debe tener la partición 1, que siempre está vacía. Esto significa que si usamos la opción RANGE RIGHT, debemos crear y mantener una partición adicional en comparación con el caso de RANGE LEFT.

**Conclusión**: el uso de RANGE LEFT en la partición deslizante es mucho más simple para la administración de la partición y evita el movimiento de datos. Sin embargo, la definición de los límites de partición con la opción RANGE RIGHT es un poco más simple, ya que no tiene que tratar con problemas de marca de tiempo de fecha y hora.

## <a name="using-custom-cleanup-script-approach"></a>Uso del enfoque de script de limpieza personalizado

En los casos en los que los enfoques de particiones de tabla y Stretch Database no sean opciones viables, el tercer enfoque consiste en eliminar los datos de la tabla de historial con un script de limpieza personalizado. La eliminación de los datos de la tabla de historial es posible solo cuando aplica **SYSTEM_VERSIONING = OFF**. Para evitar la incoherencia de datos, realice la limpieza durante la ventana de mantenimiento (cuando las cargas de trabajo que modifican datos no están activas) o dentro de una transacción (bloqueando de forma efectiva otras cargas de trabajo). Esta operación requiere el permiso de **CONTROL** sobre tablas de historial y actuales.

Para bloquear mínimamente las aplicaciones normales y las consultas de usuario, elimine los datos en fragmentos más pequeños con un retraso al realizar el script de limpieza dentro de una transacción. Aunque no hay ningún tamaño óptimo para la eliminación de cada fragmento de datos en todos los escenarios, la eliminación de más de 10 000 filas en una sola transacción puede suponer un impacto significativo.

La lógica de limpieza es la misma para todas las tablas temporales, por lo que se puede automatizar de forma relativamente sencilla a través de un procedimiento almacenado genérico que puede programar para que se ejecute periódicamente para cada tabla temporal para la que desee limitar el historial de datos.

El siguiente diagrama muestra cómo debe organizarse la lógica de limpieza para una tabla única para reducir el impacto en las cargas de trabajo en ejecución.

![Diagrama que muestra cómo debe organizarse la lógica de limpieza de una tabla única para reducir el impacto en las cargas de trabajo en ejecución.](../../relational-databases/tables/media/customcleanupscriptdiagram.png "CustomCleanUpScriptDiagram")

Estas son algunas directrices de alto nivel para implementar el proceso. Programe la lógica de limpieza para que se ejecute todos los días y realice la iteración sobre todas las tablas temporales que necesitan la limpieza de datos. Use el Agente SQL Server u otra herramienta para programar este proceso:

- Elimine los datos históricos en cada tabla temporal empezando por las filas más antiguas hasta las más recientes en varias iteraciones en pequeños fragmentos y evite la eliminación de todas las filas en una sola transacción tal como se muestra en la imagen anterior.
- Implemente cada iteración como una invocación del procedimiento almacenado genérico que quita una parte de datos de la tabla de historial (vea el ejemplo de código siguiente para este procedimiento).
- Calcule el número de filas que debe eliminar para una tabla temporal individual cada vez que se invoca el proceso. Según esto y el número de iteraciones que quiera tener, determine los puntos de división dinámicos para cada invocación del procedimiento.
- Planifique un período de retraso entre las iteraciones para una tabla única para reducir el impacto en las aplicaciones que dispongan de acceso a la tabla temporal.

Un procedimiento almacenado que permita eliminar los datos de una tabla temporal única podría ser similar al fragmento de código siguiente (revise este código con cuidado y ajústelo antes de aplicarlo a su entorno):

```sql
DROP PROCEDURE IF EXISTS sp_CleanupHistoryData;
GO

CREATE PROCEDURE sp_CleanupHistoryData
        @temporalTableSchema sysname
      , @temporalTableName sysname
      , @cleanupOlderThanDate datetime2
AS
    DECLARE @disableVersioningScript nvarchar(max) = '';
    DECLARE @deleteHistoryDataScript nvarchar(max) = '';
    DECLARE @enableVersioningScript nvarchar(max) = '';

DECLARE @historyTableName sysname
DECLARE @historyTableSchema sysname
DECLARE @periodColumnName sysname

/*Generate script to discover history table name and end of period column for given temporal table name*/
EXECUTE sp_executesql
    N'SELECT @hst_tbl_nm = t2.name, @hst_sch_nm = s.name, @period_col_nm = c.name
        FROM sys.tables t1
            JOIN sys.tables t2 on t1.history_table_id = t2.object_id
        JOIN sys.schemas s on t2.schema_id = s.schema_id
            JOIN sys.periods p on p.object_id = t1.object_id
           JOIN sys.columns c on p.end_column_id = c.column_id and c.object_id = t1.object_id
                  WHERE
                 t1.name = @tblName and s.name = @schName'
                , N'@tblName sysname
                , @schName sysname
                , @hst_tbl_nm sysname OUTPUT
                , @hst_sch_nm sysname OUTPUT
                , @period_col_nm sysname OUTPUT'
                , @tblName = @temporalTableName
                , @schName = @temporalTableSchema
                , @hst_tbl_nm = @historyTableName OUTPUT
                , @hst_sch_nm = @historyTableSchema OUTPUT
                , @period_col_nm = @periodColumnName OUTPUT
  
IF @historyTableName IS NULL OR @historyTableSchema IS NULL OR @periodColumnName IS NULL
    THROW 50010, 'History table cannot be found. Either specified table is not system-versioned temporal or you have provided incorrect argument values.', 1

/*Generate 3 statements that will run inside a transaction:
  (1) SET SYSTEM_VERSIONING = OFF,
  (2) DELETE FROM history_table,
  (3) SET SYSTEM_VERSIONING = ON
  On SQL Server 2016, it is critical that (1) and (2) run in separate EXEC statements, or SQL Server will generate the following error:
  Msg 13560, Level 16, State 1, Line XXX
  Cannot delete rows from a temporal history table '<database_name>.<history_table_schema_name>.<history_table_name>'.
*/
SET @disableVersioningScript = @disableVersioningScript + 'ALTER TABLE [' + @temporalTableSchema + '].[' + @temporalTableName + '] SET (SYSTEM_VERSIONING = OFF)'
SET @deleteHistoryDataScript = @deleteHistoryDataScript + ' DELETE FROM [' + @historyTableSchema + '].[' + @historyTableName + ']
    WHERE ['+ @periodColumnName + '] < ' + '''' + convert(varchar(128), @cleanupOlderThanDate, 126) + ''''
SET @enableVersioningScript = @enableVersioningScript + ' ALTER TABLE [' + @temporalTableSchema + '].[' + @temporalTableName + ']
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = [' + @historyTableSchema + '].[' + @historyTableName + '], DATA_CONSISTENCY_CHECK = OFF )); '

BEGIN TRAN
    EXEC (@disableVersioningScript);
    EXEC (@deleteHistoryDataScript);
    EXEC (@enableVersioningScript);
COMMIT;
```

## <a name="using-temporal-history-retention-policy-approach"></a>Uso del enfoque de la directiva de retención de historial temporal

> [!NOTE]
> El uso de la directiva de retención de historial temporal se aplica a [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] y SQL Server 2017 a partir de CTP 1.3.

La retención de historial temporal se puede configurar en el nivel de tabla individual, lo que permite a los usuarios crear directivas de vencimiento flexibles. Aplicar la retención temporal es muy sencillo: solo requiere establecer un parámetro al cambiar el esquema o al crear la tabla.

Después de definir la directiva de retención, si hay filas de historial que sean aptas para la limpieza de datos automática, Azure SQL Database inicia la comprobación periódicamente. La identificación de las filas coincidentes y su eliminación de la tabla de historial se producen de forma transparente, en la tarea en segundo plano que programa y ejecuta el sistema. Se comprueba la condición de vencimiento para las filas de la tabla de historial en función de la columna que representa el final del período SYSTEM_TIME. Si el período de retención se establece, por ejemplo, en seis meses, las filas aptas para la limpieza de la tabla cumplen la condición siguiente:

```sql
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
```
En el ejemplo anterior, se supone que la columna ValidTo corresponde al final del período SYSTEM_TIME.

### <a name="how-to-configure-retention-policy"></a>Configuración de la directiva de retención

Antes de configurar la directiva de retención para una tabla temporal, compruebe si la retención de historial temporal está habilitada en el nivel de base de datos:

```sql
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
```

La marca de base de datos **is_temporal_history_retention_enabled** se establece en ON de forma predeterminada, pero los usuarios pueden cambiarla con la instrucción ALTER DATABASE. También se establece automáticamente en OFF después de la operación de restauración a un momento dado. Para habilitar la limpieza de la retención de historial temporal de la base de datos, ejecute la instrucción siguiente:

```sql
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION ON
```

La directiva de retención se configura al crear la tabla especificando el valor del parámetro HISTORY_RETENTION_PERIOD:

```sql
CREATE TABLE dbo.WebsiteUserInfo
(
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )
 WITH
 (
    SYSTEM_VERSIONING = ON
    (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
    )
 );
```

Puede especificar el período de retención mediante distintas unidades de tiempo: DAYS, WEEKS, MONTHS y YEARS. Si se omite HISTORY_RETENTION_PERIOD, se asume la retención INFINITE. También puede usar explícitamente la palabra clave INFINITE.
En algunos escenarios, es posible que quiera configurar la retención tras crear la tabla o cambiar a un valor previamente configurado. En ese caso, use la instrucción ALTER TABLE:

```sql
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
```

Para revisar el estado actual de la directiva de retención, use la siguiente consulta, que combina la marca de habilitación de retención temporal en el nivel de base de datos con períodos de retención para tablas individuales:

```sql
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName, SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
```

### <a name="how-sql-database-deletes-aged-rows"></a>Eliminación de las filas antiguas en SQL Database

El proceso de limpieza depende del diseño del índice de la tabla de historial. Es importante tener en cuenta que *solo las tablas de historial con un índice agrupado (árbol B o almacén de columnas) pueden tener una directiva de retención finita configurada*. Se crea una tarea en segundo plano para realizar la limpieza de datos antiguos de todas las tablas temporales con el período de retención finito. La lógica de limpieza del índice agrupado de almacén de filas (árbol B) elimina las filas antiguas en fragmentos más pequeños (hasta 10 000), lo cual minimiza la presión en el registro de base de datos y el subsistema de E/S. A pesar de que la lógica de limpieza usa el índice de árbol B necesario, no se puede garantizar el orden de las eliminaciones de las filas más antiguas en relación con el período de retención. Por tanto, *no hay ninguna dependencia en el orden de limpieza en sus aplicaciones*.

La tarea de limpieza del almacén de columnas agrupadas quita los grupos de filas completos a la vez (normalmente contiene 1 millón de filas cada uno), lo que es muy eficaz, especialmente cuando los datos de historial se generan a un ritmo alto.

![Retención de almacén de columnas agrupado](../../relational-databases/tables/media/cciretention.png "Retención de almacén de columnas agrupado")

La excelente compresión de datos y la limpieza eficaz de la retención hacen que el índice de almacén de columnas agrupadas sea una elección perfecta en escenarios en los que la carga de trabajo genera rápidamente una gran cantidad de datos de historial. Este patrón es típico de las cargas de trabajo de procesamiento intensivo de transacciones que usan tablas temporales para el seguimiento de cambios y la auditoría, el análisis de tendencias o la ingesta de datos de IoT.

Para obtener más información, consulte [Administración de datos históricos en tablas temporales con directivas de retención](/azure/sql-database/sql-database-temporal-tables-retention-policy).

## <a name="next-steps"></a>Pasos siguientes

- [Tablas temporales](../../relational-databases/tables/temporal-tables.md)
- [Introducción a las tablas temporales con versión del sistema](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [Comprobaciones de coherencia del sistema de la tabla temporal](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [Creación de particiones con tablas temporales](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [Limitaciones y consideraciones de las tablas temporales](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)
- [Seguridad de la tabla temporal](../../relational-databases/tables/temporal-table-security.md)
- [Tablas temporales con control de versiones del sistema con tablas con optimización para memoria](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [Funciones y vistas de metadatos de la tabla temporal](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)