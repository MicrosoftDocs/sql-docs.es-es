---
description: CREATE XML INDEX (índices XML selectivos)
title: CREATE XML INDEX (índices XML selectivos) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 1f510151-41d5-45c2-9cd0-b1ca0246fffe
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8e86fd5f90c2e30e3a6c496a70a063f575cd259f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177824"
---
# <a name="create-xml-index-selective-xml-indexes"></a>CREATE XML INDEX (índices XML selectivos)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Crea un nuevo índice XML selectivo secundario en una única ruta de acceso que ya está indizada por un índice XML selectivo existente. También puede crear índices XML selectivos principales. Para más información, vea [Crear, modificar y quitar índices XML selectivos](../../relational-databases/xml/create-alter-and-drop-selective-xml-indexes.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE XML INDEX index_name  
    ON <table_object> ( xml_column_name )  
    USING XML INDEX sxi_index_name  
    FOR ( <xquery_or_sql_values_path> )  
    [WITH ( <index_options> )]  
  
<table_object> ::=   
{ database_name.schema_name.table_name | schema_name.table_name | table_name }  
  
<xquery_or_sql_values_path>::=   
<path_name>   
  
<path_name> ::=   
character string literal  
  
<xmlnamespace_list> ::=   
<xmlnamespace_item> [, <xmlnamespace_list>]  
  
<xmlnamespace_item> ::=   
xmlnamespace_uri AS xmlnamespace_prefix  
  
<index_options> ::=   
(    
  | PAD_INDEX  = { ON | OFF }  
  | FILLFACTOR = fillfactor  
  | SORT_IN_TEMPDB = { ON | OFF }  
  | IGNORE_DUP_KEY = OFF  
  | DROP_EXISTING = { ON | OFF }  
  | ONLINE = OFF  
  | ALLOW_ROW_LOCKS = { ON | OFF }  
  | ALLOW_PAGE_LOCKS = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
)  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argumentos  
 *index_name*  
 Es el nombre del nuevo índice que se va a crear. Los nombres de índice deben ser únicos en una tabla, pero no es necesario que sean únicos en una base de datos. Los nombres de índice deben seguir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md).  
  
 ON *\<table_object>* Es la tabla que contiene la columna XML que se va a indexar. Puede utilizar los formatos siguientes:  
  
-   `database_name.schema_name.table_name`  
  
-   `database_name..table_name`  
  
-   `schema_name.table_name`  
  
 *xml_column_name*  
 Es el nombre de la columna XML que contiene la ruta de acceso que se va a indizar.  
  
 USING XML INDEX *sxi_index_name*  
 Es el nombre del índice XML selectivo existente.  
  
 FOR **(** \<xquery_or_sql_values_path> **)** Es el nombre de la ruta de acceso indexada en la que se va a crear el índice XML selectivo secundario. La ruta de acceso que se va a indizar es el nombre asignado en la instrucción CREATE SELECTIVE XML INDEX. Para obtener más información, vea [CREAR ÍNDICE XML SELECTIVO &#40;Transact-SQL&#41;](../../t-sql/statements/create-selective-xml-index-transact-sql.md).  
  
 WITH \<index_options> Para obtener más información sobre las opciones de índice, vea [CREATE XML INDEX](../../t-sql/statements/create-xml-index-selective-xml-indexes.md).  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Observaciones
 Puede haber varios índices XML selectivos secundarios en cada columna XML de la tabla base.  
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
 Debe existir un índice XML selectivo en una columna XML para poder crear índices XML selectivos secundarios en la columna.  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER en la tabla o la vista. El usuario debe ser miembro del rol fijo de servidor **sysadmin** o de los roles fijos de base de datos **db_ddladmin** y **db_owner** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se crea un índice XML selectivo secundario en la ruta de acceso `pathabc`. La ruta de acceso que se va a indizar es el nombre asignado en [CREATE SELECTIVE XML INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-selective-xml-index-transact-sql.md).  
  
```sql  
CREATE XML INDEX filt_sxi_index_c  
ON Tbl(xmlcol)  
USING XML INDEX sxi_index  
FOR ( pathabc );  
```  
  
## <a name="see-also"></a>Consulte también  
 [Índices XML selectivos &#40;SXI&#41;](../../relational-databases/xml/selective-xml-indexes-sxi.md)   
 [Crear, modificar y quitar índices XML selectivos secundarios](../../relational-databases/xml/create-alter-and-drop-secondary-selective-xml-indexes.md)  
  
  

