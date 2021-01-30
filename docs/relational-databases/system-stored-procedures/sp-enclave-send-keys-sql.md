---
description: sp_enclave_send_keys (Transact-SQL)
title: sp_enclave_send_keys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/19/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: reference
f1_keywords:
- sp_enclave_send_keys
- sp_enclave_send_keys_TSQL
- sys.sp_enclave_send_keys
- sys.sp_enclave_send_keys_TSQL
helpviewer_keywords:
- sp_enclave_send_keys
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 22d7f8b86d5cd247691c0d7527287c901845cb58
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99157303"
---
# <a name="sp_enclave_send_keys-transact-sql"></a>sp_enclave_send_keys (Transact-SQL)
[!INCLUDE [sqlserver2019-windows-only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Envía las claves de cifrado de columnas, definidas en la base de datos, al enclave seguro de servidor que se usa con [Always Encrypted con enclaves seguro](../security/encryption/always-encrypted-enclaves.md).

`sp_enclave_send_keys` solo envía las claves que están habilitadas para enclave y cifran las columnas que usan el cifrado aleatorio y tienen índices. Para una consulta de usuario normal, un controlador cliente proporciona el enclave con las claves necesarias para los cálculos de la consulta. `sp_enclave_send_keys` envía todas las claves de cifrado de columna definidas en la base de datos y se utiliza para los índices columnas cifradas. 

`sp_enclave_send_keys` proporciona una manera sencilla de enviar claves a enclave y rellenar la memoria caché de claves de cifrado de columnas para operaciones de indexación posteriores. Use `sp_enclave_send_keys` para habilitar:
- Un DBA para recompilar o modificar índices o estadísticas en columnas de base de datos cifradas, si el DBA no tiene acceso a las claves maestras de columna. Consulte [invocar operaciones de indexación mediante claves de cifrado de columnas almacenadas en caché](../security/encryption/always-encrypted-enclaves-create-use-indexes.md#invoke-indexing-operations-using-cached-column-encryption-keys).
- [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] para completar la recuperación de índices en columnas cifradas. Consulte [recuperación de base de datos](../security/encryption/always-encrypted-enclaves.md#database-recovery).
- Una aplicación que usa .NET Framework proveedor de datos para SQL Server cargar datos de forma masiva en columnas cifradas.

Para invocar correctamente `sp_enclave_send_keys` , debe conectarse a la base de datos con los cálculos de Always Encrypted y enclave habilitados para la conexión de base de datos. También debe tener acceso a las claves maestras de columna, proteger las claves de cifrado de columna, va a enviar y necesita permisos para obtener acceso a los metadatos de la clave de Always Encrypted en la base de datos. 

## <a name="syntax"></a>Sintaxis  
  
```
sp_enclave_send_keys
[ ;]  
```

## <a name="arguments"></a>Argumentos

Este procedimiento almacenado no tiene argumentos.

## <a name="return-value"></a>Valor devuelto

Este procedimiento almacenado no tiene ningún valor devuelto.
  
## <a name="result-sets"></a>Conjuntos de resultados

Este procedimiento almacenado no tiene ningún conjunto de resultados.
  
## <a name="permissions"></a>Permisos

 Requiere los `VIEW ANY COLUMN ENCRYPTION KEY DEFINITION` `VIEW ANY COLUMN MASTER KEY DEFINITION` permisos y en la base de datos.  
  
## <a name="examples"></a>Ejemplos  
  
```sql
EXEC sp_enclave_send_keys;  
```

## <a name="see-also"></a>Vea también
- [Always Encrypted con enclaves seguros](../security/encryption/always-encrypted-enclaves.md) 
 
- [Creación y uso de índices en columnas mediante Always Encrypted con enclaves seguros](../security/encryption/always-encrypted-enclaves-create-use-indexes.md)

- [Tutorial: Creación y uso de índices en columnas habilitadas para enclave que usan cifrado aleatorio](../security/tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md)
