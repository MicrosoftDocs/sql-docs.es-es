---
description: CREAR ÍNDICE XML SELECTIVO (Transact-SQL)
title: CREATE SELECTIVE XML INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 1d769f62-f646-4057-b93a-bf5f90e935ed
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d5b2226422e413b2437ab7fc4643740a92378b51
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192679"
---
# <a name="create-selective-xml-index-transact-sql"></a>CREAR ÍNDICE XML SELECTIVO (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Crea un nuevo índice XML selectivo en la tabla y la columna XML especificadas. Los índices XML selectivos mejoran el rendimiento de la indización y las consultas XML al indizar únicamente el subconjunto de nodos que se suele consultar. También puede crear índices XML selectivos secundarios. Para obtener más información, vea [Creación, modificación y eliminación de índices XML selectivos secundarios](../../relational-databases/xml/create-alter-and-drop-secondary-selective-xml-indexes.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE SELECTIVE XML INDEX index_name  
    ON <table_object> (xml_column_name)  
    [WITH XMLNAMESPACES (<xmlnamespace_list>)]  
    FOR (<promoted_node_path_list>)  
    [WITH (<index_options>)]  
  
<table_object> ::=  
 { database_name.schema_name.table_name | schema_name.table_name | table_name }  
  
<promoted_node_path_list> ::=   
<named_promoted_node_path_item> [, <promoted_node_path_list>]  
  
<named_promoted_node_path_item> ::=   
<path_name> = <promoted_node_path_item>  
  
<promoted_node_path_item>::=  
<xquery_node_path_item> | <sql_values_node_path_item>  
  
<xquery_node_path_item> ::=   
<node_path> [AS XQUERY <xsd_type_or_node_hint>] [SINGLETON]  
  
<xsd_type_or_node_hint> ::=   
[<xsd_type>] [MAXLENGTH(x)] | node()  
  
<sql_values_node_path_item> ::=  
<node_path> AS SQL <sql_type> [SINGLETON]  
  
<node_path> ::=   
character_string_literal  
  
<xsd_type> ::=   
character_string_literal  
  
<sql_type> ::=   
identifier  
  
<path_name> ::=   
identifier  
  
<xmlnamespace_list> ::=   
<xmlnamespace_item> [, <xmlnamespace_list>]  
  
<xmlnamespace_item> ::=   
<xmlnamespace_uri> AS <xmlnamespace_prefix>  
  
<xml_namespace_uri> ::=   
character_string_literal  
  
<xml_namespace_prefix> ::=   
identifier  
  
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
  
 *\<table_object>* Es la tabla que contiene la columna XML que se va a indexar. Use uno de los formatos siguientes:  
  
-   `database_name.schema_name.table_name`  
  
-   `database_name..table_name`  
  
-   `schema_name.table_name`  
  
-   `table_name`  
  
 *xml_column_name*  
 Es el nombre de la columna XML que contiene las rutas de acceso que se van a indizar.  
  
 [WITH XMLNAMESPACES **(** \<xmlnamespace_list> **)** ] Es la lista de espacios de nombres que usan las rutas de acceso al índice. Para saber más sobre la sintaxis de la cláusula WITH XMLNAMESPACES, vea [WITH XMLNAMESPACES &#40;Transact-SQL&#41;](../../t-sql/xml/with-xmlnamespaces.md).  
  
 FOR **(** \<promoted_node_path_list> **)** Es la lista de rutas de acceso que se va a indexar con sugerencias opcionales de optimización. Para obtener información sobre las rutas de acceso y las sugerencias de optimización que se pueden especificar en la instrucción CREATE o ALTER, vea [Especificación de rutas de acceso y sugerencias de optimización para índices XML selectivos](../../relational-databases/xml/specify-paths-and-optimization-hints-for-selective-xml-indexes.md).  
  
 WITH *\<index_options>* Para obtener información sobre las opciones de índice, vea [CREATE XML INDEX &#40;Índices XML selectivos&#41;](../../t-sql/statements/create-xml-index-selective-xml-indexes.md).  
  
## <a name="best-practices"></a>Prácticas recomendadas  
 Cree un índice XML selectivo en lugar de un índice XML normal en la mayoría de los casos para mejorar el rendimiento y lograr un almacenamiento más eficiente. Sin embargo, no se recomienda usar un índice XML selectivo cuando alguna de las condiciones siguientes sea verdadera:  
  
-   Necesita asignar un gran número de rutas de acceso del nodo.  
  
-   Necesita admitir consultas para elementos desconocidos o elementos de una ubicación desconocida.  
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
 Para obtener información sobre las limitaciones y restricciones, vea [Índices XML selectivos &#40;SXI&#41;](../../relational-databases/xml/selective-xml-indexes-sxi.md).  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER en la tabla o la vista. El usuario debe ser miembro del rol fijo de servidor **sysadmin** o de los roles fijos de base de datos **db_ddladmin** y **db_owner** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se muestra la sintaxis para crear un índice XML selectivo. También se muestran varias variaciones de la sintaxis para describir las rutas de acceso que se van a indizar, con sugerencias opcionales de optimización.  
  
```sql  
CREATE TABLE Tbl ( id INT PRIMARY KEY, xmlcol XML );  
GO  
CREATE SELECTIVE XML INDEX sxi_index  
ON Tbl(xmlcol)  
FOR(  
    pathab   = '/a/b' as XQUERY 'node()',  
    pathabc  = '/a/b/c' as XQUERY 'xs:double',   
    pathdtext = '/a/b/d/text()' as XQUERY 'xs:string' MAXLENGTH(200) SINGLETON,  
    pathabe = '/a/b/e' as SQL NVARCHAR(100)  
);  
```  
  
 El ejemplo siguiente incluye una cláusula WITH XMLNAMESPACES.  
  
```sql  
CREATE SELECTIVE XML INDEX on T1(C1)  
WITH XMLNAMESPACES ('https://www.tempuri.org/' as myns)  
FOR ( path1 = '/myns:book/myns:author/text()' );  
```  
  
## <a name="see-also"></a>Consulte también  
 [Índices XML selectivos &#40;SXI&#41;](../../relational-databases/xml/selective-xml-indexes-sxi.md)   
 [Crear, modificar y quitar índices XML selectivos](../../relational-databases/xml/create-alter-and-drop-selective-xml-indexes.md)   
 [Especificar rutas de acceso y sugerencias de optimización para índices XML selectivos](../../relational-databases/xml/specify-paths-and-optimization-hints-for-selective-xml-indexes.md)  
  
  

