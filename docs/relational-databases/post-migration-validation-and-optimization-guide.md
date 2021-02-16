---
description: Guía de optimización y validación posterior a la migración
title: Guía de optimización y validación posterior a la migración | Microsoft Docs
ms.date: 01/09/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- post-migration validation and optimization
- guide, post-migration validation and optimization
ms.assetid: 11f8017e-5bc3-4bab-8060-c16282cfbac1
author: pelopes
ms.author: harinid
ms.openlocfilehash: 397e5b7acd88f8e10dea05ce3ab2dc2f7dd105c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350656"
---
# <a name="post-migration-validation-and-optimization-guide"></a>Guía de optimización y validación posterior a la migración

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

El paso posterior a la migración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] es fundamental para reconciliar cualquier precisión e integridad de los datos, así como para solucionar problemas de rendimiento con la carga de trabajo.

## <a name="common-performance-scenarios"></a>Escenarios comunes de rendimiento

A continuación se muestran algunos de los escenarios comunes de rendimiento detectados después de migrar a la plataforma [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y cómo resolverlos. Puede tratarse de escenarios que son específicos de la migración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (versiones anteriores a las versiones más recientes), así como de la plataforma externa (por ejemplo, Oracle, DB2, MySQL y Sybase) a la migración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

## <a name="query-regressions-due-to-change-in-ce-version"></a><a name="CEUpgrade"></a> Consultar las regresiones debidas a un cambio en la versión CE

**Se aplica a:** [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] para la migración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

Al migrar desde una versión anterior de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] o versiones más recientes y al actualizar el [nivel de compatibilidad de la base de datos](../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) a la versión más reciente disponible, una carga de trabajo podría quedar expuesta a sufrir una regresión del rendimiento.

Esto es debido a que, a partir de [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], todos los cambios del optimizador de consultas están vinculados al [nivel de compatibilidad de la base de datos](../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)más reciente, por lo que los planes no se cambian en el momento de la actualización, sino cuando un usuario cambia la opción de base de datos `COMPATIBILITY_LEVEL` a la versión más reciente. Esta función, junto con el Almacén de consultas, confiere al usuario un enorme control sobre el rendimiento de las consultas en el proceso de actualización. 

Para obtener más información sobre los cambios del optimizador de consultas introducidas en [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], consulte [Optimizing Your Query Plans with the SQL Server 2014 Cardinality Estimator (Optimizar los planes de consulta con el programa de estimación de cardinalidad de SQL Server 2014)](/previous-versions/dn673537(v=msdn.10)).

### <a name="steps-to-resolve"></a>Pasos para resolver

Cambie el [nivel de compatibilidad de la base de datos](../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) a la versión de origen y siga el flujo de trabajo de actualización recomendado, tal como se muestra en la siguiente imagen:

![Diagrama que muestra el flujo de trabajo de actualización recomendado.](../relational-databases/performance/media/query-store-usage-5.png "query-store-usage-5")  

Para obtener más información sobre este tema, consulte [Mantener la estabilidad del rendimiento al actualizar a SQL Server 2016](../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade).

## <a name="sensitivity-to-parameter-sniffing"></a><a name="ParameterSniffing"></a> Sensibilidad al examen de parámetros

**Se aplica a:** plataforma externa (por ejemplo, Oracle, DB2, MySQL y Sybase) a la migración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

> [!NOTE]
> Para migraciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], si este problema existía en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] de origen, migrar a una versión más reciente de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tal cual no se contempla en este escenario. 

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] compila planes de consulta en procedimientos almacenados mediante el examen de parámetros de entrada en la primera compilación, generando un plan con parámetros y reutilizable, optimizado para esa distribución de datos de entrada. Incluso si no hay procedimientos almacenados, la mayoría de instrucciones que generan planes triviales se parametrizarán. Después de que un plan se almacene en caché por primera vez, cualquier ejecución futura se asigna a un plan previamente almacenado en caché.
Surge un posible problema si esa primera compilación puede que no haya usado los conjuntos de parámetros más comunes para la carga de trabajo habitual. Para parámetros diferentes, el mismo plan de ejecución es ineficaz. Para obtener más información sobre este tema, consulte [Examen de parámetros](../relational-databases/query-processing-architecture-guide.md#ParamSniffing).

### <a name="steps-to-resolve"></a>Pasos para resolver

1.  Use la sugerencia `RECOMPILE`. Un plan se calcula cada vez adaptado a cada valor de parámetro.
2.  Vuelva a escribir el procedimiento almacenado para usar la opción `(OPTIMIZE FOR(<input parameter> = <value>))`. Decida qué valor quiere usar que se adapte a la mayor parte de la carga de trabajo relevante, creando y manteniendo un plan que sea eficaz para el valor con parámetros.
3.  Vuelva a escribir el procedimiento almacenado mediante la variable local dentro del procedimiento. Ahora, el optimizador usa el vector de densidad para las estimaciones, lo que produce el mismo plan independientemente del valor del parámetro.
4.  Vuelva a escribir el procedimiento almacenado para usar la opción `(OPTIMIZE FOR UNKNOWN)`. Se consigue el mismo efecto que al usar la técnica de variable local.
5.  Vuelva a escribir la consulta para usar la sugerencia `DISABLE_PARAMETER_SNIFFING`. Se consigue el mismo efecto que al usar la técnica de variable local al deshabilitar completamente el examen de parámetros, a menos que se usen `OPTION(RECOMPILE)`, `WITH RECOMPILE` o `OPTIMIZE FOR <value>`.

> [!TIP] 
> Aproveche la característica de análisis de plan de [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] para identificar rápidamente si se trata de un problema. Encontrará más información [aquí](/archive/blogs/sql_server_team/new-in-ssms-query-performance-troubleshooting-made-easier).

## <a name="missing-indexes"></a><a name="MissingIndexes"></a> Faltan índices

**Se aplica a:** plataforma externa (por ejemplo, Oracle, DB2, MySQL y Sybase) y la migración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

Los índices que faltan o son incorrectos causan una E/S adicional que producen una memoria adicional y se malgasta CPU. Esto puede ser porque ha cambiado el perfil de carga de trabajo, como al usar predicados diferentes, invalidando el diseño de índices existente. Evidencia de una estrategia de indexación deficiente o cambios en el perfil de carga de trabajo incluyen:
-   Busque índices duplicados, redundantes, usados con poca frecuencia y que no se han usado nunca.
-   Tenga especial cuidado con los índices no usados con actualizaciones.

### <a name="steps-to-resolve"></a>Pasos para resolver

1.  Aproveche el plan de ejecución gráfico de cualquiera referencia de índice que falte.
2.  Sugerencias de indexación generadas por el [Asistente para la optimización de motor de base de datos](../tools/dta/tutorial-database-engine-tuning-advisor.md).
3.  Aproveche la [DMV de los índices que faltan](../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md) o a través del [panel de rendimiento de SQL Server](https://www.microsoft.com/download/details.aspx?id=29063).
4.  Aproveche los scripts que existían previamente que puedan usar DMV existentes para proporcionar una visión general de los índices, duplicados, redundantes, poco usados, que no se han usado nunca o que faltan, pero también si alguna referencia de índice se sugiere o codifica de forma rígida en procedimientos y funciones existentes de la base de datos. 

> [!TIP] 
> Entre los ejemplos de estos scripts que existían previamente se encuentran la [creación de índices](https://github.com/Microsoft/tigertoolbox/tree/master/Index-Creation) y la [información del índice](https://github.com/Microsoft/tigertoolbox/tree/master/Index-Information). 

## <a name="inability-to-use-predicates-to-filter-data"></a><a name="InabilityPredicates"></a> Incapacidad de usar predicados para filtrar datos

**Se aplica a:** plataforma externa (por ejemplo, Oracle, DB2, MySQL y Sybase) y la migración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

> [!NOTE]
> Para migraciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], si este problema existía en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] de origen, migrar a una versión más reciente de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tal cual no se contempla en este escenario.

El optimizador de consultas de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] solo puede tener en cuenta información que se conoce en tiempo de compilación. Si una carga de trabajo se basa en predicados que solo se pueden conocer en tiempo de ejecución, se aumenta la posibilidad de una opción de plan deficiente. Para un plan de mejor calidad, los predicados deben ser **SARGable** o **S** earch **Arg** ument **able**.

Algunos ejemplos de predicados que no son SARGable:
-   Conversiones implícitas de datos, como VARCHAR a NVARCHAR o INT a VARCHAR. Busque las advertencias de CONVERT_IMPLICIT en tiempo de ejecución en los planes de ejecución reales. La conversión de un tipo a otro también puede provocar una pérdida de precisión.
-   Las expresiones complejas indeterminadas como `WHERE UnitPrice + 1 < 3.975`, pero no `WHERE UnitPrice < 320 * 200 * 32`.
-   Expresiones que usan funciones, como `WHERE ABS(ProductID) = 771` o `WHERE UPPER(LastName) = 'Smith'`.
-   Cadenas con un carácter comodín inicial, tales como `WHERE LastName LIKE '%Smith'`, pero no `WHERE LastName LIKE 'Smith%'`.

### <a name="steps-to-resolve"></a>Pasos para resolver

1. Declare siempre las variables o los parámetros como el [tipo de datos](../t-sql/data-types/data-types-transact-sql.md) de destino deseado. 
  -   Esto puede implicar la comparación de cualquier construcción de código definido por el usuario que se almacena en la base de datos (por ejemplo, procedimientos almacenados, vistas o funciones definidas por el usuario) con tablas del sistema que contienen información de tipos de datos usados en las tablas subyacentes (como [sys.columns](../relational-databases/system-catalog-views/sys-columns-transact-sql.md)).
2. Si no se puede recorrer todo el código hasta el punto anterior, con la misma finalidad, cambie el tipo de datos en la tabla para que coincida con las declaraciones de variable o parámetro.
3. Motivo de la utilidad de las siguientes construcciones:

  -   Funciones que se usan como predicados;
  -   Búsquedas con caracteres comodín;
  -   Expresiones complejas en función de los datos en columnas: evalúe la necesidad de crear columnas calculadas persistentes, que se pueden indexar;

> [!NOTE] 
> Todo lo anterior puede realizarse mediante programación.

## <a name="use-of-table-valued-functions-multi-statement-vs-inline"></a><a name="TableValuedFunctions"></a> Uso de funciones con valores de tabla (múltiples instrucciones frente a insertadas)

**Se aplica a:** plataforma externa (por ejemplo, Oracle, DB2, MySQL y Sybase) y la migración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

> [!NOTE]
> Para migraciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], si este problema existía en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] de origen, migrar a una versión más reciente de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tal cual no se contempla en este escenario.

Las funciones con valores de tabla devuelven un tipo de datos de tabla que puede ser una alternativa a las vistas. Mientras que las vistas se limitan a una única instrucción `SELECT`, las funciones definidas por el usuario pueden contener instrucciones adicionales que permiten una lógica más eficaz que en las vistas.

> [!IMPORTANT] 
> Puesto que no se ha creado la tabla de salida de una MSTVF (función con valores de tabla de múltiples instrucciones) en tiempo de compilación, el optimizador de consultas de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se basa en la heurística (y non en las estadísticas reales) para determinar las estimaciones de fila. Aunque los índices se agreguen a las tablas base, esto no servirá de ayuda. Para MSTVF, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usa una estimación fija de 1 para el número de filas que se espera que va a devolver una MSTVF (a partir de [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], esa estimación corregida es de 100 filas).

### <a name="steps-to-resolve"></a>Pasos para resolver

1.  Si la TVF de múltiples instrucciones es la única instrucción, conviértala en TVF insertada.

    ```sql
    CREATE FUNCTION dbo.tfnGetRecentAddress(@ID int)
    RETURNS @tblAddress TABLE
    ([Address] VARCHAR(60) NOT NULL)
    AS
    BEGIN
      INSERT INTO @tblAddress ([Address])
      SELECT TOP 1 [AddressLine1]
      FROM [Person].[Address]
      WHERE  AddressID = @ID
      ORDER BY [ModifiedDate] DESC
    RETURN
    END
    ```

    El ejemplo de formato en línea se muestra a continuación.

    ```sql
    CREATE FUNCTION dbo.tfnGetRecentAddress_inline(@ID int)
    RETURNS TABLE
    AS
    RETURN (
      SELECT TOP 1 [AddressLine1] AS [Address]
      FROM [Person].[Address]
      WHERE  AddressID = @ID
      ORDER BY [ModifiedDate] DESC
    )
    ```

2.  Si es más complejo, considere la opción de usar los resultados intermedios que se almacenan en tablas optimizadas para memoria o tablas temporales.

##  <a name="additional-reading"></a><a name="Additional_Reading"></a> Lecturas adicionales

 [Procedimiento recomendado con el Almacén de consultas](../relational-databases/performance/best-practice-with-the-query-store.md)  
[Tablas optimizadas para la memoria](./in-memory-oltp/sample-database-for-in-memory-oltp.md)  
[Funciones definidas por el usuario](../relational-databases/user-defined-functions/user-defined-functions.md)  
[Table Variables and Row Estimations - Part 1](/archive/blogs/blogdoezequiel/table-variables-and-row-estimations-part-1) (Variables de tabla y estimaciones de fila: parte 1)  
[Table Variables and Row Estimations - Part 2](/archive/blogs/blogdoezequiel/table-variables-and-row-estimations-part-2) (Variables de tabla y estimaciones de fila: parte 2)  
[Almacenar en caché y volver a utilizar un plan de ejecución](../relational-databases/query-processing-architecture-guide.md#execution-plan-caching-and-reuse)