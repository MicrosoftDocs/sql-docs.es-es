---
description: sp_syscollector_update_collection_item (Transact-SQL)
title: sp_syscollector_update_collection_item (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_update_collection_item
- sp_syscollector_update_collection_item_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- data collector [SQL Server], stored procedures
- sp_syscollector_update_collection_item
ms.assetid: 7a0d36c8-c6e9-431d-a5a4-6c1802bce846
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f21ca6f68eed789756a7dbf57f79afe194d40dab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161916"
---
# <a name="sp_syscollector_update_collection_item-transact-sql"></a>sp_syscollector_update_collection_item (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Se usa para modificar las propiedades de un elemento de recopilación definido por el usuario o para cambiar el nombre de un elemento de recopilación definido por el usuario.  
  
 
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_syscollector_update_collection_item   
      [ [ @collection_item_id = ] collection_item_id ]  
    , [ [ @name = ] 'name' ]  
    , [ [ @new_name = ] 'new_name' ]  
    , [ [ @frequency = ] frequency ]  
    , [ [ @parameters = ] 'parameters' ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @collection_item_id =] *collection_item_id*  
 Es el identifer único que identifica el elemento de recopilación. *collection_item_id* es de **tipo int** y su valor predeterminado es NULL. *collection_item_id* debe tener un valor si *el nombre* es NULL.  
  
 [ @name =] '*nombre*'  
 Es el nombre del elemento de colección. *Name* es de **tipo sysname y su** valor predeterminado es NULL. *el nombre* debe tener un valor si *collection_item_id* es NULL.  
  
 [ @new_name =] '*new_name*'  
 Es el nuevo nombre del elemento de recopilación. *new_name* es de **tipo sysname** y, si se utiliza, no puede ser una cadena vacía.  
  
 *new_name* debe ser único. Para obtener una lista de los nombres de elementos de recopilación actuales, consulte la vista del sistema syscollector_collection_items.  
  
 [ @frequency =] *frecuencia*  
 Es la frecuencia (en segundos) con que este elemento de recopilación recopila los datos. *Frequency* es de **tipo int** y su valor predeterminado es 5, el valor mínimo que se puede especificar.  
  
 [ @parameters =] '*Parameters*'  
 Parámetros de entrada para el elemento de recopilación. *Parameters* es **XML** con un valor predeterminado de NULL. El esquema de *parámetros* debe coincidir con el esquema de parámetros del tipo de recopilador.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 Si el conjunto de recopilación se establece en modo sin almacenamiento en caché, se omite el cambio de la frecuencia porque este modo hace que la recopilación de datos y la carga se produzcan en la programación especificada para el conjunto de recopilación. Para ver el estado del conjunto de recopilación, ejecute la consulta siguiente. Reemplace `<collection_item_id>` con el identificador del elemento de recopilación que se va a actualizar.  
  
```  
USE msdb;  
GO  
SELECT cs.collection_set_id, collection_set_uid, cs.name   
    ,'is running' = CASE WHEN is_running =  0 THEN 'No' ELSE 'Yes' END  
    ,'cache mode' = CASE WHEN collection_mode = 0 THEN 'Cached mode' ELSE 'Non-cached mode' END  
FROM syscollector_collection_sets AS cs  
JOIN syscollector_collection_items AS ci   
ON ci.collection_set_id = cs.collection_set_id  
WHERE collection_item_id = <collection_item_id>;  
```  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia a un rol fijo de base de datos dc_admin o dc_operator (con permiso EXECUTE) para ejecutar este procedimiento. Aunque dc_operator puede ejecutar este procedimiento almacenado, las propiedades que pueden cambiar los miembros de este rol son limitadas. Las propiedades siguientes solo puede cambiarlas dc_admin:  
  
-   @new_name  
  
-   @parameters  
  
## <a name="examples"></a>Ejemplos  
 Los ejemplos siguientes se basan en el elemento de recopilación creado en el ejemplo definido en [sp_syscollector_create_collection_item &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-item-transact-sql.md).  
  
### <a name="a-changing-the-collection-frequency"></a>A. Cambiar la frecuencia de la recopilación  
 En el ejemplo siguiente se cambia la frecuencia de recopilación para el elemento de recopilación especificado.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_update_collection_item   
@name = N'My custom TSQL query collector item',  
@frequency = 3000;  
GO  
```  
  
### <a name="b-renaming-a-collection-item"></a>B. Cambiar el nombre de un elemento de recopilación  
 En el ejemplo siguiente se cambia el nombre de un elemento de recopilación.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_update_collection_item   
@name = N'My custom TSQL query collector item',  
@new_name = N'My modified TSQL item';  
GO  
```  
  
### <a name="c-changing-the-parameters-of-a-collection-item"></a>C. Cambiar los parámetros de un elemento de recopilación  
 En el ejemplo siguiente se cambian los parámetros asociados al elemento de recopilación. Se cambia la instrucción definida dentro del atributo `<Value>` y el atributo `UseSystemDatabases` está establecido en false. Para ver los parámetros actuales para este elemento, consulte la columna de parámetros en la vista del sistema syscollector_collection_items. Puede que necesite modificar el valor de `@collection_item_id`.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_update_collection_item   
@collection_item_id = 9,   
@parameters = '  
    \<ns:TSQLQueryCollector xmlns:ns="DataCollectorType">  
        <Query>  
            <Value>SELECT * FROM sys.dm_db_index_usage_stats</Value>  
            <OutputTable>MyOutputTable</OutputTable>  
        </Query>  
        <Databases>  
            <Database> UseSystemDatabases = "false"   
                       UseUserDatabases = "true"</Database>  
        </Databases>  
    \</ns:TSQLQueryCollector>';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Recopilación de datos](../../relational-databases/data-collection/data-collection.md)   
 [sp_syscollector_create_collection_item &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-item-transact-sql.md)   
 [syscollector_collection_items &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-collection-items-transact-sql.md)  
  
  
