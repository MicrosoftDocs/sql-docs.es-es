---
description: Modificar tabla - comando SQL
title: ALTER TABLE-comando SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- alter table [ODBC]
ms.assetid: 3a01a291-f4d9-43bc-a725-5a95546ff364
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 186647c40517c5ecf77a55e6bbc08155c98ffed9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205759"
---
# <a name="alter-table---sql-command"></a>Modificar tabla - comando SQL
Modifica mediante programación la estructura de una tabla.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
ALTER TABLE TableName1  
   ADD | ALTER [COLUMN] FieldName1  
      FieldType [(nFieldWidth [, nPrecision])]  
      [NULL | NOT NULL]  
      [CHECK lExpression1 [ERROR cMessageText1]]  
      [DEFAULT eExpression1]  
      [PRIMARY KEY | UNIQUE]  
      [REFERENCES TableName2 [TAG TagName1]]  
      [NOCPTRANS]  
 - Or -  
ALTER TABLE TableName1  
   ALTER [COLUMN] FieldName2  
      [NULL | NOT NULL]  
      [SET DEFAULT eExpression2]  
      [SET CHECK lExpression2 [ERROR cMessageText2]]  
      [DROP DEFAULT]  
      [DROP CHECK]  
 - Or -  
ALTER TABLE TableName1  
   [DROP [COLUMN] FieldName3]  
   [SET CHECK lExpression3 [ERROR cMessageText3]]  
   [DROP CHECK]  
   [ADD PRIMARY KEY eExpression3 TAG TagName2]  
   [DROP PRIMARY KEY]  
   [ADD UNIQUE eExpression4 [TAG TagName3]]  
   [DROP UNIQUE TAG TagName4]  
   [ADD FOREIGN KEY [eExpression5] TAG TagName4  
      REFERENCES TableName2 [TAG TagName5]]  
   [DROP FOREIGN KEY TAG TagName6 [SAVE]]  
   [RENAME COLUMN FieldName4 TO FieldName5]  
   [NOVALIDATE]  
```  
  
## <a name="arguments"></a>Argumentos  
 *TableName1*  
 Especifica el nombre de la tabla cuya estructura se ha modificado.  
  
 Agregar [columna] *FieldName1*  
 Especifica el nombre del campo que se va a agregar.  
  
 ALTER [columna] *FieldName1*  
 Especifica el nombre de un campo existente que se va a modificar.  
  
 *FieldType* [( *NFieldWidth* [, *nPrecision*]])  
 Especifica el tipo de campo, el ancho de campo y la precisión de campo (número de posiciones decimales) de un campo nuevo o modificado.  
  
 *FieldType* es una sola letra que indica el tipo de [datos](../../odbc/microsoft/visual-foxpro-field-data-types.md)del campo. Algunos tipos de datos de campo requieren que especifique *nFieldWidth* o *nPrecision* o ambos.  
  
 *nFieldWidth* y *nPrecision* se omiten para los tipos D, G, I, L, M, P, T e y. De forma predeterminada, *nPrecision* es cero (sin posiciones decimales) si *nPrecision* no se incluye para los tipos B, F o N.  
  
 NULL &#124; NOT NULL  
 Permite o impide valores NULL en el campo.  
  
 Si omite NULL y NOT NULL, el valor actual de SET NULL determina si se permiten valores NULL en el campo. Sin embargo, si omite NULL y NOT NULL e incluye la cláusula PRIMAry KEY o UNIQUE, se omite la configuración actual de SET NULL y el campo no es NULL de forma predeterminada.  
  
 COMPROBAR *lExpression1*  
 Especifica una regla de validación para el campo. *lExpression1* debe evaluarse como una expresión lógica y puede ser una función definida por el usuario o un procedimiento almacenado. Siempre que se anexa un registro en blanco, se comprueba la regla de validación. Se genera un error si la regla de validación no permite un valor de campo en blanco en un registro anexado.  
  
 ERROR *cMessageText1*  
 Especifica el mensaje de error que se muestra cuando la regla de validación de campo genera un error.  
  
 *EEXPRESSION1* predeterminado  
 Especifica un valor predeterminado para el campo. El tipo de datos de *eExpression1* debe ser el mismo que el tipo de datos del campo.  
  
 PRIMARY KEY  
 Crea una etiqueta de índice principal. La etiqueta de índice tiene el mismo nombre que el campo.  
  
 UNIQUE  
 Crea una etiqueta de índice candidata con el mismo nombre que el campo.  
  
> [!NOTE]  
>  Los índices candidatos (creados mediante la opción UNIQUE, que se proporciona para la compatibilidad con ANSI en ALTER TABLE o CREATE TABLE) difieren de los índices creados mediante la opción UNIQUE del comando INDEX. Un índice creado con UNIQUE en el comando INDEX permite claves de índice duplicadas; los índices candidatos no permiten claves de índice duplicadas.  
  
 Los valores NULL y los registros duplicados no se permiten en un campo que se usa para un índice principal o candidato.  
  
 Si va a crear un nuevo campo mediante agregar columna, Visual FoxPro no generará un error si crea un índice principal o candidato para un campo que admita valores NULL. Sin embargo, Visual FoxPro generará un error si intenta especificar un valor null o duplicado en un campo que se usa para un índice principal o candidato.  
  
 Si va a modificar un campo existente y la expresión de índice principal o candidato se compone de campos de la tabla, Visual FoxPro comprobará los campos para ver si contienen valores NULL o registros duplicados. Si lo hacen, Visual FoxPro genera un error y la tabla no se modifica.  
  
 HACE referencia a la etiqueta *TableName2* *TagName1*  
 Especifica la tabla primaria en la que se establece una relación persistente. TAG *TagName1* especifica la etiqueta de índice de la tabla primaria en la que se basa la relación. Los nombres de etiqueta de índice pueden contener hasta 10 caracteres.  
  
 NOCPTRANS  
 Impide la traducción a una página de códigos diferente para los campos de caracteres y de memorando. Si la tabla se convierte en otra página de códigos, los campos para los que se ha especificado NOCPTRANS no se convierten. NOCPTRANS solo se puede especificar para los campos de caracteres y de memorando.  
  
 En el ejemplo siguiente se crea una tabla denominada MyTable que contiene dos campos de caracteres y dos campos memo. El segundo campo de carácter, char2, y el segundo campo de memorando, memo2, incluyen NOCPTRANS para evitar la traducción.  
  
```  
CREATE TABLE mytable (char1 C(10), char2 C(10) NOCPTRANS,;  
   memo1 M, memo2 M NOCPTRANS)  
```  
  
 ALTER [columna] *FieldName2*  
 Especifica el nombre de un campo existente que se va a modificar.  
  
 ESTABLECER *EEXPRESSION2* predeterminado  
 Especifica un nuevo valor predeterminado para un campo existente. El tipo de datos de *eExpression2* debe ser el mismo que el tipo de datos del campo.  
  
 ESTABLECER *lExpression2* de comprobación  
 Especifica una nueva regla de validación para un campo existente. *lExpression2* debe evaluarse como una expresión lógica y puede ser una función definida por el usuario o un procedimiento almacenado.  
  
 ERROR *cMessageText2*  
 Especifica el mensaje de error que se muestra cuando la regla de validación de campo genera un error. El mensaje solo se muestra cuando se cambian los datos dentro de una ventana de exploración o de edición.  
  
 DROP DEFAULT  
 Quita el valor predeterminado de un campo existente.  
  
 QUITAR COMPROBACIÓN  
 Quita la regla de validación de un campo existente.  
  
 DROP [columna] *FieldName3*  
 Especifica un campo que se va a quitar de la tabla. Al quitar un campo de la tabla, también se quita la configuración de valor predeterminado y la regla de validación de campo del campo.  
  
 Si la clave de índice o las expresiones de desencadenador hacen referencia al campo, las expresiones dejan de ser válidas cuando se quita el campo. En este caso, no se genera un error cuando se quita el campo, pero la clave de índice o las expresiones de desencadenador no válidas generarán errores en tiempo de ejecución.  
  
 ESTABLECER *lExpression3* de comprobación  
 Especifica la regla de validación de la tabla. *lExpression3* debe evaluarse como una expresión lógica y puede ser una función definida por el usuario o un procedimiento almacenado.  
  
 ERROR *cMessageText3*  
 Especifica el mensaje de error que se muestra cuando la regla de validación de tabla genera un error. El mensaje solo se muestra cuando se cambian los datos dentro de una ventana de exploración o de edición.  
  
 QUITAR COMPROBACIÓN  
 Quita la regla de validación de la tabla.  
  
 Agregar clave principal *eExpression3* etiqueta *TagName2*  
 Agrega un índice principal a la tabla. *eExpression3* especifica la expresión de clave de índice principal y *TagName2* especifica el nombre de la etiqueta de índice principal. Los nombres de etiqueta de índice pueden contener hasta 10 caracteres. Si se omite la etiqueta *TagName2* y *eExpression3* es un campo único, la etiqueta del índice principal tiene el mismo nombre que el campo especificado en *eExpression3*.  
  
 QUITAR CLAVE PRINCIPAL  
 Quita el índice principal y su etiqueta de índice. Dado que una tabla solo puede tener una clave principal, no es necesario especificar el nombre de la clave principal. Al quitar el índice principal, también se eliminan todas las relaciones persistentes basadas en la clave principal.  
  
 AGREGAR *EEXPRESSION4* único [tag *TagName3*]  
 Agrega un índice candidato a la tabla. *eExpression4* especifica la expresión de clave de índice candidata y *TagName3* especifica el nombre de la etiqueta de índice candidata. Los nombres de etiqueta de índice pueden contener hasta 10 caracteres. Si omite la etiqueta *TagName3* y si *eExpression4* es un campo único, la etiqueta del índice candidato tiene el mismo nombre que el campo especificado en *eExpression4*.  
  
 DROP UNIQUE TAG *TagName4*  
 Quita el índice candidato y su etiqueta de índice. Dado que una tabla puede tener varias claves candidatas, debe especificar el nombre de la etiqueta de índice candidata.  
  
 Agregar etiqueta de clave externa [ *eExpression5*] *TagName4*  
 Agrega un índice externo (no principal) a la tabla. *eExpression5* especifica la expresión de clave de índice externa y *TagName4* especifica el nombre de la etiqueta de índice externo. Los nombres de etiqueta de índice pueden contener hasta 10 caracteres.  
  
 REFERENCEs *TableName2*[tag *TagName5*]  
 Especifica la tabla primaria en la que se establece una relación persistente. Incluya la etiqueta *TagName5* para establecer una relación basada en una etiqueta de índice existente para la tabla primaria. Los nombres de etiqueta de índice pueden contener hasta 10 caracteres. Si omite la etiqueta *TagName5*, la relación se establece mediante la etiqueta de índice principal de la tabla primaria.  
  
 QUITAR etiqueta de clave externa *TagName6*[Guardar]  
 Elimina una clave externa cuya etiqueta de índice es *TagName6*. Si omite SAVE, la etiqueta de índice se elimina del índice estructural. Incluya SAVE para evitar la eliminación de la etiqueta de índice del índice estructural.  
  
 Cambiar el nombre de la columna *FieldName4* a *FieldName5*  
 Permite cambiar el nombre de un campo de la tabla. *FieldName4* especifica el nombre del campo al que se cambia el nombre. *FieldName5* especifica el nuevo nombre del campo.  
  
> [!CAUTION]  
>  Tenga cuidado al cambiar el nombre de los campos de tabla porque las expresiones de índice, las reglas de validación de tablas y campos, los comandos y las funciones pueden hacer referencia a los nombres de campo originales.  
  
 Novalidate  
 Especifica que Visual FoxPro permite realizar cambios en la estructura de la tabla; Estos cambios podrían infringir la integridad de los datos de la tabla. De forma predeterminada, Visual FoxPro evita que ALTER TABLE realice cambios que infrinjan la integridad de los datos de la tabla. Incluya novalidate para invalidar este comportamiento predeterminado.  
  
## <a name="remarks"></a>Observaciones  
 ALTER TABLE se puede usar para modificar la estructura de una tabla que no se ha agregado a una base de datos. Sin embargo, Visual FoxPro genera un error si se incluyen las cláusulas DEFAULT, FOREIGN KEY, PRIMAry KEY, REFERENCEs o SET cuando se modifica una tabla libre.  
  
 ALTER TABLE puede volver a generar la tabla creando un nuevo encabezado de tabla y anexando registros al encabezado de la tabla. Por ejemplo, cambiar el tipo o el ancho de un campo podría hacer que se vuelva a generar la tabla.  
  
 Después de volver a generar una tabla, se ejecutan las reglas de validación de campos para los campos cuyo tipo o ancho se cambie. Si cambia el tipo o el ancho de cualquier campo de la tabla, se ejecuta la regla de la tabla.  
  
 Si modifica las reglas de validación de campos o tablas para una tabla que contiene registros, Visual FoxPro prueba las nuevas reglas de validación de tablas o campos con los datos existentes y emite una advertencia en la primera aparición de una regla de validación de tabla o de campo o de una infracción del desencadenador.  
  
 Si la tabla que se modifica está en una base de datos, ALTER TABLE-SQL requiere el uso exclusivo de la base de datos. Para abrir una base de datos para su uso exclusivo, incluya EXCLUSIVe en OPEN DATABASE.  
  
## <a name="see-also"></a>Consulte también  
 [Comando CREATE TABLE-SQL](../../odbc/microsoft/create-table-sql-command.md)   
 [Comando de índice](../../odbc/microsoft/index-command.md)
