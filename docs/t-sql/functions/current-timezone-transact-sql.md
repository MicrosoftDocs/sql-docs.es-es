---
description: CURRENT_TIMEZONE (Transact-SQL)
title: CURRENT_TIMEZONE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/28/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CURRENT_TIMEZONE
- CURRENT_TIMEZONE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- current time zone [SQL Server]
- current timezone [SQL Server]
- system time zone [SQL Server]
- system timezone [SQL Server]
- functions [SQL Server], time zone
- functions [SQL Server], timezone
- timezone [SQL Server], functions
- time zone [SQL Server], functions
- CURRENT_TIMEZONE function [SQL Server]
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 84b9c6c51831c911fdd90741839a49a621225bae
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184048"
---
# <a name="current_timezone-transact-sql"></a>CURRENT_TIMEZONE (Transact-SQL)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

Esta función devuelve el nombre de la zona horaria observada por un servidor o una instancia. Para SQL Managed Instance, el valor devuelto se basa en la zona horaria de la propia instancia asignada durante la creación de la instancia, no en la zona horaria del sistema operativo subyacente.
  
> [!NOTE]  
> Para SQL Database, la zona horaria siempre se establece en UTC y `CURRENT_TIMEZONE` devuelve el nombre de la zona horaria UTC.
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CURRENT_TIMEZONE ( )  
```
  
## <a name="arguments"></a>Argumentos

Esta función no toma ningún argumento.
  
## <a name="return-type"></a>Tipo de valor devuelto  

**varchar**
  
## <a name="remarks"></a>Observaciones  

`CURRENT_TIMEZONE` es una función no determinista. Las vistas y las expresiones que hacen referencia a esta columna no se pueden indizar.
  
## <a name="example"></a>Ejemplo

Tenga en cuenta que el valor devuelto reflejará la configuración real de la zona horaria y el idioma del servidor o de la instancia.

```sql
SELECT CURRENT_TIMEZONE();  
/* Returned:  
(UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna 
*/
```  
  
## <a name="see-also"></a>Consulte también

[Zonas horarias de SQL Managed Instance](/azure/sql-database/sql-database-managed-instance-timezone)

[CURRENT_TIMEZONE_ID()](./current-timezone-id-transact-sql.md)