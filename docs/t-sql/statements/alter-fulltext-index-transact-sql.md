---
description: ALTER FULLTEXT INDEX (Transact-SQL)
title: ALTER FULLTEXT INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER FULLTEXT INDEX
- ALTER_FULLTEXT_INDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], search property lists
- modifying full-text indexes
- full-text indexes [SQL Server], modifying
- search property lists [SQL Server], associating with full-text indexes
- ALTER FULLTEXT INDEX statement
ms.assetid: b6fbe9e6-3033-4d1b-b6bf-1437baeefec3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 17981c3c04c17b0c0291714a245dfa1856a69af7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204223"
---
# <a name="alter-fulltext-index-transact-sql"></a>ALTER FULLTEXT INDEX (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Cambia las propiedades de un índice de texto completo en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER FULLTEXT INDEX ON table_name  
   { ENABLE   
   | DISABLE  
   | SET CHANGE_TRACKING [ = ] { MANUAL | AUTO | OFF }  
   | ADD ( column_name   
           [ TYPE COLUMN type_column_name ]   
           [ LANGUAGE language_term ]  
           [ STATISTICAL_SEMANTICS ]  
           [,...n]   
         )  
     [ WITH NO POPULATION ]  
   | ALTER COLUMN column_name  
     { ADD | DROP } STATISTICAL_SEMANTICS  
     [ WITH NO POPULATION ]  
   | DROP ( column_name [,...n] )  
     [ WITH NO POPULATION ]   
   | START { FULL | INCREMENTAL | UPDATE } POPULATION  
   | {STOP | PAUSE | RESUME } POPULATION   
   | SET STOPLIST [ = ] { OFF| SYSTEM | stoplist_name }  
     [ WITH NO POPULATION ]   
   | SET SEARCH PROPERTY LIST [ = ] { OFF | property_list_name }  
     [ WITH NO POPULATION ]   
   }  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *table_name*  
 Es el nombre de la tabla o vista indizada que contiene la columna o columnas incluidas en el índice de texto completo. Especificar los nombres de la base de datos y del propietario de la tabla es opcional.  
  
 ENABLE | DISABLE  
 Indica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si debe recopilar datos de índice de texto completo para *table_name*. ENABLE activa el índice de texto completo; DISABLE lo desactiva. La tabla no admitirá las consultas de texto completo mientras esté deshabilitado el índice.  
  
 La deshabilitación de un índice de texto completo permite desactivar el seguimiento de cambios y mantener el índice de texto completo, que puede reactivar en cualquier momento con ENABLE. Cuando se deshabilita el índice de texto completo, los metadatos del índice de texto completo permanecen en las tablas del sistema. Si CHANGE_TRACKING está habilitado (actualización automática o manual) cuando se deshabilita el índice de texto completo, el estado del índice se inmoviliza, los rastreos en curso se detienen y no se mantiene un seguimiento de los nuevos cambios en los datos de la tabla ni se propagan los cambios al índice.  
  
 SET CHANGE_TRACKING {MANUAL | AUTO | OFF}   
 Especifica si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] propagará al índice de texto completo los cambios (actualizaciones, eliminaciones o inserciones) efectuados en las columnas de la tabla que cubre el índice de texto completo. Los cambios realizados en los datos con WRITETEXT y UPDATETEXT no se reflejan en el índice de texto completo y no se recopilan con el seguimiento de cambios.  
  
> [!NOTE]  
>  Para más información, vea [Interacciones del seguimiento de cambios y del parámetro NO POPULATION](#change-tracking-no-population).
  
 MANUAL  
 Especifica que se propagarán manualmente los cambios controlados mediante una llamada a la instrucción ALTER FULLTEXT INDEX... START UPDATE POPULATION [!INCLUDE[tsql](../../includes/tsql-md.md)] statement (*manual population*). Puede utilizar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para llamar a esta instrucción de [!INCLUDE[tsql](../../includes/tsql-md.md)] de forma periódica.  
  
 AUTO  
 Especifica que los cambios sometidos a seguimiento se propagarán automáticamente cuando los datos se modifiquen en la tabla base (*rellenado automático*). Aunque los cambios se propagan de forma automática, podrían no reflejarse de inmediato en el índice de texto completo. AUTO es el valor predeterminado.  
  
 Apagado  
 Especifica que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no mantendrá una lista de cambios en los datos indizados.  
  
 ADD | DROP *column_name*  
 Especifica las columnas que se agregarán o eliminarán de un índice de texto completo. La columna o columnas deben ser de tipo **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml**, **varbinary**, o **varbinary(max)**.  
  
 Utilice la cláusula DROP solamente en columnas que se hayan habilitado previamente para la indización de texto completo.  
  
 Use TYPE COLUMN y LANGUAGE con la cláusula ADD para establecer estas propiedades en *column_name*. Cuando se agrega una columna, el índice de texto completo de la tabla se debe rellenar de nuevo para que funcionen las consultas de texto completo en esta columna.  
  
> [!NOTE]  
>  Que se rellene el índice de texto completo una vez agregada o quitada una columna en un índice de texto completo depende de si está habilitado el seguimiento de cambios y de si se especifica WITH NO POPULATION. Para más información, vea [Interacciones del seguimiento de cambios y del parámetro NO POPULATION](#change-tracking-no-population).
  
 TYPE COLUMN *type_column_name*  
 Especifica el nombre de una columna de tabla, *type_column_name*, que se usa para almacenar el tipo de documento para un documento **varbinary**, **varbinary(max)** o **image**. Esta columna, denominada columna de tipo, contiene una extensión de archivo proporcionada por el usuario (.doc, .pdf, .xls, etc.). La columna de tipo debe ser de tipo **char**, **nchar**, **varchar** o **nvarchar**.  
  
 Especifique TYPE COLUMN *type_column_name* únicamente si *column_name* especifica una columna de tipo **varbinary**, **varbinary(max)** o **image**, en la que los datos se almacenan como datos binarios; de lo contrario, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve un error.  
  
> [!NOTE]  
>  En el momento de la indización, el motor de texto completo usa la abreviatura de la columna de tipo de cada fila de la tabla para identificar el filtro de la búsqueda de texto completo que se va a usar para el documento en *column_name*. El filtro carga el documento como un flujo binario, quita la información del formato y envía el texto del documento al componente de separador de palabras. Para obtener más información, vea [Configurar y administrar filtros para búsquedas](../../relational-databases/search/configure-and-manage-filters-for-search.md).  
  
 LANGUAGE *language_term*  
 Se trata del idioma de los datos almacenados en **column_name**.  
  
 *language_term* es opcional y puede especificarse como un valor hexadecimal, un entero o una cadena correspondiente al identificador de configuración regional (LCID) de un idioma. Si se especifica *language_term*, el idioma que representa se aplicará a todos los elementos de la condición de búsqueda. Si no se especifica ningún valor, se usa el idioma de texto completo predeterminado de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Use el procedimiento almacenado **sp_configure** para acceder a la información sobre el idioma de texto completo predeterminado de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Si se especifica como una cadena, *language_term* corresponde al valor de columna **alias** de la tabla del sistema **syslanguages**. La cadena debe estar delimitada con comillas sencillas, como en '*language_term*'. Cuando se especifica como un entero, *language_term* es el LCID real que identifica el idioma. Cuando se especifica como un valor hexadecimal, *language_term* es 0x seguido del valor hexadecimal del LCID. El valor hexadecimal no puede superar los ocho dígitos, incluidos los ceros a la izquierda.  
  
 Si el valor está en formato DBCS (juego de caracteres de doble byte), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo convertirá a Unicode.  
  
 Se deben habilitar recursos, como los separadores de palabras y lematizadores, para el idioma especificado como *language_term*. Si estos recursos no admiten el idioma especificado, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve un error.  
  
 Para las columnas no BLOB y no XML que contienen datos de texto en varios idiomas o en los casos en que se desconoce el idioma del texto almacenado en la columna, utilice el recurso de idioma neutro (0x0). Para los documentos almacenados en columnas de tipo XML o BLOB, la codificación de idioma del documento se utilizará en el momento de la indización. Por ejemplo, en las columnas XML, el atributo xml:lang de los documentos XML identifica el idioma. En el momento de la consulta, el valor especificado previamente en *language_term* se convierte en el idioma predeterminado que se usa para las consultas de texto completo, a menos que *language_term* se especifique como parte de una consulta de texto completo.  
  
 STATISTICAL_SEMANTICS  
 **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
 Crea los índices adicionales de similitud de documentos y frases clave que forman parte de la indización semántica estadística. Para obtener más información, vea [Búsqueda semántica &#40;SQL Server&#41;](../../relational-databases/search/semantic-search-sql-server.md).  
  
 [ **,**_...n_]  
 Indica que se pueden especificar varias columnas para las cláusulas ADD, ALTER o DROP. Si se especifican varias columnas, sepárelas con comas.  
  
 WITH NO POPULATION  
 Especifica que el índice de texto completo no se rellenará después de que se realice una de las operaciones de columna ADD y DROP, o una operación SET STOPLIST. El índice se rellenará solamente si el usuario ejecuta un comando START...POPULATION.  
  
 Si se especifica NO POPULATION, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no rellena un índice. El índice se rellena únicamente cuando el usuario usa un comando ALTER FULLTEXT INDEX...START POPULATION. Cuando no se especifica NO POPULATION, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rellena el índice.  
  
 Si CHANGE_TRACKING está habilitado y se especifica WITH NO POPULATION, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve un error. Si CHANGE_TRACKING está habilitado y no se especifica WITH NO POPULATION, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] realiza un rellenado completo del índice.  
  
> [!NOTE]  
>  Para más información, vea [Interacciones del seguimiento de cambios y del parámetro NO POPULATION](#change-tracking-no-population).
  
 {ADD | DROP } STATISTICAL_SEMANTICS  
 **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
 Habilita o deshabilita la indización semántica estadística de las columnas especificadas. Para obtener más información, vea [Búsqueda semántica &#40;SQL Server&#41;](../../relational-databases/search/semantic-search-sql-server.md).  
  
 START {FULL|INCREMENTAL|UPDATE} POPULATION  
 Indica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que empiece el rellenado del índice de texto completo de *language_term*. Si ya hay un rellenado de índice de texto completo en curso, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve una advertencia y no inicia un nuevo rellenado.  
  
 FULL  
 Especifica que se recuperarán todas las filas de la tabla para el indizado de texto completo, incluso si las filas ya se han indizado.  
  
 INCREMENTAL  
 Especifica que solo se recuperarán para el índice de texto completo las filas modificadas desde el último rellenado. INCREMENTAL se puede aplicar únicamente si la tabla tiene una columna de tipo **timestamp**. Si una tabla del catálogo de texto completo no contiene una columna de tipo **timestamp**, se lleva a cabo un rellenado completo en la tabla.  
  
 UPDATE  
 Especifica el procesamiento de todas las inserciones, actualizaciones o eliminaciones desde la última vez que se actualizó el índice de seguimiento de cambios. Debe estar habilitado el rellenado de seguimiento de cambios en la tabla, pero el índice de actualización en segundo plano o el seguimiento automático de cambios no se deben activar.  
  
 {STOP | PAUSE | RESUME } POPULATION   
 Detiene o pausa cualquier operación de rellenado en curso; o bien detiene o reanuda cualquier operación de rellenado en pausa.  
  
 STOP POPULATION no detiene el seguimiento automático de cambios ni el índice de actualización en segundo plano. Para detener el seguimiento de cambios, utilice SET CHANGE_TRACKING OFF.  
  
 PAUSE POPULATION y RESUME POPULATION solo se pueden usar para operaciones de rellenado completas. No son relevantes para otros tipos de operación de rellenado porque las otras operaciones de rellenado reanudan los rastreos desde el punto en que se detuvieron.  
  
 SET STOPLIST { OFF| SYSTEM | *stoplist_name* }  
 Cambia la lista de palabras irrelevantes de texto completo asociada al índice, si hay alguna.  
  
 Apagado  
 Especifica que no se asocie al índice de texto completo ninguna lista de palabras irrelevantes.  
  
 SYSTEM  
 Especifica que la lista de palabras irrelevantes predeterminada de texto completo del sistema se debe usar para este índice de texto completo.  
  
 *stoplist_name*  
 Especifica el nombre de la lista de palabras irrelevantes que se va a asociar al índice de texto completo.  
  
 Para obtener más información, vea [Configurar y administrar palabras irrelevantes y listas de palabras irrelevantes para la búsqueda de texto completo](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md).  
  
 SET SEARCH PROPERTY LIST { OFF | *property_list_name* } [ WITH NO POPULATION ]  
 **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
 Cambia la lista de propiedades de búsqueda asociada al índice, si hay alguna.  
  
 Apagado  
 Especifica que no se asocie al índice de texto completo ninguna lista de propiedades. Al desactivar la lista de propiedades de búsqueda de un índice de texto completo (ALTER FULLTEXT INDEX... SET SEARCH PROPERTY LIST OFF), la búsqueda de propiedades en la tabla base ya no es posible.  
  
 De forma predeterminada, cuando se desactiva una lista de propiedades de búsqueda existente, el índice de texto completo se vuelve a rellenar automáticamente. Si se especifica WITH NO POPULATION al desactivar la lista de propiedades de búsqueda, este rellenado automático no sucede. Sin embargo, se recomienda ejecutar, si procede, un rellenado completo del índice de texto completo para mayor comodidad. Al rellenar el índice de texto completo, se eliminan los metadatos específicos de cada propiedad de búsqueda que se haya quitado, con lo que el índice de texto completo resultará más pequeño y eficaz.  
  
 *property_list_name*  
 Especifica el nombre de la lista de propiedades de búsqueda que se va a asociar al índice de texto completo.  
  
 Para agregar una lista de propiedades de búsqueda a un índice de texto completo es preciso rellenar el índice, a fin de indizar las propiedades de búsqueda que se han registrado para la lista de propiedades de búsqueda asociada. Si se especifica WITH NO POPULATION al agregar la lista de propiedades de búsqueda, se deberá ejecutar un rellenado del índice en un momento adecuado.  
  
> [!IMPORTANT]  
>  Si el índice de texto completo estaba asociado previamente a una lista de propiedades de búsqueda diferente, habrá que volver a generar la lista de propiedades para que el índice tenga un estado coherente. El índice se trunca inmediatamente y está vacío hasta que se ejecuta el rellenado completo. Para más información, vea [Cambiar la lista de propiedades de búsqueda provoca un rellenado del índice](#change-search-property-rebuild-index). 
  
> [!NOTE]  
>  Puede asociar una lista de propiedades de búsqueda a más de un índice de texto completo en la misma base de datos.  
  
 **Para buscar las listas de propiedades de búsqueda en la base de datos actual**  
  
-   [sys.registered_search_property_lists](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)  
  
 Para más información, vea [Buscar propiedades de documento con listas de propiedades de búsqueda](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
## <a name="interactions-of-change-tracking-and-no-population-parameter"></a><a name="change-tracking-no-population"></a> Interacciones del seguimiento de cambios y del parámetro NO POPULATION  
 Que se rellene el índice de texto completo depende de si el seguimiento de cambios está habilitado y si se especifica WITH NO POPULATION en la instrucción ALTER FULLTEXT INDEX. En la tabla siguiente se resume el resultado de su interacción.  
  
|Seguimiento de cambios|WITH NO POPULATION|Resultado|  
|---------------------|------------------------|------------|  
|No se ha habilitado|Sin especificar|Se realiza un rellenado completo en el índice.|  
|No se ha habilitado|Specified|No se produce el rellenado del índice hasta que se emite una instrucción ALTER FULLTEXT INDEX...START POPULATION.|  
|habilitado|Specified|Se produce un error y no se altera el índice.|  
|habilitado|Sin especificar|Se realiza un rellenado completo en el índice.|  
  
 Para más información sobre rellenar índices de texto completo, vea [Rellenar índices de texto completo](../../relational-databases/search/populate-full-text-indexes.md).  
  
## <a name="changing-the-search-property-list-causes-rebuilding-the-index"></a><a name="change-search-property-rebuild-index"></a> Cambiar la lista de propiedades de búsqueda provoca un rellenado del índice  
 La primera vez que el índice de texto completo se asocia con una lista de propiedades de búsqueda, el índice debe volver a llenarse para incluir los términos de búsqueda específicos de la propiedad. Los datos del índice existente no se truncan.  
  
 Sin embargo, si se asocia el índice de texto completo con otra lista de propiedades, se vuelve a generar el índice. Una generación inmediata trunca el índice de texto completo y quita todos los datos existentes, de tal forma que el índice se debe volver a llenar. A medida que avanza el rellenado, las consultas de texto completo de la tabla base buscan únicamente en las filas de la tabla que ya se han indizado al efectuar el rellenado. Los datos del índice rellenado incluirán metadatos de las propiedades registradas en la lista de propiedades agregada recientemente.  
  
 Los escenarios que provocan una nueva generación son:  
  
-   Cambiar directamente a otra lista de propiedades de búsqueda (vea "Escenario A", más adelante en esta sección).  
  
-   Desactivar la lista de propiedades de lista y asociar después el índice con cualquier lista de propiedades de búsqueda (vea "Escenario B", más adelante en esta sección).  
  
> [!NOTE]  
>  Para más información sobre el funcionamiento de la búsqueda de texto completo con listas de propiedades de búsqueda, vea [Buscar propiedades de documento con listas de propiedades de búsqueda](../../relational-databases/search/search-document-properties-with-search-property-lists.md). Para más información sobre los rellenados completos, vea [Rellenar índices de texto completo](../../relational-databases/search/populate-full-text-indexes.md).  
  
### <a name="scenario-a-switching-directly-to-a-different-search-property-list"></a>Escenario A: cambiar directamente a otra lista de propiedades de búsqueda  
  
1.  Se crea un índice de texto completo en `table_1` con una lista de propiedades de búsqueda `spl_1`:  
  
    ```sql  
    CREATE FULLTEXT INDEX ON table_1 (column_name) KEY INDEX unique_key_index   
       WITH SEARCH PROPERTY LIST=spl_1,   
       CHANGE_TRACKING OFF, NO POPULATION;   
    ```  
  
2.  Se ejecuta un rellenado completo del índice de texto completo:  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1 START FULL POPULATION;  
    ```  
  
3.  Después, el índice de texto completo se asocia a otra lista de propiedades de búsqueda, `spl_2`, mediante la siguiente instrucción:  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1 SET SEARCH PROPERTY LIST spl_2;  
    ```  
  
     Esta instrucción produce un rellenado completo, el comportamiento predeterminado.  Sin embargo, antes de empezar este rellenado, el motor de texto completo automáticamente trunca el índice.  
  
### <a name="scenario-b-turning-off-the-search-property-list-and-later-associating-the-index-with-any-search-property-list"></a>Escenario B: desactivar la lista de propiedades de búsqueda lista y después asociar el índice con cualquier lista de propiedades de búsqueda  
  
1.  Se crea un índice de texto completo en `table_1` con una lista de propiedades de búsqueda `spl_1`, seguido de un rellenado completo automático (comportamiento predeterminado):  
  
    ```sql  
    CREATE FULLTEXT INDEX ON table_1 (column_name) KEY INDEX unique_key_index   
       WITH SEARCH PROPERTY LIST=spl_1;   
    ```  
  
2.  La lista de propiedades de búsqueda se desactiva, como sigue:  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1   
       SET SEARCH PROPERTY LIST OFF WITH NO POPULATION;   
    ```  
  
3.  El índice de texto completo se asocia de nuevo a la misma lista de propiedades de búsqueda o a otra distinta.  
  
     Por ejemplo, la siguiente instrucción vuelve a asociar el índice de texto completo con la lista de propiedades de búsqueda original, `spl_1`:  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1 SET SEARCH PROPERTY LIST spl_1;  
    ```  
  
     Esta instrucción inicia un rellenado completo, el comportamiento predeterminado.  
  
    > [!NOTE]  
    >  También se debería realizar la regeneración para otra lista de propiedades de búsqueda, como `spl_2`.  
  
## <a name="permissions"></a>Permisos  
 El usuario debe tener el permiso ALTER en la tabla o vista indizada, o ser miembro del rol fijo de servidor **sysadmin** o de los roles fijos de base de datos **db_owner** o **db_ddladmin**.  
  
 Si se especifica SET STOPLIST, el usuario debe tener el permiso REFERENCES en la lista de palabras irrelevantes. Si se especifica SET SEARCH PROPERTY LIST, el usuario debe tener el permiso REFERENCES para la lista de propiedades de búsqueda. El propietario de la lista de palabras irrelevantes o de la lista de propiedades de búsqueda especificadas puede conceder el permiso REFERENCES, si el propietario tiene permisos ALTER FULLTEXT CATALOG.  
  
> [!NOTE]  
>  Los usuarios tienen el permiso REFERENCES para la lista de palabras irrelevantes predeterminada que se incluye con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-setting-manual-change-tracking"></a>A. Configurar el seguimiento de cambios manual  
 En el ejemplo siguiente se establece el seguimiento de cambios manual en el índice de texto completo en la tabla `JobCandidate`.  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON HumanResources.JobCandidate  
   SET CHANGE_TRACKING MANUAL;  
GO  
```  
  
### <a name="b-associating-a-property-list-with-a-full-text-index"></a>B. Asociar una lista de propiedades con un índice de texto completo  
  
**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
 En este ejemplo se asocia la lista de propiedades `DocumentPropertyList` al índice de texto completo en la tabla `Production.Document`. Esta instrucción ALTER FULLTEXT INDEX inicia un rellenado completo, que es el comportamiento predeterminado de la cláusula SET SEARCH PROPERTY LIST.  
  
> [!NOTE]  
>  Para ver un ejemplo que crea la lista de propiedades `DocumentPropertyList`, vea [CREATE SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-search-property-list-transact-sql.md).  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON Production.Document   
   SET SEARCH PROPERTY LIST DocumentPropertyList;   
GO  
```  
  
### <a name="c-removing-a-search-property-list"></a>C. Quitar una lista de propiedades de búsqueda  
  
**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
 En el siguiente ejemplo, se quita la lista de propiedades `DocumentPropertyList` del índice de texto completo en la tabla `Production.Document`. En este ejemplo, no hay prisa para quitar las propiedades del índice, de modo que se especifica la opción WITH NO POPULATION. Sin embargo, la búsqueda en el nivel de propiedades ya no se permite mediante este índice de texto completo.  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON Production.Document   
   SET SEARCH PROPERTY LIST OFF WITH NO POPULATION;   
GO  
```  
  
### <a name="d-starting-a-full-population"></a>D. Iniciar un rellenado completo  
 En este ejemplo se inicia un rellenado completo del índice de texto completo en la tabla `JobCandidate`.  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON HumanResources.JobCandidate   
   START FULL POPULATION;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [sys.fulltext_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [DROP FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-index-transact-sql.md)   
 [Búsqueda de texto completo](../../relational-databases/search/full-text-search.md)   
 [Rellenar índices de texto completo](../../relational-databases/search/populate-full-text-indexes.md)  
  
  
