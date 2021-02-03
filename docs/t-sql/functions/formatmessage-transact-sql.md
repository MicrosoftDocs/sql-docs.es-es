---
description: FORMATMESSAGE (Transact-SQL)
title: FORMATMESSAGE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FORMATMESSAGE
- FORMATMESSAGE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmessages system table
- sys.messages catalog view
- FORMATMESSAGE function
- messages [SQL Server], formats
- errors [SQL Server], formats
ms.assetid: 83f18102-2035-4a87-acd0-8d96d03efad5
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4af8d17ee5c75fb5df6690add3c290cef5c52075
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237493"
---
# <a name="formatmessage-transact-sql"></a>FORMATMESSAGE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Genera un mensaje a partir de un mensaje existente en sys.messages o de una cadena proporcionada. La funcionalidad de FORMATMESSAGE es similar a la de la instrucción RAISERROR. Sin embargo, RAISERROR imprime el mensaje inmediatamente, mientras que FORMATMESSAGE devuelve el mensaje formateado para que se pueda seguir procesando.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
FORMATMESSAGE ( { msg_number  | ' msg_string ' | @msg_variable} , [ param_value [ ,...n ] ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *msg_number*  
 Es el identificador del mensaje almacenado en sys.messages. Si *msg_number* es <= 13000 o si el mensaje no existe en sys.messages, se devuelve NULL.  
  
 *msg_string*  
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
 Es una cadena entre comillas simples que contiene marcadores de posición de valor de parámetro. El mensaje de error puede tener 2.047 caracteres como máximo. Si el mensaje contiene más de 2.048 caracteres, solamente aparecerán los 2.044 primeros y se agregarán puntos suspensivos para indicar que el mensaje se ha truncado. Tenga en cuenta que los parámetros de sustitución utilizan más caracteres de lo que muestra la salida debido al comportamiento del almacenamiento interno.  Para más información sobre la estructura de una cadena de mensaje y el uso de parámetros en la cadena, vea la descripción del argumento *msg_str* en [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md).  

 *@msg_variable*  
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
 Es una variable nvarchar o varchar que contiene una cadena que cumple con los criterios de *msg_string* anteriores.  
  
 *param_value*  
 Valor de parámetro que se utiliza en el mensaje. Puede ser más de un valor. Los valores se deben especificar en el orden en el que las variables de marcador de posición aparezcan en el mensaje. El número máximo de valores es 20.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **nvarchar**  
  
## <a name="remarks"></a>Observaciones  
 Al igual que la instrucción RAISERROR, FORMATMESSAGE modifica el mensaje sustituyendo los valores de los parámetros suministrados por variables de marcador de posición del mensaje. Para más información sobre los marcadores de posición permitidos en los mensajes de error y el proceso de edición, vea [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md).  
  
 FORMATMESSAGE busca el mensaje en el idioma actual del usuario. En el caso de los mensajes del sistema (*msg_number* <=50000), si no hay ninguna versión localizada del mensaje, se usa la versión del idioma del sistema operativo. Para los mensajes del usuario (*msg_number* >50000), si no hay ninguna versión localizada del mensaje, se usa la versión en inglés.
  
 En los mensajes en otros idiomas, los valores de los parámetros suministrados deben corresponder a los marcadores de posición de los parámetros de la Versión en inglés. Es decir, el parámetro 1 de la versión en otros idiomas debe corresponder al parámetro 1 de la versión en inglés, el parámetro 2 debe corresponder al parámetro 2, etc.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-example-with-a-message-number"></a>A. Ejemplo con un número de mensaje  
 En el siguiente ejemplo se usa un mensaje de replicación `20009` almacenado en sys.messages como "No se pudo agregar el artículo '%s' a la publicación '%s'". FORMATMESSAGE sustituye los valores `First Variable` y `Second Variable` por los marcadores de posición de los parámetros. La cadena resultante, "No se pudo agregar el artículo 'First Variable' a la publicación 'Second Variable'", se almacena en la variable local `@var1`.  
  
```sql
SELECT text FROM sys.messages WHERE message_id = 20009 AND language_id = 1033;  
DECLARE @var1 VARCHAR(200);   
SELECT @var1 = FORMATMESSAGE(20009, 'First Variable', 'Second Variable');   
SELECT @var1;  
```  
  
### <a name="b-example-with-a-message-string"></a>B. Ejemplo con una cadena de mensaje  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
 En el siguiente ejemplo se toma una cadena como entrada.  
  
```sql
SELECT FORMATMESSAGE('This is the %s and this is the %s.', 'first variable', 'second variable') AS Result;  
```  
  
 Devuelve: `This is the first variable and this is the second variable.`  
  
### <a name="c-additional-message-string-formatting-examples"></a>C. Más ejemplos de formato de cadena de mensaje  
 En los siguientes ejemplos se muestran distintas opciones de formato.  
  
```sql
SELECT FORMATMESSAGE('Signed int %i, %d %i, %d, %+i, %+d, %+i, %+d', 5, -5, 50, -50, -11, -11, 11, 11);
SELECT FORMATMESSAGE('Signed int with up to 3 leading zeros %03i', 5);  
SELECT FORMATMESSAGE('Signed int with up to 20 leading zeros %020i', 5);  
SELECT FORMATMESSAGE('Signed int with leading zero 0 %020i', -55);  
SELECT FORMATMESSAGE('Bigint %I64d', 3000000000);
SELECT FORMATMESSAGE('Unsigned int %u, %u', 50, -50);  
SELECT FORMATMESSAGE('Unsigned octal %o, %o', 50, -50);  
SELECT FORMATMESSAGE('Unsigned hexadecimal %x, %X, %X, %X, %x', 11, 11, -11, 50, -50);  
SELECT FORMATMESSAGE('Unsigned octal with prefix: %#o, %#o', 50, -50);  
SELECT FORMATMESSAGE('Unsigned hexadecimal with prefix: %#x, %#X, %#X, %X, %x', 11, 11, -11, 50, -50);  
SELECT FORMATMESSAGE('Hello %s!', 'TEST');  
SELECT FORMATMESSAGE('Hello %20s!', 'TEST');  
SELECT FORMATMESSAGE('Hello %-20s!', 'TEST');  
```  
  
## <a name="see-also"></a>Consulte también  
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)  
 [THROW &#40;Transact-SQL&#41;](../../t-sql/language-elements/throw-transact-sql.md)   
 [sp_addmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)   
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)  
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
 [Funciones del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
