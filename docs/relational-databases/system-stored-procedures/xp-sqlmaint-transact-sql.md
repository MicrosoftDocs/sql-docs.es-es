---
description: xp_sqlmaint (Transact-SQL)
title: xp_sqlmaint (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_sqlmaint
- xp_sqlmaint_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- xp_sqlmaint
ms.assetid: bda66e1b-6bbd-49be-b86e-37efc920e912
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 5b83e5e84d5712e9b1cf3e253222d1ef6d212720
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187958"
---
# <a name="xp_sqlmaint-transact-sql"></a>xp_sqlmaint (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Llama a la utilidad **SQLMAINT** con una cadena que contiene los modificadores **SQLMAINT**. La utilidad **SQLMAINT** realiza un conjunto de operaciones de mantenimiento en una o varias bases de datos.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
xp_sqlmaint 'switch_string'     
```  
  
## <a name="arguments"></a>Argumentos  
 **'** *switch_string* **'**  
 Es una cadena que contiene los modificadores de la utilidad **SQLMAINT** . Los modificadores y sus valores tienen que estar separados por un espacio.  
  
 El  el modificador no es válido para **xp_sqlmaint**.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 Ninguno. Devuelve un error si se produce un error en la utilidad **SQLMAINT** .  
  
## <a name="remarks"></a>Observaciones  
 Si un usuario que ha iniciado sesión con SQL Server autenticación llama a este procedimiento, los modificadores **-U "**_login_id_*_"_* y **-P "**_contraseña_*_"_* se anteponen a *switch_string* antes de la ejecución. Si el usuario ha iniciado sesión con la autenticación de Windows, *switch_string* se pasa sin cambiar a **SQLMAINT**.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente, `xp_sqlmaint` llama a `sqlmaint` para realizar comprobaciones de integridad, crear un archivo de informe y actualizar `msdb.dbo.sysdbmaintplan_history`.  
  
```  
EXEC xp_sqlmaint '-D AdventureWorks2012 -PlanID 02A52657-D546-11D1-9D8A-00A0C9054212   
   -Rpt "C:\Program Files\Microsoft SQL Server\MSSQL\LOG\DBMaintPlan2.txt" -WriteHistory  -CkDB -CkAl';   
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
The command(s) executed successfully.  
```  
  
## <a name="see-also"></a>Consulte también  
 [SQLMAINT (utilidad)](../../tools/sqlmaint-utility.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
