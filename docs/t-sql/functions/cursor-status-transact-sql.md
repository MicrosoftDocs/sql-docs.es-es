---
description: CURSOR_STATUS (Transact-SQL)
title: CURSOR_STATUS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CURSOR_STATUS
- CURSOR_STATUS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- status information [SQL Server], cursors
- CURSOR_STATUS function
- cursors [SQL Server], status information
ms.assetid: 3a4a840e-04f8-43bd-aada-35d78c3cb6b0
author: cawrites
ms.author: chadam
ms.openlocfilehash: 416e368e9aa33e0ff3b5073bb169576b22938e6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184072"
---
# <a name="cursor_status-transact-sql"></a>CURSOR_STATUS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Para un parámetro en concreto, `CURSOR_STATUS` muestra si una declaración de cursor ha devuelto un cursor y un conjunto de resultados.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CURSOR_STATUS   
     (  
          { 'local' , 'cursor_name' }   
          | { 'global' , 'cursor_name' }   
          | { 'variable' , 'cursor_variable' }   
     )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
'local'  
Especifica una constante que indica que el origen del cursor es un nombre de cursor local.
  
'*cursor_name*'  
El nombre del cursor. Un nombre de cursor debe ajustarse a las [reglas de los identificadores de base de datos](../../relational-databases/databases/database-identifiers.md).
  
'global'  
Especifica una constante que indica que el origen del cursor es un nombre de cursor global.
  
'variable'  
Especifica una constante que indica que el origen del cursor es una variable local.
  
'*cursor_variable*'  
El nombre de la variable de cursor. Una variable de cursor debe definirse mediante el tipo de datos **cursor**.
  
## <a name="return-types"></a>Tipos de valores devueltos
**smallint**
  
|Valor devuelto|Nombre de cursor|Variable de cursor|  
|---|---|---|
|1|El conjunto de resultados del cursor tiene al menos una fila.<br /><br /> Para los cursores INSENSITIVE y de conjunto de claves, el conjunto de resultados tiene al menos una fila.<br /><br /> Para los cursores dinámicos, el conjunto de resultados puede tener cero, una o más filas.|El cursor asignado a esta variable está abierto.<br /><br /> Para los cursores INSENSITIVE y de conjunto de claves, el conjunto de resultados tiene al menos una fila.<br /><br /> Para los cursores dinámicos, el conjunto de resultados puede tener cero, una o más filas.|  
|0|El conjunto de resultados del cursor está vacío.*|El cursor asignado a esta variable está abierto, pero el conjunto de resultados está definitivamente vacío.*|  
|-1|El cursor está cerrado.|El cursor asignado a esta variable está cerrado.|  
|-2|No aplicable.|Tiene una de estas posibilidades:<br /><br /> El procedimiento llamado anteriormente no asignó ningún cursor a esta variable OUTPUT.<br /><br /> El procedimiento asignado anteriormente asignó un cursor a esta variable OUTPUT, pero el cursor se encontraba en un estado cerrado al concluir el procedimiento. Por tanto, se cancela la asignación del cursor y no se devuelve al procedimiento que hace la llamada.<br /><br /> No se asigna ningún cursor a la variable declarada de cursor.|  
|-3|No existe ningún cursor con el nombre indicado.|No existe ninguna variable de cursor con el nombre indicado o, si existe, aún no tiene ningún cursor asignado.|  
  
*Los cursores dinámicos no devuelven nunca este resultado.
  
## <a name="examples"></a>Ejemplos  
Este ejemplo usa la función `CURSOR_STATUS` para mostrar el estado de un cursor después de su declaración, una vez abierto y una vez cerrado.
  
```sql
CREATE TABLE #TMP  
(  
   ii INT  
)  
GO  
  
INSERT INTO #TMP(ii) VALUES(1)  
INSERT INTO #TMP(ii) VALUES(2)  
INSERT INTO #TMP(ii) VALUES(3)  
  
GO  
  
--Create a cursor.  
DECLARE cur CURSOR  
FOR SELECT * FROM #TMP  
  
--Display the status of the cursor before and after opening  
--closing the cursor.  
  
SELECT CURSOR_STATUS('global','cur') AS 'After declare'  
OPEN cur  
SELECT CURSOR_STATUS('global','cur') AS 'After Open'  
CLOSE cur  
SELECT CURSOR_STATUS('global','cur') AS 'After Close'  
  
--Remove the cursor.  
DEALLOCATE cur  
  
--Drop the table.  
DROP TABLE #TMP  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
After declare
---------------
-1  
  
After Open
----------
1  
  
After Close
-----------
-1
```  
  
## <a name="see-also"></a>Vea también
[Funciones del cursor &#40;Transact-SQL&#41;](../../t-sql/functions/cursor-functions-transact-sql.md)  
[Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
  
  
