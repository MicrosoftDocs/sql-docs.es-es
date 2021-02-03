---
description: '&#x40;&#x40;IDENTITY (Transact-SQL)'
title: '@@IDENTITY (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 08/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@IDENTITY_TSQL'
- '@@IDENTITY'
dev_langs:
- TSQL
helpviewer_keywords:
- last-inserted identity values
- identity values [SQL Server], last-inserted
- '@@IDENTITY function'
ms.assetid: 912e4485-683c-41c2-97b3-8831c0289ee4
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: a9366f4b63cace1787d2bdb777c9765830b87843
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211458"
---
# <a name="x40x40identity-transact-sql"></a>&#x40;&#x40;IDENTITY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Se trata de una función del sistema que devuelve el último valor de identidad insertado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
@@IDENTITY  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 **numeric(38,0)**  
  
## <a name="remarks"></a>Observaciones  
 Cuando se completa una instrucción INSERT, SELECT INTO o de copia masiva, @@IDENTITY contiene el último valor de identidad generado por la instrucción. Si la instrucción no afectó a ninguna tabla con columnas de identidad, @@IDENTITY devuelve NULL. Si se insertan varias filas, lo que genera varios valores de identidad, @@IDENTITY devuelve el último valor de identidad generado. Si la instrucción activa uno o más desencadenadores que realizan inserciones que, a su vez, generan valores de identidad, al llamar a @@IDENTITY inmediatamente después de la instrucción se obtiene el último valor de identidad generado por los desencadenadores. Si un desencadenador se activa tras una acción de inserción en una tabla que tiene una columna de identidad y se inserta en otra tabla que no tiene una columna de identidad, @@IDENTITY devuelve el valor de identidad de la primera inserción. Si se produce un error en la instrucción INSERT o SELECT INTO o en la copia masiva o se revierte la transacción, el valor de @@IDENTITY no revierte a un valor anterior.  
  
 Las instrucciones y transacciones con errores pueden cambiar la identidad actual de una tabla y crear huecos en los valores de columna de identidad. El valor de identidad jamás se revierte, aun cuando no se haya confirmado la transacción que intentó insertar el valor en la tabla. Por ejemplo, si se produce un error en una instrucción INSERT debido a una infracción de tipo IGNORE_DUP_KEY, el valor de identidad actual de la tabla se sigue incrementando.  
  
 Las funciones @@IDENTITY, SCOPE_IDENTITY e IDENT_CURRENT se parecen, porque devuelven el último valor insertado en la columna IDENTITY de una tabla.  
  
 @@IDENTITY y SCOPE_IDENTITY devuelven el último valor de identidad generado en una tabla en la sesión actual. SCOPE_IDENTITY solo devuelve el valor en el ámbito actual y @@IDENTITY no se limita a un ámbito específico.  
  
 IDENT_CURRENT no está limitado por el ámbito y la sesión; se limita a una tabla especificada. IDENT_CURRENT devuelve el valor de identidad generado para una tabla específica en cualquier sesión y cualquier ámbito. Para obtener más información, vea [IDENT_CURRENT &#40;Transact-SQL&#41;](../../t-sql/functions/ident-current-transact-sql.md).  
  
 El ámbito de la función @@IDENTITY es la sesión actual en el servidor local en el que se ejecuta. Esta función no se puede aplicar a servidores remotos o vinculados. Para obtener un valor de identidad de un servidor diferente, ejecute un procedimiento almacenado en ese servidor remoto o vinculado y haga que dicho procedimiento (que se está ejecutando en el contexto del servidor remoto o vinculado) recopile el valor de identidad y lo devuelva a la conexión que llama del servidor local.  
  
 La replicación puede afectar al valor @@IDENTITY, ya que se usa en los desencadenadores de replicación y en los procedimientos almacenados. @@IDENTITY no es un indicador confiable de la identidad más reciente creada por el usuario si la columna forma parte de un artículo de replicación. Puede usar la sintaxis de función SCOPE_IDENTITY() en lugar de @@IDENTITY. Para más información, vea [SCOPE_IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/scope-identity-transact-sql.md)  
  
> [!NOTE]  
>  Es necesario reescribir la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] o el procedimiento almacenado que realiza la llamada para que usen la función `SCOPE_IDENTITY()`, lo que devuelve la última identidad usada en el ámbito de esa instrucción de usuario en lugar de la identidad en el ámbito del desencadenador anidado usado por la replicación.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se inserta una fila en una tabla con una columna de identidad (`LocationID`) y se utiliza `@@IDENTITY` para mostrar el valor de identidad empleado en la nueva fila.  
  
```sql  
USE AdventureWorks2012;  
GO  
--Display the value of LocationID in the last row in the table.  
SELECT MAX(LocationID) FROM Production.Location;  
GO  
INSERT INTO Production.Location (Name, CostRate, Availability, ModifiedDate)  
VALUES ('Damaged Goods', 5, 2.5, GETDATE());  
GO  
SELECT @@IDENTITY AS 'Identity';  
GO  
--Display the value of LocationID of the newly inserted row.  
SELECT MAX(LocationID) FROM Production.Location;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [IDENT_CURRENT &#40;Transact-SQL&#41;](../../t-sql/functions/ident-current-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [SCOPE_IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/scope-identity-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)  
  
  
