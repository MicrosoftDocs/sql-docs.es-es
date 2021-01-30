---
description: sp_procoption (Transact-SQL)
title: sp_procoption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_procoption
- sp_procoption_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_procoption
ms.assetid: 6f0221bd-70b4-4b04-b15d-722235aceb3c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ed9d1056a751633f208a38cbb03f6a0d50057c88
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205120"
---
# <a name="sp_procoption-transact-sql"></a>sp_procoption (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Establece o borra la configuración de ejecución automática de un procedimiento almacenado. Un procedimiento almacenado que se establece en ejecución automática se ejecuta cada vez que se inicia una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_procoption [ @ProcName = ] 'procedure'   
    , [ @OptionName = ] 'option'   
    , [ @OptionValue = ] 'value'   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @ProcName = ] 'procedure'` Es el nombre del procedimiento para el que se va a establecer una opción. *procedure* es **nvarchar (776)** y no tiene ningún valor predeterminado.  
  
`[ @OptionName = ] 'option'` Es el nombre de la opción que se va a establecer. El único valor de la *opción* es **Startup**.  
  
`[ @OptionValue = ] 'value'` Indica si se debe establecer la opción en (**true** u **on**) o en OFF (**false** u **OFF**). el *valor* es **VARCHAR (12)** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o número de error (error)  
  
## <a name="remarks"></a>Observaciones  
 Los procedimientos de inicio deben estar en la base de datos **maestra** y no pueden contener parámetros de entrada ni de salida. La ejecución de los procedimientos almacenados comienza cuando se recuperan todas las bases de datos y se registra el mensaje "Se completó la recuperación" en el inicio.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se establece un procedimiento para la ejecución automática.  
  
```  
EXEC sp_procoption @ProcName = N'<procedure name>'   
    , @OptionName = 'startup'   
    , @OptionValue = 'on';   
```  
  
 En el ejemplo siguiente se detiene la ejecución automática de un procedimiento.  
  
```  
EXEC sp_procoption @ProcName = N'<procedure name>'      
    , @OptionName = 'startup'
    , @OptionValue = 'off';   
```  
  
## <a name="see-also"></a>Consulte también  
 [Ejecutar un procedimiento almacenado](../../relational-databases/stored-procedures/execute-a-stored-procedure.md)  
  
  
