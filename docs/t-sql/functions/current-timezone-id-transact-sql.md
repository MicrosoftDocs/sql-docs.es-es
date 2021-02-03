---
description: CURRENT_TIMEZONE_ID (Transact-SQL)
title: CURRENT_TIMEZONE_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/18/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CURRENT_TIMEZONE)ID
- CURRENT_TIMEZONE_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- current time zone id [SQL Server]
- current timezoneid [SQL Server]
- system time zone id[SQL Server]
- system timezone id[SQL Server]
- functions [SQL Server], time zone id
- functions [SQL Server], timezoneid
- timezoneid [SQL Server], functions
- time zone id [SQL Server], functions
- CURRENT_TIMEZONE_ID function [SQL Server]
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 70f4fbb5e683aa7bc6436b27a727f2c3952b6592
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184054"
---
# <a name="current_timezone_id-transact-sql"></a>CURRENT_TIMEZONE_ID (Transact-SQL)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

Esta función devuelve el id. de la zona horaria que observa un servidor o una instancia. Para Azure SQL Managed Instance, el valor devuelto se basa en la zona horaria de la propia instancia asignada durante la creación de la instancia, no en la zona horaria del sistema operativo subyacente.
  
> [!NOTE]  
> Para SQL Database, la zona horaria siempre se establece en UTC y `CURRENT_TIMEZONE_ID` devuelve el id. de la zona horaria UTC.
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CURRENT_TIMEZONE_ID ( )  
```
  
## <a name="arguments"></a>Argumentos

Esta función no toma ningún argumento.
  
## <a name="return-type"></a>Tipo de valor devuelto  

**varchar**
  
## <a name="remarks"></a>Observaciones  

`CURRENT_TIMEZONE_ID` es una función no determinista. Las vistas y las expresiones que hacen referencia a esta columna no se pueden indizar.
  
## <a name="example"></a>Ejemplo

El valor devuelto reflejará la configuración real de la zona horaria y el idioma del servidor o de la instancia.

```sql
SELECT CURRENT_TIMEZONE_ID();  
/* Returned:  
W. Europe Standard Time
*/
```  
  
## <a name="see-also"></a>Consulte también

[Zonas horarias de SQL Managed Instance](/azure/sql-database/sql-database-managed-instance-timezone)

[CURRENT_TIMEZONE()](./current-timezone-transact-sql.md)