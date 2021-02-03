---
description: CREATE RULE (Transact-SQL)
title: CREATE RULE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RULE_TSQL
- CREATE RULE
- CREATE_RULE_TSQL
- RULE
dev_langs:
- TSQL
helpviewer_keywords:
- list rules [SQL Server]
- unbinding rules
- pattern rules [SQL Server]
- range rules [SQL Server]
- alias data types [SQL Server], rules
- CREATE RULE statement
- reports [SQL Server], rules
- status information [SQL Server], rules
- rules [SQL Server], precedence
- binding rules [SQL Server]
- rules [SQL Server], creating
ms.assetid: b016a289-3a74-46b1-befc-a13183be51e4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a1751cef0c499a3fff4da9bf86d08e74f35a7eb7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192691"
---
# <a name="create-rule-transact-sql"></a>CREATE RULE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un objeto denominado regla. Cuando se enlaza a una columna o a un tipo de datos de alias, la regla especifica los valores aceptables que se pueden insertar en esa columna.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]Se recomienda utilizar restricciones CHECK en su lugar. Las restricciones CHECK se crean mediante la palabra clave CHECK de CREATE TABLE o ALTER TABLE. Para más información, consulte [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md).  
  
 Una columna o un tipo de datos de alias solo puede tener enlazada una regla. Sin embargo, una columna puede tener una regla y una o más restricciones CHECK asociadas a ella. Cuando esto es así, se evalúan todas las restricciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE RULE [ schema_name . ] rule_name   
AS condition_expression  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *schema_name*  
 Es el nombre del esquema al que pertenece la regla.  
  
 *rule_name*  
 Es el nombre de la nueva regla. Los nombres de reglas deben ajustarse a las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md). La especificación del propietario de la regla es opcional.  
  
 *condition_expression*  
 Es la condición o condiciones que definen la regla. Una regla puede ser cualquier expresión válida en una cláusula WHERE y puede incluir elementos como operadores aritméticos, operadores relacionales y predicados (por ejemplo, IN, LIKE, BETWEEN). Una regla no puede hacer referencia a columnas u otros objetos de base de datos. Se pueden incluir funciones integradas que no hagan referencia a objetos de base de datos. No es posible utilizar funciones definidas por el usuario.  
  
 *condition_expression* incluye una variable. El carácter de arroba ( **@** ) precede a cada variable local. La expresión hace referencia al valor especificado con la instrucción UPDATE o INSERT. Se puede usar cualquier nombre o símbolo para representar el valor cuando se crea la regla, pero el primer carácter debe ser la arroba ( **@** ).  
  
> [!NOTE]  
>  Evite crear reglas en expresiones que utilicen tipos de datos de alias. Aunque es posible crear reglas en expresiones que utilicen tipos de datos de alias, después de enlazar las reglas a las columnas o a los tipos de datos de alias, cuando se hace referencia a las expresiones, éstas no se compilan.  
  
## <a name="remarks"></a>Comentarios  
 CREATE RULE no se puede combinar con otras instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] en un único lote. Las reglas no se aplican a los datos ya existentes en la base de datos en el momento en que se crean las reglas y no se pueden enlazar a los tipos de datos del sistema.  
  
 Una regla solo se puede crear en la base de datos actual. Una vez creada la regla, ejecute **sp_bindrule** para enlazarla a una columna o a un tipo de datos de alias. Una regla debe ser compatible con el tipo de datos de la columna. Por ejemplo, no se puede usar "\@value LIKE A%" como regla para una columna numérica. Una regla no se puede enlazar a una columna **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)**, **xml**, de tipo definido por el usuario CLR o **timestamp**. Una regla no se puede enlazar a una columna calculada.  
  
 Incluya las constantes de fecha y de caracteres entre comillas simples (') y preceda las constantes binarias de 0x. Si la regla no es compatible con la columna a la que se ha enlazado, el [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] devuelve un mensaje de error cuando se inserta un valor, pero no cuando se enlaza la regla.  
  
 Una regla enlazada a un tipo de datos de alias solo se activa cuando se intenta actualizar o insertar un valor en una columna de la base de datos del tipo de datos de alias. Dado que las reglas no prueban las variables, no asigne un valor a una variable de tipo de datos de alias que sería rechazada por una regla enlazada a una columna del mismo tipo de datos.  
  
 Para obtener un informe sobre una regla, use **sp_help**. Para que se muestre el texto de una regla, ejecute **sp_helptext** con el nombre de la regla como parámetro. Para cambiar el nombre de una regla, use **sp_rename**.  
  
 Una regla se debe quitar mediante DROP RULE para poder crear otra con el mismo nombre y, antes de quitarla, el enlace se debe cancelar mediante **sp_unbindrule**. Para cancelar el enlace de una regla a una columna, use **sp_unbindrule**.  
  
 Una nueva regla se puede enlazar a una columna o tipo de datos sin cancelar el enlace de la anterior; la nueva regla anula la anterior. Las reglas enlazadas a columnas siempre tienen prioridad sobre las enlazadas a tipos de datos de alias. Enlazar una regla a una columna sustituye una regla ya enlazada al tipo de datos de alias de esa columna. Sin embargo, el enlace de una regla a un tipo de datos no sustituye una regla enlazada a una columna de ese tipo de datos de alias. La tabla siguiente muestra la prioridad cuando se enlazan reglas a columnas y a tipos de datos de alias en los que ya existen reglas.  
  
|Nueva regla enlazada a|Regla antigua enlazada a<br /><br /> Tipo de datos de alias|Regla antigua enlazada a<br /><br /> Columna|  
|-----------------------|-------------------------------------------|----------------------------------|  
|Tipo de datos de alias|Regla antigua sustituida|Sin cambios|  
|Columna|Regla antigua sustituida|Regla antigua sustituida|  
  
 Si una columna tiene un valor predeterminado y una regla asociada a ella, el valor predeterminado debe encontrarse en el dominio definido por la regla. Un valor predeterminado que esté en conflicto con una regla no se inserta nunca. El motor de base de datos de SQL Server genera un mensaje de error cada vez que intenta insertar un valor predeterminado con esas características.  
  
## <a name="permissions"></a>Permisos  
 Para ejecutar CREATE RULE, un usuario necesita, como mínimo, el permiso CREATE RULE en la base de datos actual y el permiso ALTER en el esquema en el que se está creando la regla.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-rule-with-a-range"></a>A. Crear una regla con un rango  
 En el siguiente ejemplo se crea una regla que restringe el intervalo de enteros insertado en las columnas a las que la regla está enlazada.  
  
```sql  
CREATE RULE range_rule  
AS   
@range>= $1000 AND @range <$20000;  
```  
  
### <a name="b-creating-a-rule-with-a-list"></a>B. Crear una regla con una lista  
 En el siguiente ejemplo se crea una regla que restringe los valores reales especificados en las columnas (a las que la regla está enlazada) a solo aquellos enumerados en la regla.  
  
```sql  
CREATE RULE list_rule  
AS   
@list IN ('1389', '0736', '0877');  
```  
  
### <a name="c-creating-a-rule-with-a-pattern"></a>C. Crear una regla con un patrón  
 En el siguiente ejemplo se crea una regla que sigue un patrón de dos caracteres cualquiera seguidos de un guión (`-`), cualquier número de caracteres o ningún carácter y un entero entre `0` y `9` al final.  
  
```sql  
CREATE RULE pattern_rule   
AS  
@value LIKE '__-%[0-9]'  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DROP DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-default-transact-sql.md)   
 [DROP RULE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-rule-transact-sql.md)   
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [sp_bindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sp_unbindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindrule-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  
