---
description: DROP PARTITION SCHEME (Transact-SQL)
title: DROP PARTITION SCHEME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP PARTITION SCHEME
- DROP_PARTITION_SCHEME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DROP PARTITION SCHEME statement
- deleting partition schemes
- dropping partition schemes
- removing partition schemes
- partition schemes [SQL Server], removing
ms.assetid: 6efbc87c-1c92-4e43-96a7-e0f30f1db185
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c3900b7323ed0312cdcfd265540f3f8e323bed2f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201106"
---
# <a name="drop-partition-scheme-transact-sql"></a>DROP PARTITION SCHEME (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/applies-to-version/sqlserver.md)]

  Quita un esquema de partición de la base de datos actual. Los esquemas de partición se crean mediante [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) y se modifican mediante [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DROP PARTITION SCHEME partition_scheme_name [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *partition_scheme_name*  
 Es el nombre del esquema de partición que se va a quitar.  
  
## <a name="remarks"></a>Comentarios  
 Los esquemas de partición solo se pueden quitar si no hay ninguna tabla ni índice que utilice el esquema de partición actualmente. Si hay alguna tabla o índice que utilice el esquema de partición, DROP PARTITION SCHEME devolverá un error. DROP PARTITION SCHEME no quita los grupos de archivos.  
  
## <a name="permissions"></a>Permisos  
 Se pueden utilizar los permisos siguientes para ejecutar DROP PARTITION SCHEME:  
  
-   Permiso ALTER ANY DATASPACE. De forma predeterminada, este permiso corresponde a los miembros del rol fijo de servidor **sysadmin** y a los roles fijos de base de datos **db_owner** y **db_ddladmin** .  
  
-   Permiso CONTROL o ALTER en la base de datos en la que se ha creado el esquema de partición.  
  
-   Permiso CONTROL SERVER o ALTER ANY DATABASE en el servidor de la base de datos en la que se ha creado el esquema de partición.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se quita el esquema de partición `myRangePS1` de la base de datos actual:  
  
```sql  
DROP PARTITION SCHEME myRangePS1;  
```  
  
## <a name="see-also"></a>Vea también  
 [CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [ALTER PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-scheme-transact-sql.md)   
 [sys.partition_schemes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-schemes-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)   
 [sys.destination_data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-destination-data-spaces-transact-sql.md)   
 [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
