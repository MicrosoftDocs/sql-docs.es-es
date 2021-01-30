---
description: sp_msx_get_account (Transact-SQL)
title: sp_msx_get_account (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_msx_get_account_TSQL
- sp_msx_get_account
dev_langs:
- TSQL
helpviewer_keywords:
- sp_msx_get_account
ms.assetid: 7b478049-e2d0-4bac-865a-b97fd1d8dfbc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 932fd01c930878902eb1636ed8d42083e2dcad10
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180229"
---
# <a name="sp_msx_get_account-transact-sql"></a>sp_msx_get_account (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra información sobre la credencial que el servidor de destino utiliza para conectarse al servidor maestro.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_msx_get_account  
```  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Devuelve el siguiente conjunto de resultados:  
  
|Nombre de columna|Tipo|Descripción|  
|-----------------|----------|-----------------|  
|msx_connection|**int**|Número de conexión del servidor maestro.|  
|msx_credential_id|**int**|Id. de la credencial utilizada para esta conexión al servidor maestro.|  
|msx_credential_name|**sysname**|Nombre de la credencial utilizada para esta conexión al servidor maestro.|  
|msx_login_name|**nvarchar(4000)**|Nombre de dominio y nombre de usuario de Windows para la credencial.|  
  
## <a name="remarks"></a>Observaciones  
 Devuelve un conjunto de resultados vacío si no se especifica ninguna credencial para este servidor de destino. Para configurar la credencial, utilice sp_msx_set_account.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor sysadmin.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se muestra información de la credencial que utiliza este servidor de destino para iniciar una sesión en el servidor maestro.  
  
```  
USE msdb ;  
GO  
  
EXECUTE dbo.sp_msx_get_account ;  
GO  
```  
  
 A continuación se muestra un conjunto de resultados de ejemplo:  
  
 `msx_connection msx_credential_id msx_credential_name  msx_login_name`  
  
 `-------------- ----------------- -------------------- ------------------------------`  
  
 `1              65538             MsxAccount           AdventureWorks2012\MsxAccount`  
  
## <a name="see-also"></a>Consulte también  
 [Agente SQL Server procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)   
 [sp_msx_set_account &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-msx-set-account-transact-sql.md)  
  
  
