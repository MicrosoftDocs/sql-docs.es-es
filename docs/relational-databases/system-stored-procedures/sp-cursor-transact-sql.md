---
description: sp_cursor (Transact-SQL)
title: sp_cursor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursor_TSQL
- sp_cursor
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursor
ms.assetid: 41ade0ca-5f11-469d-bd4d-c8302ccd93b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 530093d48d1fa811231a5883b1b2b7e0f3922bf8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205174"
---
# <a name="sp_cursor-transact-sql"></a>sp_cursor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Solicita actualizaciones posicionadas. Este procedimiento realiza las operaciones en una o más filas dentro del búfer de captura de un cursor. sp_cursor se invoca especificando el identificador 1 en un paquete de flujo TDS.  
  
||  
|-|  
|**Se aplica a**: SQL Server ( [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] a través de la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).|  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_cursor  cursor, optype, rownum, table  
    [ , value[...n]]]  
```  
  
## <a name="arguments"></a>Argumentos  
 *cursor*  
 El identificador de cursor. *cursor* es un parámetro necesario que requiere un valor de entrada **int** . *cursor* es el valor de *identificador* generado por SQL Server y devuelto por el procedimiento Sp_cursoropen.  
  
 *optype*  
 Es un parámetro necesario que designa qué operación realizará el cursor. *optype* requiere uno de los siguientes valores de entrada **int** .  
  
|Value|Nombre|Descripción|  
|-----------|----------|-----------------|  
|0X0001|UPDATE|Se usa para actualizar una o varias filas en el búfer de captura.  Se vuelve a tener acceso a las filas especificadas en *ROWNUM* y se actualizan.|  
|0x0002|DELETE|Se usa para eliminar una o varias filas en el búfer de captura. Se vuelve a tener acceso a las filas especificadas en *ROWNUM* y se eliminan.|  
|0X0004|INSERT|Inserta datos sin generar una instrucción **Insert** de SQL.|  
|0X0008|REFRESH|Se usa para rellenar el búfer a partir de las tablas subyacentes y se puede usar para actualizar la fila si una actualización o eliminación no puede realizarse debido a un control de simultaneidad optimista o después de una operación UPDATE.|  
|0X10|LOCK|Hace que se adquiera un SQL Server U-Lock en la página que contiene la fila especificada. Este bloqueo es compatible con los bloqueos S. No es compatible con los bloqueos X ni U. Se puede usar para implementar el bloqueo a corto plazo.|  
|0X20|SETPOSITION|Solo se usa cuando el programa va a emitir una instrucción DELETE o UPDATE colocada de SQL Server posterior.|  
|0X40|ABSOLUTE|Solo se puede usar junto con UPDATE o DELETE.  ABSOLUTE solo se emplea con cursores KEYSET (se omite para los cursores DYNAMIC y los cursores STATIC no se pueden actualizar).<br /><br /> Nota: si se especifica ABSOLUTE en una fila del conjunto de claves que no se ha capturado, la operación puede producir un error en la comprobación de simultaneidad y no se puede garantizar el resultado devuelto.|  
  
 *ROWNUM*  
 Especifica con cuál de las filas del búfer de captura operará el cursor, cuál actualizará o cuál eliminará.  
  
> [!NOTE]  
>  Esto no afecta al punto inicial de ninguna operación de captura RELATIVE, NEXT o PREVIOUS, ni a las actualizaciones ni eliminaciones realizadas con sp_cursor.  
  
 *ROWNUM* es un parámetro necesario que requiere un valor de entrada **int** .  
  
 1  
 Indica la primera fila del búfer de captura.  
  
 2  
 Indica la segunda fila del búfer de captura.  
  
 3, 4, 5  
 Indica la tercera fila, etc.  
  
 n  
 Indica la enésima fila del búfer de captura.  
  
 0  
 Indica todas las filas del búfer de captura.  
  
> [!NOTE]  
>  Solo es válido para su uso con los valores de actualización, eliminación, actualización o bloqueo de *optype* .  
  
 *table*  
 Nombre de tabla que identifica la tabla a la que se aplica *optype* cuando la definición del cursor implica la combinación o los nombres de columna ambiguos devueltos por el parámetro de *valor* . Si no se designa ninguna tabla específica, el valor predeterminado es la primera tabla de la cláusula FROM. *TABLE* es un parámetro opcional que requiere un valor de entrada de cadena. La cadena se puede especificar como cualquier tipo de datos UNICODE o carácter. la *tabla* puede ser un nombre de tabla de varias partes.  
  
 *value*  
 Se usa para insertar o actualizar valores. El parámetro de cadena *Value* solo se usa con los valores Update e Insert *optype* . La cadena se puede especificar como cualquier tipo de datos UNICODE o carácter.  
  
> [!NOTE]  
>  El usuario puede asignar los nombres de parámetro para el *valor* .  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 Cuando se usa una RPC, una operación de eliminación o actualización posicionada con un número de búfer 0 devolverá un mensaje de finalización con un *recuento de filas* de 0 (error) o 1 (correcto) para cada fila del búfer de captura.  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="optype-parameter"></a>Parámetro optype  
 Con la excepción de las combinaciones de SETPOSITION con UPDATE, DELETE, REFRESH o LOCK; o ABSOLUTE con UPDATE o DELETE, los valores de *optype* son mutuamente excluyentes.  
  
 La cláusula SET del valor de actualización se construye a partir del parámetro *Value* .  
  
 Una ventaja de usar el valor de INSERT *optype* es que puede evitar la conversión de datos que no son caracteres en formato de caracteres para las inserciones. Los valores se especifican de la misma manera que para UPDATE. Si alguna columna necesaria no está incluida, se produce un error en INSERT.  
  
-   El valor SETPOSITION no afecta al punto inicial de ninguna operación de captura RELATIVE, NEXT o PREVIOUS, ni hace ninguna actualización ni eliminación que se efectúe con la interfaz sp_cursor. Cualquier número que no especifique una fila en el búfer de captura hará que la posición se establezca en 1 y no se devuelva ningún error. Una vez que se ejecuta SETPOSITION, la posición permanece en vigor hasta la siguiente operación de sp_cursorfetch, operación de **captura** de T-SQL o SP_CURSOR operación SETPOSITION a través del mismo cursor. La siguiente operación sp_cursorfetch establecerá la posición del cursor en la primera fila del nuevo búfer de captura, mientras que otras llamadas del cursor no afectarán al valor de la posición. Una cláusula OR con REFRESH, UPDATE, DELETE o LOCK puede vincular SETPOSITION para establecer el valor de la posición en la última fila modificada.  
  
 Si no se especifica una fila en el búfer de captura a través del parámetro *ROWNUM* , la posición se establecerá en 1 y no se devolverá ningún error. Una vez establecida la posición, permanece en vigor hasta que se realiza la siguiente operación sp_cursorfetch, FETCH de T-SQL o sp_cursor SETPOSITION en el mismo cursor.  
  
 SETPOSITION puede vincularse con cualquier cláusula REFRESH, UPDATE, DELETE o LOCK para establecer la posición del cursor en la última fila modificada.  
  
## <a name="rownum-parameter"></a>Parámetro rownum  
 Si se especifica, el parámetro *ROWNUM* puede interpretarse como el número de fila dentro del conjunto de claves en lugar del número de fila dentro del búfer de captura. El usuario es responsable de asegurarse de que el control de simultaneidad se mantenga. Esto significa que para los cursores SCROLL_LOCKS, debe mantener independientemente un bloqueo en la fila dada (esto se puede hacer a través de una transacción). En el caso de los cursores optimistas, debe haber capturado previamente la fila para realizar esta operación.  
  
## <a name="table-parameter"></a>Parámetro table  
 Si el valor de *optype* es Update o INSERT y se envía una instrucción UPDATE o INSERT completa como parámetro de *valor* , se omite el valor especificado para *TABLE* .  
  
> [!NOTE]  
>  En relación con las vistas, solo se puede modificar una tabla que participa en la vista. Los nombres de columna del parámetro de *valor* deben reflejar los nombres de columna de la vista, pero el nombre de la tabla puede ser el de la tabla base subyacente (en cuyo caso sp_cursor sustituirá el nombre de la vista).  
  
## <a name="value-parameter"></a>Parámetro value  
 Existen dos alternativas a las reglas para usar el *valor* tal y como se indicó anteriormente en la sección argumentos:  
  
1.  Puede usar un nombre que esté precedido por \@ el nombre de la columna en la lista de selección para cualquier parámetro de *valor* con nombre. Una ventaja de esta alternativa es que la conversión de datos puede no ser necesaria.  
  
2.  Use un parámetro para enviar una instrucción UPDATE o INSERT completa o usar varios parámetros para enviar partes de una instrucción UPDATE o INSERT que SQL Server compilará en una instrucción completa. Puede encontrar ejemplos de esto en la sección Ejemplos, posteriormente en este tema.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="alternative-value-parameter-uses"></a>Usos alternativos del parámetro value  
 Para UPDATE:  
  
 Cuando se utiliza un solo parámetro, se puede enviar una instrucción UPDATE con la siguiente sintaxis:  
  
 `[ [ UPDATE <table name> ] SET ] {<column name> = expression} [,...n]`  
  
> [!NOTE]  
>  Si \<table name> se especifica Update, se omitirá cualquier valor especificado para el parámetro *TABLE* .  
  
 Cuando se utilizan varios parámetros, el primero debe ser una cadena con el siguiente formato:  
  
 `[ SET ] <column name> = expression  [,...n]`  
  
 y los parámetros subsiguientes deben estar en el formato:  
  
 `<column name> = expression  [,...n]`  
  
 En este caso, el \<table name> de la instrucción UPDATE construida es el especificado por el parámetro de *tabla* o su valor predeterminado.  
  
 Para INSERT:  
  
 Cuando se utiliza un solo parámetro, se puede enviar una instrucción INSERT con la siguiente sintaxis:  
  
 `[ [ INSERT [INTO] <table name> ] VALUES ] ( <expression> [,...n] )`  
  
> [!NOTE]  
>  Si *\<table name>* se especifica INSERT, se omitirá cualquier valor especificado para el parámetro de *tabla* .  
  
 Cuando se utilizan varios parámetros, el primero debe ser una cadena con el siguiente formato:  
  
 `[ VALUES ( ] <expression>  [,...n]`  
  
 y los parámetros subsiguientes deben estar en el formato:  
  
 `expression [,...n]`  
  
 excepto cuando se especificó VALUES, en cuyo caso debe haber un carácter ")" final después de la última expresión. En este caso, el *\<table name>* de la instrucción UPDATE construida es el especificado por el parámetro de *tabla* o su valor predeterminado.  
  
> [!NOTE]  
>  Es posible enviar un parámetro como un parámetro con nombre, es decir "`@VALUES`". En este caso no se pueden usar otros parámetros con nombre.  
  
## <a name="see-also"></a>Consulte también  
 [sp_cursoropen &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorfetch &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
