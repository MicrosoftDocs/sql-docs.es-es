---
description: CREATE STATISTICS (Transact-SQL)
title: CREATE STATISTICS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/04/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STATISTICS
- STATISTICS_TSQL
- CREATE STATISTICS
- CREATE_STATISTICS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- query optimization statistics [SQL Server], creating
- indexed views [SQL Server], statistics
- FULLSCAN option
- CREATE STATISTICS statement
- filtered statistics [SQL Server]
- creating statistics [SQL Server]
- NORECOMPUTE clause
ms.assetid: b23e2f6b-076c-4e6d-9281-764bdb616ad2
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7a86a2cfbe236aad6e37e0f8895f2be29ec1c5ae
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100344025"
---
# <a name="create-statistics-transact-sql"></a>CREATE STATISTICS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Crea estadísticas de optimización de consultas en una o más columnas de una tabla, en una vista indizada o en una tabla externa. Para la mayoría de las consultas, el optimizador de consultas genera ya las estadísticas necesarias para un plan de consulta de alta calidad; en algunos casos, para mejorar el rendimiento de la consulta necesita crear estadísticas adicionales con CREATE STATISTICS modificar el diseño de la consulta.  
  
 Para obtener más información, consulte [Estadísticas](../../relational-databases/statistics/statistics.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Create statistics on an external table  
CREATE STATISTICS statistics_name   
ON { table_or_indexed_view_name } ( column [ ,...n ] )   
    [ WITH FULLSCAN ] ;  
  
-- Create statistics on a regular table or indexed view  
CREATE STATISTICS statistics_name   
ON { table_or_indexed_view_name } ( column [ ,...n ] )   
    [ WHERE <filter_predicate> ]  
    [ WITH   
        [ [ FULLSCAN   
            [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
          | SAMPLE number { PERCENT | ROWS }   
            [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
          | <update_stats_stream_option> [ ,...n ]    
        [ [ , ] NORECOMPUTE ]   
        [ [ , ] INCREMENTAL = { ON | OFF } ] 
        [ [ , ] MAXDOP = max_degree_of_parallelism ]
    ] ;  
  
<filter_predicate> ::=   
    <conjunct> [AND <conjunct>]  
  
<conjunct> ::=  
    <disjunct> | <comparison>  
  
<disjunct> ::=  
        column_name IN (constant ,...)  
  
<comparison> ::=  
        column_name <comparison_op> constant  
  
<comparison_op> ::=  
    IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !<  
    
<update_stats_stream_option> ::=  
    [ STATS_STREAM = stats_stream ]  
    [ ROWCOUNT = numeric_constant ]  
    [ PAGECOUNT = numeric_contant ] 
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE STATISTICS statistics_name   
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( column_name  [ ,...n ] )   
    [ WHERE <filter_predicate> ]  
    [ WITH {  
           FULLSCAN   
           | SAMPLE number PERCENT   
      }  
    ]  
[;]  
  
<filter_predicate> ::=   
    <conjunct> [AND <conjunct>]  
  
<conjunct> ::=  
    <disjunct> | <comparison>  
  
<disjunct> ::=  
        column_name IN (constant ,...)  
  
<comparison> ::=  
        column_name <comparison_op> constant  
  
<comparison_op> ::=  
    IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !<  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *statistics_name*  
 Es el nombre de las estadísticas que se van a crear.  
  
 *table_or_indexed_view_name*  
 Es el nombre de la tabla, de la vista indizada o de la tabla externa en que se van a crear las estadísticas. Para crear estadísticas en otra base de datos, especifique un nombre de tabla completa.  
  
 *column [ ,...n]*  
 Una o varias columnas que se van a incluir en las estadísticas. Las columnas deben estar en orden de prioridad, de izquierda a derecha. Solo se usa la primera columna para crear el histograma. Todas las columnas se usan para las estadísticas de correlación entre columnas denominadas densidades.  
  
 Se puede especificar cualquier columna que pueda ser especificada como columna de clave de índice con las siguientes excepciones:  
  
-   Las columnas FILESTREAM, de texto completo y **xml** no se pueden especificar.  
  
-   Solamente se pueden especificar columnas calculadas si las opciones de base de datos ARITHABORT y QUOTED_IDENTIFIER están establecidas en ON.  
  
-   Se pueden especificar columnas de tipo CLR definido por el usuario si el tipo admite el orden binario. Es posible especificar columnas calculadas definidas como llamadas a métodos de una columna de un tipo definido por el usuario si los métodos están marcados como deterministas.  
  
 WHERE \<filter_predicate> Especifica una expresión para seleccionar un subconjunto de filas que se va a incluir al crear el objeto de estadísticas. Las estadísticas que se crean con un predicado de filtro se llaman estadísticas filtradas. El predicado de filtro utiliza la lógica de comparación simple y no puede hacer referencia a una columna calculada, a una columna UDT, a una columna de tipo de datos espacial o a una columna de tipo de datos **hierarchyID**. Las comparaciones que utilizan literales NULL no se admiten con los operadores de comparación. En su lugar, use los operadores IS NULL e IS NOT NULL.  
  
 Los siguientes son algunos ejemplos de predicados de filtro para la tabla Production.BillOfMaterials:  
  
 * `WHERE StartDate > '20000101' AND EndDate <= '20000630'`  
  
 * `WHERE ComponentID IN (533, 324, 753)`  
  
 * `WHERE StartDate IN ('20000404', '20000905') AND EndDate IS NOT NULL`  
  
 Para obtener más información sobre los índices filtrados, vea [Crear índices filtrados](../../relational-databases/indexes/create-filtered-indexes.md).  
  
 FULLSCAN  
 Calcula estadísticas examinando todas las filas. FULLSCAN y SAMPLE 100 PERCENT tienen los mismos resultados. FULLSCAN no se puede utilizar con la opción SAMPLE.  
  
 Cuando se omite, SQL Server utiliza el muestreo para crear las estadísticas y determina el tamaño de la muestra que se requiere para crear un plan de consulta de alta calidad.  
  
 SAMPLE *number* { PERCENT | ROWS }  
 Especifica el porcentaje o número de filas aproximado de la tabla o vista indizada que usa el optimizador de consultas al crear las estadísticas. En PERCENT, *number* puede tener un valor comprendido entre 0 y 100, mientras que en ROWS, *number* puede tener un valor comprendido entre 0 y el número total de filas. El porcentaje o número de filas real de los ejemplos del optimizador de consultas podría no coincidir con el porcentaje o el número especificado. Por ejemplo, el optimizador de consultas examina todas las filas en una página de datos.  
  
 SAMPLE es útil para los casos especiales en los que el plan de consulta, basado en el muestreo predeterminado, no es óptimo. En la mayoría de las situaciones, no es necesario especificar SAMPLE porque el optimizador de consultas ya utiliza el muestreo y determina el tamaño de muestra estadísticamente significativo de forma predeterminada, tal y como se exige para crear planes de consulta de alta calidad.  
  
 SAMPLE no se puede utilizar con la opción FULLSCAN. Cuando no se especifica SAMPLE ni FULLSCAN, el optimizador de consultas utiliza los datos muestreados y calcula el tamaño de la muestra de forma predeterminada.  
  
 Recomendamos no especificar 0 PERCENT ni 0 ROWS. Cuando se especifican 0 PERCENT o ROWS, el objeto de estadísticas se crea pero no contiene datos de estadísticas.  
 
 PERSIST_SAMPLE_PERCENT = { ON | OFF }  
 Cuando es **ON**, las estadísticas conservan el porcentaje de muestreo de la creación para las actualizaciones posteriores que no especifican explícitamente un porcentaje de muestreo. Cuando es **OFF**, el porcentaje de muestreo de estadísticas se restablece al muestreo predeterminado en actualizaciones posteriores que no especifiquen explícitamente un porcentaje de muestreo. El valor predeterminado es **OFF**. 
 
 > [!NOTE]
 > Si se trunca la tabla, todas las estadísticas creadas en el HoBT truncado volverán a usar el porcentaje de muestreo predeterminado.

 **Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU4) y versiones posteriores (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU1)    
  
 STATS_STREAM **=** _stats_stream_  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 NORECOMPUTE  
 Deshabilite la opción automática de actualización de las estadísticas, AUTO_UPDATE_STATISTICS, para *statistics_name*. Si se especifica esta opción, el optimizador de consultas finalizará cualquier actualización de las estadísticas que se esté realizando para *statistics_name* y deshabilitará las actualizaciones futuras.  
  
 Para volver a habilitar las actualizaciones de las estadísticas, quite las estadísticas con [DROP STATISTICS](../../t-sql/statements/drop-statistics-transact-sql.md) y, a continuación, ejecute CREATE STATISTICS sin la opción NORECOMPUTE.  
  
> [!WARNING]  
> Utilizar esta opción puede producir planes de consulta poco óptimos. Se recomienda usar esta opción con moderación y que lo haga únicamente un administrador de sistemas cualificado.  
  
 Para obtener más información sobre la opción AUTO_STATISTICS_UPDATE, vea [Opciones de ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md). Para obtener más información sobre cómo deshabilitar y volver a habilitar las actualizaciones de estadísticas, vea [Estadísticas](../../relational-databases/statistics/statistics.md).  
  
 INCREMENTAL = { ON | OFF }  
 Cuando se establece en **ON**, se crean estadísticas por cada partición. Cuando se establece en **OFF**, se combinan las estadísticas de todas las particiones. El valor predeterminado es **OFF**.  
  
 Si no se admiten las estadísticas por partición, se genera un error. Las estadísticas incrementales no se admiten para los siguientes tipos de estadísticas:  
  
-   Estadísticas creadas con índices que no están alineados por partición con la tabla base.  
-   Estadísticas creadas sobre bases de datos secundarias legibles AlwaysOn.  
-   Estadísticas creadas sobre bases de datos de solo lectura.  
-   Estadísticas creadas sobre índices filtrados.  
-   Estadísticas creadas sobre vistas.  
-   Estadísticas creadas sobre tablas internas.  
-   Estadísticas creadas con índices espaciales o índices XML.  
  
**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.  
  
MAXDOP = *max_degree_of_parallelism*  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3).  
  
 Reemplaza la opción de configuración **max degree of parallelism** durante la operación estadística. Para obtener más información, vea [Establecer la opción de configuración del servidor Grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). Utilice MAXDOP para establecer un límite para el número de procesadores utilizados en la ejecución de un plan paralelo. El máximo es 64 procesadores.  
  
 *max_degree_of_parallelism* puede tener estos valores:  
  
 1  
 Suprime la generación de planes paralelos.  
  
 \>1  
 Restringe el número máximo de procesadores usados en una operación estadística paralela al número especificado o a un número inferior, en función de la carga de trabajo actual del sistema.  
  
 0 (predeterminado)  
 Usa el número real de procesadores o menos, según la carga de trabajo actual del sistema.  
  
 \<update_stats_stream_option> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  

## <a name="permissions"></a>Permisos  
 Se requiere uno de estos permisos:  
  
-   ALTER TABLE  
-   El usuario es el propietario de la tabla  
-   Pertenencia al rol fijo de base de **db_ddladmin**.  
  
## <a name="general-remarks"></a>Notas generales  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede usar tempdb para ordenar las filas buscadas antes de crear las estadísticas.  
  
### <a name="statistics-for-external-tables"></a>Estadísticas para tablas externas  
 Al crear las estadísticas de tabla externa, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] importa la tabla externa en una tabla [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] temporal y crea las estadísticas. En las estadísticas de muestra, solo se importan las filas muestreadas. Si la tabla externa es grande, es mucho más rápido utilizar el muestreo predeterminado en vez de la opción de examen completo.  
  
### <a name="statistics-with-a-filtered-condition"></a>Estadísticas con una condición de filtrado  
 Las estadísticas filtradas pueden mejorar el rendimiento de las consultas que se seleccionan desde subconjuntos de datos bien definidos. Las estadísticas filtradas utilizan un predicado de filtro de la cláusula WHERE para seleccionar el subconjunto de datos que se incluye en las estadísticas.  
  
### <a name="when-to-use-create-statistics"></a>Cuándo utilizar CREATE STATISTICS  
 Para obtener más información sobre cuándo usar CREATE STATISTICS, vea [Estadísticas](../../relational-databases/statistics/statistics.md).  
  
### <a name="referencing-dependencies-for-filtered-statistics"></a>Hacer referencia a las dependencias para las estadísticas filtradas  
 La vista de catálogo [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) realiza el seguimiento de cada columna en la expresión del predicado de estadísticas filtradas como una dependencia de referencia. Tenga en cuenta las operaciones que realiza en las columnas de la tabla antes de crear estadísticas filtradas, porque no puede quitar, modificar, cambiar el nombre de la definición de una columna de tabla definida en un predicado de estadísticas filtradas.  
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
* No se admite la actualización de estadísticas en tablas externas. Para actualizar las estadísticas en una tabla externa, quite las estadísticas y vuelva a crearlas.  
* Puede mostrar hasta 64 columnas por objeto de estadísticas.
* La opción MAXDOP no es compatible con opciones STATS_STREAM, ROWCOUNT y PAGECOUNT.
* La opción MAXDOP está limitada por la configuración MaX_DOP del grupo de cargas de trabajo de Resource Governor, si se usa.
* En Azure SQL Database no se admiten CREATE y DROP STATISTICS en las tablas externas.
  
## <a name="examples"></a>Ejemplos  

### <a name="examples-use-the-adventureworks-database"></a>En los ejemplos se usa la base de datos de AdventureWorks.  

### <a name="a-using-create-statistics-with-sample-number-percent"></a>A. Utilizar CREATE STATISTICS con SAMPLE number PERCENT  
 En el ejemplo siguiente, se crean las estadísticas `ContactMail1` a partir de una muestra aleatoria del 5 por ciento de las columnas `BusinessEntityID` y `EmailPromotion` de la tabla `Person` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
CREATE STATISTICS ContactMail1  
    ON Person.Person (BusinessEntityID, EmailPromotion)  
    WITH SAMPLE 5 PERCENT;  
```  
  
### <a name="b-using-create-statistics-with-fullscan-and-norecompute"></a>B. Utilizar CREATE STATISTICS con FULLSCAN y NORECOMPUTE  
 En el ejemplo siguiente se crean las estadísticas `NamePurchase` para todas las filas de las columnas `BusinessEntityID` y `EmailPromotion` de la tabla `Person` y se deshabilita la posibilidad de volver a calcular las estadísticas automáticamente.  
  
```sql  
CREATE STATISTICS NamePurchase  
    ON AdventureWorks2012.Person.Person (BusinessEntityID, EmailPromotion)  
    WITH FULLSCAN, NORECOMPUTE;  
```  
  
### <a name="c-using-create-statistics-to-create-filtered-statistics"></a>C. Utilizar CREATE STATISTICS para crear estadísticas filtradas  
 En el ejemplo siguiente se crean las estadísticas filtradas `ContactPromotion1`. El [!INCLUDE[ssDE](../../includes/ssde-md.md)] muestrea el 50 por ciento de los datos y, a continuación, selecciona las filas cuyo valor `EmailPromotion` es igual a 2.  
  
```sql  
CREATE STATISTICS ContactPromotion1  
    ON Person.Person (BusinessEntityID, LastName, EmailPromotion)  
WHERE EmailPromotion = 2  
WITH SAMPLE 50 PERCENT;  
GO  
```  
  
### <a name="d-create-statistics-on-an-external-table"></a>D. Crear estadísticas en una tabla externa  
 La única decisión que debe tomar al crear las estadísticas en una tabla externa, además de proporcionar la lista de columnas, es si desea crear las estadísticas mediante el muestreo de las filas o examinando todas las filas. En Azure SQL Database no se admiten CREATE y DROP STATISTICS en las tablas externas.
  
 Puesto que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] importa datos de la tabla externa en una tabla temporal para crear estadísticas, la opción de examen completo tardará mucho más tiempo. En una tabla grande, el método de muestreo predeterminado generalmente es suficiente.  
  
```sql  
--Create statistics on an external table and use default sampling.  
CREATE STATISTICS CustomerStats1 ON DimCustomer (CustomerKey, EmailAddress);  
  
--Create statistics on an external table and scan all the rows  
CREATE STATISTICS CustomerStats1 ON DimCustomer (CustomerKey, EmailAddress) WITH FULLSCAN;  
```  

### <a name="e-using-create-statistics-with-fullscan-and-persist_sample_percent"></a>E. Utilizar CREATE STATISTICS con FULLSCAN y PERSIST_SAMPLE_PERCENT  
 En el ejemplo siguiente se crean las estadísticas `NamePurchase` para todas las filas de las columnas `BusinessEntityID` y `EmailPromotion` de la tabla `Person`, y se establece un porcentaje de muestreo del 100 % para todas las actualizaciones siguientes en las que no se especifique un porcentaje de muestreo de forma explícita.  
  
```sql  
CREATE STATISTICS NamePurchase  
    ON AdventureWorks2012.Person.Person (BusinessEntityID, EmailPromotion)  
    WITH FULLSCAN, PERSIST_SAMPLE_PERCENT = ON;  
```  
  
### <a name="examples-using-adventureworksdw-database"></a>Ejemplos en los que se usa la base de datos AdventureWorksDW. 
  
### <a name="f-create-statistics-on-two-columns"></a>F. Crear estadísticas en dos columnas  
 En el ejemplo siguiente se crean estadísticas de `CustomerStats1` basadas en las columnas `CustomerKey` y `EmailAddress` de la tabla `DimCustomer`. Las estadísticas se crean en función de un muestreo estadísticamente significativo de las filas de la tabla `Customer`.  
  
```sql  
CREATE STATISTICS CustomerStats1 ON DimCustomer (CustomerKey, EmailAddress);  
```  
  
### <a name="g-create-statistics-by-using-a-full-scan"></a>G. Crear estadísticas mediante un examen completo  
 En este ejemplo se crean las estadísticas de `CustomerStatsFullScan`, en función del examen de todas las filas de la tabla `DimCustomer`.  
  
```sql  
CREATE STATISTICS CustomerStatsFullScan 
ON DimCustomer (CustomerKey, EmailAddress) WITH FULLSCAN;  
```  
  
### <a name="h-create-statistics-by-specifying-the-sample-percentage"></a>H. Crear las estadísticas especificando el porcentaje de muestreo  
 En este ejemplo se crean las estadísticas de `CustomerStatsSampleScan`, en función del examen del 50 % de las filas de la tabla `DimCustomer`.  
  
```sql  
CREATE STATISTICS CustomerStatsSampleScan 
ON DimCustomer (CustomerKey, EmailAddress) WITH SAMPLE 50 PERCENT;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Utilizar las estadísticas para mejorar el rendimiento de las consultas](../../relational-databases/statistics/statistics.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [sp_updatestats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
 [sys.stats_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)  
  
  

