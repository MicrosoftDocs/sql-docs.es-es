---
description: sys.fn_cdc_map_time_to_lsn (Transact-SQL)
title: sys.fn_cdc_map_time_to_lsn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_cdc_map_time_to_lsn
- fn_cdc_map_time_to_lsn_TSQL
- sys.fn_cdc_map_time_to_lsn_TSQL
- fn_cdc_map_time_to_lsn
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_map_time_to_lsn
- sys.fn_cdc_map_time_to_lsn
ms.assetid: 6feb051d-77ae-4c93-818a-849fe518d1d4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6db62a3241b86bdac8dcc955aef865ef8ad99a21
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198747"
---
# <a name="sysfn_cdc_map_time_to_lsn-transact-sql"></a>sys.fn_cdc_map_time_to_lsn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve el valor de número de secuencia de registro (LSN) de la **start_lsn** columna de la [CDC.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md) tabla del sistema durante el tiempo especificado. Puede usar esta función para asignar sistemáticamente los intervalos de fecha y hora en el intervalo basado en LSN que necesitan las funciones de enumeración de captura de datos modificados [cdc.fn_cdc_get_all_changes_<capture_instance>](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md) y [CDC.fn_cdc_get_net_changes_ ](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)<capture_instance>para devolver los cambios de los datos dentro de ese intervalo.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.fn_cdc_map_time_to_lsn ( '<relational_operator>', tracking_time )  
  
<relational_operator> ::=  
{  largest less than  
 | largest less than or equal  
 | smallest greater than  
 | smallest greater than or equal  
}  
```  
  
## <a name="arguments"></a>Argumentos  
 **'**<relational_operator>**'** {mayor menor que el más grande menor o igual que el más pequeño mayor que el más \| \| \| pequeño mayor o igual que}  
 Se utiliza para identificar un valor de LSN distinto en dentro de la tabla de **CDC.lsn_time_mapping** con una **tran_end_time** asociada que satisface la relación en comparación con el valor de *tracking_time* .  
  
 *relational_operator* es **nvarchar (30)**.  
  
 *tracking_time*  
 Es el valor de fecha y hora con el que se hará la comparación. *tracking_time* es **DateTime**.  
  
## <a name="return-type"></a>Tipo de valor devuelto  
 **binary(10)**  
  
## <a name="remarks"></a>Observaciones  
 Para entender cómo se puede usar el **Sys.fn_cdc_map_time_lsn** para asignar intervalos de fecha y hora a intervalos de LSN, considere el siguiente escenario. Suponga que un consumidor desea extraer los datos de cambios cada día. Es decir, el consumidor solo desea obtener los cambios que se han realizado durante un día determinado hasta la medianoche incluida. El límite inferior del intervalo temporal sería hasta las doce de la noche (no inclusive) del día anterior. El límite superior sería hasta las doce de la noche (inclusive) del día en cuestión. En el ejemplo siguiente se muestra cómo se puede usar la función **Sys.fn_cdc_map_time_to_lsn** para asignar sistemáticamente este intervalo basado en el LSN en el intervalo basado en LSN que necesitan las funciones de enumeración de captura de datos modificados para devolver todos los cambios dentro de ese intervalo.  
  
 `DECLARE @begin_time datetime, @end_time datetime, @begin_lsn binary(10), @end_lsn binary(10);`  
  
 `SET @begin_time = '2007-01-01 12:00:00.000';`  
  
 `SET @end_time = '2007-01-02 12:00:00.000';`  
  
 `SELECT @begin_lsn = sys.fn_cdc_map_time_to_lsn('smallest greater than', @begin_time);`  
  
 `SELECT @end_lsn = sys.fn_cdc_map_time_to_lsn('largest less than or equal', @end_time);`  
  
 `SELECT * FROM cdc.fn_cdc_get_net_changes_HR_Department(@begin_lsn, @end_lsn, 'all` `');`  
  
 El operador relacional '`smallest greater than`' se usa para restringir los cambios a los que se han producido después de las doce de la noche del día anterior. Si varias entradas con distintos valores LSN comparten el valor **tran_end_time** identificado como límite inferior en la tabla [CDC.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md) , la función devolverá el LSN más pequeño asegurándose de que se incluyan todas las entradas. En el límite superior, se usa el operador relacional ' `largest less than or equal to` ' para asegurarse de que el intervalo incluye todas las entradas del día, incluidas las que tienen una medianoche como valor **tran_end_time** . Si varias entradas con distintos valores de LSN comparten el valor **tran_end_time** identificado como límite superior, la función devolverá el LSN más grande asegurándose de que se incluyan todas las entradas.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se usa la `sys.fn_cdc_map_time_lsn` función para determinar si hay alguna fila en la tabla [CDC.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md) con un valor **tran_end_time** mayor o igual que la medianoche. Esta consulta se puede usar para determinar, por ejemplo, si el proceso captura ya ha procesado los cambios confirmados hasta las doce de la noche del día anterior, para que la extracción de los datos cambiados durante ese día pueda llevarse a cabo.  
  
```  
DECLARE @extraction_time datetime, @lsn binary(10);  
SET @extraction_time = '2007-01-01 12:00:00.000';  
SELECT @lsn = sys.fn_cdc_map_time_to_lsn ('smallest greater than or equal', @extraction_time);  
IF @lsn IS NOT NULL  
BEGIN  
<some action>  
END  
```  
  
## <a name="see-also"></a>Consulte también  
 [cdc.lsn_time_mapping &#40;&#41;de Transact-SQL ](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md)   
 [sys.fn_cdc_map_lsn_to_time &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-cdc-map-lsn-to-time-transact-sql.md)   
 [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)   
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)  
  
  
