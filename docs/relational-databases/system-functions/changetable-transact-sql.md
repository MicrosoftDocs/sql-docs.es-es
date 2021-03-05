---
description: CHANGETABLE (Transact-SQL)
title: CHANGETABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/12/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- CHANGETABLE_TSQL
- CHANGETABLE
dev_langs:
- TSQL
helpviewer_keywords:
- CHANGETABLE
- change tracking [SQL Server], CHANGETABLE
ms.assetid: d405fb8d-3b02-4327-8d45-f643df7f501a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2de815ad24a41604f18d0083a800df3a56feb021
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186639"
---
# <a name="changetable-transact-sql"></a>CHANGETABLE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve información de seguimiento de cambios para una tabla. Puede utilizar esta instrucción para devolver todos los cambios de una tabla o información de seguimiento de cambios de una fila concreta.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CHANGETABLE (  
    { CHANGES <table_name> , <last_sync_version> 
    | VERSION <table_name> , <primary_key_values> } 
    , [ FORCESEEK ] 
    )  
[AS] <table_alias> [ ( <column_alias> [ ,...n ] )  
  
<primary_key_values> ::=  
( <column_name> [ , ...n ] ) , ( <value> [ , ...n ] )  
```  
  
## <a name="arguments"></a>Argumentos  
 CAMBIOS *TABLE_NAME* , *last_sync_version*  
 Devuelve información de seguimiento de todos los cambios en una tabla que se han producido desde la versión especificada por *last_sync_version*.  
  
 *table_name*  
 Es la tabla definida por el usuario de la que obtener cambios a los que se ha realizado el seguimiento. El seguimiento de cambios debe estar habilitado en la tabla. Puede utilizarse un nombre de tabla de uno, dos, tres o cuatro partes. El nombre de tabla puede ser un sinónimo de la tabla.  
  
 *last_sync_version*  
 Valor escalar **BIGINT** que acepta valores NULL. Una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) producirá un error de sintaxis. Si el valor es NULL, se devuelven todos los cambios a los que se ha realizado el seguimiento.
Cuando obtiene cambios, la aplicación que realiza la llamada debe especificar el punto del que se requieren cambios. El *last_sync_version* especifica ese punto. La función devuelve de todas las filas que se han cambiado desde esa versión. La aplicación está consultando para recibir los cambios con una versión mayor que *last_sync_version*. Normalmente, antes de obtener los cambios, la aplicación llamará `CHANGE_TRACKING_CURRENT_VERSION()` a para obtener la versión que se usará la próxima vez que se requieran cambios. Por consiguiente, la aplicación no tiene que interpretar ni conocer el valor real. Dado que la aplicación que realiza la llamada obtiene *last_sync_version* , la aplicación tiene que conservar el valor. Si la aplicación pierde este valor, será necesario reinicializar los datos. 
 *last_sync_version* debe validarse para asegurarse de que no es demasiado antigua, ya que es posible que se haya limpiado parte o toda la información de los cambios según el período de retención configurado para la base de datos. Para obtener más información, vea [CHANGE_TRACKING_MIN_VALID_VERSION &#40;Transact-sql&#41;](../../relational-databases/system-functions/change-tracking-min-valid-version-transact-sql.md) y [opciones set de ALTER DATABASE &#40;transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
 VERSIÓN *TABLE_NAME*, { *primary_key_values* }  
 Devuelve la última información de seguimiento de cambios para una fila especificada. Los valores de clave principal deben identificar la fila. *primary_key_values* identifica las columnas de clave principal y especifica los valores. Los nombres de columna de clave principal se pueden especificar en cualquier orden.  
  
 *table_name*  
 Es la tabla definida por el usuario de la que obtener la información de seguimiento de cambios. El seguimiento de cambios debe estar habilitado en la tabla. Puede utilizarse un nombre de tabla de uno, dos, tres o cuatro partes. El nombre de tabla puede ser un sinónimo de la tabla.  
  
 *column_name*  
 Especifica el nombre de la columna o columnas de clave principal. Se pueden especificar varios nombres de columna y en cualquier orden.  
  
 *value*  
 Es el valor de la clave principal. Si hay varias columnas de clave principal, los valores se deben especificar en el mismo orden en el que aparecen las columnas en la lista de *column_name* .  

 FORCESEEK   
 **Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU16 y [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] ), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]    
 
 Parámetro opcional que fuerza la utilización de una operación de búsqueda para tener acceso a la *TABLE_NAME*. En algunos casos en los que muy pocas filas han cambiado, se puede seguir usando una operación de examen para tener acceso a la *TABLE_NAME*. Si una operación de examen presenta un problema de rendimiento, use el `FORCESEEK` parámetro.

 APLICAR *table_alias* [(*column_alias* [,... *n* ])]  
 Proporciona los nombres de los resultados devueltos por CHANGETABLE.  
  
 *table_alias*  
 Es el nombre del alias de la tabla que es devuelta por CHANGETABLE. *table_alias* es obligatorio y debe ser un [identificador](../../relational-databases/databases/database-identifiers.md)válido.  
  
 *column_alias*  
 Es un alias de columna opcional o lista de alias de columna para las columnas devueltas por CHANGETABLE. Esto permite personalizar los nombres de columna en caso de que haya nombres duplicados en los resultados.  

## <a name="return-types"></a>Tipos de valor devuelto  
 **table**  
  
## <a name="return-values"></a>Valores devueltos  
  
### <a name="changetable-changes"></a>CHANGETABLE CHANGES  
 Si se especifica CHANGES, se devuelven cero o varias filas con las columnas siguientes.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|SYS_CHANGE_VERSION|**bigint**|Valor de versión asociado al último cambio de la fila|  
|SYS_CHANGE_CREATION_VERSION|**bigint**|Valores de versión asociados a la última operación de inserción.|  
|SYS_CHANGE_OPERATION|**NCHAR (1)**|Especifica el tipo de cambio:<br /><br /> **U** = actualización<br /><br /> **I** = insertar<br /><br /> **D** = eliminar|  
|SYS_CHANGE_COLUMNS|**varbinary (4100)**|Enumera las columnas que han cambiado desde last_sync_version (la básica). Tenga en cuenta que las columnas calculadas nunca aparecen como cambiadas.<br /><br /> El valor es NULL si se cumple una cualquiera de las siguientes condiciones:<br /><br /> El seguimiento de cambios de columna no está habilitado.<br /><br /> Se trata de una operación de eliminación o de inserción.<br /><br /> Se actualizaron en una única operación todas las columnas de clave no principal. No debería interpretarse directamente este valor binario. En su lugar, para interpretarlo, utilice [CHANGE_TRACKING_IS_COLUMN_IN_MASK ()](../../relational-databases/system-functions/change-tracking-is-column-in-mask-transact-sql.md).|  
|SYS_CHANGE_CONTEXT|**varbinary(128)**|Cambiar la información de contexto que se puede especificar opcionalmente mediante la cláusula [with](../../relational-databases/system-functions/with-change-tracking-context-transact-sql.md) como parte de una instrucción INSERT, Update o DELETE.|  
|\<primary key column value>|Igual que las columnas de tabla de usuario|Los valores de clave principal de la tabla con seguimiento. Estos valores identifican de manera única cada fila en la tabla de usuario.|  
  
### <a name="changetable-version"></a>CHANGETABLE VERSION  
 Si se especifica VERSION, se devuelve una fila con las siguientes columnas.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|SYS_CHANGE_VERSION|**bigint**|Valor de versión de cambios actual asociado a la fila.<br /><br /> El valor es NULL si no se ha realizado ningún cambio durante un periodo más largo que el periodo de retención del seguimiento de cambios, o no se ha cambiado la fila desde que se habilitó el seguimiento de cambios.|  
|SYS_CHANGE_CONTEXT|**varbinary(128)**|Cambie la información de contexto que se puede especificar opcionalmente usando la cláusula WITH como parte de una instrucción INSERT, UPDATE o DELETE.|  
|\<primary key column value>|Igual que las columnas de tabla de usuario|Los valores de clave principal de la tabla con seguimiento. Estos valores identifican de manera única cada fila en la tabla de usuario.|  
  
## <a name="remarks"></a>Observaciones  
 Normalmente, la función CHANGETABLE se utiliza en la cláusula FROM de una consulta como si fuera una tabla.  
  
## <a name="changetablechanges"></a>CHANGETABLE(CHANGES...)  
 Para obtener los datos de fila para las filas nuevas o modificadas, una el conjunto de resultados a la tabla de usuario mediante las columnas de clave principal. Solo se devuelve una fila para cada fila de la tabla de usuario que se ha cambiado, incluso si se han producido varios cambios en la misma fila desde el *last_sync_version* valor.  
  
 Los cambios de la columna de clave principal nunca se marcan como actualizaciones. Si un valor de clave principal cambia, se considera que es una eliminación del valor anterior y una inserción del valor nuevo.  
  
 Si elimina una fila y, a continuación, inserta una fila con la clave principal anterior, el cambio se considera una actualización para todas las columnas de la fila.  
  
 Los valores que se devuelven para las `SYS_CHANGE_OPERATION` `SYS_CHANGE_COLUMNS` columnas y son relativos a la línea de base (last_sync_version) especificada. Por ejemplo, si se realiza una operación de inserción en `10` la versión y una operación de actualización en `15` la versión, y si la línea base *last_sync_version* es, se informará de `12` una actualización. Si el valor de *last_sync_version* es `8` , se informará de una inserción. `SYS_CHANGE_COLUMNS` nunca notificará columnas calculadas como si hubieran sido actualizadas.  
  
 Generalmente, se someten a seguimiento todas las operaciones que insertan, actualizan o eliminan datos en tablas de usuario, incluida la instrucción MERGE.  
  
 Las siguientes operaciones, que afectan a datos de tablas de usuario, no se someten a seguimiento:  
  
-   Ejecutar la `UPDATETEXT` instrucción. Esta instrucción ha quedado desusada y no estará presente en una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sin embargo, se realiza un seguimiento de los cambios que se realizan mediante la `.WRITE` cláusula de la instrucción UPDATE.  
  
-   Eliminar filas mediante el uso de `TRUNCATE TABLE` . Cuando se trunca una tabla, la información de versión del seguimiento de cambios asociada a la tabla se restablece como si el seguimiento de cambios se hubiera habilitado recientemente en la tabla. Una aplicación cliente debe validar siempre su última versión sincronizada. Se producirá un error en la validación si se ha truncado la tabla.  
  
## <a name="changetableversion"></a>CHANGETABLE(VERSION...)  
 Se devuelve un conjunto de resultados vacío si se especifica una clave principal inexistente.  
  
 El valor de `SYS_CHANGE_VERSION` puede ser null si no se ha realizado un cambio durante más tiempo que el período de retención (por ejemplo, si la limpieza ha quitado la información de cambios) o si la fila nunca ha cambiado desde que se habilitó el seguimiento de cambios para la tabla.  
  
## <a name="permissions"></a>Permisos  
 Requiere el `SELECT` permiso en las columnas de clave principal y el `VIEW CHANGE TRACKING` permiso en la tabla especificada por el *<table_name* valor de>para obtener información del seguimiento de cambios.
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-rows-for-an-initial-synchronization-of-data"></a>A. Devolver las filas de una sincronización inicial de los datos  
 El ejemplo siguiente obtiene los datos para una sincronización inicial de los datos de la tabla. La consulta devuelve todos los datos de fila y sus versiones asociadas. A continuación, pueden insertarse o agregarse estos datos al sistema que contendrá los datos sincronizados.  
  
```sql  
-- Get all current rows with associated version  
SELECT e.[Emp ID], e.SSN, e.FirstName, e.LastName,  
    c.SYS_CHANGE_VERSION, c.SYS_CHANGE_CONTEXT  
FROM Employees AS e  
CROSS APPLY CHANGETABLE   
    (VERSION Employees, ([Emp ID], SSN), (e.[Emp ID], e.SSN)) AS c;  
```  
  
### <a name="b-listing-all-changes-that-were-made-since-a-specific-version"></a>B. Enumerar todos los cambios realizados desde una versión concreta  
 El siguiente ejemplo enumera todos los cambios realizados en una tabla desde una versión concreta (`@last_sync_version)`. [Emp ID] y SSN son las columnas de una clave principal compuesta.  
  
```sql  
DECLARE @last_sync_version bigint;  
SET @last_sync_version = <value obtained from query>;  
SELECT [Emp ID], SSN,  
    SYS_CHANGE_VERSION, SYS_CHANGE_OPERATION,  
    SYS_CHANGE_COLUMNS, SYS_CHANGE_CONTEXT   
FROM CHANGETABLE (CHANGES Employees, @last_sync_version) AS C;  
```  
  
### <a name="c-obtaining-all-changed-data-for-a-synchronization"></a>C. Obtener todos los datos cambiados para una sincronización  
 El siguiente ejemplo muestra cómo se pueden obtener todos los datos que han cambiado. Esta consulta une la información del seguimiento de cambios con la tabla de usuario para devolver la información de tabla de usuario. Una `LEFT OUTER JOIN` se usa para devolver una fila para las filas eliminadas.  
  
```sql  
-- Get all changes (inserts, updates, deletes)  
DECLARE @last_sync_version bigint;  
SET @last_sync_version = <value obtained from query>;  
SELECT e.FirstName, e.LastName, c.[Emp ID], c.SSN,  
    c.SYS_CHANGE_VERSION, c.SYS_CHANGE_OPERATION,  
    c.SYS_CHANGE_COLUMNS, c.SYS_CHANGE_CONTEXT   
FROM CHANGETABLE (CHANGES Employees, @last_sync_version) AS c  
    LEFT OUTER JOIN Employees AS e  
        ON e.[Emp ID] = c.[Emp ID] AND e.SSN = c.SSN;  
```  
  
### <a name="d-detecting-conflicts-by-using-changetableversion"></a>D. Detectar conflictos mediante CHANGETABLE (VERSION...)  
 El siguiente ejemplo actualiza una fila solo si la fila no ha cambiado desde la última sincronización. El número de versión de la fila concreta se obtiene mediante `CHANGETABLE`. Si se ha actualizado la fila, no se realizan los cambios y la consulta devuelve información sobre el último cambio de la fila.  
  
```sql  
-- @last_sync_version must be set to a valid value  
UPDATE  
    SalesLT.Product  
SET  
    ListPrice = @new_listprice  
FROM  
    SalesLT.Product AS P  
WHERE  
    ProductID = @product_id AND  
    @last_sync_version >= ISNULL (  
        (SELECT CT.SYS_CHANGE_VERSION FROM   
            CHANGETABLE(VERSION SalesLT.Product,  
            (ProductID), (P.ProductID)) AS CT),  
        0);  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de seguimiento de cambios &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)   
 [Seguimiento de cambios de datos &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [CHANGE_TRACKING_IS_COLUMN_IN_MASK &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/change-tracking-is-column-in-mask-transact-sql.md)   
 [CHANGE_TRACKING_CURRENT_VERSION &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-current-version-transact-sql.md)   
 [CHANGE_TRACKING_MIN_VALID_VERSION &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-min-valid-version-transact-sql.md)  
  
