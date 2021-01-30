---
description: sp_syscollector_create_collection_item (Transact-SQL)
title: sp_syscollector_create_collection_item (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_create_collection_item
- sp_syscollector_create_collection_item_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_create_collection_item
- data collector [SQL Server], stored procedures
ms.assetid: 60dacf13-ca12-4844-b417-0bc0a8bf0ddb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e58f8407c152be375099e9eb70e3fe63d0100e67
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207034"
---
# <a name="sp_syscollector_create_collection_item-transact-sql"></a>sp_syscollector_create_collection_item (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un elemento de recopilación en un conjunto de recopilación definido por el usuario. Los elementos de recopilación definen los datos que se van a recopilar y la frecuencia con la que se realizará dicha recopilación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_syscollector_create_collection_item   
      [ @collection_set_id = ] collection_set_id   
    , [ @collector_type_uid = ] 'collector_type_uid'  
    , [ @name = ] 'name'   
    , [ [ @frequency = ] frequency ]  
    , [ @parameters = ] 'parameters'  
    , [ @collection_item_id = ] collection_item_id OUTPUT  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @collection_set_id =] *collection_set_id*  
 Es el identificador único local del conjunto de recopilaciones. *collection_set_id* es de **tipo int**.  
  
 [ @collector_type_uid =] '*collector_type_uid*'  
 Es el GUID que identifica el tipo de recopilador que se va a usar para este elemento *collector_type_uid* es de tipo **uniqueidentifier** y no tiene ningún valor predeterminado. Para obtener una lista de los tipos de recopilador, consulte la vista del sistema syscollector_collector_types.  
  
 [ @name =] '*nombre*'  
 Es el nombre del elemento de colección. *Name* es de **tipo sysname** y no puede ser una cadena vacía o null.  
  
 *el nombre* debe ser único. Para obtener una lista de los nombres de elementos de recopilación actuales, consulte la vista del sistema syscollector_collection_items.  
  
 [ @frequency =] *frecuencia*  
 Se utiliza para especificar (en segundos) la frecuencia con la que este elemento de recopilación reúne los datos. *Frequency* es de **tipo int** y su valor predeterminado es 5. El valor mínimo que se puede especificar es 5 segundos.  
  
 Si el conjunto de recopilación se establece en modo sin almacenamiento en caché, se omite la frecuencia porque este modo hace que la recopilación de datos y la carga se produzcan con la programación especificada para el conjunto de recopilación. Para ver el modo de recopilación del conjunto de recopilación, consulte la vista del sistema [syscollector_collection_sets](../../relational-databases/system-catalog-views/syscollector-collection-sets-transact-sql.md) .  
  
 [ @parameters =] '*Parameters*'  
 Parámetros de entrada del tipo de recopilador. *Parameters* es **XML** con un valor predeterminado de NULL. El esquema de *parámetros* debe coincidir con el esquema de parámetros del tipo de recopilador.  
  
 [ @collection_item_id =] *collection_item_id*  
 Es el identifer único que identifica el elemento del conjunto de recopilación. *collection_item_id* es **int** y tiene output.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 sp_syscollector_create_collection_item se debe ejecutar en el contexto de la base de datos del sistema msdb.  
  
 El conjunto de recopilación al que se agrega el elemento de recopilación debe detenerse antes de crear dicho elemento. Los elementos de recopilación no se pueden agregar a los conjuntos de recopilación del sistema.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol fijo de base de datos dc_admin (con permiso EXECUTE) para ejecutar este procedimiento.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se crea un elemento de recopilación en función del tipo de recopilador `Generic T-SQL Query Collector Type` y se agrega al conjunto de recopilación denominado `Simple collection set test 2`. Para crear el conjunto de recopilación especificado, ejecute el ejemplo B en [sp_syscollector_create_collection_set &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-set-transact-sql.md).  
  
```  
USE msdb;  
GO  
DECLARE @collection_item_id int;  
DECLARE @collection_set_id int = (SELECT collection_set_id   
                                  FROM syscollector_collection_sets  
                                  WHERE name = N'Simple collection set test 2');  
DECLARE @collector_type_uid uniqueidentifier =   
    (SELECT collector_type_uid  
     FROM syscollector_collector_types  
     WHERE name = N'Generic T-SQL Query Collector Type');  
DECLARE @params xml =   
    CONVERT(xml, N'\<ns:TSQLQueryCollector xmlns:ns="DataCollectorType">  
            <Query>  
                <Value>SELECT * FROM sys.objects</Value>  
                <OutputTable>MyOutputTable</OutputTable>  
            </Query>  
            <Databases>   
                <Database> UseSystemDatabases = "true"   
                           UseUserDatabases = "true"  
                </Database>  
            </Databases>  
         \</ns:TSQLQueryCollector>');  
  
EXEC sp_syscollector_create_collection_item  
    @collection_set_id = @collection_set_id,  
    @collector_type_uid = @collector_type_uid,  
    @name = 'My custom TSQL query collector item',  
    @frequency = 6000,  
    @parameters = @params,  
    @collection_item_id = @collection_item_id OUTPUT;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Recopilación de datos](../../relational-databases/data-collection/data-collection.md)   
 [sp_syscollector_update_collection_item &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syscollector-update-collection-item-transact-sql.md)   
 [sp_syscollector_delete_collection_item &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syscollector-delete-collection-item-transact-sql.md)   
 [syscollector_collector_types &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/syscollector-collector-types-transact-sql.md)   
 [sp_syscollector_create_collection_set &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-set-transact-sql.md)   
 [syscollector_collection_items &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-collection-items-transact-sql.md)  
  
  
