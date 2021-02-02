---
description: DECLARE CURSOR (Transact-SQL)
title: DECLARE CURSOR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DECLARE_CURSOR_TSQL
- CURSOR_TSQL
- DECLARE CURSOR
- CURSOR
dev_langs:
- TSQL
helpviewer_keywords:
- DECLARE CURSOR statement
- cursors [SQL Server], attributes
- local cursors [SQL Server]
- nesting cursors
- Transact-SQL cursors, attributes
- global cursors [SQL Server]
ms.assetid: 5a3a27aa-03e8-4c98-a27e-809282379b21
author: cawrites
ms.author: chadam
ms.openlocfilehash: e40722b9330fed0e4be9f48c4463369572659cc9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99120529"
---
# <a name="declare-cursor-transact-sql"></a>DECLARE CURSOR (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Define los atributos de un cursor de servidor de [!INCLUDE[tsql](../../includes/tsql-md.md)], como su comportamiento de desplazamiento y la consulta utilizada para generar el conjunto de resultados sobre el que opera el cursor. `DECLARE CURSOR` acepta tanto una sintaxis basada en el estándar ISO como una sintaxis que usa un conjunto de extensiones [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ISO Syntax  
DECLARE cursor_name [ INSENSITIVE ] [ SCROLL ] CURSOR   
     FOR select_statement   
     [ FOR { READ ONLY | UPDATE [ OF column_name [ ,...n ] ] } ]  
[;]  
Transact-SQL Extended Syntax  
DECLARE cursor_name CURSOR [ LOCAL | GLOBAL ]   
     [ FORWARD_ONLY | SCROLL ]   
     [ STATIC | KEYSET | DYNAMIC | FAST_FORWARD ]   
     [ READ_ONLY | SCROLL_LOCKS | OPTIMISTIC ]   
     [ TYPE_WARNING ]   
     FOR select_statement   
     [ FOR UPDATE [ OF column_name [ ,...n ] ] ]  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *cursor_name*  
 Es el nombre del cursor de servidor de [!INCLUDE[tsql](../../includes/tsql-md.md)] definido. *cursor_name* debe respetar las reglas de los identificadores.  
  
 INSENSITIVE  
 Define un cursor que hace una copia temporal de los datos que utiliza. Todas las solicitudes que se realizan al cursor se responden desde esta tabla temporal de **tempdb**; por tanto, las modificaciones realizadas en las tablas base no se reflejan en los datos devueltos por las operaciones de captura realizadas en el cursor y, además, este cursor no admite modificaciones. Cuando se usa la sintaxis ISO, si se omite `INSENSITIVE`, las eliminaciones y actualizaciones confirmadas que se hayan realizado en las tablas subyacentes (por cualquier usuario) se reflejan en capturas posteriores.  
  
 SCROLL  
 Especifica que todas las opciones de captura (`FIRST`, `LAST`, `PRIOR`, `NEXT`, `RELATIVE`, `ABSOLUTE`) están disponibles. Si no se especifica `SCROLL` con un `DECLARE CURSOR` de estilo ISO, `NEXT` es la única opción de captura admitida. `SCROLL` no se puede especificar si también se especifica `FAST_FORWARD`. Si no se especifica `SCROLL`, solo está disponible la opción de captura `NEXT` y el cursor se convierte en `FORWARD_ONLY`.
  
 *select_statement*  
 Es una instrucción `SELECT` estándar que define el conjunto de resultados del cursor. Las palabras clave `FOR BROWSE` y `INTO` no se permiten dentro de una instrucción *select_statement* de una declaración de cursor.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convierte implícitamente el cursor a otro tipo si las cláusulas de la instrucción *select_statement* entran en conflicto con la funcionalidad del tipo de cursor solicitado.  
  
 READ ONLY  
 Evita que se efectúen actualizaciones a través de este cursor. No se puede hacer referencia al cursor en una cláusula `WHERE CURRENT OF` de una instrucción `UPDATE` o `DELETE`. Esta opción reemplaza la capacidad predeterminada de actualizar el cursor.  
  
 UPDATE [OF *column_name* [**,**...*n*]]  
 Define las columnas actualizables en el cursor. Si se especifica OF <column_name> [, <… n>], solo las columnas enumeradas admiten modificaciones. Si se especifica `UPDATE` sin indicar una lista de columnas, se pueden actualizar todas las columnas.  
  
*cursor_name*  
Es el nombre del cursor de servidor de [!INCLUDE[tsql](../../includes/tsql-md.md)] definido. *cursor_name* debe respetar las reglas de los identificadores.  
  
LOCAL  
Especifica que el alcance del cursor es local para el proceso por lotes, procedimiento almacenado o desencadenador en que se creó el cursor. El nombre del cursor solo es válido en este ámbito. Se puede hacer referencia al cursor mediante variables de cursor locales del lote, procedimiento almacenado, desencadenador o parámetro `OUTPUT` del procedimiento almacenado. El parámetro `OUTPUT` se usa para devolver el cursor local al proceso por lotes, procedimiento almacenado o desencadenador que realiza la llamada, que puede asignar el parámetro a una variable de cursor para hacer referencia al cursor cuando finalice el procedimiento almacenado. La asignación del cursor se cancela implícitamente cuando el proceso por lotes, procedimiento almacenado o desencadenador finaliza, a menos que el cursor se haya devuelto en un parámetro `OUTPUT`. Si se vuelve a pasar el parámetro `OUTPUT`, la asignación del cursor se cancela cuando se cancela la asignación de la última variable que hace referencia a este, o bien cuando esta se sale del ámbito.  
  
GLOBAL  
Especifica que el alcance del cursor es global para la conexión. Se puede hacer referencia al nombre del cursor en cualquier procedimiento almacenado o lote que se ejecute durante la conexión. La asignación del cursor solo se cancela implícitamente cuando se produce la desconexión.  
  
> [!NOTE]  
>  Si no se especifica `GLOBAL` ni `LOCAL`, el valor predeterminado se controla mediante la configuración de la opción de base de datos **default to local cursor**.  
  
FORWARD_ONLY  
Especifica que el cursor solo se puede desplazar hacia delante y de la primera a la última fila. `FETCH NEXT` es la única opción de captura admitida. Las instrucciones INSERT, UPDATE y DELETE que ejecuta el usuario actual (o que confirman otros usuarios) y que afectan a las filas del conjunto de resultados son visibles a medida que se capturan las filas. Pero como el cursor no se puede desplazar hacia atrás, los cambios realizados en las filas de la base de datos tras capturar la fila no son visibles a través del cursor. Los cursores de solo avance son dinámicos de forma predeterminada, lo que significa que todos los cambios se detectan cuando se procesa la fila actual. Esto proporciona una apertura del cursor más rápida y permite que el conjunto de resultados muestre las actualizaciones realizadas en las tablas subyacentes. Aunque los cursores de solo avance no admiten el desplazamiento hacia atrás, las aplicaciones pueden volver al principio del conjunto de resultados si se cierra el cursor y se vuelve a abrir. Si se especifica `FORWARD_ONLY` sin las palabras clave `STATIC`, `KEYSET` o `DYNAMIC`, el cursor opera como un cursor dinámico. Si no se especifica `FORWARD_ONLY` ni `SCROLL`, `FORWARD_ONLY` será el valor predeterminado, excepto si se especifican las palabras clave `STATIC`, `KEYSET` o `DYNAMIC`. El valor predeterminado de los cursores `STATIC`, `KEYSET` y `DYNAMIC` es `SCROLL`. Al contrario que las API de bases de datos (como ODBC y ADO), `FORWARD_ONLY` es compatible con los cursores `STATIC`, `KEYSET` y `DYNAMIC` de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
   
 STATIC  
Especifica que el cursor siempre muestra el conjunto de resultados como estaba al abrir el cursor por primera vez, y realiza una copia temporal de los datos que va a usar el cursor. Todas las solicitudes al cursor se responden desde esta tabla temporal de **tempdb**. Por tanto, las operaciones de inserción, actualización y eliminación realizadas en las tablas base no se reflejan en los datos devueltos por las capturas realizadas por este cursor, que no detecta los cambios realizados en la pertenencia, el orden o los valores del conjunto de resultados una vez abierto el cursor. Los cursores estáticos pueden detectar sus propias operaciones de inserción, actualización y eliminación, aunque no está obligados a hacerlo. Por ejemplo, suponga que un cursor estático captura una fila y, después, otra aplicación la actualiza. Si la aplicación vuelve a capturar la fila del cursor estático, los valores que ve son iguales, a pesar de los cambios realizados por la otra aplicación. Se admiten todos los tipos de desplazamiento. 
  
KEYSET  
Especifica que la pertenencia y el orden de las filas del cursor se fijan cuando se abre este cursor. El conjunto de claves que identifica las filas de forma única está integrado en la tabla denominada **tempdb** de **keyset**. Este cursor proporciona la funcionalidad entre un cursor estático y dinámico en su capacidad para detectar los cambios. Al igual que un cursor estático, no siempre detecta los cambios realizados en la pertenencia y el orden del conjunto de resultados. Como un cursor dinámico, detecta cambios en los valores de las filas del conjunto de resultados. Los cursores controlados por conjunto de claves se supervisan mediante un conjunto de identificadores únicos (claves) denominado conjunto de claves. Las claves se generan a partir de un conjunto de columnas que identifican las filas del conjunto de resultados de forma unívoca. El conjunto de claves es el conjunto de valores de clave de todas las filas devueltas por la instrucción de consulta. Con los cursores controlados por conjunto de claves, se crea y se guarda una clave para cada fila del cursor y se almacena en la estación de trabajo cliente o en el servidor. Al acceder a cada fila, se usa la clave almacenada para capturar los valores de datos actuales desde el origen de datos. En un cursor controlado por conjunto de claves, la pertenencia al conjunto de resultados se inmoviliza cuando se completa totalmente el conjunto de claves. Por tanto, las adiciones o actualizaciones que afectan a la pertenencia no forman parte del conjunto de resultados hasta que se vuelva a abrir. Los cambios en los valores de datos (realizados por el propietario del conjunto de claves o por otros procesos) son visibles cuando el usuario se desplaza por el conjunto de resultados:
-  Si se elimina una fila, un intento de capturar la fila devuelve un valor de -2 para `@@FETCH_STATUS`, porque la fila eliminada aparece como un hueco en el conjunto de resultados. La clave para la fila existe en el conjunto de claves, pero la fila ya no existe en el conjunto de resultados. 
-  Las operaciones de inserción realizadas fuera del cursor (por otros procesos) son visibles solo si el cursor se cierra y se vuelve a abrir. Las operaciones de inserción realizadas desde dentro del cursor son visibles al final del conjunto de resultados.
-  Las actualizaciones de valores de clave de fuera del cursor son similares a la eliminación de la fila anterior seguida por la inserción de la nueva fila. La fila con los nuevos valores no es visible y los intentos de capturar la fila con los valores anteriores devuelven un `@@FETCH_STATUS` de -2. Los nuevos valores son visibles si la actualización se realiza a través del cursor especificando la cláusula `WHERE CURRENT OF`. 

> [!NOTE]  
> Si la consulta hace referencia por lo menos a una tabla sin un índice único, el cursor de conjunto de claves se convierte en cursor estático.  
  
DYNAMIC  
Define un cursor que refleja todos los cambios realizados en las filas de su conjunto de resultados cuando se desplaza alrededor del cursor y se captura un registro nuevo, independientemente de si los cambios se producen desde dentro del cursor o los realizan otros usuarios fuera del cursor. Por tanto, todas las instrucciones UPDATE, INSERT y DELETE que realizan todos los usuarios son visibles a través del cursor. Los valores de los datos, el orden y la pertenencia de las filas pueden cambiar en cada captura. La opción de captura `ABSOLUTE` no se puede usar con los cursores dinámicos. Las actualizaciones realizadas fuera del cursor no son visibles hasta que se confirman (a menos que el nivel de aislamiento de transacciones del cursor se establezca en `UNCOMMITTED`). Por ejemplo, suponga que un cursor dinámico captura dos filas y después otra aplicación actualiza una de esas filas y elimina la otra. Si después el cursor dinámico captura esas filas, no encontrará la que se ha eliminado, pero mostrará los valores nuevos para la fila actualizada. 
  
FAST_FORWARD  
Especifica un cursor `FORWARD_ONLY`, `READ_ONLY` con optimizaciones de rendimiento habilitadas. `FAST_FORWARD` no se puede especificar si también se especifica `SCROLL` o `FOR_UPDATE`. Este tipo de cursor no permite las modificaciones de datos desde dentro del cursor.  
  
> [!NOTE]  
> Tanto `FAST_FORWARD` como `FORWARD_ONLY` se pueden usar en la misma instrucción `DECLARE CURSOR`.  
  
READ_ONLY  
Evita que se efectúen actualizaciones a través de este cursor. No se puede hacer referencia al cursor en una cláusula `WHERE CURRENT OF` de una instrucción `UPDATE` o `DELETE`. Esta opción reemplaza la capacidad predeterminada de actualizar el cursor.  
  
SCROLL_LOCKS  
Especifica que existan garantías de que las actualizaciones o las cancelaciones posicionadas realizadas a través del cursor se lleven a cabo correctamente. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bloquea las filas mientras se leen en el cursor para garantizar su disponibilidad en modificaciones posteriores. `SCROLL_LOCKS` no se puede especificar si también se especifica `FAST_FORWARD` o `STATIC`.  
  
OPTIMISTIC  
Especifica que las actualizaciones o las cancelaciones posicionadas realizadas a través del cursor no se lleven a cabo correctamente si la fila se ha actualizado desde que se leyó en el cursor. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no bloquea las filas cuando se leen en el cursor. En su lugar, usa comparaciones de valores de columna **timestamp** o un valor de suma de comprobación si la tabla no tiene columnas **timestamp** para determinar si la fila se ha modificado después de leerla en el cursor. Si la fila se ha modificado, la actualización o eliminación posicionada fracasa. `OPTIMISTIC` no se puede especificar si también se especifica `FAST_FORWARD`.  
  
 TYPE_WARNING  
 Especifica que se envía un mensaje de advertencia al cliente si el cursor se convierte implícitamente del tipo solicitado a otro.  
  
 *select_statement*  
 Es una instrucción SELECT estándar que define el conjunto de resultados del cursor. Las palabras clave `COMPUTE`, `COMPUTE BY`, `FOR BROWSE` y `INTO` no se permiten dentro de una instrucción *select_statement* de una declaración de cursor.  
  
> [!NOTE]  
> Puede usar una sugerencia de consulta dentro de una declaración de cursor; pero, si también usa la cláusula `FOR UPDATE OF`, especifique `OPTION (<query_hint>)` después de `FOR UPDATE OF`.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convierte implícitamente el cursor a otro tipo si las cláusulas de la instrucción *select_statement* entran en conflicto con la funcionalidad del tipo de cursor solicitado. Para obtener más información, vea el tema relativo a las conversiones de cursor implícitas.  
  
FOR UPDATE [OF *column_name* [**,**...*n*]]  
Define las columnas actualizables en el cursor. Si se especifica `OF <column_name> [, <... n>]`, solo las columnas enumeradas admiten modificaciones. Si se especifica `UPDATE` sin una lista de columnas, se pueden actualizar todas las columnas, a menos que se haya especificado la opción de simultaneidad `READ_ONLY`.  
  
## <a name="remarks"></a>Observaciones  
`DECLARE CURSOR` define los atributos de un cursor de servidor de [!INCLUDE[tsql](../../includes/tsql-md.md)], como su comportamiento de desplazamiento y la consulta usada para generar el conjunto de resultados sobre el que opera el cursor. La instrucción `OPEN` rellena el conjunto de resultados y la instrucción `FETCH` devuelve una fila del conjunto de resultados. La instrucción `CLOSE` libera el conjunto de resultados actual asociado al cursor. La instrucción `DEALLOCATE` libera los recursos que usa el cursor.  
  
La primera forma de la instrucción `DECLARE CURSOR` usa la sintaxis ISO para declarar comportamientos del cursor. La segunda forma de `DECLARE CURSOR` usa extensiones de [!INCLUDE[tsql](../../includes/tsql-md.md)] que permiten definir cursores con los mismos tipos de cursor usados en las funciones de cursor de la API de base de datos de ODBC o ADO.  
  
No se pueden combinar las dos formas. Si especifica las palabras clave `SCROLL` o `INSENSITIVE` antes de la palabra clave `CURSOR`, no podrá usar ninguna palabra clave entre las palabras clave `CURSOR` y `FOR <select_statement>`. Si especifica una palabra clave entre las palabras clave `CURSOR` y `FOR <select_statement>`, no podrá especificar `SCROLL` o `INSENSITIVE` antes de la palabra clave `CURSOR`.  
  
Si una `DECLARE CURSOR` que use la sintaxis [!INCLUDE[tsql](../../includes/tsql-md.md)] no especifica `READ_ONLY`, `OPTIMISTIC` o `SCROLL_LOCKS`, el valor predeterminado será el siguiente:  
  
-   Si la instrucción `SELECT` no acepta actualizaciones (permisos insuficientes, acceso a tablas remotas que no aceptan actualizaciones, etc.), el cursor es del tipo `READ_ONLY`.  
  
-   El valor predeterminado de los cursores `STATIC` y `FAST_FORWARD` es `READ_ONLY`.  
  
-   El valor predeterminado de los cursores `DYNAMIC` y `KEYSET` es `OPTIMISTIC`.  
  
Solo se puede hacer referencia a nombres de cursores mediante otras instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)]. No se puede hacer referencia a nombres de cursores mediante funciones de la API de base de datos. Por ejemplo, después de declarar un cursor, no se puede hacer referencia al nombre del cursor desde funciones o métodos de OLE DB, ODBC o ADO. No se pueden capturar las filas del cursor con las funciones o métodos de captura de las API; las filas solo se pueden capturar mediante instrucciones FETCH de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
Una vez que se ha declarado un cursor, se pueden utilizar estos procedimientos almacenados del sistema para determinar las características del cursor.  
  
|Procedimientos almacenados del sistema|Descripción|  
|------------------------------|-----------------|  
|**sp_cursor_list**|Devuelve la lista de los cursores que están visibles actualmente en la conexión y sus atributos.|  
|**sp_describe_cursor**|Describe los atributos de un cursor, por ejemplo si es de solo avance o de desplazamiento.|  
|**sp_describe_cursor_columns**|Describe los atributos de las columnas en el conjunto de resultados del cursor.|  
|**sp_describe_cursor_tables**|Describe las tablas base a las que tiene acceso el cursor.|  
  
 Se pueden usar variables como parte de la instrucción *select_statement* que declara un cursor. Los valores de las variables de cursor no cambian después de que se declara el cursor.  
  
## <a name="permissions"></a>Permisos  
 Los permisos para usar `DECLARE CURSOR` se asignan de forma predeterminada a los usuarios que tengan permisos para usar `SELECT` en las vistas, tablas y columnas usadas en el cursor.
 
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones

No puede usar cursores ni desencadenadores en una tabla con un índice clúster de almacén de columnas. Esta restricción no se aplica a los índices no clúster de almacén de columnas; puede usar cursores y desencadenadores en una tabla con un índice no clúster de almacén de columnas. 
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-simple-cursor-and-syntax"></a>A. Uso de cursores simples y su sintaxis  

El conjunto de resultados generado al abrir este cursor contiene todas las filas y todas las columnas de la tabla. Este cursor se puede actualizar, y todas las actualizaciones y eliminaciones se representan en las búsquedas realizadas para este cursor. `FETCH NEXT` es la única búsqueda disponible porque la opción `SCROLL` no se ha especificado.  
 
```sql  
DECLARE vend_cursor CURSOR  
    FOR SELECT * FROM Purchasing.Vendor  
OPEN vend_cursor  
FETCH NEXT FROM vend_cursor;  
```  
  
### <a name="b-using-nested-cursors-to-produce-report-output"></a>B. Uso de cursores anidados para elaborar resultados de informes  
 En el ejemplo siguiente se muestra cómo se pueden anidar los cursores para elaborar informes complejos. El cursor interno se declara para cada proveedor.  
  
```sql  
SET NOCOUNT ON;  
  
DECLARE @vendor_id INT, @vendor_name NVARCHAR(50),  
    @message VARCHAR(80), @product NVARCHAR(50);  
  
PRINT '-------- Vendor Products Report --------';  
  
DECLARE vendor_cursor CURSOR FOR   
SELECT VendorID, Name  
FROM Purchasing.Vendor  
WHERE PreferredVendorStatus = 1  
ORDER BY VendorID;  
  
OPEN vendor_cursor  
  
FETCH NEXT FROM vendor_cursor   
INTO @vendor_id, @vendor_name  
  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    PRINT ' '  
    SELECT @message = '----- Products From Vendor: ' +   
        @vendor_name  
  
    PRINT @message  
  
    -- Declare an inner cursor based     
    -- on vendor_id from the outer cursor.  
  
    DECLARE product_cursor CURSOR FOR   
    SELECT v.Name  
    FROM Purchasing.ProductVendor pv, Production.Product v  
    WHERE pv.ProductID = v.ProductID AND  
    pv.VendorID = @vendor_id  -- Variable value from the outer cursor  
  
    OPEN product_cursor  
    FETCH NEXT FROM product_cursor INTO @product  
  
    IF @@FETCH_STATUS <> 0   
        PRINT '         <<None>>'       
  
    WHILE @@FETCH_STATUS = 0  
    BEGIN  
  
        SELECT @message = '         ' + @product  
        PRINT @message  
        FETCH NEXT FROM product_cursor INTO @product  
        END  
  
    CLOSE product_cursor  
    DEALLOCATE product_cursor  
        -- Get the next vendor.  
    FETCH NEXT FROM vendor_cursor   
    INTO @vendor_id, @vendor_name  
END   
CLOSE vendor_cursor;  
DEALLOCATE vendor_cursor;  
```  
  
## <a name="see-also"></a>Consulte también  
 [@@FETCH_STATUS &#40;Transact-SQL&#41;](../../t-sql/functions/fetch-status-transact-sql.md)   
 [CLOSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/close-transact-sql.md)   
 [Cursores &#40;Transact-SQL&#41;](../../t-sql/language-elements/cursors-transact-sql.md)   
 [DEALLOCATE &#40;Transact-SQL&#41;](../../t-sql/language-elements/deallocate-transact-sql.md)   
 [FETCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/fetch-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
