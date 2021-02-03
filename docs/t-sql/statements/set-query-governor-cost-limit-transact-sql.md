---
description: SET QUERY_GOVERNOR_COST_LIMIT (Transact-SQL)
title: SET QUERY_GOVERNOR_COST_LIMIT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET QUERY_GOVERNOR_COST_LIMIT
- SET_QUERY_GOVERNOR_COST_LIMIT_TSQL
- QUERY_GOVERNOR_COST_LIMIT
- QUERY_GOVERNOR_COST_LIMIT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET QUERY_GOVERNOR_COST_LIMIT statement
- connections [SQL Server], overriding
- QUERY_GOVERNOR_COST_LIMIT option
- overriding connection values
ms.assetid: 3424bb44-6915-462d-a8d7-fe834af81387
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9204c77bdc1a3e99d66cb2ee6000bf4c1ecb1609
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206982"
---
# <a name="set-query_governor_cost_limit-transact-sql"></a>SET QUERY_GOVERNOR_COST_LIMIT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Invalida el valor de **query governor cost limit** de la configuración para la conexión actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
SET QUERY_GOVERNOR_COST_LIMIT value  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *value*  
 Es un valor numérico o entero que especifica el tiempo máximo que puede durar la ejecución de una consulta. Los valores se redondean por defecto al entero más próximo. Los valores negativos se redondean a 0. El regulador de consultas no permite la ejecución de consultas que tengan un costo estimado superior a ese valor. Si especifica 0 (el valor predeterminado) en esta opción, se desactiva el regulador de consultas y se permite que todas las consultas se ejecuten de manera indefinida.  
  
 El término "costo de consultas" hace referencia al tiempo estimado, en segundos, necesario para completar una consulta con una configuración de hardware específica.  
  
## <a name="remarks"></a>Observaciones  
 SET QUERY_GOVERNOR_COST_LIMIT solo se aplica a la conexión actual y solo está activo durante la conexión. Use la opción [Establecer la opción de configuración del servidor Límite de costo de regulador de consultas](../../database-engine/configure-windows/configure-the-query-governor-cost-limit-server-configuration-option.md) de **sp_configure** para cambiar el valor de límite de costo del regulador de consultas aplicado a todo el servidor. Para más información sobre cómo configurar esta opción, vea [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) y [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
 La opción SET QUERY_GOVERNOR_COST_LIMIT se establece en tiempo de ejecución, no en tiempo de análisis.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="see-also"></a>Vea también  
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
