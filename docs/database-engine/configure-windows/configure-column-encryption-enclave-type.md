---
title: Configuración del tipo de enclave como opción de configuración del servidor Always Encrypted | Microsoft Docs
description: Descubra cómo habilitar o deshabilitar un enclave seguro para Always Encrypted. Obtenga información sobre cómo confirmar si un enclave se ha inicializado correctamente.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: a35a08152dcb1c551c493ac88b0073f07e1280dd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059500"
---
# <a name="configure-the-enclave-type-for-always-encrypted-server-configuration-option"></a>Configuración del tipo de enclave como opción de configuración del servidor Always Encrypted

[!INCLUDE [sqlserver2019-windows-only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

En este artículo se describe cómo habilitar o deshabilitar un enclave seguro para Always Encrypted con enclaves seguros. Para obtener más información, vea [Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves.md) y [Configuración del enclave seguro en SQL Server](../../relational-databases/security/encryption/always-encrypted-enclaves-configure-enclave-type.md).

La opción de configuración del servidor relativa al **tipo de enclave de cifrado de columnas** controla el tipo de un enclave seguro que se usa para Always Encrypted. La opción se puede establecer con los siguientes valores:  
  
|Value|Descripción|  
|-------------------|-----------------| 
|0|**Sin enclave seguro**. El [!INCLUDE[ssDE](../../includes/ssde-md.md)] no inicializará el enclave seguro para Always Encrypted. Como resultado, la funcionalidad de Always Encrypted con enclaves seguros no estará disponible.|  
|1|**Seguridad basada en la virtualización (VBS)** . [!INCLUDE[ssDE](../../includes/ssde-md.md)] intentará inicializar un enclave de seguridad basada en la virtualización (VBS).

> [!IMPORTANT]
> Los cambios efectuados en el **tipo de enclave de cifrado de columnas** no surtirán efecto hasta que se reinicie la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
   
Puede comprobar el valor del tipo enclave configurado y el valor del tipo enclave actualmente en vigor mediante la vista [sys.configurations (Transact-SQL)](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md). 

Para confirmar que un enclave del tipo (mayor que 0) actualmente en vigor se ha inicializado correctamente después del último reinicio de [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)], compruebe la vista [sys.dm_column_encryption_enclave (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-column-encryption-enclave.md):
 - Si la vista contiene exactamente una fila, el enclave está inicializado correctamente. 
 - Si la vista no contiene ninguna fila, consulte en el registro de errores de SQL Server los errores de inicialización del enclave. Consulte [Ver el registro de errores de SQL Server (SQL Server Management Studio)](../../relational-databases/performance/view-the-sql-server-error-log-sql-server-management-studio.md).

Para obtener instrucciones paso a paso sobre cómo configurar un enclave de VBS, consulte [Configurar Always Encrypted con enclaves seguros en SQL Server](../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md#step-3-enable-always-encrypted-with-secure-enclaves-in-sql-server).

## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se habilita el enclave seguro y se establece el tipo enclave en VBS:

```sql  
sp_configure 'column encryption enclave type', 1;  
GO  
RECONFIGURE;  
GO  
```  

En el ejemplo siguiente se deshabilita el enclave seguro:  

```sql  
sp_configure 'column encryption enclave type', 0;  
GO  
RECONFIGURE;  
GO  
```  

La consulta siguiente recupera el tipo de enclave configurado y el tipo de enclave que está actualmente en vigor:

```sql  
USE [master];
GO
SELECT
[value]
, CASE [value] WHEN 0 THEN 'No enclave' WHEN 1 THEN 'VBS' ELSE 'Other' END AS [value_description]
, [value_in_use]
, CASE [value_in_use] WHEN 0 THEN 'No enclave' WHEN 1 THEN 'VBS' ELSE 'Other' END AS [value_in_use_description]
FROM sys.configurations
WHERE [name] = 'column encryption enclave type'; 
```  
## <a name="next-steps"></a>Pasos siguientes
 [Administración de claves para Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md)

## <a name="see-also"></a>Consulte también  
 [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [sys.configurations (Transact-SQL)](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)   
 [sys.dm_column_encryption_enclave (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-column-encryption-enclave.md)   
