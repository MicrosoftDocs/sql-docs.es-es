---
description: sp_syspolicy_delete_policy_execution_history (Transact-SQL)
title: sp_syspolicy_delete_policy_execution_history (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_delete_policy_execution_history
- sp_syspolicy_delete_policy_execution_history_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_delete_policy_execution_history
ms.assetid: fe651af9-267e-45ec-b4e7-4b0698fb1be3
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 221a295d5c1bcf3b5f8890bca991edc6675e67af
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201220"
---
# <a name="sp_syspolicy_delete_policy_execution_history-transact-sql"></a>sp_syspolicy_delete_policy_execution_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina el historial de ejecución de directivas para la administración basada en directivas. Puede utilizar este procedimiento almacenado para eliminar el historial de ejecución de una directiva concreta o de todas las directivas, y el historial de ejecución anterior a una fecha concreta.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_syspolicy_delete_policy_execution_history [ @policy_id = ] policy_id ]  
    [ , [ @oldest_date = ] 'oldest_date' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @policy_id = ] policy_id` Es el identificador de la Directiva para la que desea eliminar el historial de ejecución. *policy_id* es de **tipo int** y es obligatorio. Puede ser NULL.  
  
`[ @oldest_date = ] 'oldest_date'` Es la fecha más antigua para la que desea mantener el historial de ejecución de directivas. Se eliminan todos los historiales de ejecución anteriores a esta fecha. *oldest_date* es **DateTime** y es obligatorio. Puede ser NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 Debe ejecutar sp_syspolicy_delete_policy_execution_history en el contexto de la base de datos del sistema msdb.  
  
 Para obtener los valores de *policy_id* y para ver las fechas del historial de ejecución, puede usar la siguiente consulta:  
  
```  
SELECT a.name AS N'policy_name', b.policy_id, b.start_date, b.end_date  
FROM msdb.dbo.syspolicy_policies AS a   
INNER JOIN msdb.dbo.syspolicy_policy_execution_history AS b  
ON a.policy_id = b.policy_id  
```  
  
 El comportamiento siguiente se aplica si especifica NULL para uno o ambos valores:  
  
-   Para eliminar todo el historial de ejecución de directivas, especifique NULL tanto para *policy_id* como para *oldest_date*.  
  
-   Para eliminar todo el historial de ejecución de directivas de una directiva específica, especifique un identificador de directiva para *policy_id* y especifique NULL como *oldest_date*.  
  
-   Para eliminar el historial de ejecución de directivas de todas las directivas anteriores a una fecha concreta, especifique NULL para *policy_id* y especifique una fecha para *oldest_date*.  
  
 Para almacenar el historial de ejecución de directivas, puede abrir el registro de historial de directivas en el Explorador de objetos y exportar el historial de ejecución a un archivo. Para tener acceso al registro de historial de directivas, expanda **Administración**, haga clic con el botón secundario en **Administración de directivas** y, a continuación, haga clic en **Ver historial**.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de base de datos PolicyAdministratorRole.  
  
> [!IMPORTANT]  
>  Posible elevación de credenciales: los usuarios del rol PolicyAdministratorRole pueden crear desencadenadores del servidor y programar ejecuciones de directivas que pueden afectar al funcionamiento de la instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Por ejemplo, los usuarios del rol PolicyAdministratorRole pueden crear una directiva que puede evitar que la mayoría de los objetos se creen en [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Debido a esta posible elevación de credenciales, el rol PolicyAdministratorRole se debería conceder únicamente a los usuarios que sean de confianza para controlar la configuración de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se elimina el historial de ejecución de directivas anterior a una fecha concreta de una directiva con el identificador 7.  
  
```  
EXEC msdb.dbo.sp_syspolicy_delete_policy_execution_history @policy_id = 7  
, @oldest_date = '2009-02-16 16:00:00.000';  
  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de administración basada en directivas &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_set_config_history_retention &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-set-config-history-retention-transact-sql.md)   
 [sp_syspolicy_purge_history &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-purge-history-transact-sql.md)  
  
  
