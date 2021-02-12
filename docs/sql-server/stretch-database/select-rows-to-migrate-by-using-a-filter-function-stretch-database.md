---
description: Seleccionar las filas que se van a migrar mediante una función de filtro (Stretch Database)
title: Selección de las filas que se van a migrar mediante una función de filtro
ms.date: 06/27/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, predicates
- predicates for Stretch Database
- Stretch Database, inline table-valued functions
- inline table-valued functions for Stretch Database
ms.assetid: 090890ee-7620-4a08-8e15-d2fbc71dd12f
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 72162b4ed6339bcf7939e1903886170e3df36e63
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030917"
---
# <a name="select-rows-to-migrate-by-using-a-filter-function-stretch-database"></a>Seleccionar las filas que se van a migrar mediante una función de filtro (Stretch Database)
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Si almacena los datos inactivos en una tabla independiente, puede configurar Stretch Database para migrar toda la tabla. Por otro lado, si la tabla contiene datos activos e inactivos, puede especificar un predicado de filtro para seleccionar las filas que se migrarán. El predicado de filtro es una función insertada con valores de tabla. En este artículo se describe cómo escribir una función insertada con valores de tabla para seleccionar las filas que se migrarán.  
  
> [!IMPORTANT]
> Si se indica una función de filtro que tiene un rendimiento bajo, la migración de datos también tendrá un rendimiento bajo. Stretch Database aplica la función de filtro a la tabla por medio del operador CROSS APPLY.  
  
 Si no se especifica una función de filtro, se migrará toda la tabla.  
  
 Cuando se ejecuta el Asistente para habilitar base de datos para Stretch, es posible migrar toda una tabla o especificar una función de filtro simple en el asistente. Si quiere usar un tipo de función de filtro diferente para seleccionar las filas que va a migrar, realice una de las siguientes acciones.  
  
-   Salga del asistente y ejecute la instrucción ALTER TABLE para habilitar Stretch para la tabla y especificar una función de filtro.  
  
-   Ejecute la instrucción ALTER TABLE para especificar una función de filtro después de salir del asistente.  
  
 Más adelante en este artículo se describe la sintaxis de ALTER TABLE para agregar una función.  
  
## <a name="basic-requirements-for-the-filter-function"></a>Requisitos básicos para la función de filtro  
 La función insertada con valores de tabla necesaria para un predicado de Stretch Database es parecida al ejemplo siguiente.  
  
```sql  
CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datatype1, @column2 datatype2 [, ...n])  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE <predicate>  
```  
  
 Los parámetros para la función deben ser identificadores para las columnas de la tabla.  
  
 Los enlaces de esquema son necesarios para impedir que se eliminen o modifiquen las columnas que usa la función de filtro.  
  
### <a name="return-value"></a>Valor devuelto  
 Si la función devuelve un resultado que no está vacío, la fila será apta para la migración. De lo contrario, si la función no devuelve ningún resultado, la fila no será apta para la migración.  
  
### <a name="conditions"></a>Condiciones  
 El &lt;*predicado*&gt; puede constar de una condición o de varias condiciones unidas con el operador lógico AND.  
  
```  
<predicate> ::= <condition> [ AND <condition> ] [ ...n ]  
```  
  
 A su vez, cada condición puede constar de una condición primitiva o de varias condiciones primitivas unidas con el operador lógico OR.  
  
```  
<condition> ::= <primitive_condition> [ OR <primitive_condition> ] [ ...n ]  
```  
  
### <a name="primitive-conditions"></a>Condiciones primitivas  
 Una condición primitiva permite realizar una de las comparaciones siguientes.  
  
```  
<primitive_condition> ::=   
{  
<function_parameter> <comparison_operator> constant  
| <function_parameter> { IS NULL | IS NOT NULL }  
| <function_parameter> IN ( constant [ ,...n ] )  
}  
  
```  
  
-   Comparar un parámetro de función con una expresión constante. Por ejemplo, `@column1 < 1000`.  
  
     En este ejemplo se comprueba si el valor de una columna *fecha* es &lt; 1/1/2016.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datetime)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)  
    GO  
  
    ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date),  
        MIGRATION_STATE = OUTBOUND  
    ) )  
  
    ```  
  
-   Aplicar el operador IS NULL o IS NOT NULL a un parámetro de función.  
  
-   Utilizar el operador IN para comparar un parámetro de función con una lista de valores constantes.  
  
     En este ejemplo se comprueba si el valor de una columna *estado_envío*  es `IN (N'Completed', N'Returned', N'Cancelled')`.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 nvarchar(15))  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 IN (N'Completed', N'Returned', N'Cancelled')  
    GO  
  
    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(shipment_status),  
        MIGRATION_STATE = OUTBOUND  
    ) )  
  
    ```  
  
### <a name="comparison-operators"></a>Operadores de comparación  
 Se admiten los siguientes operadores de comparación.  
  
 `<, <=, >, >=, =, <>, !=, !<, !>`  
  
```  
<comparison_operator> ::= { < | <= | > | >= | = | <> | != | !< | !> }  
```  
  
### <a name="constant-expressions"></a>Expresiones constantes  
 Las constantes que se usan en una función de filtro pueden ser cualquier expresión determinista que se puede evaluar cuando se define la función. Las expresiones constantes pueden contener lo siguiente.  
  
-   Literales. Por ejemplo, `N'abc', 123`.  
  
-   Expresiones algebraicas. Por ejemplo, `123 + 456`.  
  
-   Funciones deterministas. Por ejemplo, `SQRT(900)`.  
  
-   Conversiones deterministas con CAST o CONVERT. Por ejemplo, `CONVERT(datetime, '1/1/2016', 101)`.  
  
### <a name="other-expressions"></a>Otras expresiones  
 Se pueden usar los operadores BETWEEN y NOT BETWEEN si la función resultante cumple las reglas descritas aquí después de reemplazar los operadores BETWEEN y NOT BETWEEN por las expresiones AND y OR equivalentes.  
  
 No se pueden utilizar subconsultas o funciones no deterministas como RAND() o GETDATE().  
  
## <a name="add-a-filter-function-to-a-table"></a>Agregar una función de filtro a una tabla  
 Agregue una función de filtro a una tabla ejecutando la instrucción **ALTER TABLE** y especificando una función insertada con valores de tabla existente como valor del parámetro **FILTER_PREDICATE** . Por ejemplo:  
  
```sql  
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (  
    FILTER_PREDICATE = dbo.fn_stretchpredicate(column1, column2),  
    MIGRATION_STATE = <desired_migration_state>  
) )  
  
```  
  
 Después de enlazar la función a la tabla como predicado, las siguientes afirmaciones son verdaderas.  
  
-   La próxima vez que se produzca una migración de datos, solo se migrarán las filas para las que la función devuelva un valor que no esté vacío.  
  
-   Las columnas utilizadas por la función son columnas enlazadas a un esquema. Estas columnas no se podrán modificar mientras una tabla utilice la función como su predicado de filtro.  
  
 La función insertada con valores de tabla no se podrá eliminar mientras una tabla utilice la función como su predicado de filtro. 

> [!TIP]
> Para mejorar el rendimiento de la función de filtro, cree un índice en las columnas que usa la función.

 ### <a name="passing-column-names-to-the-filter-function"></a>Pasar nombres de columna a la función de filtro
 
 Al asignar una función de filtro a una tabla, especifique los nombres de columna que se pasan a la función de filtro con un nombre compuesto por una sola parte. Si especifica un nombre con tres partes cuando pase los nombres de columna, se producirá un error en las consultas posteriores en la tabla habilitada para Stretch.

Por ejemplo, si especifica un nombre de columna con tres partes, como se muestra en el ejemplo siguiente, la instrucción se ejecutará correctamente, pero se producirá un error en las consultas posteriores en la tabla.

```sql
ALTER TABLE SensorTelemetry 
  SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(dbo.SensorTelemetry.ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

En su lugar, debe especificar la función de filtro con un nombre de columna compuesto por una sola parte, tal como se muestra en el ejemplo siguiente.

```sql
ALTER TABLE SensorTelemetry 
  SET ( REMOTE_DATA_ARCHIVE = ON  (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```
  
## <a name="add-a-filter-function-after-running-the-wizard"></a><a name="addafterwiz"></a>Agregar una función de filtro después de ejecutar el asistente  
  
Si quiere usar una función que no se puede crear en el Asistente para **habilitar base de datos para Stretch** , puede ejecutar la instrucción **ALTER TABLE** para especificar una función después de salir del asistente. No obstante, antes de aplicar una función, tendrá que detener la migración de datos que ya está en curso y devolver los datos migrados. (Para obtener más información en la que se explica por qué esto es necesario, vea [Reemplazar una función de filtro existente](#replacePredicate)).
  
1. Invierta la dirección de la migración y devuelva los datos ya migrados. Una vez que se haya iniciado esta operación, no se puede cancelar. Además, incurrirá en costos en Azure para realizar las transferencias de datos de salida. Para obtener más información, vea los detalles [de los precios de Azure](https://azure.microsoft.com/pricing/details/data-transfers/).  
  
    ```sql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;   
    ```  
  
2. Espere a que finalice la migración. Puede comprobar el estado en **Stretch Database Monitor** en SQL Server Management Studio, o bien puede consultar la vista **sys.dm_db_rda_migration_status** . Para obtener más información, vea [Supervisión y solución de problemas de migración de datos](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md) o [sys.dm_db_rda_migration_status](../../relational-databases/system-dynamic-management-views/stretch-database-sys-dm-db-rda-migration-status.md).  
  
3. Cree la función de filtro que quiere aplicar a la tabla.  
  
4. Agregue la función a la tabla y reinicie la migración de datos a Azure.  
  
    ```sql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE  
            (           
                FILTER_PREDICATE = <predicate>,  
                MIGRATION_STATE = OUTBOUND  
            )  
            );   
    ```  
  
## <a name="filter-rows-by-date"></a>Filtrar filas por fecha  
 En el ejemplo siguiente se migran filas en las que la columna **fecha** contiene un valor anterior al 1 de enero de 2016.  
  
```sql  
-- Filter by date  
--  
CREATE FUNCTION dbo.fn_stretch_by_date(@date datetime2)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
       RETURN SELECT 1 AS is_eligible WHERE @date < CONVERT(datetime2, '1/1/2016', 101)  
GO  
  
```  
  
## <a name="filter-rows-by-the-value-in-a-status-column"></a>Filtrar filas por el valor de la columna de estado  
 En el ejemplo siguiente se migran filas en las que la columna **estado** contiene uno de los valores especificados.  
  
```sql  
-- Filter by status column  
--  
CREATE FUNCTION dbo.fn_stretch_by_status(@status nvarchar(128))  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
       RETURN SELECT 1 AS is_eligible WHERE @status IN (N'Completed', N'Returned', N'Cancelled')  
GO  
  
```  
  
## <a name="filter-rows-by-using-a-sliding-window"></a>Filtrar filas mediante una ventana deslizante  
 Para filtrar filas mediante una ventana deslizante, tenga en cuenta los requisitos siguientes para la función de filtro.  
  
-   La función debe ser determinista. Por lo tanto, no se puede crear una función que actualice automáticamente la ventana deslizante con el paso del tiempo.  
  
-   La función usa enlaces de esquema. Por lo tanto, no se puede simplemente actualizar la función "en contexto" cada día llamando a **ALTER FUNCTION** para mover la ventana deslizante.  
  
 Empiece con una función de filtro parecida al ejemplo siguiente, que migra las filas en las que la columna **systemEndTime** contiene un valor anterior al 1 de enero de 2016.  
  
```sql  
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160101(@systemEndTime datetime2)   
RETURNS TABLE   
WITH SCHEMABINDING    
AS    
RETURN SELECT 1 AS is_eligible   
  WHERE @systemEndTime < CONVERT(datetime2, '2016-01-01T00:00:00', 101) ;  
  
```  
  
 Aplique la función de filtro a la tabla.  
  
```sql  
ALTER TABLE <table name>   
SET (   
        REMOTE_DATA_ARCHIVE = ON   
                (   
                        FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160101(SysEndTime)  
                                , MIGRATION_STATE = OUTBOUND   
                )  
        )   
;  
  
```  
  
 Para actualizar la ventana deslizante, proceda del modo siguiente.  
  
1.  Cree una nueva función que especifique la nueva ventana deslizante. En el ejemplo siguiente se seleccionan fechas anteriores al 2 de enero de 2016, en lugar del 1 de enero de 2016.  
  
2.  Reemplace la función de filtro anterior por la nueva llamando a **ALTER TABLE**, como se muestra en el ejemplo siguiente.  
  
3.  Opcionalmente, puede eliminar la función de filtro anterior que ya no utiliza llamando a **DROP FUNCTION**. (Este paso no se muestra en el ejemplo).  
  
```sql  
BEGIN TRAN  
GO  
        /*(1) Create new predicate function definition */  
        CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160102(@systemEndTime datetime2)  
        RETURNS TABLE  
        WITH SCHEMABINDING   
        AS   
        RETURN SELECT 1 AS is_eligible  
               WHERE @systemEndTime < CONVERT(datetime2,'2016-01-02T00:00:00', 101)  
        GO  
  
        /*(2) Set the new function as the filter predicate */  
        ALTER TABLE <table name>  
        SET   
        (  
               REMOTE_DATA_ARCHIVE = ON  
               (  
                       FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160102(SysEndTime),  
                       MIGRATION_STATE = OUTBOUND  
               )  
        )   
COMMIT ;  
  
```  
  
## <a name="more-examples-of-valid-filter-functions"></a>Más ejemplos de funciones de filtro válidas  
  
-   En el ejemplo siguiente se combinan dos condiciones primitivas con el operador lógico AND.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate((@column1 datetime, @column2 nvarchar(15))  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
      WHERE @column1 < N'20150101' AND @column2 IN (N'Completed', N'Returned', N'Cancelled')  
    GO  
  
    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date, shipment_status),  
        MIGRATION_STATE = OUTBOUND  
    ) )  
  
    ```  
  
-   En el ejemplo siguiente se utilizan varias condiciones y una conversión determinista con CONVERT.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate_example1(@column1 datetime, @column2 int, @column3 nvarchar)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101) AND (@column2 < -100 OR @column2 > 100 OR @column2 IS NULL) AND @column3 IN (N'Completed', N'Returned', N'Cancelled')  
    GO  
  
    ```  
  
-   En el ejemplo siguiente se utilizan funciones y operadores matemáticos.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate_example2(@column1 float)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 < SQRT(400) + 10  
    GO  
  
    ```  
  
-   En el ejemplo siguiente se usan los operadores BETWEEN y NOT BETWEEN. Este uso es válido porque la función resultante cumple las reglas descritas aquí después de reemplazar los operadores BETWEEN y NOT BETWEEN por las expresiones AND y OR equivalentes.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate_example3(@column1 int, @column2 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 BETWEEN 0 AND 100  
                AND (@column2 NOT BETWEEN 200 AND 300 OR @column1 = 50)  
    GO  
  
    ```  
  
     La función anterior es equivalente a la función siguiente después de reemplazar los operadores BETWEEN y NOT BETWEEN por expresiones AND y OR equivalentes.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate_example4(@column1 int, @column2 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 >= 0 AND @column1 <= 100 AND (@column2 < 200 OR @column2 > 300 OR @column1 = 50)  
    GO  
  
    ```  
  
## <a name="examples-of-filter-functions-that-arent-valid"></a>Ejemplos de funciones de filtro no válidas  
  
-   La función siguiente no es válida porque contiene una conversión no determinista.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example5(@column1 datetime)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 < CONVERT(datetime, '1/1/2016')  
    GO  
  
    ```  
  
-   La función siguiente no es válida porque contiene una llamada de función no determinista.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example6(@column1 datetime)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 < DATEADD(day, -60, GETDATE())  
    GO  
  
    ```  
  
-   La función siguiente no es válida porque contiene una subconsulta.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example7(@column1 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 IN (SELECT SupplierID FROM Supplier WHERE Status = 'Defunct')  
    GO  
  
    ```  
  
-   Las funciones siguientes no son válidas porque las expresiones que utilizan operadores algebraicos o funciones integradas se deben evaluar como constante cuando se define la función. No puede incluir referencias de columna en expresiones algebraicas o llamadas de función.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example8(@column1 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 % 2 =  0  
    GO  
  
    CREATE FUNCTION dbo.fn_example9(@column1 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE SQRT(@column1) = 30  
    GO  
  
    ```  
  
-   La función siguiente no es válida porque infringe las reglas descritas aquí después de reemplazar el operador BETWEEN por la expresión AND equivalente.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example10(@column1 int, @column2 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING  
    AS  
    RETURN  SELECT 1 AS is_eligible  
            WHERE (@column1 BETWEEN 1 AND 200 OR @column1 = 300) AND @column2 > 1000  
    GO  
  
    ```  
  
     La función anterior es equivalente a la función siguiente después de reemplazar el operador BETWEEN por la expresión AND equivalente. Esta función no es válida porque las condiciones primitivas solo pueden utilizar el operador lógico OR.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example11(@column1 int, @column2 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE (@column1 >= 1 AND @column1 <= 200 OR @column1 = 300) AND @column2 > 1000  
    GO  
  
    ```  
  
## <a name="how-stretch-database-applies-the-filter-function"></a>Cómo aplica Stretch Database la función de filtro  
 Stretch Database aplica la función de filtro a la tabla y determina las filas aptas mediante el operador CROSS APPLY. Por ejemplo:  
  
```sql  
SELECT * FROM stretch_table_name CROSS APPLY fn_stretchpredicate(column1, column2)  
```  
  
 Si la función devuelve un resultado no vacío para la fila, la fila será apta para la migración.  
  
## <a name="replace-an-existing-filter-function"></a><a name="replacePredicate"></a>Reemplazar una función de filtro existente  
 Para reemplazar una función de filtro especificada anteriormente, vuelva a ejecutar la instrucción **ALTER TABLE** y especifique un nuevo valor para el parámetro **FILTER_PREDICATE** . Por ejemplo:  
  
```sql  
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (  
    FILTER_PREDICATE = dbo.fn_stretchpredicate2(column1, column2),  
    MIGRATION_STATE = <desired_migration_state>  
    ) ) 
  
```  
  
 La nueva función insertada con valores de tabla tiene los requisitos siguientes.  
  
-   La nueva función debe ser menos restrictiva que la anterior.  
  
-   Todos los operadores de la función anterior deben existir en la nueva.  
  
-   La función nueva no puede contener operadores que no existan en la anterior.  
  
-   No se puede cambiar el orden de los argumentos del operador.  
  
-   Solo los valores constantes que forman parte de una comparación de `<, <=, >, >=`  se pueden cambiar de forma que la función sea menos restrictiva.  
  
### <a name="example-of-a-valid-replacement"></a>Ejemplo de un reemplazo válido  
 Suponga que la función siguiente es la función de filtro actual.  
  
```sql  
CREATE FUNCTION dbo.fn_stretchpredicate_old(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)  
            AND (@column2 < -100 OR @column2 > 100)  
GO  
  
```  
  
 La función siguiente es un reemplazo válido porque la nueva constante de fecha (que especifica una fecha límite posterior) hace que la función sea menos restrictiva.  
  
```sql  
CREATE FUNCTION dbo.fn_stretchpredicate_new(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '2/1/2016', 101)  
            AND (@column2 < -50 OR @column2 > 50)  
GO  
  
```  
  
### <a name="examples-of-replacements-that-arent-valid"></a>Ejemplos de reemplazos no válidos  
 La función siguiente no es un reemplazo válido porque la nueva constante de fecha (que especifica una fecha límite anterior) no hace que la función sea menos restrictiva.  
  
```sql  
CREATE FUNCTION dbo.fn_notvalidreplacement_1(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)  
            AND (@column2 < -100 OR @column2 > 100)  
GO  
  
```  
  
 La función siguiente no es un reemplazo válido porque se ha eliminado uno de los operadores de comparación.  
  
```sql  
CREATE FUNCTION dbo.fn_notvalidreplacement_2(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)  
            AND (@column2 < -50)  
GO  
  
```  
  
 La función siguiente no es un reemplazo válido porque se ha agregado una nueva condición con el operador lógico AND.  
  
```sql  
CREATE FUNCTION dbo.fn_notvalidreplacement_3(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)  
            AND (@column2 < -100 OR @column2 > 100)  
            AND (@column2 <> 0)  
GO  
  
```  
  
## <a name="remove-a-filter-function-from-a-table"></a>Quitar una función de filtro de una tabla  
 Para migrar toda la tabla en lugar de solo las filas seleccionadas, debe quitar la función existente. Para ello, establezca el parámetro **FILTER_PREDICATE**  como NULL. Por ejemplo:  
  
```sql  
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (  
    FILTER_PREDICATE = NULL,  
    MIGRATION_STATE = <desired_migration_state>  
) )  
  
```  
  
 Después de quitar la función de filtro, todas las filas de la tabla son aptas para la migración. Por consiguiente, no se podrá especificar una función de filtro para la misma tabla posteriormente, a menos que primero se recuperen de Azure todos los datos remotos de la tabla. Esta restricción se aplica para evitar los casos en los que las filas que no son aptas para la migración cuando se proporciona una nueva función de filtro ya se hayan migrado a Azure.  
  
## <a name="check-the-filter-function-applied-to-a-table"></a>Comprobar la función de filtro aplicada a una tabla  
 Para comprobar la función de filtro aplicada a una tabla, abra la vista de catálogo **sys.remote_data_archive_tables** y compruebe el valor de la columna **filter_predicate** . Si el valor es nulo, toda la tabla será apta para el archivado. Para obtener más información, vea [sys.remote_data_archive_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-tables.md).  
  
## <a name="security-notes-for-filter-functions"></a>Notas de seguridad para las funciones de filtro  
Una cuenta en peligro con privilegios db_owner puede hacer lo siguiente.  
  
-   Crear y aplicar una función con valores de tabla que use gran cantidad de recursos del servidor o que espere durante un período prolongado, lo que provoca una denegación de servicio.  
  
-   Crear y aplicar una función con valores de tabla que permite deducir el contenido de una tabla para la que el usuario ha denegado explícitamente el acceso de lectura.  
  
## <a name="see-also"></a>Consulte también  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
  
  
