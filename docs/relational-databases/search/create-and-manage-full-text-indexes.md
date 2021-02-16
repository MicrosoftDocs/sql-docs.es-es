---
description: Crear y administrar índices de texto completo
title: Creación y administración de índices de texto completo | Microsoft Docs
ms.custom: ''
ms.date: 03/31/2020
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- full-text indexes [SQL Server], about
ms.assetid: f8a98486-5438-44a8-b454-9e6ecbc74f83
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8a3c3792fa2aca009e02fc8fcdce6bf613b5e370
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341686"
---
# <a name="create-and-manage-full-text-indexes"></a>Crear y administrar índices de texto completo
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
En este tema se describe cómo crear, rellenar y administrar índices de texto completo en SQL Server.
  
## <a name="prerequisite---create-a-full-text-catalog"></a>Requisito previo: crear un catálogo de texto completo
Antes de poder crear un índice de texto completo, necesita tener un catálogo de texto completo. El catálogo es un contenedor virtual de uno o más índices de texto completo. Para obtener más información, vea [Create and Manage Full-Text Catalogs](../../relational-databases/search/create-and-manage-full-text-catalogs.md) (Crear y administrar catálogos de texto completo).
  
##  <a name="create-alter-or-drop-a-full-text-index"></a><a name="tasks"></a> Crear, modificar o quitar un índice de texto completo  
### <a name="create-a-full-text-index"></a>Crear un índice de texto completo  
  
-   [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)  
  
### <a name="alter-a-full-text-index"></a>Alterar un índice de texto completo
  
-   [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
### <a name="drop-a-full-text-index"></a>Quitar un índice de texto completo 
  
-   [DROP FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-index-transact-sql.md)

## <a name="populate-a-full-text-index"></a>Rellenar un índice de texto completo
El proceso para crear y mantener un índice de texto completo se denomina *rellenado* (o *rastreo*). Hay tres tipos de rellenado de índice de texto completo:
-   Rellenado completo
-   Rellenado basado en el seguimiento de cambios
-   Rellenado incremental basado en una marca de tiempo

Para obtener más información, vea [Populate Full-Text Indexes](../../relational-databases/search/populate-full-text-indexes.md) (Rellenar índices de texto completo).

##  <a name="view-the-properties-of-a-full-text-index"></a><a name="view"></a> Ver las propiedades de un índice de texto completo
### <a name="view-the-properties-of-a-full-text-index-with-transact-sql"></a>Ver las propiedades de un índice de texto completo con Transact-SQL

|Catálogo o vista de administración dinámica|Descripción|  
|----------------------------------------|-----------------|  
|[sys.fulltext_index_catalog_usages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-catalog-usages-transact-sql.md)|Devuelve una fila por cada referencia de catálogo de texto completo a índice de texto completo.|  
|[sys.fulltext_index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md)|Contiene una fila para cada columna que forma parte de un índice de texto completo.|  
|[sys.fulltext_index_fragments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-fragments-transact-sql.md)|Un índice de texto completo utiliza tablas internas denominadas fragmentos de índice de texto completo para almacenar los datos de índice invertidos. Esta vista se puede utilizar para consultar los metadatos sobre estos fragmentos. Esta vista contiene una fila para cada fragmento de índice de texto completo en cada tabla que contiene un índice de texto completo.|  
|[sys.fulltext_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)|Contiene una fila por índice de texto completo de un objeto tabular.|  
|[sys.dm_fts_index_keywords &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)|Devuelve información sobre el contenido de un índice de texto completo para la tabla especificada.|  
|[sys.dm_fts_index_keywords_by_document &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)|Devuelve información sobre el contenido de nivel de documento de un índice de texto completo para la tabla especificada. Una palabra clave determinada puede aparecer en varios documentos.|  
|[sys.dm_fts_index_population &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-population-transact-sql.md)|Devuelve información acerca de los rellenados de índices de texto completo actualmente en progreso.|  
 
### <a name="view-the-properties-of-a-full-text-index-with-management-studio"></a>Ver las propiedades de un índice de texto completo con Management Studio 
1.  En el Explorador de objetos de Management Studio, expanda el servidor.  
  
2.  Expanda **Bases de datos** y, después, la base de datos que contiene el índice de texto completo.  
  
3.  Expanda **Tablas**.  
  
4.  Haga clic con el botón derecho en la tabla en la que esté definido el índice de texto completo, seleccione **Índice de texto completo** y, en el menú contextual **Índice de texto completo** , haga clic en **Propiedades**. De esta forma se abre el cuadro de diálogo **Propiedades del índice de texto completo** .  
  
5.  En el panel **Seleccionar una página** , puede seleccionar cualquiera de las páginas siguientes:  
  
    |Página|Descripción|  
    |----------|-----------------|  
    |**General**|Muestra las propiedades básicas de un índice de texto completo. Entre estas propiedades se incluyen varias propiedades modificables y varias propiedades invariables, como el nombre de base de datos, el nombre de tabla y el nombre de columna de clave de texto completo. Las propiedades modificables son:<br /><br /> **Lista de palabras irrelevantes de índice de texto completo**<br /><br /> **Indexación de texto completo habilitada**<br /><br /> **Seguimiento de cambios**<br /><br /> **Lista de propiedades de búsqueda**|  
    |**Columnas**|Muestra las columnas de tabla que están disponibles para la indización de texto completo. La columna o columnas seleccionadas son de índices de texto completo. Puede seleccionar tantas columnas disponibles como desee incluir en el índice de texto completo. Para obtener más información, vea [Populate Full-Text Indexes](populate-full-text-indexes.md) (Rellenar índices de texto completo).|
    |**Programaciones**|Utilice esta página para crear o administrar programaciones para un trabajo del Agente SQL Server que inicie un rellenado de tabla incremental para los rellenados del índice de texto completo. Para obtener más información, vea [Populate Full-Text Indexes](../../relational-databases/search/populate-full-text-indexes.md) (Rellenar índices de texto completo).<br /><br /> Nota: Después de salir del cuadro de diálogo **Propiedades del índice de texto completo**, cualquier programación que se cree se asocia a un trabajo del Agente SQL Server (Iniciar rellenado incremental de tablas en *database_name*.*table_name*).|  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)] para guardar cualquier cambio y salir del cuadro de diálogo **Propiedades del índice de texto completo**.  
  
##  <a name="view-the-properties-of-indexed-tables-and-columns"></a><a name="props"></a> Ver las propiedades de tablas y columnas indexadas  
 Varias de las funciones de [!INCLUDE[tsql](../../includes/tsql-md.md)], como OBJECTPROPERTYEX, se pueden usar para obtener el valor de diversas propiedades de indización de texto completo. Esta información es útil para administrar y solucionar problemas de la búsqueda de texto completo.  
  
 En la siguiente tabla se enumeran las propiedades de texto completo relacionadas con las tablas y columnas indexadas y sus funciones de [!INCLUDE[tsql](../../includes/tsql-md.md)] relacionadas.  
  
|Propiedad|Descripción|Función|  
|--------------|-----------------|--------------|  
|**FullTextTypeColumn**|TYPE COLUMN de la tabla que contiene la información del tipo de documento de la columna.|[COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md)|  
|**IsFulltextIndexed**|Si una columna se ha habilitado para la indización de texto completo.|COLUMNPROPERTY|  
|**IsFulltextKey**|Si el índice es la clave de texto completo de una tabla.|[INDEXPROPERTY](../../t-sql/functions/indexproperty-transact-sql.md)|  
|**TableFulltextBackgroundUpdateIndexOn**|Si una tabla tiene actualización de índices de texto completo en segundo plano.|[OBJECTPROPERTYEX](../../t-sql/functions/objectpropertyex-transact-sql.md)|  
|**TableFulltextCatalogId**|Identificador del catálogo de texto completo en el que residen los datos de índice de texto completo para la tabla.|OBJECTPROPERTYEX|  
|**TableFulltextChangeTrackingOn**|Si una tabla tiene habilitado el seguimiento de cambios de texto completo.|OBJECTPROPERTYEX|  
|**TableFulltextDocsProcessed**|Número de filas procesadas desde el comienzo de la indización de texto completo.|OBJECTPROPERTYEX|  
|**TableFulltextFailCount**|Número de filas no indizadas por Búsqueda de texto completo.|OBJECTPROPERTYEX|  
|**TableFulltextItemCount**|Número de filas para las que se crearon índices de texto completo correctamente.|OBJECTPROPERTYEX|  
|**TableFulltextKeyColumn**|Proporciona el identificador de la columna de clave única de texto completo.|OBJECTPROPERTYEX|  
|**TableFullTextMergeStatus**|Si una tabla que tiene un índice de texto completo se está combinando actualmente.|OBJECTPROPERTYEX|  
|**TableFulltextPendingChanges**|Número de entradas de seguimiento de cambios pendientes de procesamiento.|OBJECTPROPERTYEX|  
|**TableFulltextPopulateStatus**|Estado del rellenado de una tabla de texto completo.|OBJECTPROPERTYEX|  
|**TableHasActiveFulltextIndex**|Si la tabla tiene un índice de texto completo activo.|OBJECTPROPERTYEX|  
  
##  <a name="get-info-about-the-full-text-key-column"></a><a name="key"></a> Obtener información sobre la columna de clave de texto completo  
 Normalmente, el resultado de las funciones de valores de conjunto de filas CONTAINSTABLE o FREETEXTTABLE tiene que combinarse con la tabla base. En esos casos, necesita conocer el nombre de la columna de clave única. Puede consultar si un índice único determinado se utiliza como clave de texto completo y obtener el identificador de la columna de clave de texto completo.  
  
### <a name="determine-whether-a-given-unique-index-is-used-as-the-full-text-key-column"></a>Determinar si un índice único determinado se utiliza como columna de clave de texto completo  
  
Utilice una instrucción [SELECT](../../t-sql/queries/select-transact-sql.md) para llamar a la función [INDEXPROPERTY](../../t-sql/functions/indexproperty-transact-sql.md) . En la llamada a la función, use la función OBJECT_ID para convertir el nombre de la tabla (*nombre_tabla*) en el identificador de tabla, especifique el nombre de un índice único para la tabla y especifique la propiedad del índice **IsFulltextKey** como sigue:  
  
```sql  
SELECT INDEXPROPERTY( OBJECT_ID('table_name'), 'index_name',  'IsFulltextKey' );  
```  
  
 Esta instrucción devuelve el valor 1 si el índice se usa para exigir la exclusividad de la columna de clave de texto completo; de lo contrario, se devuelve 0.  
  
 **Ejemplo**  
  
 El ejemplo siguiente consulta si el índice `PK_Document_DocumentNode` se usa para exigir la exclusividad de la columna de clave de texto completo como sigue:  
  
```sql  
USE AdventureWorks  
GO  
SELECT INDEXPROPERTY ( OBJECT_ID('Production.Document'), 'PK_Document_DocumentNode',  'IsFulltextKey' )  
```  
  
 Este ejemplo devuelve 1 si el índice `PK_Document_DocumentNode` se utiliza para exigir la exclusividad de la columna de clave de texto completo. De lo contrario, devuelve 0 o NULL. NULL implica que está usando un nombre de índice no válido, el nombre de índice no corresponde a la tabla, la tabla no existe, etcétera.  
  
### <a name="find-the-identifier-of-the-full-text-key-column"></a>Buscar el identificador de la columna de clave de texto completo  
  
Todas las tablas habilitadas para texto completo tienen una columna que se usa para aplicar las filas únicas de la tabla (*columna de clave**única*). La propiedad **TableFulltextKeyColumn** obtenida mediante la función OBJECTPROPERTY proporciona la identidad de esta columna de clave única.  
 
Para obtener este identificador, puede utilizar una instrucción SELECT con el fin de llamar a la función OBJECTPROPERTYEX. Use la función OBJECT_ID para convertir el nombre de la tabla (*nombre_tabla*) en el identificador de tabla y especificar la propiedad **TableFulltextKeyColumn** como sigue:  
  
```sql  
SELECT OBJECTPROPERTYEX(OBJECT_ID( 'table_name'), 'TableFulltextKeyColumn' ) AS 'Column Identifier';  
```  
  
 **Ejemplos**  
  
 En el ejemplo siguiente se devuelve el identificador de la columna de clave de texto completo o NULL. NULL implica que está usando un nombre de índice no válido, el nombre de índice no corresponde a la tabla, la tabla no existe, etcétera.  
  
```sql
USE AdventureWorks;  
GO  
SELECT OBJECTPROPERTYEX(OBJECT_ID('Production.Document'), 'TableFulltextKeyColumn');  
GO  
```  
  
 En el ejemplo siguiente se muestra cómo usar el identificador de la columna de clave única para obtener el nombre de la columna.  
  
```sql  
USE AdventureWorks;  
GO  
DECLARE @key_column sysname  
SET @key_column = Col_Name(Object_Id('Production.Document'),  
ObjectProperty(Object_id('Production.Document'),  
'TableFulltextKeyColumn')   
)  
SELECT @key_column AS 'Unique Key Column';  
GO  
```  
  
 Este ejemplo devuelve una columna de conjunto de resultados denominada `Unique Key Column`, que muestra una única fila que contiene el nombre de la columna de clave única de la tabla Document, DocumentNode. Tenga en cuenta que, si esta consulta contuviera un nombre de índice no válido, el nombre del índice no correspondiera a la tabla, la tabla no existiera, etc., devolvería NULL.  

## <a name="index-varbinarymax-and-xml-columns"></a>Indexar columnas varbinary(max) y xml  
 Si una columna **varbinary(max)** , **varbinary** o **xml** está indexada con texto completo, se puede consultar con los predicados de texto completo (CONTAINS y FREETEXT) y las funciones (CONTAINSTABLE y FREETEXTTABLE), igual que cualquier otra columna indexada de texto completo.
   
### <a name="index-varbinarymax-or-varbinary-data"></a>Indexar datos varbinary(máximo) o varbinary  
 Una sola columna **varbinary(max)** o **varbinary** puede almacenar muchos tipos de documentos. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite cualquier tipo de documento para el que tenga un filtro instalado que esté disponible en el sistema operativo. La extensión de archivo del documento identifica el tipo de cada documento. Por ejemplo, para la extensión de archivo .doc, la búsqueda de texto completo utiliza el filtro que admite los documentos de Microsoft Word. Para obtener una lista de los tipos de documentos disponibles, consulte la vista de catálogo [sys.fulltext_document_types](../../relational-databases/system-catalog-views/sys-fulltext-document-types-transact-sql.md) .  
  
Observe que el motor de búsqueda de texto completo puede aprovechar los filtros existentes que se instalan en el sistema operativo. Para poder utilizar los separadores de palabras, los lematizadores y los filtros del sistema operativo, debe cargarlos en la instancia del servidor, como sigue:  
  
```sql  
EXEC sp_fulltext_service @action='load_os_resources', @value=1  
```  
  
Para crear un índice de texto completo en una columna **varbinary(max)** , el motor de búsqueda de texto completo necesita tener acceso a las extensiones de archivo de los documentos en la columna **varbinary(max)** . Esta información debe almacenarse en una columna de la tabla, denominada columna de tipo, que debe estar asociada a la columna **varbinary(max)** en el índice de texto completo. Al indizar un documento, el motor de búsqueda de texto completo usa la extensión de archivo de la columna de tipo para identificar qué filtro usar.  
   
### <a name="index-xml-data"></a>Indexar Datos xml  
 Una columna del tipo de datos **xml** solo almacena los documentos y fragmentos XML, y solo se usa el filtro XML para los documentos. Por consiguiente, una columna de tipo es innecesaria. En las columnas **xml** , el índice de texto completo indexa el contenido de los elementos XML, pero omite el formato XML. Los valores de los atributos se incluyen en el índice de texto completo a menos que sean valores numéricos. Las etiquetas de elemento se usan como límites de token. Se admiten fragmentos y documentos con formato XML o HTML correcto que contengan varios idiomas.  
  
 Para obtener más información sobre cómo indexar y consultar en una columna **xml**, vea [Usar la búsqueda de texto completo con columnas XML](../../relational-databases/xml/use-full-text-search-with-xml-columns.md).  
  
##  <a name="disable-or-re-enable-full-text-indexing-for-a-table"></a><a name="disable"></a> Deshabilitación o rehabilitación de la indización de texto completo para una tabla   
 En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], todas las bases de datos creadas por el usuario están habilitadas para texto completo de forma predeterminada. Además, una tabla individual se habilitará automáticamente para la indización de texto completo en cuanto se cree un índice de texto completo en la misma y se agregue una columna al índice. Una tabla se deshabilitará automáticamente para la indización de texto completo cuando se quite la última columna de su índice de texto completo.  
  
 En una tabla que tiene un índice de texto completo, se puede deshabilitar o volver a habilitar manualmente una tabla para la indización de texto completo utilizando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  

1.  Expanda el grupo de servidores, expanda **Bases de datos** y luego expanda la base de datos que contenga la tabla que quiera habilitar para la indexación de texto completo.  
  
2.  Expanda **Tablas** y haga clic con el botón derecho en la tabla que quiere deshabilitar o volver a habilitar para la indexación de texto completo.  
  
3.  Seleccione **Índice de texto completo** y luego haga clic en **Deshabilitar índice de texto completo** o en **Habilitar índice de texto completo**.  
  
##  <a name="remove-a-full-text-index-from-a-table"></a><a name="remove"></a> Quitar un índice de texto completo de una tabla  
  
1.  En el Explorador de objetos, haga clic con el botón secundario en la tabla que contiene el índice de texto completo que desea eliminar.  
  
2.  Seleccione **Eliminar índice de texto completo**.  
  
3.  Cuando se le pida, haga clic en **Aceptar** para confirmar que quiere eliminar el índice de texto completo.  
  
  
