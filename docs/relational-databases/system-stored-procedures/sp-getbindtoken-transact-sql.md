---
description: sp_getbindtoken (Transact-SQL)
title: sp_getbindtoken (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_getbindtoken
- sp_getbindtoken_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_getbindtoken
ms.assetid: 5db87d77-85fa-45a3-a23a-3ea500f9a5ac
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4613c83384aa7ff2cd36512f6c172a00fbbb047e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185753"
---
# <a name="sp_getbindtoken-transact-sql"></a>sp_getbindtoken (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve un identificador único para la transacción. Este identificador único es una cadena que se utiliza para enlazar sesiones mediante sp_bindsession.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, utilice conjuntos de resultados activos múltiples (MARS) o transacciones distribuidas. Para obtener más información, vea [Usar conjuntos de resultados activos múltiples &#40;MARS&#41;](../../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_getbindtoken [@out_token =] 'return_value' OUTPUT   
```  
  
## <a name="arguments"></a>Argumentos  
 [ @out_token =] '*RETURN_VALUE*'  
 Es el token que se va a utilizar para enlazar las sesiones. *RETURN_VALUE* es de tipo **VARCHAR (255)** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 sp_getbindtoken devolverá un token válido solo cuando el procedimiento almacenado se ejecute dentro de una transacción activa. En caso contrario, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] devuelve un mensaje de error. Por ejemplo:  
  
```  
-- Declare a variable to hold the bind token.  
-- No active transaction.  
DECLARE @bind_token varchar(255);  
-- Trying to get the bind token returns an error 3921.  
EXECUTE sp_getbindtoken @bind_token OUTPUT;  
Server: Msg 3921, Level 16, State 1, Procedure sp_getbindtoken, Line 4  
Cannot get a transaction token if there is no transaction active.  
Reissue the statement after a transaction has been started.  
```  
  
 Cuando se usa sp_getbindtoken para dar de alta una conexión de transacción distribuida dentro de una transacción abierta, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve el mismo token. Por ejemplo:  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @bind_token varchar(255);  
  
BEGIN TRAN;  
  
EXECUTE sp_getbindtoken @bind_token OUTPUT;  
SELECT @bind_token AS Token;  
  
BEGIN DISTRIBUTED TRAN;  
  
EXECUTE sp_getbindtoken @bind_token OUTPUT;  
SELECT @bind_token AS Token;  
```  
  
 Las dos instrucciones `SELECT` devuelven el mismo símbolo (token):  
  
```  
Token  
-----  
PKb'gN5<9aGEedk_16>8U=5---/5G=--  
(1 row(s_) affected)  
  
Token  
-----  
PKb'gN5<9aGEedk_16>8U=5---/5G=--  
(1 row(s_) affected)  
```  
  
 El símbolo (token) de enlace se puede utilizar con sp_bindsession para enlazar nuevas sesiones a la misma transacción. El símbolo (token) de enlace solo es válido localmente dentro de cada instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)], y no se puede compartir entre varias instancias.  
  
 Para obtener y pasar un símbolo (token) de enlace, debe ejecutar sp_getbindtoken antes de ejecutar sp_bindsession para compartir el mismo espacio de bloqueo. Si obtiene un símbolo (token) de enlace, sp_bindsession se ejecuta correctamente.  
  
> [!NOTE]  
>  Se recomienda utilizar la interfaz de programación de aplicaciones (API) de Servicios abiertos de datos srv_getbindtoken para obtener un símbolo (token) de enlace que se pueda utilizar desde un procedimiento almacenado extendido.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol public.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se obtiene un símbolo (token) de enlace y se muestra su nombre.  
  
```  
DECLARE @bind_token varchar(255);  
BEGIN TRAN;  
EXECUTE sp_getbindtoken @bind_token OUTPUT;  
SELECT @bind_token AS Token;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `Token`  
  
 `----------------------------------------------------------`  
  
 `\0]---5^PJK51bP<1F<-7U-]ANZ`  
  
## <a name="see-also"></a>Consulte también  
 [sp_bindsession &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindsession-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [srv_getbindtoken API de procedimiento almacenado extendido &#40;&#41;](../../relational-databases/extended-stored-procedures-reference/srv-getbindtoken-extended-stored-procedure-api.md)  
  
  
