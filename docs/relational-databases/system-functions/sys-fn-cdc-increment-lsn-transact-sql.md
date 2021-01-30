---
description: sys.fn_cdc_increment_lsn (Transact-SQL)
title: sys.fn_cdc_increment_lsn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_cdc_increment_lsn_TSQL
- sys.fn_cdc_increment_lsn_TSQL
- sys.fn_cdc_increment_lsn
- fn_cdc_increment_lsn
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_increment_lsn
- sys.fn_cdc_increment_lsn
ms.assetid: e53b6703-358b-4c9a-912a-8f7c7331069b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2e20b1d3146ee9e8f08cafdac3d2a4b1771a2081
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194396"
---
# <a name="sysfn_cdc_increment_lsn-transact-sql"></a>sys.fn_cdc_increment_lsn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve el número de secuencia de registro (LSN) siguiente de la secuencia basándose en el LSN especificado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.fn_cdc_increment_lsn ( lsn_value )  
```  
  
## <a name="arguments"></a>Argumentos  
 *lsn_value*  
 Valor de LSN. *lsn_value* es **binario (10)**.  
  
## <a name="return-type"></a>Tipo de valor devuelto  
 **binary(10)**  
  
## <a name="remarks"></a>Observaciones  
 El valor LSN devuelto por la función siempre es mayor que el valor especificado y no existe ningún valor LSN entre los dos valores.  
  
 Para consultar sistemáticamente un flujo de datos de cambio durante un periodo de tiempo, puede repetir periódicamente la llamada de función de la consulta, especificando cada vez un nuevo intervalo de la consulta para limitar los cambios devueltos en la misma. Para ayudar a asegurarse de que no se pierden datos, el límite superior de la consulta anterior se utiliza a menudo para generar el límite inferior de la consulta subsiguiente. Dado que el intervalo de la consulta es cerrado, el nuevo límite inferior debe ser mayor que el límite superior anterior, pero lo suficientemente pequeño para asegurarse de que ningún cambio contiene valores LSN comprendidos entre este valor y el límite superior anterior. La función sys.fn_cdc_increment_lsn se utiliza para obtener este valor.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol de base de datos public.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se utiliza `sys.fn_cdc_increment_lsn` para generar un nuevo valor de límite inferior para una consulta de captura de datos modificados basado en el límite superior guardado de una consulta anterior y guardado en la variable `@save_to_lsn`.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @from_lsn binary(10), @to_lsn binary(10), @save_to_lsn binary(10);  
SET @save_to_lsn = <previous_upper_bound_value>;  
SET @from_lsn = sys.fn_cdc_increment_lsn(@save_to_lsn);  
SET @to_lsn = sys.fn_cdc_get_max_lsn();  
SELECT * from cdc.fn_cdc_get_all_changes_HumanResources_Employee( @from_lsn, @to_lsn, 'all' );  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.fn_cdc_decrement_lsn &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-cdc-decrement-lsn-transact-sql.md)   
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)   
 [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)   
 [El registro de transacciones &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [Acerca de la captura de datos modificados &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
