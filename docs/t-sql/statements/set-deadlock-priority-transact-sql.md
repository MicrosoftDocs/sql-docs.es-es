---
description: SET DEADLOCK_PRIORITY (Transact-SQL)
title: SET DEADLOCK_PRIORITY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET DEADLOCK_PRIORITY
- DEADLOCK_PRIORITY_TSQL
- SET_DEADLOCK_PRIORITY_TSQL
- DEADLOCK_PRIORITY
dev_langs:
- TSQL
helpviewer_keywords:
- deadlocks [SQL Server], priority settings
- DEADLOCK_PRIORITY option
- locking [SQL Server], deadlocks
- priority deadlock settings [SQL Server]
- SET DEADLOCK_PRIORITY statement
ms.assetid: 810a3a8e-3da3-4bf9-bb15-7b069685a1b6
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e40536f3e183ac7a4a737eb5f611a6242d92c998
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189530"
---
# <a name="set-deadlock_priority-transact-sql"></a>SET DEADLOCK_PRIORITY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Especifica la importancia relativa de que la sesión actual se siga procesando si existe un interbloqueo con otra sesión.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
SET DEADLOCK_PRIORITY { LOW | NORMAL | HIGH | <numeric-priority> | @deadlock_var | @deadlock_intvar }  
  
<numeric-priority> ::= { -10 | -9 | -8 | ... | 0 | ... | 8 | 9 | 10 }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 LOW  
 Especifica que la sesión actual será el sujeto de interbloqueo si está implicada en un interbloqueo y el resto de las sesiones implicadas en la cadena de interbloqueos tienen la prioridad de interbloqueo establecida en NORMAL o HIGH o en un valor entero mayor que -5. La sesión actual no será la víctima del interbloqueo si las demás sesiones tienen el conjunto de prioridades de interbloqueo establecido en un valor entero menor que -5. También especifica que la sesión actual puede ser víctima de interbloqueo si otra sesión tiene establecido un valor de prioridad de interbloqueo LOW o un valor entero igual a -5.  
  
 NORMAL  
 Especifica que la sesión actual será el sujeto de interbloqueo si otras sesiones implicadas en la cadena de interbloqueos tienen la prioridad de interbloqueo establecida en HIGH o en un valor entero mayor que 0, pero no serán el sujeto de interbloqueo si las demás sesiones tienen la prioridad de interbloqueo establecida en LOW o en un valor entero menor que 0. También especifica que la sesión actual puede ser víctima de interbloqueo si otra sesión tiene establecido un valor de prioridad de interbloqueo NORMAL o un valor entero igual a 0. NORMAL es la prioridad predeterminada.  
  
 HIGH  
 Especifica que la sesión actual será el sujeto de interbloqueo si el resto de las sesiones implicadas en la cadena de interbloqueos tienen la prioridad de interbloqueo establecida en un valor entero superior a 5 o puede ser el sujeto de interbloqueo si otra sesión tiene la prioridad de interbloqueo establecida en HIGH o en un valor entero igual a 5.  
  
 \<numeric-priority>  
 Es un intervalo de valores enteros (-de 10 a 10) para proporcionar 21 niveles de prioridad de interbloqueo. Especifica que la sesión actual será el sujeto de interbloqueo si el resto de las sesiones de la cadena de interbloqueos se ejecutan con un valor de prioridad de interbloqueo superior, pero no será el sujeto de interbloqueo si el resto de las sesiones se ejecutan con un valor de prioridad de interbloqueo inferior al valor de la sesión actual. Además, especifica que la sesión actual puede ser el sujeto de interbloqueo si se ejecuta otra sesión con un valor de prioridad de interbloqueo igual al de la sesión actual. LOW se asigna a -5, NORMAL a 0 y HIGH a 5.  
  
 **@** *deadlock_var*  
 Es una variable de carácter que especifica la prioridad del interbloqueo. La variable se debe establecer en el valor 'LOW', 'NORMAL' o 'HIGH'. La variable debe tener la longitud suficiente para contener la cadena completa.  
  
 **@** *deadlock_intvar*  
 Es una variable de entero que especifica la prioridad del interbloqueo. La variable se debe establecer en un valor entero en el intervalo (de -10 a 10).  
  
## <a name="remarks"></a>Observaciones  
 Los interbloqueos se producen cuando dos sesiones esperan a tener acceso a los recursos bloqueados por la otra sesión. Si una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] detecta que dos sesiones están interbloqueadas, resuelve el interbloqueo mediante la elección de una de las sesiones como el sujeto de interbloqueo. La transacción actual del sujeto se revierte y se devuelve el mensaje de error de interbloqueo 1205 al cliente. De este modo, se desbloquea dicha sesión para que pueda continuar la otra sesión.  
  
 La selección de la sesión como sujeto de interbloqueo depende de la prioridad de interbloqueo de cada sesión:  
  
-   Si ambas sesiones tienen la misma prioridad de interbloqueo, la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] elige la sesión cuya reversión como sujeto de interbloqueo resulta menos costosa. Por ejemplo, si ambas sesiones tienen la prioridad de interbloqueo establecida en HIGH, la instancia elige como sujeto de interbloqueo la sesión cuya reversión considera menos costosa. El costo se determina comparando el número de bytes de registro escritos en ese punto de cada transacción. (Puede ver este valor como "Registro utilizado" en un gráfico de interbloqueo).
  
-   Si las sesiones tienen distintas prioridades de interbloqueo, la sesión con la prioridad de interbloqueo inferior se elige como el sujeto de interbloqueo.  
  
 SET DEADLOCK_PRIORITY se establece en tiempo de ejecución, no en tiempo de análisis.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se utiliza una variable para establecer la prioridad de interbloqueo en `LOW`.  
  
```sql
DECLARE @deadlock_var NCHAR(3);  
SET @deadlock_var = N'LOW';  
  
SET DEADLOCK_PRIORITY @deadlock_var;  
GO  
```  
  
 En el siguiente ejemplo se establece la prioridad de interbloqueo en `NORMAL`.  
  
```sql
SET DEADLOCK_PRIORITY NORMAL;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [@@LOCK_TIMEOUT &#40;Transact-SQL&#41;](../../t-sql/functions/lock-timeout-transact-sql.md)   
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET LOCK_TIMEOUT &#40;Transact-SQL&#41;](../../t-sql/statements/set-lock-timeout-transact-sql.md)  
  
  
