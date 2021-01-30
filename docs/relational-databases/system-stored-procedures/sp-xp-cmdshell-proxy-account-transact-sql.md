---
description: sp_xp_cmdshell_proxy_account (Transact-SQL)
title: sp_xp_cmdshell_proxy_account (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_xp_cmdshell_proxy_account
- sp_xp_cmdshell_proxy_account_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_xp_cmdshell_proxy_account
- xp_cmdshell
ms.assetid: f807c373-7fbc-4108-a2bd-73b48a236003
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8414aa0c44c4f7d39b6be251bc47a9d23b4d783b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201764"
---
# <a name="sp_xp_cmdshell_proxy_account-transact-sql"></a>sp_xp_cmdshell_proxy_account (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crea una credencial de proxy para **xp_cmdshell**.  
  
> [!NOTE]  
>  **xp_cmdshell** está deshabilitada de forma predeterminada. Para habilitar **xp_cmdshell**, vea [Xp_cmdshell opción de configuración del servidor](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_xp_cmdshell_proxy_account [ NULL | { 'account_name' , 'password' } ]  
```  
  
## <a name="arguments"></a>Argumentos  
 NULL  
 Especifica que la credencial de proxy debe eliminarse.  
  
 *account_name*  
 Especifica un inicio de sesión de Windows que será el proxy.  
  
 *password*  
 Especifica la contraseña de la cuenta de Windows.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 La credencial de proxy se llamará **# #xp_cmdshell_proxy_account # #**.  
  
 Cuando se ejecuta con la opción NULL, **sp_xp_cmdshell_proxy_account** elimina la credencial del proxy.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CONTROL SERVER.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-the-proxy-credential"></a>A. Crear la credencial del proxy  
 En el siguiente ejemplo se muestra cómo crear una credencial de proxy para una cuenta de Windows denominada `ADVWKS\Max04` con la contraseña `ds35efg##65`.  
  
```  
EXEC sp_xp_cmdshell_proxy_account 'ADVWKS\Max04', 'ds35efg##65';  
GO  
```  
  
### <a name="b-dropping-the-proxy-credential"></a>B. Quitar la credencial del proxy  
 En el siguiente ejemplo se quita la credencial de proxy del almacén de credenciales.  
  
```  
EXEC sp_xp_cmdshell_proxy_account NULL;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [xp_cmdshell &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/xp-cmdshell-transact-sql.md)   
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)   
 [sys.credentials &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)  
  
  
