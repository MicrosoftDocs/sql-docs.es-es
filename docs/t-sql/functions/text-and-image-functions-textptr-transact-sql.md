---
description: Funciones de texto e imagen - TEXTPTR (Transact-SQL)
title: TEXTPTR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TEXTPTR_TSQL
- TEXTPTR
dev_langs:
- TSQL
helpviewer_keywords:
- TEXTPTR function
- viewing text pointer values
- text-pointer values
- displaying text pointer values
ms.assetid: 2672b8cb-f747-46f3-9358-9b49b3583b8e
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 39e1c646755c4ea2907b328938cac0f30f6f47d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181447"
---
# <a name="text-and-image-functions---textptr-transact-sql"></a>Funciones de texto e imagen - TEXTPTR (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve el valor de puntero de texto que corresponde a una columna **text**, **ntext** o **image** en formato **varbinary**. El valor del puntero de texto devuelto se puede utilizar en las instrucciones READTEXT, WRITETEXT y UPDATE.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] No hay ninguna funcionalidad alternativa disponible.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
TEXTPTR ( column )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *column*  
 Se utilizará la columna **text**, **ntext** o **image**.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **varbinary**  
  
## <a name="remarks"></a>Observaciones  
 En las tablas con texto consecutivo, TEXTPTR devuelve un controlador para el texto que se va a procesar. Es posible obtener un puntero de texto válido aunque el valor del texto sea NULL.  
  
 La función TEXTPTR no se puede utilizar en columnas de vistas. Solo se puede utilizar en columnas de tablas. Para utilizar la función TEXTPTR en una columna de una vista, debe establecerse el nivel de compatibilidad en 80 mediante el [nivel de compatibilidad de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md). Si la tabla no dispone de texto consecutivo y la instrucción UPDATETEXT no ha inicializado una columna **text**, **ntext** o **image**, TEXTPTR devuelve un puntero NULL.  
  
 Utilice TEXTVALID para comprobar si un puntero de texto existe. No es posible utilizar UPDATETEXT, WRITETEXT o READTEXT sin un puntero de texto válido.  
  
 Estas funciones e instrucciones son también útiles cuando se trabaja con datos de tipo **text**, **ntext** e **image**.  
  
|Función o instrucción|Descripción|  
|---------------------------|-----------------|  
|PATINDEX <b>('</b> _%pattern%_ **' ,** _expression_ **)**|Devuelve la posición de carácter de una cadena de caracteres especificada en columnas de tipo **text** o **ntext**.|  
|DATALENGTH <b>(</b>_expression_ **)**|Devuelve la longitud de datos en columnas **text**, **ntext** e **image**.|  
|SET TEXTSIZE|Devuelve el límite en bytes de los datos de tipo **text**, **ntext** o **image** que se devuelven con una instrucción SELECT.|  
|SUBSTRING <b>(</b>_text_column_, _start_, _length_ **)**|Devuelve una cadena **varchar** según los valores especificados en el desplazamiento *start* y *length*. La longitud debe ser inferior a 8 KB.|  
  
## <a name="examples"></a>Ejemplos  
  
> [!NOTE]  
>  Para ejecutar estos ejemplos, es necesario instalar la base de datos **pubs**.  
  
### <a name="a-using-textptr"></a>A. Usar TEXTPTR  
 En el ejemplo siguiente se utiliza la función `TEXTPTR` para encontrar la columna `logo` de **image** asociada a `New Moon Books` en la tabla `pub_info` de la base de datos `pubs`. El puntero de texto se coloca en la variable local `@ptrval.`  
  
```sql
USE pubs;  
GO  
DECLARE @ptrval VARBINARY(16);  
SELECT @ptrval = TEXTPTR(logo)  
FROM pub_info pr, publishers p  
WHERE p.pub_id = pr.pub_id   
   AND p.pub_name = 'New Moon Books';  
GO  
```  
  
### <a name="b-using-textptr-with-in-row-text"></a>B. Usar TEXTPTR con texto consecutivo  
 En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el puntero de texto consecutivo debe utilizarse dentro de una transacción, como se muestra en el ejemplo siguiente.  
  
```sql
CREATE TABLE t1 (c1 INT, c2 TEXT);  
EXEC sp_tableoption 't1', 'text in row', 'on';  
INSERT t1 VALUES ('1', 'This is text.');  
GO  
BEGIN TRAN;  
   DECLARE @ptrval VARBINARY(16);  
   SELECT @ptrval = TEXTPTR(c2)  
   FROM t1  
   WHERE c1 = 1;  
   READTEXT t1.c2 @ptrval 0 1;  
COMMIT;  
```  
  
### <a name="c-returning-text-data"></a>C. Devolver datos de texto  
 En el ejemplo siguiente se seleccionan la columna `pub_id` y el puntero de texto de 16 bits de la columna `pr_info` de la tabla `pub_info`.  
  
```sql
USE pubs;  
GO  
SELECT pub_id, TEXTPTR(pr_info)  
FROM pub_info  
ORDER BY pub_id;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
pub_id                                      
------ ----------------------------------   
0736   0x6c0000000000feffb801000001000100   
0877   0x6d0000000000feffb801000001000300   
1389   0x6e0000000000feffb801000001000500   
1622   0x700000000000feffb801000001000900   
1756   0x710000000000feffb801000001000b00   
9901   0x720000000000feffb801000001000d00   
9952   0x6f0000000000feffb801000001000700   
9999   0x730000000000feffb801000001000f00   
  
(8 row(s) affected)  
```  
  
 En el ejemplo siguiente se muestra cómo devolver los primeros `8000` bytes de texto sin utilizar TEXTPTR.  
  
```sql
USE pubs;  
GO  
SET TEXTSIZE 8000;  
SELECT pub_id, pr_info  
FROM pub_info  
ORDER BY pub_id;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
pub_id pr_info                                                                                                                                                                                                                                                           
------ -----------------------------------------------------------------  
0736   New Moon Books (NMB) has just released another top ten publication. With the latest publication this makes NMB the hottest new publisher of the year!                                                                                                             
0877   This is sample text data for Binnet & Hardley, publisher 0877 in the pubs database. Binnet & Hardley is located in Washington, D.C.  
  
This is sample text data for Binnet & Hardley, publisher 0877 in the pubs database. Binnet & Hardley is located in Washi   
1389   This is sample text data for Algodata Infosystems, publisher 1389 in the pubs database. Algodata Infosystems is located in Berkeley, California.  
  
9999   This is sample text data for Lucerne Publishing, publisher 9999 in the pubs database. Lucerne publishing is located in Paris, France.  
  
This is sample text data for Lucerne Publishing, publisher 9999 in the pubs database. Lucerne publishing is located in   
  
(8 row(s) affected)  
```  
  
### <a name="d-returning-specific-text-data"></a>D. Devolver datos de texto específicos  
 En el ejemplo siguiente se encuentra la columna `text` (`pr_info`) asociada a `pub_id``0736` en la tabla `pub_info` de la base de datos `pubs`. Primero se declara la variable local `@val`. A continuación, el puntero de texto (una cadena binaria de tipo long) se coloca en `@val` y se suministra como parámetro a la instrucción `READTEXT`. De esta forma, se devuelven 10 bytes a partir del quinto byte (el desplazamiento es 4).  
  
```sql
USE pubs;  
GO  
DECLARE @val VARBINARY(16);  
SELECT @val = TEXTPTR(pr_info)   
FROM pub_info  
WHERE pub_id = '0736';  
READTEXT pub_info.pr_info @val 4 10;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
pr_info                                                                                                                                                                                                                                                           
-----------------------------------------------------------------------  
 is sample  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Consulte también  
 [DATALENGTH &#40;Transact-SQL&#41;](../../t-sql/functions/datalength-transact-sql.md)   
 [PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)   
 [READTEXT &#40;Transact-SQL&#41;](../../t-sql/queries/readtext-transact-sql.md)   
 [SET TEXTSIZE &#40;Transact-SQL&#41;](../../t-sql/statements/set-textsize-transact-sql.md)   
 [Funciones de texto e imagen &#40;Transact-SQL&#41;]()   
 [UPDATETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/updatetext-transact-sql.md)   
 [WRITETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/writetext-transact-sql.md)  
  
