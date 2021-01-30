---
description: sys.dm_column_encryption_enclave (Transact-SQL)
title: sys.dm_column_encryption_enclave (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/11/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: system-objects
ms.topic: reference
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 402c5f6703a8639c1fbedc645da54ae821162bcd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200341"
---
# <a name="sysdm_column_encryption_enclave-transact-sql"></a>sys.dm_column_encryption_enclave (Transact-SQL)
[!INCLUDE [sqlserver2019-windows-only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Devuelve los contadores de rendimiento para el enclave seguro para Always Encrypted. Para más información, consulte [Always Encrypted con enclaves seguros](../security/encryption/always-encrypted-enclaves.md).

Si el enclave está configurado y se ha inicializado correctamente después del último reinicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la vista contiene exactamente una fila. Si el enclave no está configurado o no se ha inicializado correctamente, la vista no devuelve ninguna fila. 

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|current_enclave_session_count|**int**|Número actual de sesiones de cliente que usan enclave.|  
|current_column_encryption_key_count|**int**|Recuento de claves de cifrado de columna que el enclave contiene actualmente.|  
|current_memory_size_kb|**bigint**|Tamaño de memoria de enclave en KB|  
|total_evicted_session_count|**bigint**|Número total de sesiones de enclave expulsadas desde el último reinicio del servidor.|   
  
## <a name="permissions"></a>Permisos  
Requiere el permiso `VIEW SERVER STATE`.   
  
## <a name="examples"></a>Ejemplos  
 
```sql  
SELECT * FROM sys.dm_column_encryption_enclave;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Configuración del tipo de enclave como opción de configuración del servidor Always Encrypted](../../database-engine/configure-windows/configure-column-encryption-enclave-type.md)
  
  
