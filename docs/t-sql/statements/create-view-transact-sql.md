---
description: CREATE VIEW (Transact-SQL)
title: CREATE VIEW (Transact-SQL)
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
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
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1aeb0ce44bc48dbe208bad7ed58a94641c6e0ce9
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237029"
---
# <a name="create-view-transact-sql"></a>CREATE VIEW (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Crea una tabla virtual cuyo contenido (columnas y filas) se define mediante una consulta. Utilice esta instrucción para crear una vista de los datos de una o varias tablas de la base de datos. Por ejemplo, una vista se puede utilizar para lo siguiente:  
  
-   Para centrar, simplificar y personalizar la percepción de la base de datos para cada usuario.  
  
-   Como mecanismo de seguridad, que permite a los usuarios obtener acceso a los datos por medio de la vista, pero no les conceden el permiso de obtener acceso directo a las tablas base subyacentes de la vista.  
  
-   Para proporcionar una interfaz compatible con versiones anteriores para emular una tabla cuyo esquema ha cambiado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
CREATE [ OR ALTER ] VIEW [ schema_name . ] view_name [ (column [ ,...n ] ) ]
[ WITH <view_attribute> [ ,...n ] ]
AS select_statement
[ WITH CHECK OPTION ]
[ ; ]  
  
<view_attribute> ::=
{  
    [ ENCRYPTION ]  
    [ SCHEMABINDING ]  
    [ VIEW_METADATA ]
}
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE VIEW [ schema_name . ] view_name [  ( column_name [ ,...n ] ) ]   
AS <select_statement>   
[;]  
  
<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT <select_criteria>  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

OR ALTER  
 **Se aplica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1).   
  
 Altera la vista condicionalmente solo si ya existe. 
 
 *schema_name*  
 Es el nombre del esquema al que pertenece la vista.  
  
 *view_name*  
 Es el nombre de la vista. Los nombres de las vistas deben cumplir las reglas de los identificadores. La especificación del nombre del propietario de la vista es opcional.  
  
 *column*  
 Es el nombre que se va a utilizar para una columna en una vista. Solo se necesita un nombre de columna cuando una columna proviene de una expresión aritmética, una función o una constante; cuando dos o más columnas puedan tener el mismo nombre, normalmente debido a una combinación; o cuando una columna de una vista recibe un nombre distinto al de la columna de la que proviene. Los nombres de columna se pueden asignar también en la instrucción SELECT.  
  
 Si no se especifica el parámetro *column*, las columnas de la vista adquieren los mismos nombres que las columnas de la instrucción SELECT.  
  
> [!NOTE]  
>  En las columnas de la vista, los permisos de un nombre de columna se aplican mediante una instrucción CREATE VIEW o ALTER VIEW, independientemente del origen de los datos subyacentes. Por ejemplo, si se conceden permisos para la columna **SalesOrderID** en una instrucción CREATE VIEW, una instrucción ALTER VIEW puede denominar a la columna **SalesOrderID** con un nombre de columna distinto, por ejemplo **OrderRef**, y seguir teniendo los permisos asociados a la vista que utiliza **SalesOrderID**.  
  
 AS  
 Especifica las acciones que va a llevar a cabo la vista.  
  
 *select_statement*  
 Es la instrucción SELECT que define la vista. Dicha instrucción puede utilizar más de una tabla y otras vistas. Se necesitan permisos adecuados para seleccionar los objetos a los que se hace referencia en la cláusula SELECT de la vista que se ha creado.  
  
 Una vista no tiene por qué ser un simple subconjunto de filas y de columnas de una tabla determinada. Es posible crear una vista que utilice más de una tabla u otras vistas mediante una cláusula SELECT de cualquier complejidad.  
  
 En una definición de vista indizada, la instrucción SELECT debe ser una instrucción de una única tabla o una instrucción JOIN de varias tablas con agregación opcional.  
  
 Las cláusulas SELECT de una definición de vista no pueden incluir lo siguiente:  
  
-   Una cláusula ORDER BY, a menos que también haya una cláusula TOP en la lista de selección de la instrucción SELECT  
  
    > [!IMPORTANT]  
    >  La cláusula ORDER BY solo se usa para determinar las filas devueltas por la cláusula TOP u OFFSET en la definición de la vista. Esta cláusula no garantiza resultados ordenados cuando se consulte la vista, a menos que también se especifique ORDER BY en la propia consulta.  
  
-   La palabra clave INTO  
  
-   La cláusula OPTION  
  
-   Una referencia a una tabla temporal o a una variable de tabla  
  
 Dado que *select_statement* utiliza la instrucción SELECT, es válido utilizar las sugerencias \<join_hint> y \<table_hint>, tal como se especifican en la cláusula FROM. Para obtener más información, vea [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md) y [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md). 
  
 En *select_statement* se pueden utilizar funciones y varias instrucciones SELECT separadas por UNION o UNION ALL.  
  
 CHECK OPTION  
 Fuerza que todas las instrucciones de modificación de datos que se ejecuten en la vista sigan los criterios establecidos en *select_statement*. Cuando una fila se modifica mediante una vista, WITH CHECK OPTION garantiza que los datos permanezcan visibles en toda la vista después de confirmar la modificación.  
  
> [!NOTE]
>  La opción CHECK solo se aplica a las actualizaciones realizadas a través de la vista. No tiene ninguna aplicabilidad en las actualizaciones realizadas directamente en las tablas subyacentes de una vista.  
  
 ENCRYPTION  
 **Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Cifra las entradas de [sys.syscomments](../../relational-databases/system-compatibility-views/sys-syscomments-transact-sql.md) que contienen el texto de la instrucción CREATE VIEW. El uso de WITH ENCRYPTION evita que la vista se publique como parte de la replicación de SQL Server.  
  
 SCHEMABINDING  
 Enlaza la vista al esquema de las tablas subyacentes. Cuando se especifica SCHEMABINDING, las tablas base no se pueden modificar de una forma que afecte a la definición de la vista. En primer lugar, se debe modificar o quitar la propia definición de la vista para quitar las dependencias en la tabla que se va a modificar. Cuando se usa SCHEMABINDING, *select_statement* debe incluir los nombres en dos partes (_schema_ **.** _object_) de las tablas, las vistas o las funciones definidas por el usuario a las que se hace referencia. Todos los objetos a los que se hace referencia se deben encontrar en la misma base de datos.  
  
 Las vistas o las tablas que participan en una vista creada con la cláusula SCHEMABINDING no se pueden quitar a menos que se quite o cambie esa vista de forma que deje de tener un enlace de esquema. En caso contrario, [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un error. Además, la ejecución de las instrucciones ALTER TABLE en tablas que participan en vistas que tienen enlaces de esquema provoca un error si estas instrucciones afectan a la definición de la vista.  
  
 VIEW_METADATA  
 Especifica que la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devolverá a las API de DB-Library, ODBC y OLE DB la información de metadatos sobre la vista en vez de las tablas base cuando se soliciten los metadatos del modo de exploración para una consulta que hace referencia a la vista. Los metadatos del modo de exploración son metadatos adicionales que la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve a estas API del lado cliente. Estos metadatos permiten a las API del lado cliente implementar cursores del lado cliente actualizables. Los metadatos del modo de exploración incluyen información sobre la tabla base a la que pertenecen las columnas del conjunto de resultados.  
  
 Para las vistas creadas con VIEW_METADATA, los metadatos del modo de exploración devuelven el nombre de vista y no los nombres de tablas base cuando describen columnas de la vista en el conjunto de resultados.  
  
 Cuando se crea una vista mediante WITH VIEW_METADATA, todas sus columnas, excepto una columna **timestamp**, son actualizables si la vista tiene los desencadenadores INSTEAD OF INSERT o INSTEAD OF UPDATE. Para obtener más información acerca de las vistas actualizables, vea la sección Notas.  
  
## <a name="remarks"></a>Observaciones  
 Una vista solo se puede crear en la base de datos actual. CREATE VIEW debe ser la primera instrucción en un lote de consultas. Una vista puede tener un máximo de 1.024 columnas.  
  
 Cuando se realiza una consulta a través de una vista, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] se asegura de que todos los objetos de base de datos a los que se hace referencia en algún lugar de la instrucción existen, que son válidos en el contexto de la instrucción y que las instrucciones de modificación de datos no infringen ninguna regla de integridad de los datos. Las comprobaciones que no son correctas devuelven un mensaje de error. Las comprobaciones correctas traducen la acción a una acción con las tablas subyacentes.  
  
 Si una vista depende de una tabla o vista que se ha quitado, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un mensaje de error si alguien trata de utilizar la vista. Si se crea una nueva tabla o vista y la estructura de la tabla no cambia con respecto a la tabla base anterior para sustituir a la eliminada, se puede volver a utilizar la vista. Si cambia la estructura de la nueva tabla o vista, es necesario eliminar la vista y volver a crearla.  
  
 Si una vista no se crea con la cláusula SCHEMABINDING, ejecute [sp_refreshview](../../relational-databases/system-stored-procedures/sp-refreshview-transact-sql.md) cuando se realicen cambios en los objetos subyacentes de la vista que afecten a la definición de ésta. De lo contrario, la vista podría producir resultados inesperados en las consultas.  
  
 Cuando se crea una vista, la información sobre ella se almacena en estas vistas de catálogo: [sys.views](../../relational-databases/system-catalog-views/sys-views-transact-sql.md), [sys.columns](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md) y [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md). El texto de la instrucción CREATE VIEW se almacena en la vista de catálogo [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md).  
  
 El resultado de una consulta que utiliza un índice de una vista definido con expresiones **numeric** o **float** podría diferir del resultado de una consulta similar que no utiliza el índice de la vista. Esta diferencia se podría deber a errores de redondeo durante las acciones INSERT, DELETE o UPDATE en las tablas subyacentes.  
  
 Cuando se crea una vista, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] guarda la configuración de SET QUOTED_IDENTIFIER y SET ANSI_NULLS. Esta configuración original se utiliza para analizar la vista cuando ésta se utiliza. Por tanto, cualquier configuración de sesión de cliente de SET QUOTED_IDENTIFIER y SET ANSI_NULLS no afecta a la definición de la vista cuando se obtiene acceso a ella.  
  
## <a name="updatable-views"></a>Vistas actualizables  
 Es posible modificar los datos de una tabla base subyacente mediante una vista, siempre que se cumplan las siguientes condiciones:  
  
-   Cualquier modificación, incluidas las instrucciones UPDATE, INSERT y DELETE, debe hacer referencia a las columnas de una única tabla base.  
  
-   Las columnas que se vayan a modificar en la vista deben hacer referencia directa a los datos subyacentes de las columnas de la tabla. Las columnas no se pueden obtener de otra forma, como las siguientes:  
  
    -   Una función de agregado: AVG, COUNT, SUM, MIN, MAX, GROUPING, STDEV, STDEVP, VAR y VARP.  
  
    -   Un cálculo. La columna no se puede calcular a partir de una expresión que utilice otras columnas. Las columnas formadas mediante los operadores de conjunto UNION, UNION ALL, CROSSJOIN, EXCEPT e INTERSECT equivalen a un cálculo y tampoco son actualizables.  
  
-   Las columnas que se van a modificar no se ven afectadas por las cláusulas GROUP BY, HAVING o DISTINCT.  
  
-   No se utiliza TOP con la cláusula WITH CHECK OPTION en ningún punto de *select_statement* de la vista.  
  
 Las restricciones anteriores se aplican a cualquier subconsulta de la cláusula FROM de la vista, al igual que a la propia vista. Normalmente, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] debe poder realizar un seguimiento sin ambigüedades de las modificaciones de la definición de la vista a una tabla base. Para más información, vea [Modificar datos mediante una vista](../../relational-databases/views/modify-data-through-a-view.md).  
  
 Si las restricciones anteriores le impiden modificar datos directamente mediante una vista, considere las siguientes opciones:  
  
-   **Desencadenadores INSTEAD OF**  
  
     Es posible crear desencadenadores INSTEAD OF en una vista para que sea actualizable. El desencadenador INSTEAD OF se ejecuta en lugar de la instrucción de modificación de datos en la que se define el desencadenador. Este desencadenador permite al usuario especificar el conjunto de acciones que hay que realizar para procesar la instrucción de modificación de datos. Por lo tanto, si existe un desencadenador INSTEAD OF para una vista en una instrucción de modificación de datos determinada (INSERT, UPDATE o DELETE), la vista correspondiente se puede actualizar mediante esa instrucción. Para obtener más información acerca de los desencadenadores INSTEAD OF, vea [Desencadenadores DML](../../relational-databases/triggers/dml-triggers.md).  
  
-   **Vistas con particiones**  
  
     Si la vista es una vista con particiones, se puede actualizar con determinadas restricciones. Si es necesario, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] distingue las vistas con particiones locales como las vistas en las que todas las tablas participantes y la vista se encuentran en la misma instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y las vistas con particiones distribuidas como las vistas en las que al menos una de las tablas de la vista reside en otro servidor o en uno remoto.  
  
## <a name="partitioned-views"></a>Vistas con particiones  
 Una vista con particiones es una vista definida por un operador UNION ALL de las tablas miembro estructuradas de la misma manera pero almacenadas en diferentes tablas de la misma instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o en un grupo de instancias autónomas de servidores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] denominados servidores de bases de datos federadas.  
  
> [!NOTE]  
>  El método preferido para la partición de datos local en un servidor es a través de tablas con particiones. Para obtener más información, vea [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
 Si diseña un esquema de partición, debe tener claro qué datos pertenecen a cada partición. Por ejemplo, los datos de la tabla `Customers` se distribuyen en tres tablas miembro en tres ubicaciones de servidor: `Customers_33` en `Server1`, `Customers_66` en `Server2` y `Customers_99` en `Server3`.  
  
 Una vista con particiones de `Server1` se define de la siguiente forma:  
  
```sql
--Partitioned view as defined on Server1  
CREATE VIEW Customers  
AS  
--Select from local member table.  
SELECT *  
FROM CompanyData.dbo.Customers_33  
UNION ALL  
--Select from member table on Server2.  
SELECT *  
FROM Server2.CompanyData.dbo.Customers_66  
UNION ALL  
--Select from member table on Server3.  
SELECT *  
FROM Server3.CompanyData.dbo.Customers_99;  
```  
  
 Normalmente, se dice que una vista tiene particiones si tiene el siguiente formato:  
  
```syntaxsql
SELECT <select_list1>  
FROM T1  
UNION ALL  
SELECT <select_list2>  
FROM T2  
UNION ALL  
...  
SELECT <select_listn>  
FROM Tn;  
```  
  
## <a name="conditions-for-creating-partitioned-views"></a>Condiciones para la creación de vistas con particiones  
  
1.  La `list` de selección  
  
    -   En la lista de columnas de la definición de vistas, seleccione todas las columnas en las tablas miembro.  
  
    -   Asegúrese de que las columnas que se encuentren en la misma posición ordinal de cada `select list` son del mismo tipo, incluidas las intercalaciones. No es suficiente que las columnas sean de tipos implícitamente convertibles, como sucede normalmente con UNION.  
  
         Además, al menos una columna (por ejemplo `<col>`) debe aparecer en todas las listas de selección en la misma posición ordinal. Defina `<col>` de manera que las tablas miembro `T1, ..., Tn` tengan restricciones CHECK `C1, ..., Cn` definidas en `<col>`, respectivamente.  
  
         La restricción `C1` definida en la tabla `T1` debe tener el siguiente formato:  
  
        ```syntaxsql
        C1 ::= < simple_interval > [ OR < simple_interval > OR ...]  
        < simple_interval > :: =   
        < col > { < | > | \<= | >= | = < value >}   
        | < col > BETWEEN < value1 > AND < value2 >  
        | < col > IN ( value_list )  
        | < col > { > | >= } < value1 > AND  
        < col > { < | <= } < value2 >  
        ```
  
    -   Las restricciones deben estar definidas de manera que cualquier valor especificado de `<col>` pueda cumplir al menos una de las restricciones `C1, ..., Cn` de modo que las restricciones formen un conjunto de intervalos no combinados o que no se superpongan. La columna `<col>` en la que se definen las restricciones no combinadas se denomina columna de partición. Observe que la columna de partición puede tener diferentes nombres en las tablas subyacentes. Las restricciones deben estar habilitadas y ser de confianza para cumplir las condiciones mencionadas anteriormente de la columna de partición. Si las restricciones están deshabilitadas, vuelva a habilitarlas mediante la opción CHECK CONSTRAINT *constraint_name* de ALTER TABLE y la opción WITH CHECK para validarlas.  
  
         En los siguientes ejemplos se muestran conjuntos válidos de restricciones:  
  
        ```syntaxsql
        { [col < 10], [col between 11 and 20] , [col > 20] }  
        { [col between 11 and 20], [col between 21 and 30], [col between 31 and 100] }  
        ```  
  
    -   No se puede utilizar la misma columna varias veces en la lista de selección.  
  
2.  Columna de partición  
  
    -   La columna de partición forma parte de la restricción PRIMARY KEY de la tabla.  
  
    -   No puede ser una columna calculada, de identidad, predeterminada o **timestamp**.  
  
    -   Si existe más de una restricción en la misma columna de una tabla miembro, el Motor de base de datos omite todas las restricciones y no las tiene en cuenta al determinar si la vista tiene particiones. Para cumplir las condiciones de la vista con particiones, asegúrese de que solo hay una restricción de partición en la columna de partición.  
  
    -   No hay restricciones sobre la posibilidad de actualización de la columna de partición.  
  
3.  Tablas miembro o tablas subyacentes `T1, ..., Tn`  
  
    -   Las tablas pueden ser locales o tablas de otros equipos que ejecuten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a los que se haga referencia mediante un nombre de cuatro partes o un nombre basado en OPENDATASOURCE u OPENROWSET. La sintaxis de OPENDATASOURCE y OPENROWSET puede especificar un nombre de tabla, pero no una consulta de paso a través. Para obtener más información, consulte [OPENDATASOURCE &#40;Transact-SQL&#41;](../../t-sql/functions/opendatasource-transact-sql.md) y [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md).  
  
         Si una o más tablas miembro son remotas, la vista se denomina vista con particiones distribuida y se aplican condiciones adicionales. Se describen más adelante en esta sección.  
  
    -   La misma tabla no puede aparecer dos veces en el conjunto de tablas que se está combinando con la instrucción UNION ALL.  
  
    -   Las tablas miembro no pueden tener índices creados en columnas calculadas de la tabla.  
  
    -   Las tablas miembro deben tener todas las restricciones PRIMARY KEY en el mismo número de columnas.  
  
    -   Todas las tablas miembro de la vista deben tener el mismo valor de relleno ANSI. Éste se establece mediante la opción **user options** de **sp_configure** o la instrucción SET.  
  
## <a name="conditions-for-modifying-data-in-partitioned-views"></a>Condiciones para la modificación de datos en vistas con particiones  
 Las siguientes restricciones se aplican a instrucciones que modifican datos en vistas con particiones:  
  
-   La instrucción INSERT proporciona valores para todas las columnas de la vista, incluso si las tablas miembro subyacentes tienen una restricción DEFAULT para esas columnas o si admiten valores NULL. En las columnas de la tabla miembro con definiciones DEFAULT, las instrucciones no pueden usar explícitamente la palabra clave DEFAULT.  
  
-   El valor que se va a insertar en la columna de partición cumple al menos una de las restricciones subyacentes; en caso contrario, la acción de inserción provocará un error con una infracción de restricción.  
  
-   Las instrucciones UPDATE no pueden especificar la palabra clave DEFAULT como valor de la cláusula SET, aunque la columna tenga definido un valor DEFAULT en la tabla miembro correspondiente.  
  
-   Las columnas de la vista que sean columnas de identidad en una o varias tablas miembro no se pueden modificar mediante una instrucción INSERT o UPDATE.  
  
-   Si una de las tablas miembro contiene una columna **timestamp**, los datos no se pueden modificar mediante una instrucción INSERT o UPDATE.  
  
-   Si una de las tablas miembro contiene un desencadenador o una restricción ON UPDATE CASCADE/SET NULL/SET DEFAULT u ON DELETE CASCADE/SET NULL/SET DEFAULT, no se puede modificar la vista.  
  
-   Las acciones INSERT, UPDATE y DELETE en una vista con particiones no están permitidas si hay una autocombinación con la misma vista o con cualquiera de las tablas miembro de la instrucción.  
  
-   La importación masiva de datos a una vista con particiones no es compatible con la utilidad **bcp** ni con las instrucciones BULK INSERT e INSERT... SELECT * FROM OPENROWSET(BULK...). Sin embargo, puede insertar varias filas en una vista con particiones utilizando la instrucción [INSERT](../../t-sql/statements/insert-transact-sql.md).  
  
    > [!NOTE]  
    >  Para actualizar una vista con particiones, el usuario debe tener permisos INSERT, UPDATE y DELETE en las tablas miembro.  
  
## <a name="additional-conditions-for-distributed-partitioned-views"></a>Condiciones adicionales de las vistas con particiones distribuidas  
 A las vistas con particiones distribuidas (cuando una o varias tablas miembro son remotas) se les aplican las siguientes condiciones adicionales:  
  
-   Se iniciará una transacción distribuida para garantizar la atomicidad en todos los nodos a los que afecta la actualización.  
  
-   Establezca la opción XACT_ABORT SET en ON para que funcionen las instrucciones INSERT, UPDATE o DELETE.  
  
-   Cualquier columna de las tablas remotas de tipo **smallmoney** a la que se haga referencia en una vista con particiones se asignará como **money**. Por lo tanto, las columnas correspondientes (en la misma posición ordinal de la lista de selección) de las tablas locales deben ser también de tipo **money**.  
  
-   En el nivel 110 y posteriores de compatibilidad de bases de datos, cualquier columna de las tablas remotas de tipo **smalldatetime** a la que se haga referencia en una vista con particiones se asignará como **smalldatetime**. Las columnas correspondientes (en la misma posición ordinal en la lista de selección) de las tablas locales deben ser **smalldatetime**. Esto representa un cambio de comportamiento con respecto a versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], en las que las columnas de tablas remotas de tipo **smalldatetime** a las que se hace referencia en una vista con particiones se asignan como **datetime** y las columnas correspondientes de las tablas locales deben ser de tipo **datetime**. Para obtener más información, vea [Nivel de compatibilidad de ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
-   Ningún servidor vinculado de la vista con particiones puede ser un servidor vinculado en bucle de retorno. Se trata de un servidor vinculado que apunta a la misma instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 El valor de la opción SET ROWCOUNT se pasa por alto para las acciones INSERT, UPDATE y DELETE que implican vistas con particiones y tablas remotas actualizables.  
  
 Cuando las tablas miembro y la definición de la vista con particiones están preparadas, el optimizador de consultas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea planes inteligentes que utilizan las consultas de forma eficaz para tener acceso a los datos de las tablas miembro. Con las definiciones de la restricción CHECK, el procesador de consultas asigna la distribución de valores clave entre las tablas miembro. Cuando un usuario emite una consulta, el procesador de consultas compara la asignación con los valores especificados en la cláusula WHERE y crea un plan de ejecución con una transferencia mínima de datos entre los servidores miembro. Por lo tanto, aunque algunas tablas miembro puedan estar ubicadas en servidores remotos, la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] resuelve las consultas distribuidas de manera que la cantidad de datos distribuidos que haya que transferir sea mínima.  
  
## <a name="considerations-for-replication"></a>Consideraciones acerca de la replicación  
 Para crear vistas con particiones en tablas miembro implicadas en la replicación, deben tenerse en cuenta las consideraciones siguientes:  
  
-   Si las tablas subyacentes intervienen en la replicación de mezcla o en la replicación transaccional con suscripciones de actualización, asegúrese de que la columna **uniqueidentifier** también se incluye en la lista de selección. 
  
     Las acciones INSERT que se ejecutan en la vista con particiones deben proporcionar un valor NEWID() para la columna **uniqueidentifier**. Las acciones UPDATE en la columna **uniqueidentifier** deben proporcionar NEWID() como valor, puesto que no se puede usar la palabra clave DEFAULT.  
  
-   La replicación de actualizaciones que se realiza mediante la vista es igual que cuando las tablas se replican en dos bases de datos distintas: agentes de replicación diferentes dan servicio a las tablas y no se garantiza el orden de las actualizaciones.  
  
## <a name="permissions"></a>Permisos  
 Se necesita el permiso CREATE VIEW en la base de datos y el permiso ALTER en el esquema en que se crea la vista.  
  
## <a name="examples"></a>Ejemplos  

Para los siguientes ejemplos se usan las bases de datos AdventureWorks 2012 o AdventureWorksDW.  

### <a name="a-using-a-simple-create-view"></a>A. Usar una instrucción CREATE VIEW sencilla  
 En el ejemplo siguiente se crea una vista mediante una instrucción `SELECT` sencilla. Una vista sencilla resulta útil cuando se consulta con frecuencia una combinación de columnas. Los datos de esta vista provienen de las tablas `HumanResources.Employee` y `Person.Person` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Los datos proporcionan el nombre e información sobre la fecha de contratación de los empleados de [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)]. Esta vista puede crearse para la persona responsable del seguimiento de los aniversarios de trabajo pero sin concederle acceso a todos los datos de estas tablas.  
  
```sql
CREATE VIEW hiredate_view  
AS   
SELECT p.FirstName, p.LastName, e.BusinessEntityID, e.HireDate  
FROM HumanResources.Employee e   
JOIN Person.Person AS p ON e.BusinessEntityID = p.BusinessEntityID ;  
GO  
  
```  
  
### <a name="b-using-with-encryption"></a>B. Usar WITH ENCRYPTION  
 En el siguiente ejemplo se utiliza la opción `WITH ENCRYPTION` y se muestran columnas calculadas, columnas con el nombre cambiado y varias columnas.  
  
**Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, y [!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
  
```sql
CREATE VIEW Purchasing.PurchaseOrderReject  
WITH ENCRYPTION  
AS  
SELECT PurchaseOrderID, ReceivedQty, RejectedQty,   
    RejectedQty / ReceivedQty AS RejectRatio, DueDate  
FROM Purchasing.PurchaseOrderDetail  
WHERE RejectedQty / ReceivedQty > 0  
AND DueDate > CONVERT(DATETIME,'20010630',101) ;  
GO  
  
```  
  
### <a name="c-using-with-check-option"></a>C. Usar WITH CHECK OPTION  
 En el siguiente ejemplo se muestra una vista denominada `SeattleOnly` que hace referencia a cinco tablas y permite modificar datos aplicados únicamente a los empleados que viven en Seattle.  
  
```sql
CREATE VIEW dbo.SeattleOnly  
AS  
SELECT p.LastName, p.FirstName, e.JobTitle, a.City, sp.StateProvinceCode  
FROM HumanResources.Employee e  
INNER JOIN Person.Person p  
ON p.BusinessEntityID = e.BusinessEntityID  
    INNER JOIN Person.BusinessEntityAddress bea   
    ON bea.BusinessEntityID = e.BusinessEntityID   
    INNER JOIN Person.Address a   
    ON a.AddressID = bea.AddressID  
    INNER JOIN Person.StateProvince sp   
    ON sp.StateProvinceID = a.StateProvinceID  
WHERE a.City = 'Seattle'  
WITH CHECK OPTION ;  
GO  
```  
  
### <a name="d-using-built-in-functions-within-a-view"></a>D. Usar funciones integradas dentro de una vista  
 En el siguiente ejemplo se muestra una definición de vista que incluye una función integrada. Al utilizar funciones, es necesario especificar un nombre de columna para la columna derivada.  
  
```sql
CREATE VIEW Sales.SalesPersonPerform  
AS  
SELECT TOP (100) SalesPersonID, SUM(TotalDue) AS TotalSales  
FROM Sales.SalesOrderHeader  
WHERE OrderDate > CONVERT(DATETIME,'20001231',101)  
GROUP BY SalesPersonID;  
GO  

```  
  
### <a name="e-using-partitioned-data"></a>E. Usar datos con particiones  
 En el siguiente ejemplo se utilizan las tablas denominadas `SUPPLY1`, `SUPPLY2`, `SUPPLY3` y `SUPPLY4`. Estas tablas corresponden a las tablas de proveedores de cuatro oficinas ubicadas en diferentes países o regiones.  
  
```sql
--Create the tables and insert the values.  
CREATE TABLE dbo.SUPPLY1 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 1 and 150),  
supplier CHAR(50)  
);  
CREATE TABLE dbo.SUPPLY2 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 151 and 300),  
supplier CHAR(50)  
);  
CREATE TABLE dbo.SUPPLY3 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 301 and 450),  
supplier CHAR(50)  
);  
CREATE TABLE dbo.SUPPLY4 (  
supplyID INT PRIMARY KEY CHECK (supplyID BETWEEN 451 and 600),  
supplier CHAR(50)  
);  
GO  
--Create the view that combines all supplier tables.  
CREATE VIEW dbo.all_supplier_view  
WITH SCHEMABINDING  
AS  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY1  
UNION ALL  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY2  
UNION ALL  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY3  
UNION ALL  
SELECT supplyID, supplier  
  FROM dbo.SUPPLY4;  
GO
INSERT dbo.all_supplier_view VALUES ('1', 'CaliforniaCorp'), ('5', 'BraziliaLtd')    
, ('231', 'FarEast'), ('280', 'NZ')  
, ('321', 'EuroGroup'), ('442', 'UKArchip')  
, ('475', 'India'), ('521', 'Afrique');  
GO  
```  
  
## <a name="examples-sssdw-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="f-creating-a-simple-view"></a>F. Crear una vista simple  
 En el ejemplo siguiente se crea una vista seleccionando solo algunas de las columnas de la tabla de origen.  
  
```sql
CREATE VIEW DimEmployeeBirthDates AS  
SELECT FirstName, LastName, BirthDate   
FROM DimEmployee;  
```  
  
### <a name="g-create-a-view-by-joining-two-tables"></a>G. Crear una vista mediante la combinación de dos tablas  
 En el ejemplo siguiente se crea una vista mediante una instrucción `SELECT` con `OUTER JOIN`. Los resultados de la consulta de combinación rellenan la vista.  
  
```sql
CREATE VIEW view1  
AS 
SELECT fis.CustomerKey, fis.ProductKey, fis.OrderDateKey, 
  fis.SalesTerritoryKey, dst.SalesTerritoryRegion  
FROM FactInternetSales AS fis   
LEFT OUTER JOIN DimSalesTerritory AS dst   
ON (fis.SalesTerritoryKey=dst.SalesTerritoryKey);  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [ALTER VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/alter-view-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [DROP VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/drop-view-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [Crear un procedimiento almacenado](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)   
 [sys.dm_sql_referencing_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_refreshview &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-refreshview-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sys.views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-views-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

