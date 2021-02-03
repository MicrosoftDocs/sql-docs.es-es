---
description: CREATE DEFAULT (Transact-SQL)
title: CREATE DEFAULT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_DEFAULT_TSQL
- CREATE DEFAULT
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], default
- default objects
- CREATE DEFAULT statement
- objects [SQL Server], creating
- DEFAULT definition
ms.assetid: 08475db4-7d90-486a-814c-01a99d783d41
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c1ebdb3e7282fba1f22eedcda9278e44a22de424
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186904"
---
# <a name="create-default-transact-sql"></a>CREATE DEFAULT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Crea un objeto denominado valor predeterminado. Cuando se enlaza a una columna o a un tipo de datos de alias, un valor predeterminado especifica un valor que debe insertarse en la columna a la que está enlazado el objeto, o en todas las columnas, en el caso de un tipo de datos de alias, si no se proporciona explícitamente un valor durante la inserción.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, use definiciones predeterminadas creadas con la palabra clave DEFAULT de ALTER TABLE o CREATE TABLE.  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
CREATE DEFAULT [ schema_name . ] default_name   
AS constant_expression [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*schema_name*  
 El nombre del esquema al que pertenece el valor predeterminado.  
  
*default_name*  
 El nombre del valor predeterminado. Los nombres predeterminados deben cumplir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md). Especificar el nombre del propietario del valor predeterminado es opcional.  
  
*constant_expression*  
Una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) que contiene solo valores constantes (no puede contener el nombre de ninguna columna u otros objetos de base de datos). Puede utilizar cualquier constante, función integrada o expresión matemática, excepto las que contienen tipos de datos de alias. No es posible utilizar funciones definidas por el usuario. Incluya las constantes de caracteres y fechas entre comillas simples (**'**); las constantes de moneda, de enteros y de coma flotante no necesitan comillas. Los datos binarios deben precederse de 0x y los datos de moneda deben precederse de un signo de dólar ($). El valor predeterminado debe ser compatible con el tipo de datos de la columna.  
  
## <a name="remarks"></a>Observaciones  
 Solo puede crear un nombre predeterminado en la base de datos actual. En una base de datos, los nombres predeterminados deben ser únicos para cada esquema. Al crear un valor predeterminado, use **sp_bindefault** para enlazarlo a una columna o a un tipo de datos de alias.  
  
 Si el valor predeterminado no es compatible con la columna a la que está enlazado, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un mensaje de error al intentar insertar el valor predeterminado. Por ejemplo, N/A no se puede usar como valor predeterminado para una columna **numérica**.  
  
 Si el valor predeterminado es demasiado largo para la columna a la que está enlazado, el valor se trunca.  
  
 Las instrucciones CREATE DEFAULT no se pueden combinar con otras instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] en el mismo lote.  
  
 Un valor predeterminado debe quitarse antes de crear otro con el mismo nombre. Además, se debe anular el enlace del valor predeterminado con la ejecución de **sp_unbindefault** antes de quitarlo.  
  
 Si una columna tiene un valor predeterminado y una regla asociados, el valor predeterminado no debe infringir la regla. No se insertará nunca un valor predeterminado que esté en conflicto con una regla y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un mensaje de error cada vez que se intente insertar el valor predeterminado.  
  
 Cuando se enlaza a una columna, un valor predeterminado se inserta cuando:  
  
-   No se ha insertado un valor explícitamente.  
  
-   Se utilizan las palabras clave DEFAULT VALUES o DEFAULT con INSERT para insertar valores predeterminados.  
  
 Si especifica NOT NULL al crear una columna y no crea un valor predeterminado para esta, se generará un mensaje de error cada vez que el usuario no cree una entrada en esa columna. En la tabla siguiente se ilustra la relación entre la existencia de un valor predeterminado y la definición de una columna como NULL o NOT NULL. Las entradas de la tabla muestran el resultado.  
  
|Definición de columna|Sin entrada, sin valor predeterminado|Sin entrada, valor predeterminado|Entrada NULL, sin valor predeterminado|Entrada NULL, valor predeterminado|  
|-----------------------|--------------------------|-----------------------|----------------------------|-------------------------|  
|**NULL**|NULL|default|NULL|NULL|  
|**NOT NULL**|Error|default|error|error|  
  
 Para cambiar el nombre de un valor predeterminado, use **sp_rename**. Para obtener un informe sobre un valor predeterminado, use **sp_help**.  
  
## <a name="permissions"></a>Permisos  
 Para usar CREATE DEFAULT, como mínimo, un usuario debe tener el permiso CREATE DEFAULT en la base de datos actual y el permiso ALTER en el esquema donde se va a crear el valor predeterminado.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-simple-character-default"></a>A. Crear un valor predeterminado de caracteres de tipo sencillo  
 En el siguiente ejemplo se crea un valor predeterminado de caracteres denominado `unknown`.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE DEFAULT phonedflt AS 'unknown';  
```  
  
### <a name="b-binding-a-default"></a>B. Enlazar un valor predeterminado  
 En el siguiente ejemplo se enlaza el valor predeterminado creado en el ejemplo A. El valor predeterminado solo entra en efecto si no hay ninguna entrada especificada en la columna `Phone` de la tabla `Contact`. 
 
 > [!Note] 
 >  Omitir una entrada no es lo mismo que incluir NULL explícitamente en una instrucción INSERT.  
  
 Como no existe un valor predeterminado llamado `phonedflt`, se producirá un error en la siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)]. Este ejemplo solo tiene propósitos ilustrativos.  
  
```sql  
USE AdventureWorks2012;  
GO  
sp_bindefault 'phonedflt', 'Person.PersonPhone.PhoneNumber';  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DROP DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-default-transact-sql.md)   
 [DROP RULE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-rule-transact-sql.md)   
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [sp_bindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sp_unbindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindefault-transact-sql.md)  
  
  
