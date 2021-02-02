---
description: SYSUTCDATETIME (Transact-SQL)
title: SYSUTCDATETIME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SYSUTCDATETIME
- SYSUTCDATETIME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- system time [SQL Server]
- functions [SQL Server], date and time
- time [SQL Server], functions
- date and time [SQL Server], SYSUTCDATETIME
- SYSUTCDATETIME function [SQL Server]
- time [SQL Server], system
ms.assetid: f14fc2cd-9ea8-4daf-88f4-418cf523ab55
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fce96cdf1b28d72a7be47cbdb4dd8293431589a9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99101164"
---
# <a name="sysutcdatetime-transact-sql"></a>SYSUTCDATETIME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve un valor **datetime2** que contiene la fecha y hora del equipo en el que la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se está ejecutando. La fecha y hora se devuelven como una hora universal coordinada (UTC). La especificación de precisión de fracción de segundo tiene un intervalo de 1 a 7 dígitos. La precisión predeterminada es 7 dígitos.  
  
> [!NOTE]  
>  SYSDATETIME y SYSUTCDATETIME tienen más precisión de fracciones de segundo que GETDATE y GETUTCDATE. SYSDATETIMEOFFSET incluye el ajuste de zona horaria del sistema. SYSDATETIME, SYSUTCDATETIME y SYSDATETIMEOFFSET pueden asignarse a una variable de cualquier tipo de fecha y hora.  
  
 Para ver información de todos los tipos de datos y funciones de fecha y hora de [!INCLUDE[tsql](../../includes/tsql-md.md)], vea [Funciones y tipos de datos de fecha y hora](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
SYSUTCDATETIME ( )  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-type"></a>Tipo de valor devuelto  
 **datetime2**  
  
## <a name="remarks"></a>Comentarios  
 Las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] pueden hacer referencia a SYSUTCDATETIME en cualquier punto en el que puedan hacer referencia a una expresión **datetime2**.  
  
 SYSUTCDATETIME es una función no determinista. Las vistas y las expresiones que hacen referencia a esta función en una columna no se pueden indizar.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] obtiene los valores de fecha y hora usando la API de Windows GetSystemTimeAsFileTime(). La exactitud depende del hardware del equipo y de la versión de Windows en las que la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se esté ejecutando. La precisión de esta API se fija en 100 nanosegundos. La precisión se puede determinar por medio de la API de Windows GetSystemTimeAdjustment().  
  
## <a name="examples"></a>Ejemplos  
 En los ejemplos siguientes se usan las seis funciones del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que devuelven la fecha y hora actuales para devolver la fecha, la hora o ambas. Los valores se devuelven en serie; por consiguiente, sus fracciones de segundo podrían ser diferentes.  
  
### <a name="a-showing-the-formats-that-are-returned-by-the-date-and-time-functions"></a>A. Mostrar los formatos que devuelven las funciones de fecha y hora  
 En el ejemplo siguiente se muestran los distintos formatos que devuelven las funciones de fecha y hora.  
  
```sql
SELECT SYSDATETIME() AS [SYSDATETIME()]  
    ,SYSDATETIMEOFFSET() AS [SYSDATETIMEOFFSET()]  
    ,SYSUTCDATETIME() AS [SYSUTCDATETIME()]  
    ,CURRENT_TIMESTAMP AS [CURRENT_TIMESTAMP]  
    ,GETDATE() AS [GETDATE()]  
    ,GETUTCDATE() AS [GETUTCDATE()];  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
SYSDATETIME()      2007-04-30 13:10:02.0474381
SYSDATETIMEOFFSET()2007-04-30 13:10:02.0474381 -07:00
SYSUTCDATETIME()   2007-04-30 20:10:02.0474381
CURRENT_TIMESTAMP  2007-04-30 13:10:02.047
GETDATE()          2007-04-30 13:10:02.047
GETUTCDATE()       2007-04-30 20:10:02.047
```  
  
### <a name="b-converting-date-and-time-to-date"></a>B. Convertir valores de fecha y hora en date  
 En el ejemplo siguiente se muestra cómo convertir valores de fecha y hora en `date`.  
  
```sql
SELECT CONVERT (date, SYSDATETIME())  
    ,CONVERT (date, SYSDATETIMEOFFSET())  
    ,CONVERT (date, SYSUTCDATETIME())  
    ,CONVERT (date, CURRENT_TIMESTAMP)  
    ,CONVERT (date, GETDATE())  
    ,CONVERT (date, GETUTCDATE());  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
2007-04-30
2007-04-30
2007-04-30
2007-04-30
2007-04-30
2007-04-30
```  
  
### <a name="c-converting-date-and-time-values-to-time"></a>C. Convertir valores de fecha y hora en time  
 En el ejemplo siguiente se muestra cómo convertir valores de fecha y hora en `time`.  
  
 ```sql
DECLARE @DATETIME DATETIME = GetDate();
DECLARE @TIME TIME
SELECT @TIME = CONVERT(time, @DATETIME)
SELECT @TIME AS 'Time', @DATETIME AS 'Date Time'
```
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Time             Date Time  
13:49:33.6330000 2009-04-22 13:49:33.633
```  
  
## <a name="see-also"></a>Vea también  
 [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Tipos de datos y funciones de fecha y hora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)   
 [AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)  
  
  


