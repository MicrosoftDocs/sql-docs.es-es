---
description: sys.sp_flush_log (Transact-SQL)
title: sys.sp_flush_log (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_flush_log_TSQL
- sys.sp_flush_log
- sys.sp_flush_log_TSQL
- sp_flush_log
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_flush_log
ms.assetid: 75cc9f52-3b1f-4754-b1e7-ce0dd3323bc9
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: a165695005300627db18f28b41b944c97f5fe61f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159699"
---
# <a name="syssp_flush_log-transact-sql"></a>sys.sp_flush_log (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Vuelca en el disco el registro de transacciones de la base de datos actual, de forma que protege todas las transacciones durables diferidas que se hayan confirmado previamente.  
  
 Si elige utilizar la durabilidad diferida por sus ventajas en el rendimiento, pero también desea tener un límite garantizado de la cantidad de datos que se pueden perder en el bloqueo o conmutación por error de un servidor, ejecute `sys.sp_flush_log` periódicamente. Por ejemplo, si desea asegurarse de que no pierde más de x segundos de datos, se ejecutaría `sp_flush_log` cada x segundos.  
  
 Si se ejecuta `sys.sp_flush_log`, se garantiza que todas las transacciones durables diferidas que se hayan confirmado previamente se convierten en durables. Vea el tema conceptual [control](../../relational-databases/logs/control-transaction-durability.md) de la durabilidad de las transacciones para obtener más información.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
  
sys.sp_flush_log  
  
```  
  
#### <a name="parameters"></a>Parámetros  
 Ninguno.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 Un código de retorno con valor 1 indica un procedimiento correcto.  Cualquier otro valor indica error.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Ninguno.  
  
## <a name="sample-code"></a>Código de ejemplo  
  
```sql  
.  
EXECUTE sys.sp_flush_log  
  
```  
  
  
