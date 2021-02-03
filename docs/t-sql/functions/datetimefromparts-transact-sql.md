---
description: DATETIMEFROMPARTS (Transact-SQL)
title: DATETIMEFROMPARTS (Transact-SQL)
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATETIMEFROMPARTS_TSQL
- DATETIMEFROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATETIMEFROMPARTS function
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8cc7cb964f1cd8b56cbb06aa5bccc97aef76a7b4
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237905"
---
# <a name="datetimefromparts-transact-sql"></a>DATETIMEFROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Esta función devuelve un valor **datetime** para los argumentos de fecha y hora especificados.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DATETIMEFROMPARTS ( year, month, day, hour, minute, seconds, milliseconds )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*year*  
Expresión entera que especifica un año.
  
*month*  
Expresión entera que especifica un mes.
  
*day*  
Expresión entera que especifica un día.
  
*hour*  
Expresión entera que especifica las horas.
  
*minute*  
Expresión entera que especifica los minutos.
  
*segundos*  
Expresión entera que especifica los segundos.
  
*milliseconds*  
Expresión entera que especifica los milisegundos.
  
## <a name="return-types"></a>Tipos de valores devueltos
**datetime**
  
## <a name="remarks"></a>Observaciones  
`DATETIMEFROMPARTS` devuelve un valor **datetime** totalmente inicializado. `DATETIMEFROMPARTS` producirá un error si al menos uno de los argumentos obligatorios tiene un valor no válido. `DATETIMEFROMPARTS` devuelve NULL si al menos uno de los argumentos obligatorios tiene un valor NULL.
  
Esta función se puede enviar de forma remota a servidores de [!INCLUDE[sssql11-md](../../includes/sssql11-md.md)] y superiores. No se puede enviar de forma remota a servidores que tengan una versión inferior a [!INCLUDE[sssql11-md](../../includes/sssql11-md.md)].  
  
## <a name="examples"></a>Ejemplos  
  
```sql
SELECT DATETIMEFROMPARTS ( 2010, 12, 31, 23, 59, 59, 0 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
---------------------------  
2010-12-31 23:59:59.000  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vea también
[datetime &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime-transact-sql.md)
  
  

