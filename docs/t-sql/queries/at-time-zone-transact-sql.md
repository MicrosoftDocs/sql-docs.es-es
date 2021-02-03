---
description: AT TIME ZONE (Transact-SQL)
title: AT TIME ZONE (Transact-SQL)
ms.date: 06/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.custom: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- AT TIME ZONE
- AT_TIME_ZONE_TSQL
helpviewer_keywords:
- AT TIME ZONE function
ms.assetid: 311f682f-7f1b-43b6-9ea0-24e36b64f73a
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current||=azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: 391e045d078b2735411c51db73948e416f9e4ea5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159692"
---
# <a name="at-time-zone-transact-sql"></a>AT TIME ZONE (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

Convierte un valor *inputdate* en el valor *datetimeoffset* correspondiente en la zona horaria de destino. Si *inputdate* se proporciona sin información de desplazamiento, la función aplica el desplazamiento de la zona horaria, suponiendo que el valor *inputdate* se proporcione en la zona horaria de destino. Si *inputdate* se proporciona como un valor *datetimeoffset*, la cláusula **AT TIME ZONE** lo convierte en la zona horaria de destino mediante las reglas de conversión de zona horaria.  

La implementación de **AT TIME ZONE** se basa en un mecanismo de Windows para convertir valores **datetime** entre zonas horarias.  

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md) 

## <a name="syntax"></a>Sintaxis

```syntaxsql
inputdate AT TIME ZONE timezone  
```

## <a name="arguments"></a>Argumentos

*inputdate*  
Es una expresión que se puede resolver en un valor **smalldatetime**, **datetime**, **datetime2** o **datetimeoffset**.  

*timezone* Nombre de la zona horaria de destino. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se basa en las zonas horarias almacenadas en el Registro de Windows. Las zonas horarias instaladas en el equipo se almacenan en el siguiente subárbol del Registro: **KEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Time Zones**. También se puede exponer una lista de las zonas horarias instaladas a través de la vista [sys.time_zone_info &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-time-zone-info-transact-sql.md).  

## <a name="return-types"></a>Tipos de valor devuelto

Devuelve el tipo de datos de **datetimeoffset**.

## <a name="return-value"></a>Valor devuelto

Valor **datetimeoffset** de la zona horaria de destino.
  
## <a name="remarks"></a>Observaciones

**AT TIME ZONE** aplica reglas específicas para la conversión de valores de entrada en tipos de datos **smalldatetime**, **datetime** y **datetime2**, que se encuentran en un intervalo afectado por el cambio del horario de verano:

- Cuando los relojes están adelantados, hay un desfase en la hora local igual a la duración del ajuste de reloj. La duración suele ser de 1 hora, pero también puede ser de 30 o 45 minutos, según la zona horaria. Los puntos en tiempo que están en este desfase se convierten con el desplazamiento *después* del cambio del horario de verano.  

    ```sql
    /*  
        Moving to DST in "Central European Standard Time" zone: 
        offset changes from +01:00 -> +02:00   
        Change occurred on March 29th, 2015 at 02:00:00.   
        Adjusted local time became 2015-03-29 03:00:00.  
    */  

    --Time before DST change has standard time offset (+01:00)
    SELECT CONVERT(DATETIME2(0), '2015-03-29T01:01:00', 126)     
    AT TIME ZONE 'Central European Standard Time';  
    --Result: 2015-03-29 01:01:00 +01:00   
  
    /*
      Adjusted time from the "gap interval" (between 02:00 and 03:00)
      is moved 1 hour ahead and presented with the summer time offset
      (after the DST change) 
    */
    SELECT CONVERT(DATETIME2(0), '2015-03-29T02:01:00', 126)   
    AT TIME ZONE 'Central European Standard Time';  
    --Result: 2015-03-29 03:01:00 +02:00

    --Time after 03:00 is presented with the summer time offset (+02:00)
    SELECT CONVERT(DATETIME2(0), '2015-03-29T03:01:00', 126)   
    AT TIME ZONE 'Central European Standard Time';  
    --Result: 2015-03-29 03:01:00 +02:00  
  
    ```

- Cuando el reloj se vuelve a configurar, dos horas de la hora local se superponen en una hora.  En ese caso, los momentos que coinciden con el intervalo superpuesto se presentan con el desplazamiento *antes* del cambio del reloj:  
  
    ```sql
    /*  
        Moving back from DST to standard time in
        "Central European Standard Time" zone:
        offset changes from +02:00 -> +01:00.
        Change occurred on October 25th, 2015 at 03:00:00.
        Adjusted local time became 2015-10-25 02:00:00
    */  

    --Time before the change has DST offset (+02:00)
    SELECT CONVERT(DATETIME2(0), '2015-10-25T01:01:00', 126)
    AT TIME ZONE 'Central European Standard Time';  
    --Result: 2015-10-25 01:01:00 +02:00  

    /*
      Time from the "overlapped interval" is presented with standard time 
      offset (before the change)
    */
    SELECT CONVERT(DATETIME2(0), '2015-10-25T02:00:00', 126)
    AT TIME ZONE 'Central European Standard Time';  
    --Result: 2015-10-25 02:00:00 +02:00  


    --Time after 03:00 is regularly presented with the standard time offset (+01:00)
    SELECT CONVERT(DATETIME2(0), '2015-10-25T03:01:00', 126)
    AT TIME ZONE 'Central European Standard Time';
    --Result: 2015-10-25 03:01:00 +01:00
  
    ```

Puesto que parte de la información (por ejemplo, las reglas de zona horaria) se mantiene fuera de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] y está sujeta a cambios ocasionales, la función **AT TIME ZONE** se clasifica como no determinista. 

## <a name="examples"></a>Ejemplos

### <a name="a-add-target-time-zone-offset-to-datetime-without-offset-information"></a>A. Agregar un desplazamiento de zona horaria de destino a una fecha y hora sin información de desplazamiento  

Use **AT TIME ZONE** para agregar un desplazamiento basado en reglas de zona horaria cuando sepa que los valores **datetime** originales se proporcionan en la misma zona horaria:  

```sql
USE AdventureWorks2016;
GO  
  
SELECT SalesOrderID, OrderDate,
    OrderDate AT TIME ZONE 'Pacific Standard Time' AS OrderDate_TimeZonePST  
FROM Sales.SalesOrderHeader;
```

### <a name="b-convert-values-between-different-time-zones"></a>B. Convertir valores entre zonas horarias diferentes  

En el ejemplo siguiente se convierten valores entre zonas horarias diferentes:  

```sql
USE AdventureWorks2016;
GO

SELECT SalesOrderID, OrderDate,
    OrderDate AT TIME ZONE 'Pacific Standard Time' AS OrderDate_TimeZonePST,
    OrderDate AT TIME ZONE 'Central European Standard Time' AS OrderDate_TimeZoneCET
FROM Sales.SalesOrderHeader;
```

### <a name="c-query-temporal-tables-using-a-specific-time-zone"></a>C. Consultar tablas temporales mediante una zona horaria concreta

En el ejemplo siguiente se seleccionan datos de una tabla temporal con la hora estándar del Pacífico.  

```sql
USE AdventureWorks2016;
GO

DECLARE @ASOF datetimeoffset;  
SET @ASOF = DATEADD (month, -1, GETDATE()) AT TIME ZONE 'UTC';

-- Query state of the table a month ago projecting period
-- columns as Pacific Standard Time
SELECT BusinessEntityID, PersonType, NameStyle, Title,
    FirstName, MiddleName,
    ValidFrom AT TIME ZONE 'Pacific Standard Time'
FROM  Person.Person_Temporal
FOR SYSTEM_TIME AS OF @ASOF;
```

## <a name="next-steps"></a>Pasos siguientes

- [Tipos de fecha y hora](../../t-sql/data-types/date-and-time-types.md)
- [Tipos de datos y funciones de fecha y hora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)
