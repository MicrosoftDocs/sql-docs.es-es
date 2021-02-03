---
description: CREATE SEARCH PROPERTY LIST (Transact-SQL)
title: CREATE SEARCH PROPERTY LIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_SEARCH_PROPERTY_LIST_TSQL
- CREATE SEARCH
- CREATE_SEARCH_TSQL
- CREATE_SEARCH_PROPERTY_TSQL
- CREATE SEARCH PROPERTY
- CREATE SEARCH PROPERTY LIST
- sql13.swb.spl.newsearchpropertylist.f1
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], search property lists
- search property lists [SQL Server], creating
- CREATE SEARCH PROPERTY LIST statement
ms.assetid: 5440cbb8-3403-4d27-a2f9-8e1f5a1bc12b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 31e938250b543fbb6bb3e3443138677f8ee3befd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192698"
---
# <a name="create-search-property-list-transact-sql"></a>CREATE SEARCH PROPERTY LIST (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Crea una nueva lista de propiedades de búsqueda. Una lista de propiedades de búsqueda se utiliza para especificar una o más propiedades de búsqueda que desea incluir en un índice de texto completo.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
CREATE SEARCH PROPERTY LIST new_list_name  
   [ FROM [ database_name. ] source_list_name ]  
   [ AUTHORIZATION owner_name ]  
;  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *new_list_name*  
 Es el nombre de la nueva lista de propiedades de búsqueda. *new_list_name* es un identificador con un máximo de 128 caracteres. *new_list_name* debe ser único entre todas las listas de propiedades de la base de datos actual y cumplir las reglas de los identificadores. *new_list_name* se usará cuando se cree el índice de texto completo.  
  
 *database_name*  
 Es el nombre de la base de datos donde se encuentra la lista de propiedades especificada por *source_list_name*. Si no se especifica, *database_name* usa de manera predeterminada la base de datos actual.  
  
 *database_name* debe especificar el nombre de una base de datos existente. El inicio de sesión de la conexión actual debe estar asociado a un identificador de usuario existente en la base de datos especificada por *database_name*. También debe tener los [permisos](#Permissions) necesarios en la base de datos.  
  
 *source_list_name*  
 Especifica que la nueva lista de propiedades se crea copiando una lista de propiedades existente desde *database_name*. Si *source_list_name* no existe, CREATE SEARCH PROPERTY LIST produce un error. *new_list_name* hereda las propiedades de búsqueda de *source_list_name*.  
  
 AUTHORIZATION *owner_name*  
 Especifica el nombre de un usuario o rol que posea la lista de propiedades. *owner_name* debe ser el nombre de un rol del que el usuario actual sea miembro, o bien el usuario actual debe tener el permiso IMPERSONATE sobre *owner_name*. Si no se especifica, la propiedad se otorga al usuario actual.  
  
> [!NOTE]  
>  El propietario se puede cambiar usando la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)][ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
## <a name="remarks"></a>Comentarios  
  
> [!NOTE]  
>  Para más información sobre las listas de propiedades en general, vea [Buscar propiedades de documento con listas de propiedades de búsqueda](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
 De forma predeterminada, una nueva lista de propiedades de búsqueda está vacía y debe modificarla para agregar manualmente una o más propiedades de búsqueda. Opcionalmente, puede copiar una lista de propiedades de búsqueda existente. En este caso, la nueva lista hereda las propiedades de búsqueda de su origen, pero se puede modificar para agregar o quitar propiedades. Cualquier propiedad de la lista de propiedades de búsqueda en el momento del siguiente rellenado completo se incluye en el índice de texto completo.  
  
 Una instrucción CREATE SEARCH PROPERTY LIST da error en cualquiera de las condiciones siguientes:  
  
-   Si la base de datos que especifica *database_name* no existe.  
  
-   Si la lista que especifica *source_list_name* no existe.  
  
-   Si no tiene los permisos correctos.  
  
 **Para agregar o quitar las propiedades de una lista**  
  
-   [ALTER SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/alter-search-property-list-transact-sql.md)  
  
-   **Para quitar una lista de propiedades**  
  
-   [DROP SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/drop-search-property-list-transact-sql.md)  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere los permisos CREATE FULLTEXT CATALOG en la base de datos actual y los permisos REERENCES en cualquier base de datos de la que copie una lista de propiedades de origen.  
  
> [!NOTE]  
>  El permiso REFERENCES se necesita para asociar la lista a un índice de texto completo. El permiso CONTROL se exige para agregar y quitar propiedades o quitar la lista. El propietario de la lista de propiedades puede conceder los permisos REFERENCE o CONTROL en la lista. Los usuarios con el permiso CONTROL también pueden conceder el permiso REFERENCES a otros usuarios.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-an-empty-property-list-and-associating-it-with-an-index"></a>A. Crear una lista de propiedades vacía y asociarla a un índice  
 El siguiente ejemplo crea una nueva lista de propiedades de búsqueda denominada `DocumentPropertyList`. Luego, en el ejemplo se usa una instrucción [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) para asociar la nueva lista de propiedades al índice de texto completo de la tabla `Production.Document` en la base de datos `AdventureWorks`, sin iniciar un rellenado.  
  
> [!NOTE]  
>  Para obtener un ejemplo en el que se agregan varias propiedades de búsqueda predefinidas conocidas a esta lista de propiedades de búsqueda, vea [ALTER SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/alter-search-property-list-transact-sql.md). Después de agregar las propiedades de búsqueda a la lista, el administrador de bases de datos necesitaría utilizar otra instrucción ALTER FULLTEXT INDEX con la cláusula START FULL POPULATION.  
  
```sql 
CREATE SEARCH PROPERTY LIST DocumentPropertyList;  
GO  
USE AdventureWorks2012;  
ALTER FULLTEXT INDEX ON Production.Document   
   SET SEARCH PROPERTY LIST DocumentPropertyList  
   WITH NO POPULATION;   
GO   
```  
  
### <a name="b-creating-a-property-list-from-an-existing-one"></a>B. Crear una lista de propiedades desde otra existente  
 El siguiente ejemplo crea una nueva lista de propiedades de búsqueda, `JobCandidateProperties`, a partir de la lista creada en el ejemplo A, `DocumentPropertyList`, que se asocia a un índice de texto completo en la base de datos `AdventureWorks2012`. A continuación, el ejemplo utiliza una instrucción ALTER FULLTEXT INDEX para asociar la nueva lista de propiedades al índice de texto completo de la tabla `HumanResources.JobCandidate` en la base de datos `AdventureWorks2012`. Esta instrucción ALTER FULLTEXT INDEX inicia un rellenado completo, que es el comportamiento predeterminado de la cláusula SET SEARCH PROPERTY LIST.  
  
```sql  
CREATE SEARCH PROPERTY LIST JobCandidateProperties 
FROM AdventureWorks2012.DocumentPropertyList;  
GO  
ALTER FULLTEXT INDEX ON HumanResources.JobCandidate   
   SET SEARCH PROPERTY LIST JobCandidateProperties;  
GO
```  
  
## <a name="see-also"></a>Vea también  
 [ALTER SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/alter-search-property-list-transact-sql.md)   
 [DROP SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/drop-search-property-list-transact-sql.md)   
 [sys.registered_search_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)   
 [sys.registered_search_property_lists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)   
 [sys.dm_fts_index_keywords_by_property &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)   
 [Buscar propiedades de documento con listas de propiedades de búsqueda](../../relational-databases/search/search-document-properties-with-search-property-lists.md)   
 [Buscar GUID del conjunto de propiedades e identificadores de enteros de propiedad para las propiedades de búsqueda](../../relational-databases/search/find-property-set-guids-and-property-integer-ids-for-search-properties.md)  
  
  
