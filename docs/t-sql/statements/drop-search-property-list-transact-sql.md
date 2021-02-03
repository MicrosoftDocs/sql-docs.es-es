---
description: DROP SEARCH PROPERTY LIST (Transact-SQL)
title: DROP SEARCH PROPERTY LIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP_SEARCH_PROPERTY_LIST_TSQL
- DROP SEARCH PROPERTY LIST
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], search property lists
- DROP SEARCH PROPERTY LIST statement
- search property lists [SQL Server], dropping
- search property lists [SQL Server], deleting
ms.assetid: 7c7ce52a-6b77-4a1c-9abf-d5feb664bea8
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 48a62e9aa8137d38e47b38d0228ec617c563f572
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191831"
---
# <a name="drop-search-property-list-transact-sql"></a>DROP SEARCH PROPERTY LIST (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita una lista de propiedades de la base de datos actual si la lista de propiedades de búsqueda no está asociada actualmente a ningún índice de texto completo de la base de datos.  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DROP SEARCH PROPERTY LIST property_list_name  
;  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *property_list_name*  
 Es el nombre de la lista de propiedades de búsqueda que se va a quitar. *property_list_name* es un identificador.  
  
 Para ver los nombres de las listas de propiedades existentes, use la vista de catálogo [sys.registered_search_property_lists](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md) de la siguiente manera:  
  
```sql  
SELECT name FROM sys.registered_search_property_lists;  
```  
  
## <a name="remarks"></a>Comentarios  
 No puede quitar una lista de propiedades de búsqueda de una base de datos mientras la lista está asociada a algún índice de texto completo y los intentos de hacerlo producirán un error. Para quitar una lista de propiedades de búsqueda de un índice de texto completo determinado, use la instrucción [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) y especifique la cláusula SET SEARCH PROPERTY LIST con OFF o el nombre de otra lista de propiedades de búsqueda.  
  
 **Para ver las listas de propiedades en una instancia del servidor**  
  
-   [sys.registered_search_property_lists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)  
  
 **Para ver las listas de propiedades asociadas a índices de texto completo**  
  
-   [sys.fulltext_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)  
  
 **Para quitar una lista de propiedades de un índice de texto completo**  
  
-   [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Se necesita el permiso CONTROL en la lista de propiedades de búsqueda.  
  
> [!NOTE]  
>  El propietario de la lista de propiedades puede conceder permisos CONTROL para la lista. De manera predeterminada, el usuario que crea una lista de propiedades de búsqueda es su propietario. El propietario se puede cambiar usando la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)][ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se quita la lista de propiedades `JobCandidateProperties` de la base de datos `AdventureWorks2012`.  
  
```sql  
DROP SEARCH PROPERTY LIST JobCandidateProperties;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [ALTER SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/alter-search-property-list-transact-sql.md)   
 [CREATE SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-search-property-list-transact-sql.md)   
 [Buscar propiedades de documento con listas de propiedades de búsqueda](../../relational-databases/search/search-document-properties-with-search-property-lists.md)   
 [sys.registered_search_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)   
 [sys.registered_search_property_lists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)   
 [sys.registered_search_property_lists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)  
  
  
