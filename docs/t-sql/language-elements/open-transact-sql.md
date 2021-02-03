---
description: OPEN (Transact-SQL)
title: OPEN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPEN_TSQL
- OPEN
dev_langs:
- TSQL
helpviewer_keywords:
- opening cursors
- cursors [SQL Server], opening
- populating cursors [SQL Server]
- OPEN statement
- Transact-SQL cursors, opening
ms.assetid: fd1c5e3b-6045-4a42-b646-3fca76e874c1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 23d3a4c306d61f2fdc7f71102a3a786dd9856941
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191924"
---
# <a name="open-transact-sql"></a>OPEN (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Abre un cursor de servidor de [!INCLUDE[tsql](../../includes/tsql-md.md)] y lo rellena mediante la ejecución de la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] especificada en la instrucción DECLARE CURSOR o SET *cursor_variable*.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
OPEN { { [ GLOBAL ] cursor_name } | cursor_variable_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 GLOBAL  
 Especifica que *cursor_name* hace referencia a un cursor global.  
  
 *cursor_name*  
 Es el nombre de un cursor declarado. Si existen un cursor global y otro local denominados *cursor_name*, *cursor_name* hace referencia al cursor global si se especifica GLOBAL; en caso contrario, *cursor_name* hace referencia al cursor local.  
  
 *cursor_variable_name*  
 Es el nombre de la variable cursor que hace referencia a un cursor.  
  
## <a name="remarks"></a>Observaciones  
 Si se declara el cursor con la opción INSENSITIVE o STATIC, OPEN crea una tabla temporal para mantener el conjunto de resultados. OPEN produce un error si el tamaño de cualquier fila en el conjunto de resultados excede el tamaño máximo de fila para las tablas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si el cursor se declara con la opción KEYSET, OPEN crea una tabla temporal para mantener el conjunto de claves. Las tablas temporales se almacenan en tempdb.  
  
 Después de abrir un cursor, use la función @@CURSOR_ROWS para recuperar el número de filas habilitadas en el último cursor abierto.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no es compatible con la generación asincrónica de cursores de [!INCLUDE[tsql](../../includes/tsql-md.md)] estáticos o controlados por conjuntos de claves. [!INCLUDE[tsql](../../includes/tsql-md.md)] como OPEN o FETCH se procesan por lotes, por lo que la generación asincrónica de cursores de [!INCLUDE[tsql](../../includes/tsql-md.md)] no es necesaria. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sigue admitiendo los cursores de servidor de la interfaz de programación de aplicaciones (API) estáticos o controlados por conjuntos de claves asincrónicos en los que OPEN de baja latencia es un problema, debido a los recorridos de ida y vuelta al cliente para cada operación del cursor.  
  
## <a name="examples"></a>Ejemplos  
 Este ejemplo abre un cursor y busca todas sus filas.  
  
```sql  
DECLARE Employee_Cursor CURSOR FOR  
SELECT LastName, FirstName  
FROM AdventureWorks2012.HumanResources.vEmployee  
WHERE LastName like 'B%';  
  
OPEN Employee_Cursor;  
  
FETCH NEXT FROM Employee_Cursor;  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    FETCH NEXT FROM Employee_Cursor  
END;  
  
CLOSE Employee_Cursor;  
DEALLOCATE Employee_Cursor;  
```  
  
## <a name="see-also"></a>Vea también  
 [CLOSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/close-transact-sql.md)   
 [@@CURSOR_ROWS &#40;Transact-SQL&#41;](../../t-sql/functions/cursor-rows-transact-sql.md)   
 [DEALLOCATE &#40;Transact-SQL&#41;](../../t-sql/language-elements/deallocate-transact-sql.md)   
 [DECLARE CURSOR &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-cursor-transact-sql.md)   
 [FETCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/fetch-transact-sql.md)  
  
  
