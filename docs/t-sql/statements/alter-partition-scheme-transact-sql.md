---
description: ALTER PARTITION SCHEME (Transact-SQL)
title: ALTER PARTITION SCHEME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER PARTITION SCHEME
- ALTER_PARTITION_SCHEME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER PARTITION SCHEME statement
- partition schemes [SQL Server], modifying
- modifying partition schemes
- adding filegroups
- NEXT USED filegroups
ms.assetid: f01d6880-9800-4cfb-8d11-d4be21efc8ca
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cbf8b1f76fc50438ff403c1b2703067ddb7cee53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204111"
---
# <a name="alter-partition-scheme-transact-sql"></a>ALTER PARTITION SCHEME (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Agrega un grupo de archivos a un esquema de partición o altera la designación del grupo de archivos NEXT USED para el esquema de partición. 

>[!NOTE]
>En Azure SQL Database solamente se admiten grupos de archivos principales.  
  
 ![Icono de vínculo de artículo](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de artículo") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER PARTITION SCHEME partition_scheme_name   
NEXT USED [ filegroup_name ] [ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *partition_scheme_name*  
 Es el nombre del esquema de partición que se va a modificar.  
  
 *filegroup_name*  
 Especifica el grupo de archivos que el esquema de partición debe marcar como NEXT USED. Esto significa que el grupo de archivos aceptará una nueva partición creada mediante una instrucción [ALTER PARTITION FUNCTION](../../t-sql/statements/alter-partition-function-transact-sql.md).  
  
 En un esquema de partición, solo un grupo de archivos puede designarse como NEXT USED. Puede especificarse un grupo de archivos que no esté vacío. Si se especifica *filegroup_name* y actualmente no hay ningún grupo de archivos marcado como NEXT USED, *filegroup_name* se marca como NEXT USED. Si se especifica *filegroup_name* y ya existe un grupo de archivos con la propiedad NEXT USED, la propiedad NEXT USED se transfiere del grupo de archivos existente a *filegroup_name*.  
  
 Si no se especifica *filegroup_name* y ya existe un grupo de archivos con la propiedad NEXT USED, ese grupo de archivos pierde su estado NEXT USED para que no haya grupos de archivos NEXT USED en *partition_scheme_name*.  
  
 Si no se especifica *filegroup_name* y no hay grupos de archivos marcados como NEXT USED, ALTER PARTITION SCHEME devuelve una advertencia.  
  
## <a name="remarks"></a>Observaciones  
 Los grupos de archivos afectados por ALTER PARTITION SCHEME deben estar en línea.  
  
## <a name="permissions"></a>Permisos  
 Los siguientes permisos pueden utilizarse para ejecutar ALTER PARTITION SCHEME:  
  
-   Permiso ALTER ANY DATASPACE. De forma predeterminada, este permiso corresponde a los miembros del rol fijo de servidor **sysadmin** y a los roles fijos de base de datos **db_owner** y **db_ddladmin** .  
  
-   Permiso CONTROL o ALTER en la base de datos en la que se ha creado el esquema de partición.  
  
-   Permiso CONTROL SERVER o ALTER ANY DATABASE en el servidor de la base de datos en la que se ha creado el esquema de partición.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se asume que el esquema de partición `MyRangePS1` y el grupo de archivos `test5fg` existen en la base de datos actual.  
  
```sql  
ALTER PARTITION SCHEME MyRangePS1  
NEXT USED test5fg;  
```  
  
 El grupo de archivos `test5fg` recibirá las particiones adicionales de una tabla o índice con particiones como resultado de una instrucción ALTER PARTITION FUNCTION.  
  
## <a name="see-also"></a>Consulte también  
 [CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [DROP PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-scheme-transact-sql.md)   
 [CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)   
 [ALTER PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-function-transact-sql.md)   
 [DROP PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-function-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.partition_schemes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-schemes-transact-sql.md)   
 [sys.data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)   
 [sys.destination_data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-destination-data-spaces-transact-sql.md)   
 [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
