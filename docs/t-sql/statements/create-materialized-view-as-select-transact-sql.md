---
description: CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL)
title: CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: data-warehouse
ms.topic: reference
f1_keywords:
- CREATE VIEW
- VIEW_TSQL
- VIEW
- CREATE_VIEW_TSQL
- SCHEMABINDING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- table creation [SQL Server], CREATE VIEW
- views [SQL Server], creating
- CREATE VIEW statement
- updatable partitioned views
- tables [SQL Server], virtual
- updatable views
- modifying partitioned views
- virtual tables [SQL Server]
- number of columns per view
- partitioned views [SQL Server], creating
- WITH ENCRYPTION clause
- WITH CHECK OPTION clause
- partitioned views [SQL Server], modifying
- partitioned views [SQL Server], replication
- distributed partitioned views [SQL Server]
- views [SQL Server], indexed views
- maximum number of columns per view
ms.assetid: aecc2f73-2ab5-4db9-b1e6-2f9e3c601fb9
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 81073d458d69e28ee145c1bb1dd38f3474800b35
ms.sourcegitcommit: f10f0d604be1dce6c600a92aec4c095e7b52e19c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2021
ms.locfileid: "102770543"
---
# <a name="create-materialized-view-as-select-transact-sql"></a>CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL)  

[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

En este artículo se explica la instrucción CREATE MATERIALIZED VIEW AS SELECT de T-SQL en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] para soluciones en desarrollo. En el artículo también se proporcionan ejemplos de código.

Una vista materializada conserva los datos que ha devuelto la consulta de visualización de definición y se actualiza automáticamente a medida que cambian los datos en las tablas subyacentes.   Mejora el rendimiento de las consultas complejas (por lo general, consultas con combinaciones y agregaciones), a la vez que ofrece operaciones de mantenimiento simples.   Con su función de correspondencia automática del plan de ejecución, no es necesario que se haga referencia a una vista materializada en la consulta para que el optimizador tenga en cuenta la vista con fines de sustitución.  Esta capacidad permite a los ingenieros de datos implementar vistas materializadas como mecanismo para mejorar el tiempo de respuesta de las consultas, sin necesidad de cambiarlas.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE MATERIALIZED VIEW [ schema_name. ] materialized_view_name
    WITH (  
      <distribution_option>
    )
    AS <select_statement>
[;]

<distribution_option> ::=
    {  
        DISTRIBUTION = HASH ( distribution_column_name )  
      | DISTRIBUTION = ROUND_ROBIN  
    }

<select_statement> ::=
    SELECT select_criteria

```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Argumentos

*schema_name*     
 Es el nombre del esquema al que pertenece la vista.  
  
*materialized_view_name*   
Es el nombre de la vista. Los nombres de las vistas deben cumplir las reglas de los identificadores. La especificación del nombre del propietario de la vista es opcional.  

*opción de distribución*     
Solo se admiten distribuciones HASH y ROUND_ROBIN.

*select_statement*   
La lista SELECT de la definición de la vista materializada debe cumplir al menos uno de estos dos criterios:
- La lista SELECT contiene una función de agregado.
- Se usa GROUP BY en la definición de la vista materializada y todas las columnas de GROUP BY se incluyen en la lista SELECT.  En la cláusula GROUP BY se pueden usar hasta 32 columnas.

Las funciones de agregado son necesarias en la lista SELECT de la definición de la vista materializada.  Entre las agregaciones admitidas figuran MAX, MIN, AVG, COUNT, COUNT_BIG, SUM, VAR, STDEV.

Cuando se usan los agregados MIN/MAX en la lista SELECT de la definición de la vista materializada, es necesario cumplir estos requisitos:
 
- FOR_APPEND es necesario.  Por ejemplo:
  ```sql 
  CREATE MATERIALIZED VIEW mv_test2  
  WITH (distribution = hash(i_category_id), FOR_APPEND)  
  AS
  SELECT MAX(i.i_rec_start_date) as max_i_rec_start_date, MIN(i.i_rec_end_date) as min_i_rec_end_date, i.i_item_sk, i.i_item_id, i.i_category_id
  FROM syntheticworkload.item i  
  GROUP BY i.i_item_sk, i.i_item_id, i.i_category_id
  ```

- La vista materializada se deshabilitará cuando tenga lugar una operación UPDATE o DELETE en las tablas base de referencia.  Esta restricción no se aplica a las operaciones INSERT.  Para volver a habilitar la vista materializada, ejecute ALTER MATERIALIZED VIEW con REBUILD.
  
## <a name="remarks"></a>Observaciones

Las vistas materializadas en el almacenamiento de datos de Azure son similares a las vistas indexadas de SQL Server.Comparte casi las mismas restricciones que la vista indizada (consulte [Crear vistas indizadas](../../relational-databases/views/create-indexed-views.md) para obtener más información), salvo que una vista materializada admite las funciones de agregado.   

>[!Note]
>Aunque CREATE MATERIALIZED VIEW no admite COUNT, DISTINCT, COUNT (expresión DISTINCT) ni COUNT_BIG (expresión DISTINCT), las consultas SELECT con estas funciones se pueden seguir beneficiando de las vistas materializadas para obtener un rendimiento más rápido, ya que el optimizador de Synapse SQL puede volver a escribir automáticamente dichas agregaciones en la consulta del usuario para que coincidan con las vistas materializadas existentes.  Para obtener más información, consulte la sección ejemplo de este artículo. 

En CREATE MATERIALIZED VIEW AS SELECT no se admite APPROX_COUNT_DISTINCT.

La vista materializada solo admite CLUSTERED COLUMNSTORE INDEX. 

Una vista materializada no puede hacer referencia a otras vistas.  

No se puede crear una vista materializada en una tabla con enmascaramiento de datos dinámicos (DDM), aunque la columna DDM no forme parte de la vista materializada.  Si una columna de tabla forma parte de una vista materializada activa o una vista materializada deshabilitada, DDM no se puede agregar a esta columna.  

No se puede crear una vista materializada en una tabla con seguridad de nivel de fila habilitada.

Las vistas materializadas se pueden crear en tablas con particiones.  La partición SPLIT/MERGE se admite en las tablas base de las vistas materializadas, mientras que la partición SWITCH, no.  
 
ALTER TABLE SWITCH no se admite en las tablas a las que se hace referencia en las vistas materializadas. Deshabilite o quite las vistas materializadas antes de usar ALTER TABLE SWITCH. En los siguientes escenarios, la creación de una vista materializada requiere la adición de nuevas columnas a la vista materializada:

|Escenario|Nuevas columnas para agregar a la vista materializada|Comentario|  
|-----------------|---------------|-----------------|
|Falta COUNT_BIG() en la lista SELECT de la definición de una vista materializada.| COUNT_BIG (*) |Se agrega automáticamente al crear una vista materializada.  No se requiere ninguna acción del usuario.|
|SUM(a) lo especifican los usuarios en la lista SELECT de la definición de una vista materializada Y "a" es una expresión que admite un valor NULL. |COUNT_BIG (a) |Los usuarios deben agregar la expresión "a" manualmente en la definición de la vista materializada.|
|AVG(a) lo especifican los usuarios en la lista SELECT de la definición de una vista materializada en la que "a" es una expresión.|SUM(a), COUNT_BIG(a)|Se agrega automáticamente al crear una vista materializada.  No se requiere ninguna acción del usuario.|
|STDEV(a) lo especifican los usuarios en la lista SELECT de la definición de una vista materializada en la que "a" es una expresión.|SUM(a), COUNT_BIG(a), SUM(square(a))|Se agrega automáticamente al crear una vista materializada.  No se requiere ninguna acción del usuario. |
| | | |

Las vistas materializadas, una vez creadas, son visibles dentro de SQL Server Management Studio en la carpeta de vistas de la instancia de [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].

Los usuarios pueden ejecutar [SP_SPACEUSED](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md?view=azure-sqldw-latest&preserve-view=true) y [DBCC PDW_SHOWSPACEUSED](../database-console-commands/dbcc-pdw-showspaceused-transact-sql.md?view=azure-sqldw-latest&preserve-view=true) para determinar el espacio que consume una vista materializada.  

Una vista materializada se puede quitar mediante DROP VIEW.  Puede usar ALTER MATERIALIZED VIEW para deshabilitar o volver a generar una vista materializada.   

La vista materializada es un mecanismo de optimización automática de consultas.  No es necesario que los usuarios consulten directamente una vista materializada.  Cuando se envía una consulta de usuario, el motor comprueba los permisos del usuario en los objetos de consulta y genera un error en la consulta sin ejecución si el usuario no tiene acceso a las tablas o a las vistas normales de la consulta.  Si se ha comprobado el permiso del usuario, el optimizador usa automáticamente una vista materializada coincidente para ejecutar la consulta con el fin de mejorar el rendimiento.  Los usuarios obtienen los mismos datos, independientemente de si se usan las tablas base o la vista materializada para realizar la consulta.  

El plan de EXPLAIN y el plan de ejecución estimado gráfico en SQL Server Management Studio pueden mostrar si una vista materializada se tiene en cuenta por el optimizador de consultas para la ejecución de las consultas. y el plan de ejecución estimado gráfico en SQL Server Management Studio pueden mostrar si una vista materializada se tiene en cuenta por el optimizador de consultas para la ejecución de las consultas.

Para averiguar si una instrucción SQL puede beneficiarse de una nueva vista materializada, ejecute el comando `EXPLAIN` con `WITH_RECOMMENDATIONS`.  Para obtener más información, consulte [EXPLAIN (Transact-SQL)](../queries/explain-transact-sql.md?view=azure-sqldw-latest&preserve-view=true).

## <a name="ownership"></a>Propiedad
- No se puede crear una vista materializada si los propietarios de las tablas base y la vista materializada que se va a crear no son iguales.
- Una vista materializada y sus tablas base pueden residir en esquemas diferentes. Cuando se crea la vista materializada, el propietario del esquema de la vista se convierte automáticamente en el propietario de la vista materializada y no es posible cambiar la propiedad de esta vista.     

## <a name="permissions"></a>Permisos
Un usuario necesita los permisos siguientes para crear una vista materializada, además de cumplir los requisitos de propiedad del objeto: 
1) El permiso CREATE VIEW en la base de datos
2) El permiso SELECT en las tablas base de la vista materializada
3) El permiso REFERENCES en el esquema que contiene las tablas base
4) El permiso ALTER en el esquema que contiene la vista materializada 


## <a name="example"></a>Ejemplo
A. En este ejemplo se muestra cómo el optimizador de Synapse SQL usa automáticamente vistas materializadas para ejecutar una consulta y mejorar el rendimiento, incluso cuando la consulta usa funciones no admitidas en CREATE MATERIALIZED VIEW, como COUNT (expresión DISTINCT). Una consulta que se usa y tarda varios segundos en completarse ahora finaliza en subsegundos sin ningún cambio en la consulta del usuario.   

``` sql 

-- Create a table with ~536 million rows
create table t(a int not null, b int not null, c int not null) with (distribution=hash(a), clustered columnstore index);

insert into t values(1,1,1);

declare @p int =1;
while (@P < 30)
    begin
    insert into t select a+1,b+2,c+3 from t;  
    select @p +=1;
end

-- A SELECT query with COUNT_BIG (DISTINCT expression) took multiple seconds to complete and it reads data directly from the base table a. 
select a, count_big(distinct b) from t group by a;

-- Create two materialized views, not using COUNT_BIG(DISTINCT expression).
create materialized view V1 with(distribution=hash(a)) as select a, b from dbo.t group by a, b;

-- Clear all cache.

DBCC DROPCLEANBUFFERS;
DBCC freeproccache;

-- Check the estimated execution plan in SQL Server Management Studio.  It shows the SELECT query is first step (GET operator) is to read data from the materialized view V1, not from base table a.
select a, count_big(distinct b) from t group by a;

-- Now execute this SELECT query.  This time it took sub-second to complete because Synapse SQL engine automatically matches the query with materialized view V1 and uses it for faster query execution.  There was no change in the user query.

DECLARE @timerstart datetime2, @timerend datetime2;
SET @timerstart = sysdatetime();

select a, count_big(distinct b) from t group by a;

SET @timerend = sysdatetime()
select DATEDIFF(ms,@timerstart,@timerend);

```

B. En este ejemplo, User2 crea una vista materializada en las tablas propiedad de User1.  La vista materializada es propiedad de User1.
```sql
/****************************************************************
Setup:
SchemaX owner = DBO
SchemaX.T1 owner = User1
SchemaX.T2 owner = User1
SchemaY owner = User1
*****************************************************************/
CREATE USER User1 WITHOUT LOGIN ;
CREATE USER User2 WITHOUT LOGIN ;
CREATE SCHEMA SchemaX;  
CREATE SCHEMA SchemaY AUTHORIZATION User1;
GO
CREATE TABLE [SchemaX].[T1] (   [vendorID] [varchar](255) Not NULL, [totalAmount] [float] Not NULL, [puYear] [int] NULL );
CREATE TABLE [SchemaX].[T2] (   [vendorID] [varchar](255) Not NULL, [totalAmount] [float] Not NULL, [puYear] [int] NULL);
GO
ALTER AUTHORIZATION ON OBJECT::SchemaX.[T1] TO User1;
ALTER AUTHORIZATION ON OBJECT::SchemaX.[T2] TO User1;

/*****************************************************************************
For user2 to create a MV in SchemaY on SchemaX.T1 and SchemaX.T2, user2 needs:
1. CREATE VIEW permission in the database
2. REFERENCES permission on the schema1
3. SELECT permission on base table T1, T2  
4. ALTER permission on SchemaY
******************************************************************************/
GRANT CREATE VIEW to User2;
GRANT REFERENCES ON SCHEMA::SchemaX to User2;  
GRANT SELECT ON OBJECT::SchemaX.T1 to User2; 
GRANT SELECT ON OBJECT::SchemaX.T2 to User2;
GRANT ALTER ON SCHEMA::SchemaY to User2; 
GO
EXECUTE AS USER = 'User2';  
GO
CREATE materialized VIEW [SchemaY].MV_by_User2 with(distribution=round_robin) 
as 
        select A.vendorID, sum(A.totalamount) as S, Count_Big(*) as T 
        from [SchemaX].[T1] A
        inner join [SchemaX].[T2] B on A.vendorID = B.vendorID group by A.vendorID ;
GO
revert;
GO
```

## <a name="see-also"></a>Vea también

[Optimización del rendimiento con vista materializada](/azure/sql-data-warehouse/performance-tuning-materialized-views)   
[ALTER MATERIALIZED VIEW &#40;Transact-SQL&#41;](./alter-materialized-view-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)      
[DROP VIEW](./drop-view-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)  
[EXPLAIN &#40;Transact-SQL&#41;](../queries/explain-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_column_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-column-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_mappings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-mappings-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD &#40;Transact-SQL&#41;](../database-console-commands/dbcc-pdw-showmaterializedviewoverhead-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[Vistas de catálogo de [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
[Vistas del sistema admitidas en Azure [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views)   
[Instrucciones T-SQL admitidas en Azure [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-statements)
