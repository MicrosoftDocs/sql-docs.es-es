---
description: sp_recompile (Transact-SQL)
title: sp_recompile (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_recompile_TSQL
- sp_recompile
dev_langs:
- TSQL
helpviewer_keywords:
- sp_recompile
ms.assetid: 6192ca87-febd-4075-8199-14b4fa609b8c
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a5ff0697d2ca09677455c700b1dfc45f16f1ef72
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200651"
---
# <a name="sp_recompile-transact-sql"></a>sp_recompile (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Hace que se vuelvan a compilar los procedimientos almacenados, los desencadenadores y las funciones definidas por el usuario la próxima vez que se ejecuten. Para ello, quita el plan existente de la memoria cache de procedimientos que fuerza la creación de un nuevo plan la próxima vez que se ejecuten el procedimiento o el desencadenador. En una colección de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], se registra el evento SP:CacheInsert en lugar del evento SP:Recompile.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
  
sp_recompile [ @objname = ] 'object'  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @objname =] '*objeto*'  
 El nombre calificado o no calificado de un desencadenador, tabla, vista, procedimiento almacenado o función definida por el usuario de la base de datos actual. *Object* es de tipo **nvarchar (776)** y no tiene ningún valor predeterminado. Si *Object* es el nombre de un procedimiento almacenado, un desencadenador o una función definida por el usuario, el procedimiento almacenado, el desencadenador o la función se volverán a compilar la próxima vez que se ejecute. Si *Object* es el nombre de una tabla o vista, todos los procedimientos almacenados, desencadenadores o funciones definidas por el usuario que hacen referencia a la tabla o vista se volverán a compilar la próxima vez que se ejecuten.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o un número distinto de cero (error)  
  
## <a name="remarks"></a>Observaciones  
 sp_recompile busca un objeto solo en la base de datos actual.  
  
 Las consultas que utilizan los procedimientos almacenados o desencadenadores y funciones definidas por el usuario solo se optimizan cuando se compilan. A medida que se crean índices o se realizan otros cambios que afectan a las estadísticas de la base de datos, los procedimientos almacenados, desencadenadores y funciones definidas por el usuario compilados pueden perder eficacia. Al volver a compilar los procedimientos almacenados y desencadenadores que actúan sobre una tabla, puede volver a optimizar las consultas.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vuelve a compilar automáticamente los procedimientos almacenados, desencadenadores y funciones definidas por el usuario cuando esto supone una mejora.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER en el objeto especificado.  
  
## <a name="examples"></a>Ejemplos  
 El siguiente ejemplo provoca que los procedimientos almacenados, desencadenadores y funciones definidas por el usuario que actúan en la tabla `Customer` se vuelvan a compilar la próxima vez que se ejecuten.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_recompile N'Sales.Customer';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
