---
description: Barra diagonal y asterisco (comentario de bloque) (Transact-SQL)
title: Barra diagonal y asterisco (comentario de bloque) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- /*...*/_TSQL
- Comment
- /*...*/
dev_langs:
- TSQL
helpviewer_keywords:
- nonexecuting text strings [SQL Server]
- /*...*/ (comment)
- remarks [SQL Server]
- comments [SQL Server]
ms.assetid: 4d9ab1b2-4bbb-4c16-beb1-cafc1af7417c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 28104f9417d37d8ca1a3c561738dfde34e504749
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184647"
---
# <a name="slash-star-block-comment-transact-sql"></a>Barra diagonal y asterisco (comentario de bloque) (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]


  Indica texto proporcionado por el usuario. El servidor no evalúa el texto situado entre /* y \*/.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
/*  
text_of_comment  
*/  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *text_of_comment*  
 Es el texto del comentario. Es una o más cadenas de caracteres.  
  
## <a name="remarks"></a>Comentarios  
 Los comentarios se pueden insertar en una línea aparte o dentro de una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)]. Los comentarios con varias líneas deben indicarse con /* y \*/. Una regla de estilo que se usa a menudo para los comentarios de varias líneas es comenzar la primera línea con /\*, las siguientes con \*\* y finalizar con \*/.  
  
 No hay límite de longitud para los comentarios.  
  
 Se admiten comentarios anidados. Si el patrón de carácter /* aparece en algún lugar de un comentario existente, se trata como el comienzo de un comentario anidado y, por tanto, requiere una marca de comentario de cierre \*/. Si no existe esta marca de comentario de cierre, se genera un error.  
  
 Por ejemplo, el código siguiente genera un error.  
  
```sql  
DECLARE @comment AS VARCHAR(20);  
GO  
/*  
SELECT @comment = '/*';  
*/   
SELECT @@VERSION;  
GO   
```  
  
 Para solucionar este error, realice el cambio siguiente.  
  
```sql  
DECLARE @comment AS VARCHAR(20);  
GO  
/*  
SELECT @comment = '/*';  
*/ */  
SELECT @@VERSION;  
GO  
```  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se utilizan comentarios para explicar la finalidad de la sección del código.  
  
```sql  
USE AdventureWorks2012;  
GO  
/*  
This section of the code joins the Person table with the Address table,   
by using the Employee and BusinessEntityAddress tables in the middle to   
get a list of all the employees in the AdventureWorks2012 database   
and their contact information.  
*/  
SELECT p.FirstName, p.LastName, a.AddressLine1, a.AddressLine2, a.City, a.PostalCode  
FROM Person.Person AS p  
JOIN HumanResources.Employee AS e ON p.BusinessEntityID = e.BusinessEntityID   
JOIN Person.BusinessEntityAddress AS ea ON e.BusinessEntityID = ea.BusinessEntityID  
JOIN Person.Address AS a ON ea.AddressID = a.AddressID;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [-- &#40;Comment&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/comment-transact-sql.md)   
 [Lenguaje de control de flujo &#40;Transact-SQL&#41;](~/t-sql/language-elements/control-of-flow.md)  
  
  

