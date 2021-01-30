---
description: core.sp_update_data_source (Transact-SQL)
title: core.sp_update_data_source (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_data_source
- sp_update_data_source_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_data_source
- management data warehouse, data collector stored procedures
- core.sp_update_data_source stored procedure
- data collector [SQL Server], stored procedures
ms.assetid: 66b95f96-6df7-4657-9b3c-86a58c788ca5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 45bfb2c042ea491516aa4d3caadff72c45451202
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180587"
---
# <a name="coresp_update_data_source-transact-sql"></a>core.sp_update_data_source (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Actualiza una fila existente o inserta una fila nueva en la tabla core.source_info_internal del almacén de administración de datos. El componente en tiempo de ejecución del recopilador de datos llama a este procedimiento cada vez que un paquete de carga comienza a cargar los datos en el almacén de administración de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
core.sp_update_data_source [ @collection_set_uid = ] 'collection_set_uid'  
    ,[ @machine_name = ] 'machine_name'  
    , [ @named_instance = ] 'named_instance'  
    , [ @days_until_expiration = ] days_until_expiration  
    , [ @source_id = ] source_id OUTPUT  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @collection_set_uid =] '*collection_set_uid*'  
 GUID del conjunto de recopilación. *collection_set_uid* es de tipo **uniqueidentifier** y no tiene ningún valor predeterminado. Para obtener el GUID, consulte la vista dbo.syscollector_collection_sets en la base de datos msdb.  
  
 [ @machine_name =] '*machine_name*'  
 Nombre del servidor en el que reside el conjunto de recopilación. *machine_name* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
 [ @named_instance =] '*named_instance*'  
 Nombre de la instancia del conjunto de recopilación. *named_instance* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
> [!NOTE]  
>  *named_instance* debe ser el nombre completo de la instancia, que consta del nombre del equipo y el nombre de la instancia con el formato *NombreDeEquipo* \\ *nombreDeInstancia*.  
  
 [ @days_until_expiration =] *days_until_expiration*  
 Número de días restantes en el período de retención de datos de la instantánea. *days_until_expiration* es **smallint**.  
  
 [ @source_id =] *Source_ID*  
 El identificador único del origen de la actualización. *Source_ID* es de **tipo int** y se devuelve como salida.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 Cada vez que un paquete de carga inicia la carga de datos en el almacén de administración de datos, el componente en tiempo de ejecución del recopilador de datos llama a core.sp_update_data_source. La tabla core.source_info_internal se actualiza si se produjo alguno de los cambios siguientes desde la última actualización:  
  
-   Se agregó un nuevo conjunto de recopilación.  
  
-   El valor de days_until_expiration ha cambiado.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de base de datos **mdw_writer** (con permiso Execute).  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se actualiza el origen de datos (en este caso el conjunto de recopilación Uso de disco), establece el número de días hasta la expiración y devuelve el identificador del origen. En este ejemplo se usa la instancia predeterminada.  
  
```  
USE <management_data_warehouse>;  
GO  
DECLARE @source_id int;  
EXEC core.sp_update_data_source   
@collection_set_uid = '7B191952-8ECF-4E12-AEB2-EF646EF79FEF',   
@machine_name = '<computername>',  
@named_instance = 'MSSQLSERVER',  
@days_until_expiration = 10,  
@source_id = @source_id OUTPUT;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados del recopilador de datos &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Almacén de administración de datos](../../relational-databases/data-collection/management-data-warehouse.md)  
  
  
