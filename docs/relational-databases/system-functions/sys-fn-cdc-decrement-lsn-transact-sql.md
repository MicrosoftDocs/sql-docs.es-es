---
description: sys.fn_cdc_decrement_lsn (Transact-SQL)
title: sys.fn_cdc_decrement_lsn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_cdc_decrement_lsn
- sys.fn_cdc_decrement_lsn_TSQL
- sys.fn_cdc_decrement_lsn
- fn_cdc_decrement_lsn_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_decrement_lsn
- sys.fn_cdc_decrement_lsn
ms.assetid: 83c182ad-4713-439b-8769-9b7408aec8b4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: b40ee742575133895b6f0e3f55f87d784ab0a144
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194465"
---
# <a name="sysfn_cdc_decrement_lsn-transact-sql"></a>sys.fn_cdc_decrement_lsn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve el número de flujo de registro anterior (LSN) en la secuencia basada en el LSN especificado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.fn_cdc_decrement_lsn ( lsn_value )  
```  
  
## <a name="arguments"></a>Argumentos  
 *lsn_value*  
 Valor de LSN. *lsn_value* es **binario (10)**.  
  
## <a name="return-type"></a>Tipo de valor devuelto  
 **binary(10)**  
  
## <a name="remarks"></a>Observaciones  
 El LSN devuelto por la función siempre es menor que el valor especificado y no puede existir ningún valor de LSN entre los dos valores.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol de base de datos **Public** .  
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente utiliza `sys.fn_cdc_decrement_lsn` para establecer el límite del LSN superior en una consulta que devuelve filas de los datos del cambio que tienen valores LSN menores que el valor LSN máximo.  
  
```  
Use AdventureWorks2012;  
GO  
DECLARE @from_lsn binary(10), @to_lsn binary(10);  
SET @from_lsn = sys.fn_cdc_get_min_lsn('HumanResources_Employee');  
SET @to_lsn = sys.fn_cdc_decrement_lsn(sys.fn_cdc_get_max_lsn());  
SELECT * FROM cdc.fn_cdc_get_all_changes_HumanResources_Employee( @from_lsn, @to_lsn, 'all');   
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.fn_cdc_increment_lsn &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-cdc-increment-lsn-transact-sql.md)   
 [sys.fn_cdc_get_min_lsn &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-cdc-get-min-lsn-transact-sql.md)   
 [sys.fn_cdc_get_max_lsn &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-cdc-get-max-lsn-transact-sql.md)   
 [El registro de transacciones &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [Acerca de la captura de datos modificados &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
