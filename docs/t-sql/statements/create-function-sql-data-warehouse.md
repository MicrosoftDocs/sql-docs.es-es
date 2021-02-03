---
description: CREATE FUNCTION (Azure Synapse Analytics)
title: CREATE FUNCTION (Azure Synapse Analytics) | Microsoft Docs
ms.custom: ''
ms.date: 09/17/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 8cad1b2c-5ea0-4001-9060-2f6832ccd057
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: ffa098db4ca61746a3301702cd526498f0cd2f85
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191022"
---
# <a name="create-function-azure-synapse-analytics"></a>CREATE FUNCTION (Azure Synapse Analytics)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Crea una función definida por el usuario en [!INCLUDE[ssSDW](../../includes/ssazuresynapse_md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Una función definida por el usuario es una rutina de [!INCLUDE[tsql](../../includes/tsql-md.md)] que acepta parámetros, realiza una acción, como un cálculo complejo, y devuelve el resultado de esa acción como un valor. En [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], el valor devuelto debe ser un valor escalar (único). En [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], CREATE FUNCTION puede devolver una tabla con la sintaxis de las funciones insertadas con valores de tabla (versión preliminar) o puede devolver un valor único mediante la sintaxis de las funciones escalares. Utilice esta instrucción para crear una rutina reutilizable que se pueda utilizar de estas formas:  
  
-   En instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] como SELECT  
  
-   En las aplicaciones que llaman a la función  
  
-   En la definición de otra función definida por el usuario  
  
-   Para definir una restricción CHECK en una columna  
  
-   Para reemplazar un procedimiento almacenado  
  
-   Usar una función insertada como predicado de filtro de la directiva de seguridad  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Transact-SQL Scalar Function Syntax  (in Azure Synapse Analytics and Parallel Data Warehouse)
CREATE FUNCTION [ schema_name. ] function_name   
( [ { @parameter_name [ AS ] parameter_data_type   
    [ = default ] }   
    [ ,...n ]  
  ]  
)  
RETURNS return_data_type  
    [ WITH <function_option> [ ,...n ] ]  
    [ AS ]  
    BEGIN   
        function_body   
        RETURN scalar_expression  
    END  
[ ; ]  
  
<function_option>::=   
{  
    [ SCHEMABINDING ]  
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]  
}  
```

```syntaxsql
-- Transact-SQL Inline Table-Valued Function Syntax (Preview in Azure Synapse Analytics only)
CREATE FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ] parameter_data_type
    [ = default ] }
    [ ,...n ]
  ]
)
RETURNS TABLE
    [ WITH SCHEMABINDING ]
    [ AS ]
    RETURN [ ( ] select_stmt [ ) ]
[ ; ]
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Argumentos  
 *schema_name*  
 Nombre del esquema al que pertenece la función definida por el usuario.  
  
 *function_name*  
 Nombre de la función definida por el usuario. Los nombres de función deben cumplir las reglas de los identificadores y deben ser únicos en la base de datos y para su esquema.  
  
> [!NOTE]  
>  Los paréntesis después del nombre de la función son necesarios, aunque no se especifique un parámetro.  
  
 @*parameter_name*  
 Es un parámetro de la función definida por el usuario. Es posible declarar uno o varios parámetros.  
  
 Una función puede tener un máximo de 2.100 parámetros. El usuario debe proporcionar el valor de cada parámetro declarado cuando se ejecuta la función, a menos que se defina un valor predeterminado para el parámetro.  
  
 Especifique un nombre de parámetro con una arroba (@) como primer carácter. El nombre del parámetro debe cumplir las mismas reglas que los identificadores. Los parámetros son locales para la función; los mismos nombres de parámetro se pueden utilizar en otras funciones. Los parámetros solamente pueden ocupar el lugar de constantes; no se pueden utilizar en lugar de nombres de tablas, nombres de columnas o nombres de otros objetos de base de datos.  
  
> [!NOTE]  
>  ANSI_WARNINGS no se respeta al pasar parámetros en un procedimiento almacenado o una función definida por el usuario, ni cuando se declaran y se establecen variables en una instrucción por lotes. Por ejemplo, si una variable se define como **char(3)** y después se establece en un valor de más de tres caracteres, los datos se truncan hasta el tamaño definido y la instrucción INSERT o UPDATE se ejecuta correctamente.  
  
 *parameter_data_type*  
 Es el tipo de datos del parámetro. En las funciones de [!INCLUDE[tsql](../../includes/tsql-md.md)], se permiten todos los tipos de datos escalares admitidos en [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. El tipo de datos timestamp (rowversion) no es un tipo admitido.  
  
 [ =*default* ]  
 Es un valor predeterminado para el parámetro. Si se define un valor *default*, la función se puede ejecutar sin especificar un valor para ese parámetro.  
  
 Cuando un parámetro de la función tiene un valor predeterminado, se debe especificar la palabra clave DEFAULT al llamar a la función para recuperar el valor predeterminado. Este comportamiento es distinto del uso de parámetros con valores predeterminados en los procedimientos almacenados, donde la omisión del parámetro implica especificar el valor predeterminado.  
  
 *return_data_type*  
 Es el valor devuelto de una función escalar definida por el usuario. En las funciones de [!INCLUDE[tsql](../../includes/tsql-md.md)], se permiten todos los tipos de datos escalares admitidos en [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. El tipo de datos timestamp (rowversion) no es un tipo admitido. No se permiten los tipos de datos no escalares de cursor y tabla.  
  
 *function_body*  
 Serie de instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)].  function_body no puede contener una instrucción SELECT ni hacer referencia a datos de la base de datos.  function_body no puede hacer referencia a tablas ni vistas. El cuerpo de la función puede llamar a otras funciones deterministas, pero no a funciones no deterministas. 
  
 En las funciones escalares, *function_body* es una serie de instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)] que se evalúan como un valor escalar.  
  
 *scalar_expression*  
 Especifica el valor escalar que devuelve la función escalar.  

 *select_stmt* **SE APLICA A**: Azure Synapse Analytics  
 Es la instrucción SELECT individual que define el valor devuelto de una función con valores de tabla insertados (versión preliminar).

 TABLE **SE APLICA A**: Azure Synapse Analytics  
 Especifica que el valor devuelto de la función con valores de tabla (TVF) es una tabla. Solamente se pueden pasar constantes y @*local_variables* a las TVF.

 En las funciones con valores de tabla insertados (versión preliminar), el valor devuelto de TABLE se define mediante una única instrucción SELECT. Las funciones insertadas no tienen variables devueltas asociadas.
  
 **\<function_option>::=** 
  
 Especifica que la función tendrá una o más de las siguientes opciones.  
  
 SCHEMABINDING  
 Especifica que la función está enlazada a los objetos de base de datos a los que hace referencia. Cuando se especifica SCHEMABINDING, los objetos base no se pueden modificar de una forma que afecte a la definición de la función. En primer lugar, se debe modificar o quitar la propia definición de la función para quitar las dependencias en el objeto que se va a modificar.  
  
 El enlace de la función a los objetos a los que hace referencia solamente se quita cuando se ejecuta una de estas acciones:  
  
-   Se quita la función.  
  
-   La función se modifica con la instrucción ALTER sin especificar la opción SCHEMABINDING.  
  
 Una función se puede enlazar a esquema solamente si se cumplen las siguientes condiciones:  
  
-   Las funciones definidas por el usuario a las que hace referencia la función también están enlazadas a esquema.  
  
-   Se hace referencia a las funciones y otras funciones definidas por el usuario a las que hace referencia la función mediante un nombre de una o dos partes.  
  
-   En el cuerpo de las funciones definidas por el usuario solo se puede hacer referencia a las funciones integradas y a otras funciones definidas por el usuario de la misma base de datos.  
  
-   El usuario que ejecutó la instrucción CREATE FUNCTION tiene permisos REFERENCES para los objetos de base de datos a los que hace referencia la función.  
  
 Para quitar SCHEMABINDING, use ALTER  
  
 RETURNS NULL ON NULL INPUT | **CALLED ON NULL INPUT**  
 Especifica el atributo **OnNULLCall** de una función con valores escalares. Si no se especifica, se utiliza CALLED ON NULL INPUT de manera predeterminada. Esto significa que el cuerpo de la función se ejecuta aunque se envíe NULL como argumento.  
  
## <a name="best-practices"></a>Procedimientos recomendados  
 Si una función definida por el usuario no se crea con la cláusula SCHEMABINDING, los cambios que se realicen en los objetos subyacentes pueden afectar a la definición de la función y generar resultados inesperados al invocarla. Recomendamos implementar uno de los siguientes métodos para garantizar que la función no queda sin actualizar como consecuencia de los cambios realizados en sus objetos subyacentes:  
  
-   Especifique la cláusula WITH SCHEMABINDING al crear la función. Así se asegura de que no se pueden modificar los objetos a los que se hace referencia en la definición de la función a menos que también se modifique la función.  
  
## <a name="interoperability"></a>Interoperabilidad  
 Las siguientes instrucciones son válidas en una función con valores escalares:  
  
-   Instrucciones de asignación.  
  
-   Instrucciones de control de flujo, excepto las instrucciones TRY...CATCH.  
  
-   Instrucciones DECLARE que definen variables de datos locales.  

En una función con valores de tabla insertados (versión preliminar), solo se permite una única instrucción SELECT.
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
 Las funciones definidas por el usuario no se pueden utilizar para realizar acciones que modifican el estado de la base de datos.  
  
 Las funciones definidas por el usuario se pueden anidar; es decir, una función definida por el usuario puede llamar a otra. El nivel de anidamiento aumenta cuando se empieza a ejecutar la función llamada y disminuye cuando se termina de ejecutar la función llamada. Las funciones definidas por el usuario se pueden anidar hasta un máximo de 32 niveles. Si se superan los niveles máximos de anidamiento, la cadena completa de funciones de llamada produce un error.   
  
## <a name="metadata"></a>Metadatos  
 En esta sección se enumeran las vistas de catálogo del sistema que se pueden usar para devolver metadatos sobre funciones definidas por el usuario.  
  
 [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md): Muestra la definición de funciones definidas por el usuario de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Por ejemplo:  
  
```sql  
SELECT definition, type   
FROM sys.sql_modules AS m  
JOIN sys.objects AS o   
    ON m.object_id = o.object_id   
    AND type = ('FN');  
GO  
  
```  
  
 [sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md): Muestra información sobre los parámetros definidos en funciones definidas por el usuario.  
  
 [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md): Muestra los objetos subyacentes a los que hace referencia una función.  
  
## <a name="permissions"></a>Permisos  
 Se requiere el permiso CREATE FUNCTION en la base de datos y el permiso ALTER en el esquema en el que se va a crear la función.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-using-a-scalar-valued-user-defined-function-to-change-a-data-type"></a>A. Usar una función con valores escalares definida por el usuario para modificar un tipo de datos  
 Esta función simple toma un tipo de datos **int** como entrada y devuelve un tipo de datos **decimal (10,2)** como salida.  
  
```sql  
CREATE FUNCTION dbo.ConvertInput (@MyValueIn int)  
RETURNS decimal(10,2)  
AS  
BEGIN  
    DECLARE @MyValueOut int;  
    SET @MyValueOut= CAST( @MyValueIn AS decimal(10,2));  
    RETURN(@MyValueOut);  
END;  
GO  
  
SELECT dbo.ConvertInput(15) AS 'ConvertedValue';  
```  

## <a name="examples-sssdwfull"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]  

### <a name="a-creating-an-inline-table-valued-function-preview"></a>A. Creación de una función con valores de tabla insertados (versión preliminar)
 En el ejemplo siguiente se crea una función con valores de tabla insertados para devolver cierta información clave sobre los módulos, filtrando por el parámetro `objectType`. Incluye un valor predeterminado para devolver todos los módulos cuando se llama a la función con el parámetro DEFAULT. En este ejemplo se usan algunas de las vistas de catálogo del sistema mencionadas en [Metadatos](#metadata).

```sql
CREATE FUNCTION dbo.ModulesByType(@objectType CHAR(2) = '%%')
RETURNS TABLE
AS
RETURN
(
    SELECT 
        sm.object_id AS 'Object Id',
        o.create_date AS 'Date Created',
        OBJECT_NAME(sm.object_id) AS 'Name',
        o.type AS 'Type',
        o.type_desc AS 'Type Description', 
        sm.definition AS 'Module Description'
    FROM sys.sql_modules AS sm  
    JOIN sys.objects AS o ON sm.object_id = o.object_id
    WHERE o.type like '%' + @objectType + '%'
);
GO
```
A continuación, se puede llamar a la función para devolver todos los objetos de vista (**V**) con:
```sql
select * from dbo.ModulesByType('V');
```

### <a name="b-combining-results-of-an-inline-table-valued-function-preview"></a>B. Combinación de los resultados de una función con valores de tabla insertados (versión preliminar)
 En este sencillo ejemplo se usa la función con valores de tabla insertados creada anteriormente para mostrar cómo se pueden combinar sus resultados con otras tablas mediante CROSS APPLY. Aquí se seleccionan todas las columnas de sys.objects y los resultados de `ModulesByType` de todas las filas que coincidan con la columna *type*. Para obtener más información sobre cómo usar APPLY, vea [Cláusula FROM más JOIN, APPLY, PIVOT](../../t-sql/queries/from-transact-sql.md).

```sql
SELECT * 
FROM sys.objects o
CROSS APPLY dbo.ModulesByType(o.type);
GO
```
  
## <a name="see-also"></a>Consulte también  
 [ALTER FUNCTION (SQL Server PDW)](/previous-versions/sql/)   
 [DROP FUNCTION (SQL Server PDW)](/previous-versions/sql/)  
  
  

