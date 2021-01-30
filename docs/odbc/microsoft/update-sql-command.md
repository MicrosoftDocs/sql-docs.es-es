---
description: Comando de SQL de actualización
title: Comando UPDATE-SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- update [ODBC]
ms.assetid: ff1e0331-c060-4304-b280-039725b45f63
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3a3e38ad98f69faf2ed6288c767d43167314465b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209823"
---
# <a name="update---sql-command"></a>Comando de SQL de actualización
Actualiza los registros de una tabla con nuevos valores.  
  
 El controlador ODBC de Visual FoxPro admite la sintaxis nativa del lenguaje Visual FoxPro para este comando. Para obtener información específica del controlador, consulte los **comentarios del controlador**.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
UPDATE [DatabaseName1!]TableName1  
SET Column_Name1 = eExpression1  
   [, Column_Name2 = eExpression2 ...]  
   WHERE FilterCondition1 [AND | OR FilterCondition2 ...]  
```  
  
## <a name="arguments"></a>Argumentos  
 ACTUALIZAR [ *DatabaseName1!*] *TableName1*  
 Especifica la tabla en la que los registros se actualizan con nuevos valores.  
  
 *DatabaseName1!* especifica el nombre de una base de datos distinta de la especificada con el origen de datos que contiene la tabla. Debe incluir el nombre de la base de datos que contiene la tabla si la base de datos no es la actual. Incluya el delimitador de signo de exclamación (!) después del nombre de la base de datos y antes del nombre de la tabla.  
  
 SET *Column_Name1* =  *eExpression1*[, *Column_Name2* =  *eExpression2*  
 Especifica las columnas que se actualizan y sus nuevos valores. Si se omite la cláusula WHERE, se actualizan todas las filas de la columna con el mismo valor.  
  
 WHERE *FilterCondition1*[and &#124; or *FilterCondition2*...]  
 Especifica los registros que se actualizan con los nuevos valores.  
  
 *FilterCondition* especifica los criterios que los registros deben cumplir para que se actualicen con nuevos valores. Puede incluir tantas condiciones de filtro como desee, conectarlas con el operador AND u OR. También puede usar el operador NOT para invertir el valor de una expresión lógica, o puede usar **Empty**() para comprobar si hay un campo vacío.  
  
## <a name="remarks"></a>Observaciones  
 UPDATE-SQL solo puede actualizar registros en una sola tabla.  
  
 A diferencia de Replace, UPDATE-SQL usa el bloqueo de registros al actualizar varios registros en las tablas abiertas para el acceso compartido. Esto reduce la contención de registros en situaciones multiusuario, pero puede reducir el rendimiento. Para obtener el máximo rendimiento, abra la tabla para uso exclusivo o utilice el **rebaño**() para bloquear la tabla.  
  
## <a name="driver-remarks"></a>Notas del controlador  
 Cuando la aplicación envía la actualización de la instrucción SQL ODBC al origen de datos, el controlador ODBC de Visual FoxPro convierte el comando en el comando visual FoxProUPDATE sin conversión.  
  
## <a name="see-also"></a>Consulte también  
 [DELETE-SQL (comando)](../../odbc/microsoft/delete-sql-command.md)   
 [Insertar: comando SQL](../../odbc/microsoft/insert-sql-command.md)
