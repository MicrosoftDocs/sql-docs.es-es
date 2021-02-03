---
description: ALTER SEARCH PROPERTY LIST (Transact-SQL)
title: ALTER SEARCH PROPERTY LIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_SEARCH_PROPERTY_TSQL
- ALTER_SEARCH_PROPERTY_LIST_TSQL
- ALTER SEARCH PROPERTY LIST
- ALTER SEARCH PROPERTY
- ALTER_SEARCH_TSQL
- ALTER SEARCH
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], search property lists
- search property lists [SQL Server], altering
- ALTER SEARCH PROPERTY LIST statement
ms.assetid: 0436e4a8-ca26-4d23-93f1-e31e2a1c8bfb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c39fb37a0df34e7f7cf2afcf34678fa4c225a9f2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182235"
---
# <a name="alter-search-property-list-transact-sql"></a>ALTER SEARCH PROPERTY LIST (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Agrega una propiedad de búsqueda especificada o la quita de la lista de propiedades de búsqueda especificada.  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER SEARCH PROPERTY LIST list_name  
{  
   ADD 'property_name'  
     WITH   
      (   
          PROPERTY_SET_GUID = 'property_set_guid'  
        , PROPERTY_INT_ID = property_int_id  
      [ , PROPERTY_DESCRIPTION = 'property_description' ]  
      )  
 | DROP 'property_name'   
}  
;  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *list_name*  
 Es el nombre de la lista de propiedades que se está modificando. *list_name* es un identificador.  
  
 Para ver los nombres de las listas de propiedades existentes, use la vista de catálogo [sys.registered_search_property_lists](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md) de la siguiente manera:  
  
```sql  
SELECT name FROM sys.registered_search_property_lists;  
```  
  
 ADD  
 Agrega una propiedad de búsqueda especificada a la lista de propiedades especificada por *list_name*. La propiedad se registra para la lista de propiedades. Antes de que se puedan usar propiedades recién agregadas para la búsqueda de propiedades, el índice o los índices de texto completo asociados se deben volver a rellenar. Para obtener más información, vea [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md).  
  
> [!NOTE]  
>  Para agregar una propiedad de búsqueda dada a una lista de propiedades de búsqueda, debe proporcionar el GUID del conjunto de propiedades (*property_set_guid*) y el identificador entero de propiedad (*property_int_id*). Para obtener más información, vea "Obtener los GUID e identificadores de los conjuntos de propiedades", más adelante en este tema.  
  
 *property_name*  
 Especifica el nombre que se va a usar para identificar la propiedad en consultas de texto completo. *property_name* debe identificar exclusivamente la propiedad en el conjunto de propiedades. El nombre de una propiedad puede contener espacios internos. La longitud máxima del atributo *property_name* es de 256 caracteres. Este nombre puede ser un nombre descriptivo, como Autor o Dirección particular, o bien el nombre canónico de Windows de la propiedad, como **System.Author** o **System.Contact.HomeAddress**.  
  
 Los desarrolladores deberán usar el valor especificado para *property_name* con el fin de identificar la propiedad en el predicado [CONTAINS](../../t-sql/queries/contains-transact-sql.md). Por lo tanto, cuando se agregue una propiedad es importante especificar un valor que represente significativamente la propiedad definida por el GUID del conjunto de propiedades (*property_set_guid*) y el identificador de propiedad (*property_int_id*). especificados. Para obtener más información sobre nombres de propiedades, vea la sección "Comentarios" más adelante en este tema.  
  
 Para ver los nombres de las propiedades que existen actualmente en una lista de propiedades de búsqueda de la base de datos actual, use la vista de catálogo [sys.registered_search_properties](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md) de la siguiente manera:  
  
```sql  
SELECT property_name FROM sys.registered_search_properties;  
```  
  
 PROPERTY_SET_GUID ='*property_set_guid*'  
 Especifica el identificador del conjunto de propiedades al que pertenece la propiedad. Se trata de un identificador único global (GUID). Para obtener más información cómo obtener este valor, vea la sección "Comentarios" más adelante en este tema.  
  
 Para ver el GUID del conjunto de propiedades de cualquier propiedad que exista actualmente en una lista de propiedades de búsqueda de la base de datos actual, use la vista de catálogo [sys.registered_search_properties](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md) de la siguiente manera:  
  
```sql  
SELECT property_set_guid FROM sys.registered_search_properties;  
```  
  
 PROPERTY_INT_ID =*property_int_id*  
 Especifica el entero que identifica la propiedad en su conjunto de propiedades. Para obtener información sobre cómo obtener este valor, vea "Comentarios".  
  
 Para ver el identificador entero de cualquier propiedad que exista actualmente en una lista de propiedades de búsqueda de la base de datos actual, use la vista de catálogo [sys.registered_search_properties](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md) de la siguiente manera:  
  
```sql  
SELECT property_int_id FROM sys.registered_search_properties;  
```  
  
> [!NOTE]  
>  Una determinada combinación formada por *property_set_guid* y *property_int_id* debe ser única en una lista de propiedades de búsqueda. Si intenta agregar una combinación existente, se produce un error en la operación ALTER SEARCH PROPERTY LIST y se emite un error. Es decir, puede definir solo un nombre para una propiedad dada.  
  
 PROPERTY_DESCRIPTION ='*property_description*'  
 Especifica una descripción definida por el usuario de la propiedad. *property_description* es una cadena de hasta 512 caracteres. Esta opción es opcional.  
  
 DROP  
 Quita la propiedad especificada de la lista de propiedades especificada por *list_name*. Si se quita una propiedad, se elimina del registro y, por tanto, ya no se pueden realizar búsquedas en ella.  
  
## <a name="remarks"></a>Observaciones  
 Cada índice de texto completo puede tener solo una lista de propiedades de búsqueda.  
  
 Para habilitar la realización de consultas en una propiedad de búsqueda dada, debe agregarla a la lista de propiedades de búsqueda del índice de texto completo y, a continuación, volver a rellenar el índice.  
  
 Cuando especifique una propiedad puede organizar las cláusulas PROPERTY_SET_GUID, PROPERTY_INT_ID y PROPERTY_DESCRIPTION en cualquier orden, como una lista separada por comas entre paréntesis, por ejemplo:  
  
```sql  
ALTER SEARCH PROPERTY LIST CVitaProperties  
ADD 'System.Author'   
WITH (   
      PROPERTY_DESCRIPTION = 'Author or authors of a given document.',  
      PROPERTY_SET_GUID   = 'F29F85E0-4FF9-1068-AB91-08002B27B3D9',   
      PROPERTY_INT_ID = 4   
      );  
```  
  
> [!NOTE]  
>  En este ejemplo, se usa el nombre de propiedad, `System.Author`, que es similar al concepto de los nombres de propiedad canónicos presentados en Windows Vista (nombre canónico de Windows).  
  
## <a name="obtaining-property-values"></a>Obtener valores de propiedad  
 La búsqueda de texto completo asigna una propiedad de búsqueda a un índice de texto completo con su GUID de conjunto de propiedades y su identificador entero de propiedad. Para obtener información sobre cómo obtener estos valores para las propiedades definidas por Microsoft, vea [Buscar GUID del conjunto de propiedades e identificadores de enteros de propiedad para las propiedades de búsqueda](../../relational-databases/search/find-property-set-guids-and-property-integer-ids-for-search-properties.md). Para obtener información sobre propiedades definidas por un fabricante de software independiente (ISV), vea la documentación de ese fabricante.  
  
## <a name="making-added-properties-searchable"></a>Convertir las propiedades agregadas para que se puedan realizar búsquedas en ellas  
 Si se agrega una propiedad de búsqueda a una lista de propiedades, la propiedad se registra. Se puede especificar inmediatamente una propiedad recién agregada en consultas [CONTAINS](../../t-sql/queries/contains-transact-sql.md). No obstante, las consultas de texto completo referentes a propiedades de una propiedad recién agregada no devolverán documentos hasta que se vuelva a rellenar el índice de texto completo asociado. Por ejemplo, la siguiente consulta referente a propiedades de una propiedad recién agregada, *new_search_property*, no devolverá ningún documento hasta que se vuelva a rellenar el índice de texto completo asociado con la tabla de destino (*table_name*):  
  
```sql  
SELECT column_name  
FROM table_name  
WHERE CONTAINS( PROPERTY( column_name, 'new_search_property' ), 
               'contains_search_condition');  
GO   
```  
  
 Para iniciar un llenado completo, use la siguiente instrucción [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md):  
  
```sql  
USE database_name;  
GO  
ALTER FULLTEXT INDEX ON table_name START FULL POPULATION;  
GO  
```  
  
> [!NOTE]  
>  No es necesario el rellenado después de que se quite una propiedad de una lista de propiedades, ya que solo las propiedades que permanecen en la lista de propiedades de búsqueda están disponibles para realizar consultas de texto completo.  
  
## <a name="related-references"></a>Referencias relacionadas  
 **Para crear una lista de propiedades**  
  
-   [CREATE SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-search-property-list-transact-sql.md)  
  
 **Para quitar una lista de propiedades**  
  
-   [DROP SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/drop-search-property-list-transact-sql.md)  
  
 **Para agregar o quitar una lista de propiedades de un índice de texto completo**  
  
-   [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
 **Para ejecutar un rellenado en un índice de texto completo**  
  
-   [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso CONTROL en la lista de propiedades.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-adding-a-property"></a>A. Agregar una propiedad  
 En los siguientes ejemplos se agregan varias propiedades (`Title`, `Author` y `Tags`) a una lista de propiedades denominada `DocumentPropertyList`.  
  
> [!NOTE]  
>  Para ver un ejemplo que crea la lista de propiedades `DocumentPropertyList`, vea [CREATE SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-search-property-list-transact-sql.md).  
  
```sql  
ALTER SEARCH PROPERTY LIST DocumentPropertyList  
   ADD 'Title'   
   WITH ( PROPERTY_SET_GUID = 'F29F85E0-4FF9-1068-AB91-08002B27B3D9', PROPERTY_INT_ID = 2,   
      PROPERTY_DESCRIPTION = 'System.Title - Title of the item.' );  
  
ALTER SEARCH PROPERTY LIST DocumentPropertyList   
    ADD 'Author'  
   WITH ( PROPERTY_SET_GUID = 'F29F85E0-4FF9-1068-AB91-08002B27B3D9', PROPERTY_INT_ID = 4,   
      PROPERTY_DESCRIPTION = 'System.Author - Author or authors of the item.' );  
  
ALTER SEARCH PROPERTY LIST DocumentPropertyList   
    ADD 'Tags'  
   WITH ( PROPERTY_SET_GUID = 'F29F85E0-4FF9-1068-AB91-08002B27B3D9', PROPERTY_INT_ID = 5,   
      PROPERTY_DESCRIPTION = 
          'System.Keywords - Set of keywords (also known as tags) assigned to the item.' );  
```  
  
> [!NOTE]  
>  Debe asociar una lista de propiedades de búsqueda dada con un índice de texto completo antes de usarla para consultas referentes a propiedades. Para ello, use una instrucción [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) y especifique la cláusula SET SEARCH PROPERTY LIST.  
  
### <a name="b-dropping-a-property"></a>B. Quitar una propiedad  
 En el siguiente ejemplo se quita la propiedad `Comments` de la lista de propiedades `DocumentPropertyList`.  
  
```sql  
ALTER SEARCH PROPERTY LIST DocumentPropertyList  
DROP 'Comments' ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-search-property-list-transact-sql.md)   
 [DROP SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/drop-search-property-list-transact-sql.md)   
 [sys.registered_search_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)   
 [sys.registered_search_property_lists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)   
 [sys.dm_fts_index_keywords_by_property &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)   
 [Buscar propiedades de documento con listas de propiedades de búsqueda](../../relational-databases/search/search-document-properties-with-search-property-lists.md)   
 [Buscar GUID del conjunto de propiedades e identificadores de enteros de propiedad para las propiedades de búsqueda](../../relational-databases/search/find-property-set-guids-and-property-integer-ids-for-search-properties.md)  
  
  
