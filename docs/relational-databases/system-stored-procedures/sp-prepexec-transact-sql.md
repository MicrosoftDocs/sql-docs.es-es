---
description: sp_prepexec (Transact-SQL)
title: sp_prepexec (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursor_prepexec
- sp_cursor_prepexec_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_prepexec
ms.assetid: f9141850-a62b-43bf-8e46-b2f92b75ca56
author: markingmyname
ms.author: maghan
ms.openlocfilehash: df6a482aeca510fde3d89dd74ddafc0ebe713287
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196008"
---
# <a name="sp_prepexec-transact-sql"></a>sp_prepexec (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Prepara y ejecuta una instrucción con parámetros [!INCLUDE[tsql](../../includes/tsql-md.md)] . sp_prepexec combina las funciones de sp_prepare y sp_execute. Esta acción se invoca mediante el identificador 13 en un paquete de flujo TDS.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_prepexec handle OUTPUT, params , stmt  
    [ , bound param ] [ ,...n]]  
```  
  
## <a name="arguments"></a>Argumentos  
 *asa*  
 Es el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identificador de *identificador* generado por. *Handle* es un parámetro necesario con un valor devuelto **int** .  
  
 *params*  
 Identifica instrucciones con parámetros. La definición de *params* de variables se sustituye por los marcadores de parámetros de la instrucción. *params* es un parámetro necesario que requiere un valor de entrada **ntext**, **nchar** o **nvarchar** . Escriba un valor NULL si la instrucción no tiene parámetros.  
  
 *instrucción*  
 Define el conjunto de resultados del cursor. El parámetro *stmt* es obligatorio y llama a para un valor de entrada **ntext**, **nchar** o **nvarchar** .  
  
 *bound_param*  
 Indica el uso opcional de parámetros adicionales. *bound_param* llama a un valor de entrada de cualquier tipo de datos para designar los parámetros adicionales que se usan.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se prepara y se ejecuta una instrucción simple:  
  
```  
Declare @Out int;  
EXEC sp_prepexec @Out output,   
    N'@P1 nvarchar(128), @P2 nvarchar(100)',  
    N'SELECT database_id, name  
      FROM sys.databases  
      WHERE name=@P1 AND state_desc = @P2',   
          @P1 = 'tempdb', @P2 = 'ONLINE';   
EXEC sp_unprepare @Out;  
```  
  
## <a name="see-also"></a>Vea también  
 [sp_prepare &#40;Transact SQL&#41;](../../relational-databases/system-stored-procedures/sp-prepare-transact-sql.md)   
 [sp_execute &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-execute-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
