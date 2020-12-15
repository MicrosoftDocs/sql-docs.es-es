---
title: sp_pdw_database_encryption_regenerate_system_keys (Azure Synapse Analytics) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: bb13e323-a984-4462-8b6d-6019c38ddd9d
author: ronortloff
ms.author: rortloff
ms.reviewer: ''
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 89be1645a93d10e09b43dd3c32fc1fee72785791
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97410520"
---
# <a name="sp_pdw_database_encryption_regenerate_system_keys-azure-synapse-analytics"></a>sp_pdw_database_encryption_regenerate_system_keys (Azure Synapse Analytics)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Use **sp_pdw_database_encryption_regenerate_system_keys** para rotar el certificado y la clave de cifrado de base de datos para las bases de datos internas que se cifran cuando TDE está habilitado en el dispositivo. incluidos `tempdb`. Esto solo se realizará correctamente si TDE está habilitado.  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_pdw_database_encryption_regenerate_system_keys  ;  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Comentarios  
 El procedimiento no tiene parámetros.  
  
 Este procedimiento se debe usar cuando el tráfico del dispositivo es bajo.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de base de datos **sysadmin** o el permiso **Control Server** .  
  
## <a name="example"></a>Ejemplo  
 En el siguiente ejemplo se regeneran las claves de cifrado de base de datos.  
  
```sql  
EXEC sys.sp_pdw_database_encryption_regenerate_system_keys;  
```  
  
## <a name="see-also"></a>Vea también  
 [sp_pdw_database_encryption &#40;Azure Synapse Analytics&#41;](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md)   
 [sp_pdw_log_user_data_masking &#40;Azure Synapse Analytics&#41;](../../relational-databases/system-stored-procedures/sp-pdw-log-user-data-masking-sql-data-warehouse.md)  
  
