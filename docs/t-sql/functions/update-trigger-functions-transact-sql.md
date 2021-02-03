---
description: 'UPDATE: funciones de desencadenador (Transact-SQL)'
title: UPDATE() (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- UPDATE()_TSQL
- UPDATE()
dev_langs:
- TSQL
helpviewer_keywords:
- INSERT statement [SQL Server], UPDATE function
- testing column updates
- UPDATE function
- UPDATE() function
- detecting changes
- columns [SQL Server], change detection
- UPDATE statement [SQL Server], UPDATE function
- verifying column updates
- checking column updates
ms.assetid: 8e3be25b-2e3b-4d1f-a610-dcbbd8d72084
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: e1f7ebd214fc4cef8a9c9ba68355451cc34f6e8b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210637"
---
# <a name="update---trigger-functions-transact-sql"></a>UPDATE: funciones de desencadenador (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve un valor booleano que indica si se intentó utilizar INSERT o UPDATE en una columna especificada de una tabla o vista. UPDATE() se utiliza en cualquier lugar del cuerpo de un desencadenador INSERT o UPDATE de [!INCLUDE[tsql](../../includes/tsql-md.md)] para probar si el desencadenador debe ejecutar ciertas acciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
UPDATE ( column )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *column*  
 Es el nombre de la columna que se va a probar para una acción INSERT o UPDATE. Debido a que el nombre de la tabla se especifica en la cláusula ON del desencadenador, no lo incluya antes del nombre de la columna. Esta columna puede ser de cualquier [tipo de datos](../../t-sql/data-types/data-types-transact-sql.md) admitido por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. No obstante, no se pueden utilizar columnas calculadas en este contexto.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Boolean  
  
## <a name="remarks"></a>Comentarios  
 UPDATE() devuelve TRUE independientemente de si un intento de INSERT o UPDATE tiene éxito.  
  
 Para probar una acción INSERT o UPDATE en más de una columna, especifique una cláusula UPDATE(*column*) distinta a continuación de la primera. También puede probar acciones INSERT o UPDATE en varias columnas con COLUMNS_UPDATED, que devuelve un patrón de bits que indica las columnas que se insertaron o se actualizaron.  
  
 IF UPDATE devuelve el valor TRUE en las acciones INSERT porque en las columnas se insertaron valores explícitos o implícitos (NULL).  
  
> [!NOTE]  
>  La cláusula IF UPDATE(*columna*) funciona de forma idéntica a una instrucción IF, IF…ELSE o WHILE, y puede usar el bloque BEGIN…END. Para más información, vea [Lenguaje de control de flujo &#40;Transact-SQL&#41;](~/t-sql/language-elements/control-of-flow.md).  
  
 UPDATE(*column*) se puede usar en cualquier lugar del cuerpo de un desencadenador [!INCLUDE[tsql](../../includes/tsql-md.md)].  
 
Si un desencadenador se aplica a una columna, el valor `UPDATED` se devolverá como `true` o `1`, incluso si el valor de columna permanece sin cambios. Esto es así por diseño y el desencadenador debe implementar la lógica de negocios que determina si la operación de inserción, actualización o eliminación está permitida o no. 
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se crea un desencadenador que imprime un mensaje para el cliente si alguien intenta actualizar las columnas `StateProvinceID` o `PostalCode` de la tabla `Address`.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT name FROM sys.objects  
      WHERE name = 'reminder' AND type = 'TR')  
   DROP TRIGGER Person.reminder;  
GO  
CREATE TRIGGER reminder  
ON Person.Address  
AFTER UPDATE   
AS   
IF ( UPDATE (StateProvinceID) OR UPDATE (PostalCode) )  
BEGIN  
RAISERROR (50009, 16, 10)  
END;  
GO  
-- Test the trigger.  
UPDATE Person.Address  
SET PostalCode = 99999  
WHERE PostalCode = '12345';  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [COLUMNS_UPDATED &#40;Transact-SQL&#41;](../../t-sql/functions/columns-updated-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)  
  
  
