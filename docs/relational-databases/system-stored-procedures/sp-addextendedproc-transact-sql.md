---
description: sp_addextendedproc (Transact-SQL)
title: sp_addextendedproc (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addextendedproc_TSQL
- sp_addextendedproc
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addextendedproc
ms.assetid: c0d4b47b-a855-451e-90e5-5fb2d836ebfa
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5d3ca884a585b76eb62d9af20bf09757d7d3c84e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182787"
---
# <a name="sp_addextendedproc-transact-sql"></a>sp_addextendedproc (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Registra el nombre de un nuevo procedimiento almacenado extendido en [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, use la [integración con CLR](../../relational-databases/clr-integration/common-language-runtime-integration-overview.md) .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addextendedproc [ @functname = ] 'procedure' ,   
     [ @dllname = ] 'dll'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @functname = ] 'procedure'` Es el nombre de la función a la que se va a llamar dentro de la biblioteca de vínculos dinámicos (DLL). *procedure* es de tipo **nvarchar (** de-bajo) y no tiene ningún valor predeterminado. el *procedimiento* puede incluir opcionalmente el nombre del propietario con el formato *Owner. Function*.  
  
`[ @dllname = ] 'dll'` Es el nombre de la DLL que contiene la función. *dll* es de tipo **VARCHAR (255)** y no tiene ningún valor predeterminado. Se recomienda especificar la ruta de acceso completa del archivo DLL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 Una vez creado un procedimiento almacenado extendido, se debe agregar a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante **sp_addextendedproc**. Para obtener más información, vea [Agregar un procedimiento almacenado extendido a SQL Server](../../relational-databases/extended-stored-procedures-programming/adding-an-extended-stored-procedure-to-sql-server.md).  
  
 Este procedimiento solo se puede ejecutar en la base de datos **maestra** . Para ejecutar un procedimiento almacenado extendido desde una base de datos que no sea **maestra**, califique el nombre del procedimiento almacenado extendido con **Master**.  
  
 **sp_addextendedproc** agrega entradas a la vista de catálogo [Sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) , registrando el nombre del nuevo procedimiento almacenado extendido con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . También agrega una entrada en la vista de catálogo [Sys.extended_procedures](../../relational-databases/system-catalog-views/sys-extended-procedures-transact-sql.md) .  
  
> [!IMPORTANT]  
>  Las DLL existentes que no se registraron con una ruta completa no funcionarán tras una actualización a [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Para corregir el problema, use **sp_dropextendedproc** para anular el registro del archivo dll y, a continuación, vuelva a registrarlo con **sp_addextendedproc**, especificando la ruta de acceso completa.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_addextendedproc**.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se agrega el procedimiento almacenado extendido **xp_hello** .  
  
```  
USE master;  
GO  
EXEC sp_addextendedproc xp_hello, 'c:\xp_hello.dll';  
```  
  
## <a name="see-also"></a>Consulte también  
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [sp_dropextendedproc &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropextendedproc-transact-sql.md)   
 [sp_helpextendedproc &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpextendedproc-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
