---
description: DBCC SHOWCONTIG (Transact-SQL)
title: DBCC SHOWCONTIG (Transact-SQL)
ms.custom: ''
ms.date: 07/17/2017
ms.prod: sql
ms.prod_service: sql-database
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DBCC_SHOWCONTIG_TSQL
- DBCC SHOWCONTIG
- SHOWCONTIG
- SHOWCONTIG_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- displaying defragmentation information
- DBCC SHOWCONTIG statement
- defragmenting indexes
- leaf level defragmenting
- fragmentation [SQL Server]
- index defragmenting [SQL Server]
ms.assetid: 1df2123a-1197-4fff-91a3-25e3d8848aaa
author: pmasl
ms.author: umajay
ms.openlocfilehash: 26dd5c74d56cf279157c9c4d34f15c765b47f763
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813228"
---
# <a name="dbcc-showcontig-transact-sql"></a>DBCC SHOWCONTIG (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Muestra información sobre la fragmentación de los datos y los índices de la tabla o vista especificada.
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Use [sys.dm_db_index_physical_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md) en su lugar.  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hasta la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level))
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DBCC SHOWCONTIG   
[ (   
    { table_name | table_id | view_name | view_id }   
    [ , index_name | index_id ]   
) ]   
    [ WITH   
        {   
         [ , [ ALL_INDEXES ] ]   
         [ , [ TABLERESULTS ] ]   
         [ , [ FAST ] ]  
         [ , [ ALL_LEVELS ] ]   
         [ NO_INFOMSGS ]  
         }  
    ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *table_name* \| *table_id* \| *view_name* \| *view_id*  
 Es la tabla o vista cuya información de fragmentación se va a comprobar. Si no se especifica, se comprueban todas las tablas y vistas indizadas de la base de datos actual. Para obtener el id. de la tabla o la vista, use la función [OBJECT_ID](../../t-sql/functions/object-id-transact-sql.md).  
  
 *index_name* \| *index_id*  
 Es el índice cuya información de fragmentación se va a comprobar. Si no se especifica, la instrucción procesa el índice base de la tabla o la vista especificada. Para obtener el id. del índice, use la vista de catálogo [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 WITH  
 Especifica las opciones del tipo de información que devuelve la instrucción DBCC.  
  
 FAST  
 Especifica si se realiza un examen rápido del índice y se ofrece la mínima información de salida. Un examen rápido no lee las páginas hoja o de nivel de datos del índice.  
  
 ALL_INDEXES  
 Muestra el resultado de todos los índices para las tablas y vistas especificadas, aunque se haya especificado un índice determinado.  
  
 TABLERESULTS  
 Muestra el resultado como un conjunto de filas, con información adicional.  
  
 ALL_LEVELS  
 Se mantiene únicamente por compatibilidad con versiones anteriores. Aunque se especifique ALL_LEVELS, solo se procesa el nivel hoja de índice o el nivel de datos de tabla.  
  
 NO_INFOMSGS  
 Suprime todos los mensajes informativos con niveles de gravedad entre 0 y 10.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
En la tabla siguiente se describe la información del conjunto de resultados.
  
|Estadísticas|Descripción|  
|---|---|
|**Páginas examinadas**|Número de páginas de la tabla o el índice.|  
|**Extensiones examinadas**|Número de extensiones de la tabla o el índice.|  
|**Cambios de extensión**|Número de veces que la instrucción DBCC se ha movido de una extensión a otra al examinar las páginas de la tabla o el índice.|  
|**Avg. páginas por extensión**|Número de páginas por extensión en la cadena de páginas.|  
|**Densidad de examen [Mejor recuento: Recuento real]**|Es un porcentaje. Es la relación entre **Mejor recuento** y **Recuento real**. Este valor es 100 si todo es contiguo; si dicho valor es inferior a 100, existe fragmentación.<br /><br /> **Mejor recuento** es el número ideal de cambios de extensión si todo está vinculado de forma contigua. **Recuento real** es el número real de cambios de extensión.|  
|**Fragmentación de examen lógico**|Porcentaje de páginas que no funcionan resultante del examen de las páginas hoja del índice. Este número no es relevante para los montones. Una página no ordenada es aquella en la que la siguiente página física asignada al índice no es la que señala el puntero de *página siguiente* en la página hoja actual.|  
|**Fragmentación de examen de extensión**|Porcentaje de extensiones que no funcionan resultante del examen de las páginas hoja del índice. Este número no es relevante para los montones. Una extensión que no funciona es aquella en que la extensión que contiene la página actual de un índice no es físicamente la extensión siguiente a la que contiene la página anterior de un índice.<br /><br /> Nota: Este número carece de significado si el índice abarca varios archivos.|  
|**Avg. bytes libres por página**|Valor promedio de los bytes libres de las páginas exploradas. Cuanto más alto es el número, menos llenas estarán las páginas. Los números más bajos funcionan mejor si el índice no contiene muchas inserciones aleatorias. Este número también está influido por el tamaño de la fila; un tamaño de fila grande puede provocar un número más alto.|  
|**Avg. densidad de página (completa)**|Promedio de densidad de página en porcentaje. Este valor tiene en cuenta el tamaño de la fila. Por consiguiente, dicho valor es una medida más precisa del grado de llenado de las páginas. Cuanto mayor sea el porcentaje, mejor.|  
  
Cuando se especifica *id_de_tabla* y FAST, DBCC SHOWCONTIG devuelve un conjunto de resultados con solo las columnas siguientes.
-   **Páginas examinadas**  
-   **Cambios de extensión**  
-   **Densidad de examen [Mejor recuento:Recuento real]**  
-   **Fragmentación de examen de extensión**  
-   **Fragmentación de examen lógico**  
  
Si se especifica TABLERESULTS, DBCC SHOWCONTIG devuelve las siguientes columnas además de las nueve columnas descritas en la tabla anterior.
  
|Estadísticas|Descripción|  
|---|---|
|**Nombre de objeto**|Nombre de la tabla o la vista procesada.|  
|**ObjectId**|Id. del nombre del objeto.|  
|**IndexName**|Nombre del índice procesado. Es NULL para un montón.|  
|**IndexId**|Id. del índice. Es 0 para un montón.|  
|**Level**|Nivel del índice. El nivel 0 es el nivel hoja o datos del índice.<br /><br /> Para un montón, Level es 0.|  
|**Páginas**|Número de páginas que componen el nivel del índice o de todo el montón.|  
|**Filas**|Número de registros de datos o índices en este nivel del índice. Para un montón, este valor es el número de registros de datos en todo el montón.<br /><br /> En el caso de un montón, es posible que el número de registros devueltos por esta función no coincida con el número de filas devueltas al ejecutar SELECT COUNT(*) en el montón. Esto es debido a que una fila puede contener varios registros. Por ejemplo, en algunas situaciones de una actualización, una única fila del montón puede tener un registro de reenvío y un registro reenviado como resultado de la actualización. Asimismo, la mayoría de las filas LOB de gran tamaño se dividen en varios registros en almacenamiento de LOB_DATA.|  
|**MinimumRecordSize**|Tamaño mínimo del registro en el nivel de índice o en todo el montón.|  
|**MaximumRecordSize**|Tamaño máximo del registro en el nivel del índice o en todo el montón.|  
|**AverageRecordSize**|Promedio de tamaño del registro en el nivel de índice o en todo el montón.|  
|**ForwardedRecords**|Número de registros reenviados en el nivel de índice o en todo el montón.|  
|**Extents**|Número de extensiones en el nivel de índice o en todo el montón.|  
|**ExtentSwitches**|Número de veces que la instrucción DBCC se ha movido de una extensión a otra al examinar las páginas de la tabla o el índice.|  
|**AverageFreeBytes**|Valor promedio de los bytes libres de las páginas exploradas. Cuanto más alto es el número, menos llenas estarán las páginas. Los números más bajos funcionan mejor si el índice no contiene muchas inserciones aleatorias. Este número también está influido por el tamaño de la fila; un tamaño de fila grande puede provocar un número más alto.|  
|**AveragePageDensity**|Promedio de densidad de página en porcentaje. Este valor tiene en cuenta el tamaño de la fila. Por consiguiente, dicho valor es una medida más precisa del grado de llenado de las páginas. Cuanto mayor sea el porcentaje, mejor.|  
|**ScanDensity**|Es un porcentaje. Es la relación entre **BestCount** y **ActualCount**. Este valor es 100 si todo es contiguo; si dicho valor es inferior a 100, existe fragmentación.|  
|**BestCount**|Es el número idóneo de cambios de extensión si todo está vinculado de forma contigua.|  
|**ActualCount**|Es el número real de cambios de extensión.|  
|**LogicalFragmentation**|Porcentaje de páginas que no funcionan resultante del examen de las páginas hoja del índice. Este número no es relevante para los montones. Una página no ordenada es aquella en la que la siguiente página física asignada al índice no es la que señala el puntero de *página siguiente* en la página hoja actual.|  
|**ExtentFragmentation**|Porcentaje de extensiones que no funcionan resultante del examen de las páginas hoja del índice. Este número no es relevante para los montones. Una extensión que no funciona es aquella en que la extensión que contiene la página actual de un índice no es físicamente la extensión siguiente a la que contiene la página anterior de un índice.<br /><br /> Nota: Este número carece de significado si el índice abarca varios archivos.|  
  
Cuando se especifican WITH TABLERESULTS y FAST, el conjunto de resultados es el mismo que cuando se especifica WITH TABLERESULTS, con la diferencia de que las siguientes columnas tendrán valores NULL:

| Filas| Extents |
|---|---|
|**MinimumRecordSize**|**AverageFreeBytes**|  
|**MaximumRecordSize**|**AveragePageDensity**|  
|**AverageRecordSize**|**ExtentFragmentation**|  
|**ForwardedRecords**||  
  
## <a name="remarks"></a>Observaciones  
Cuando se especifica *id_de_índice*, la instrucción DBCC SHOWCONTIG recorre la cadena de páginas en el nivel hoja del índice especificado. Si solo se especifica *id_de_tabla* o si *id_de_índice* es 0, se examinan las páginas de datos de la tabla especificada. Esta operación solo requiere un bloqueo de tabla con intención compartida (IS). De este modo, se pueden realizar todas las actualizaciones e inserciones excepto las que requieren un bloqueo de tabla exclusivo (X). Esto permite un equilibrio entre la velocidad de ejecución y la no reducción de la simultaneidad con respecto al número de estadísticas devueltas. No obstante, si el comando se va a utilizar solo para medir la fragmentación, se recomienda utilizar la opción WITH FAST para que el rendimiento sea óptimo. Un examen rápido no lee las páginas hoja o de nivel de datos del índice. La opción WITH FAST no se aplica a un montón.
  
## <a name="restrictions"></a>Restricciones  
DBCC SHOWCONTIG no muestra los datos con los tipos de datos **ntext**, **text** e **image**. Esto se debe a que ya no existen índices de texto que almacenan datos de texto e imagen.
  
Además, DBCC SHOWCONTIG no admite algunas características nuevas. Por ejemplo:
-   Si la tabla o el índice especificados tienen particiones, DBCC SHOWCONTIG solo muestra la primera partición de la tabla o el índice especificados.  
-   DBCC SHOWCONTIG no muestra la información de almacenamiento de desbordamiento de fila y otros tipos de datos no consecutivos nuevos como **nvarchar(max)** , **varchar(max)** , **varbinary(max)** y **xml**.  
-   DBCC SHOWCONTIG no admite los índices espaciales.  
  
Todas las características nuevas son totalmente compatibles con la vista de administración dinámica [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md).
  
## <a name="table-fragmentation"></a>Fragmentación de tablas  
DBCC SHOWCONTIG determina si la tabla está muy fragmentada. La fragmentación de las tablas es consecuencia de los procesos de modificación de los datos (instrucciones INSERT, UPDATE y DELETE) efectuados en las tablas. Como dichas modificaciones no suelen estar distribuidas de forma equilibrada entre todas las filas de la tabla, el llenado de cada página puede variar con el paso del tiempo. En las consultas que examinan la totalidad o parte de una tabla, esta fragmentación de tabla puede ocasionar lecturas de páginas adicionales. Esto impide el examen paralelo de los datos.
  
Cuando un índice está muy fragmentado, existen dos opciones para reducir la fragmentación:
-   Quite y vuelva a crear un índice clúster.  
     La reconstrucción de un índice clúster reorganiza los datos y hace que las páginas de datos se llenen. El grado de llenado puede configurarse con la opción FILLFACTOR de CREATE INDEX. El inconveniente de este método es que el índice está sin conexión durante el proceso de eliminación y nueva creación, y que la operación es atómica. Si se interrumpe la creación del índice, no vuelve a crearse.  
-   Reordene las páginas de nivel hoja del índice en un orden lógico.  
     Use ALTER INDEX…REORGANIZE para cambiar el orden de las páginas de nivel hoja del índice en un orden lógico. Dado que esta operación se realiza en línea, el índice está disponible mientras se ejecuta la instrucción. También es posible interrumpir la operación sin perder todo el trabajo. El inconveniente de este método es que no es una forma tan buena de reorganizar los datos como la operación de quitar y volver a crear el índice clúster.  
-   Vuelva a generar el índice.  
     Para volver a generar el índice, utilice ALTER INDEX con REBUILD. Para obtener más información, vea [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
Las estadísticas **Avg. bytes libres por página** y **Promedio de densidad de página (completa)** del conjunto de resultados indican el llenado de las páginas de índice. Las estadísticas **Avg. bytes libres por página** debería ser bajo y el de **Promedio de densidad de página (completa)** debería ser alto para un índice que no tenga muchas inserciones aleatorias. Quitar y volver a crear un índice con la opción FILLFACTOR especificada puede mejorar estas estadísticas. Además, ALTER INDEX con REORGANIZE compactará un índice, teniendo en cuenta FILLFACTOR, lo que mejorará las estadísticas.
  
> [!NOTE]  
>  En un índice que contenga muchas inserciones aleatorias y páginas muy llenas se produce un aumento de las divisiones de páginas. Esto causa más fragmentación.  
  
El nivel de fragmentación de un índice puede determinarse de las siguientes formas:
-   Mediante la comparación de los valores de **Cambios de extensión** y **Extensiones examinadas**.  
     El valor de **Cambios de extensión** debe ser lo más parecido posible al de **Extensiones examinadas**. Esta relación se calcula como el valor de **Densidad del examen**. Dicho valor debe ser lo más alto posible y se puede aumentar mediante la reducción de la fragmentación del índice.  
  
    > [!NOTE]  
    >  Este método no funciona si el índice abarca varios archivos.  
  
-   Mediante la comprensión de los valores de **Fragmentación de examen lógico** y **Fragmentación de examen de extensión**.  
     Los valores de **Fragmentación de examen lógico** y, en menor medida, los de **Fragmentación de examen de extensión**, ofrecen la mejor indicación del nivel de fragmentación de una tabla. Ambos valores deberían tender a cero tanto como fuera posible, aunque puede ser aceptable un valor entre el 0 y el 10 por ciento.  
  
    > [!NOTE]  
    >  El valor de **Fragmentación de examen de extensión** es alto si el índice abarca varios archivos. Para reducir estos valores, debe reducir la fragmentación del índice.  
  
## <a name="permissions"></a>Permisos  
El usuario debe ser propietario de la tabla o ser un miembro del rol fijo de servidor **sysadmin** o de los roles fijos de base de datos **db_owner** o **db_ddladmin**.
  
## <a name="examples"></a>Ejemplos  
### <a name="a-displaying-fragmentation-information-for-a-table"></a>A. Presentar la información de fragmentación de una tabla  
En el siguiente ejemplo se muestra la información de fragmentación para la tabla `Employee`.
  
```sql  
USE AdventureWorks2012;  
GO  
DBCC SHOWCONTIG ('HumanResources.Employee');  
GO  
```  
  
### <a name="b-using-object_id-to-obtain-the-table-id-and-sysindexes-to-obtain-the-index-id"></a>B. Usar OBJECT_ID para obtener el Id. de la tabla y sys.indexes para obtener el Id. del índice  
En el siguiente ejemplo se usa `OBJECT_ID` y la vista de catálogo `sys.indexes` para obtener el id. de tabla y el id. de índice para el índice `AK_Product_Name` de la tabla `Production.Product` de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @id INT, @indid INT  
SET @id = OBJECT_ID('Production.Product')  
SELECT @indid = index_id   
FROM sys.indexes  
WHERE object_id = @id   
   AND name = 'AK_Product_Name'  
DBCC SHOWCONTIG (@id, @indid);  
GO  
```  
  
### <a name="c-displaying-an-abbreviated-result-set-for-a-table"></a>C. Mostrar un conjunto de resultados resumido de una tabla  
En el siguiente ejemplo se devuelve un conjunto de resultados resumido para la tabla `Product` de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].
  
```sql  
USE AdventureWorks2012;  
GO  
DBCC SHOWCONTIG ('Production.Product', 1) WITH FAST;  
GO  
```  
  
### <a name="d-displaying-the-full-result-set-for-every-index-on-every-table-in-a-database"></a>D. Mostrar el conjunto de resultados completo para todos los índices de todas las tablas de la base de datos  
En el siguiente ejemplo se devuelve un conjunto de resultados de tabla completo para todos los índices de todas las tablas de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].
  
```sql  
USE AdventureWorks2012;  
GO  
DBCC SHOWCONTIG WITH TABLERESULTS, ALL_INDEXES;  
GO  
```  
  
### <a name="e-using-dbcc-showcontig-and-dbcc-indexdefrag-to-defragment-the-indexes-in-a-database"></a>E. Usar DBCC SHOWCONTIG y DBCC INDEXDEFRAG para desfragmentar los índices de una base de datos  
En el siguiente ejemplo se muestra una forma sencilla de desfragmentar todos los índices de una base de datos que está fragmentada por encima de un umbral declarado.
  
```sql
/*Perform a 'USE <database name>' to select the database in which to run the script.*/  
-- Declare variables  
SET NOCOUNT ON;  
DECLARE @tablename VARCHAR(255);  
DECLARE @execstr   VARCHAR(400);  
DECLARE @objectid  INT;  
DECLARE @indexid   INT;  
DECLARE @frag      DECIMAL;  
DECLARE @maxfrag   DECIMAL;  
  
-- Decide on the maximum fragmentation to allow for.  
SELECT @maxfrag = 30.0;  
  
-- Declare a cursor.  
DECLARE tables CURSOR FOR  
   SELECT TABLE_SCHEMA + '.' + TABLE_NAME  
   FROM INFORMATION_SCHEMA.TABLES  
   WHERE TABLE_TYPE = 'BASE TABLE';  
  
-- Create the table.  
CREATE TABLE #fraglist (  
   ObjectName CHAR(255),  
   ObjectId INT,  
   IndexName CHAR(255),  
   IndexId INT,  
   Lvl INT,  
   CountPages INT,  
   CountRows INT,  
   MinRecSize INT,  
   MaxRecSize INT,  
   AvgRecSize INT,  
   ForRecCount INT,  
   Extents INT,  
   ExtentSwitches INT,  
   AvgFreeBytes INT,  
   AvgPageDensity INT,  
   ScanDensity DECIMAL,  
   BestCount INT,  
   ActualCount INT,  
   LogicalFrag DECIMAL,  
   ExtentFrag DECIMAL);  
  
-- Open the cursor.  
OPEN tables;  
  
-- Loop through all the tables in the database.  
FETCH NEXT  
   FROM tables  
   INTO @tablename;  
  
WHILE @@FETCH_STATUS = 0  
BEGIN  
-- Do the showcontig of all indexes of the table  
   INSERT INTO #fraglist   
   EXEC ('DBCC SHOWCONTIG (''' + @tablename + ''')   
      WITH FAST, TABLERESULTS, ALL_INDEXES, NO_INFOMSGS');  
   FETCH NEXT  
      FROM tables  
      INTO @tablename;  
END;  
  
-- Close and deallocate the cursor.  
CLOSE tables;  
DEALLOCATE tables;  
  
-- Declare the cursor for the list of indexes to be defragged.  
DECLARE indexes CURSOR FOR  
   SELECT ObjectName, ObjectId, IndexId, LogicalFrag  
   FROM #fraglist  
   WHERE LogicalFrag >= @maxfrag  
      AND INDEXPROPERTY (ObjectId, IndexName, 'IndexDepth') > 0;  
  
-- Open the cursor.  
OPEN indexes;  
  
-- Loop through the indexes.  
FETCH NEXT  
   FROM indexes  
   INTO @tablename, @objectid, @indexid, @frag;  
  
WHILE @@FETCH_STATUS = 0  
BEGIN  
   PRINT 'Executing DBCC INDEXDEFRAG (0, ' + RTRIM(@tablename) + ',  
      ' + RTRIM(@indexid) + ') - fragmentation currently '  
       + RTRIM(CONVERT(varchar(15),@frag)) + '%';  
   SELECT @execstr = 'DBCC INDEXDEFRAG (0, ' + RTRIM(@objectid) + ',  
       ' + RTRIM(@indexid) + ')';  
   EXEC (@execstr);  
  
   FETCH NEXT  
      FROM indexes  
      INTO @tablename, @objectid, @indexid, @frag;  
END;  
  
-- Close and deallocate the cursor.  
CLOSE indexes;  
DEALLOCATE indexes;  
  
-- Delete the temporary table.  
DROP TABLE #fraglist;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)  
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)  
[sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)  
[OBJECT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/object-id-transact-sql.md)  
[sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)
  
