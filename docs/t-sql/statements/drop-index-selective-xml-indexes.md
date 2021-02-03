---
description: DROP INDEX (índices XML selectivos)
title: DROP INDEX (índices XML selectivos) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP XML INDEX statement
dev_langs:
- TSQL
ms.assetid: 4779ae84-e5f4-4d04-8fc1-e24a6631b428
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 988bd46d3e25b862bfcbc479896cc8cc0151b956
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193145"
---
# <a name="drop-index-selective-xml-indexes"></a>DROP INDEX (índices XML selectivos)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Quita un índice XML selectivo existente o un índice XML selectivo secundario en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, consulte [Índices XML selectivos &#40;SXI&#41;](../../relational-databases/xml/selective-xml-indexes-sxi.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DROP INDEX index_name ON <object>  
    [ WITH ( <drop_index_option> [ ,...n ] ) ]  
  
<object> ::=  
{ database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
  
<drop_index_option> ::=  
{  
    MAXDOP = max_degree_of_parallelism  
    | ONLINE = { ON | OFF }  
}  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argumentos  
 *index_name*  
 Es el nombre del índice existente que se va a quitar.  
  
 *\< object>* Es la tabla que contiene la columna XML indizada. Use uno de los formatos siguientes:  
  
-   `database_name.schema_name.table_name`  
  
-   `database_name..table_name`  
  
-   `schema_name.table_name`  
  
-   `table_name`  
  
 *\<drop_index_option>* Para obtener información sobre las opciones para quitar índices, vea [DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md).  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Se necesita el permiso ALTER en la tabla o la vista para poder ejecutar DROP INDEX. Este permiso se concede de forma predeterminada al rol fijo de servidor sysadmin y a los roles fijos de base de datos db_ddladmin y db_owner.  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente se muestra una instrucción DROP INDEX.  
  
```sql  
DROP INDEX sxi_index ON tbl;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Índices XML selectivos &#40;SXI&#41;](../../relational-databases/xml/selective-xml-indexes-sxi.md)   
 [Crear, modificar y quitar índices XML selectivos](../../relational-databases/xml/create-alter-and-drop-selective-xml-indexes.md)  
  
  

