---
description: core.sp_add_collector_type (Transact-SQL)
title: core.sp_add_collector_type (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_collector_type
- sp_add_collector_type_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- core.sp_add_collector_type stored procedure
- management data warehouse, data collector stored procedures
- sp_add_collector_type
- data collector [SQL Server], stored procedures
ms.assetid: 1d981037-2147-464e-a456-7d8e479bce89
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0db58f15641ab57d7eb35782c62334106a4bcdf7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181328"
---
# <a name="coresp_add_collector_type-transact-sql"></a>core.sp_add_collector_type (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Agrega una entrada nueva a la vista core.supported_collector_types en el almacén de administración de datos. El procedimiento se debe ejecutar en el contexto de la base de datos de almacén de administración de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
core.sp_add_collector_type [ @collector_type_uid = ] 'collector_type_uid'  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @collector_type_uid =] '*collector_type_uid*'  
 El GUID para el tipo de recopilador. *collector_type_uid* es de tipo **uniqueidentifier** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de base de datos **mdw_admin** (con permiso Execute).  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se agrega el tipo de recopilador Consulta T-SQL genérico a la vista core.supported_collector_types. De forma predeterminada, el tipo de recopilador Consulta T-SQL genérico ya existe. Por consiguiente, si ejecuta este código en una instalación predeterminada, recibirá un mensaje que indica que el tipo de recopilador ya existe.  
  
 Este código se ejecutaría correctamente si hubiera quitado el tipo de recopilador Consulta T-SQL genérico utilizando el procedimiento almacenado core.sp_remove_collector_type y, a continuación, deseara volver a agregarlo como un tipo de recopilador registrado que pueda cargar los datos al almacén de administración de datos.  
  
```  
USE <management_data_warehouse>;  
GO  
DECLARE @RC int;  
DECLARE @collector_type_uid uniqueidentifier;  
SELECT @collector_type_uid = (SELECT collector_type_uid FROM msdb.dbo.syscollector_collector_types WHERE name = N'Generic T-SQL Query Collector Type');  
EXECUTE @RC = core.sp_add_collector_type @collector_type_uid;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados del recopilador de datos &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Almacén de administración de datos](../../relational-databases/data-collection/management-data-warehouse.md)  
  
  
