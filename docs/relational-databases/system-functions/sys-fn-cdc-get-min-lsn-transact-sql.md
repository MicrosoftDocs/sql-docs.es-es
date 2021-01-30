---
description: sys.fn_cdc_get_min_lsn (Transact-SQL)
title: sys.fn_cdc_get_min_lsn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_cdc_get_min_lsn
- fn_cdc_get_min_lsn
- fn_cdc_get_min_lsn_TSQL
- sys.fn_cdc_get_min_lsn_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_get_min_lsn
- sys.fn_cdc_get_min_lsn
ms.assetid: bd49e28a-128b-4f6b-8545-6a2ec3f4afb3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f33aa8bfc9f1e2c7160f849bea541dbf70299630
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194435"
---
# <a name="sysfn_cdc_get_min_lsn-transact-sql"></a>sys.fn_cdc_get_min_lsn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve el valor de start_lsn columna de la instancia de captura especificada de la tabla del sistema [CDC.change_tables](../../relational-databases/system-tables/cdc-change-tables-transact-sql.md) . Este valor representa el extremo inferior del intervalo de validez para la instancia de captura.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.fn_cdc_get_min_lsn ( 'capture_instance_name' )  
```  
  
## <a name="arguments"></a>Argumentos  
 **'** *capture_instance_name* **'**  
 Es el nombre de la instancia de captura. *capture_instance_name* es de **tipo sysname**.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **binary(10)**  
  
## <a name="remarks"></a>Observaciones  
 Devuelve 0x00000000000000000000 cuando la instancia de captura no existe o cuando la persona que llama no está autorizada para tener acceso a los datos del cambio asociados con la instancia de captura.  
  
 Esta función se utiliza normalmente para identificar el extremo inferior de la escala de tiempo de captura de los datos del cambio asociada con una instancia de captura. También puede utilizar esta función para validar que los extremos de un intervalo de consultas se encuentran dentro de la escala de tiempo de la instancia de captura antes de solicitar los datos del cambio. Es importante realizar tales comprobaciones porque el extremo inferior de una instancia de captura cambia cuando el proceso de limpieza se realiza en las tablas de cambio. Si el tiempo transcurrido entre las solicitudes para los datos del cambio es significativo, incluso un extremo inferior que está establecido en el extremo alto de la solicitud de datos del cambio anterior podría quedar fuera de la escala de tiempo actual.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol fijo de servidor sysadmin o al rol fijo de base de datos db_owner. Para el resto de usuarios, requiere el permiso SELECT en todas las columnas capturadas en la tabla de origen y, si se ha definido un rol de acceso para la instancia de captura, la pertenencia a ese rol de base de datos.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-the-minimum-lsn-value-for-a-specified-capture-instance"></a>A. Devolver el valor LSN mínimo para una instancia de captura especificada  
 El ejemplo siguiente devuelve el valor LSN mínimo para la instancia de captura `HumanResources_Employee` en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```  
USE AdventureWorks2-12;  
GO  
SELECT sys.fn_cdc_get_min_lsn ('HumanResources_Employee')AS min_lsn;  
  
```  
  
### <a name="b-verifying-the-low-endpoint-of-a-query-range"></a>B. Comprobar el extremo inferior de un rango de consulta  
 El ejemplo siguiente utiliza el valor LSN mínimo devuelto por `sys.fn_cdc_get_min_lsn` para comprobar que el extremo inferior propuesto para una consulta de datos de cambio es válido para la escala de tiempo actual para la instancia de captura el `HumanResources_Employee`. Este ejemplo supone que el LSN del extremo alto anterior para la instancia de captura se guardó y está disponible para establecer la variable `@save_to_lsn`. Para los propósitos de este ejemplo, `@save_to_lsn` está establecido en 0x000000000000000000 para forzar la ejecución de la sección de control de errores.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @min_lsn binary(10), @from_lsn binary(10),@save_to_lsn binary(10), @to_lsn binary(10);  
-- Sets @save_to_lsn to the previous high endpoint saved from the last change data request.  
SET @save_to_lsn = 0x000000000000000000;  
-- Sets the upper endpoint for the query range to the current maximum LSN.  
SET @to_lsn = sys.fn_cdc_get_max_lsn();  
-- Sets the @min_lsn parameter to the current minimum LSN for the capture instance.  
SET @min_lsn = sys.fn_cdc_get_min_lsn ('HumanResources_Employee');  
-- Sets the low endpoint for the query range to the LSN that follows the previous high endpoint.  
SET @from_lsn = sys.fn_cdc_increment_lsn(@save_to_lsn);  
-- Tests to verify the low endpoint is valid for the current capture instance.  
IF (@from_lsn < @min_lsn)  
    BEGIN  
        RAISERROR('Low endpoint of the request interval is invalid.', 16, -1);  
    END  
ELSE  
-- Return the changes occurring within the query range.  
    SELECT * FROM cdc.fn_cdc_get_all_changes_HumanResources_Employee(@from_lsn, @to_lsn, 'all');  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.fn_cdc_get_max_lsn &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-cdc-get-max-lsn-transact-sql.md)   
 [El registro de transacciones &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)  
  
  
