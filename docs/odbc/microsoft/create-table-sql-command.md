---
description: Crear tabla - comando SQL
title: Comando CREATE TABLE-SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- CREATE TABLE [ODBC]
ms.assetid: be2143ba-fc16-42c9-84f7-8985cd924860
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0fb1e91c03f7a1a1fee98ce887be991370450999
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183203"
---
# <a name="create-table---sql-command"></a>Crear tabla - comando SQL
Crea una tabla con los campos especificados.  
  
 El controlador ODBC de Visual FoxPro admite la sintaxis nativa del lenguaje Visual FoxPro para este comando. Para obtener información específica del controlador, consulte los **comentarios del controlador**.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
CREATE TABLE | DBF TableName1 [NAME LongTableName] [FREE]  
   (FieldName1FieldType [(nFieldWidth [, nPrecision])]  
      [NULL | NOT NULL]   
      [CHECK lExpression1 [ERROR cMessageText1]]  
      [DEFAULT eExpression1]  
      [PRIMARY KEY | UNIQUE]  
      [REFERENCES TableName2 [TAG TagName1]]  
      [NOCPTRANS]  
   [, FieldName2 ...]  
      [, PRIMARY KEY eExpression2 TAG TagName2  
      |, UNIQUE eExpression3 TAG TagName3]  
      [, FOREIGN KEY eExpression4 TAG TagName4 [NODUP]  
            REFERENCES TableName3 [TAG TagName5]]  
      [, CHECK lExpression2 [ERROR cMessageText2]])  
| FROM ARRAY ArrayName  
```  
  
## <a name="arguments"></a>Argumentos  
 CREATE TABLE &#124; DBF *TableName1*  
 Especifica el nombre de la tabla que se va a crear. Las opciones TABLE y DBF son idénticas.  
  
 NOMBRE *LongTableName*  
 Especifica un nombre largo para la tabla. Solo se puede especificar un nombre de tabla largo cuando una base de datos está abierta, ya que los nombres de tabla largos se almacenan en bases de datos.  
  
 Los nombres largos pueden contener hasta 128 caracteres y se pueden usar en lugar de nombres de archivo cortos en la base de datos.  
  
 FREE  
 Especifica que la tabla no se agregará a una base de datos abierta. FREE no es necesario si una base de datos no está abierta.  
  
 *(FieldName1 FieldType* [( *NFieldWidth* [, *nPrecision*])]  
 Especifica el nombre del campo, el tipo de campo, el ancho de campo y la precisión del campo (número de posiciones decimales), respectivamente.  
  
 *FieldType* es una sola letra que indica el [tipo de datos](../../odbc/microsoft/visual-foxpro-field-data-types.md)del campo. Algunos tipos de datos de campo requieren que especifique *nFieldWidth* o *nPrecision* o ambos.  
  
 *nFieldWidth* y *nPrecision* se omiten para los tipos D, G, I, L, M, P, T e y. de forma predeterminada, *nPrecision* se establece en cero (sin posiciones decimales) si *nPrecision* no se incluye para los tipos B, F o N.  
  
 NULL  
 Permite valores NULL en el campo.  
  
 NOT NULL  
 Evita valores NULL en el campo.  
  
 Si omite NULL y NOT NULL, el valor actual de SET NULL determina si se permiten valores NULL en el campo. Sin embargo, si omite NULL y NOT NULL e incluye la cláusula PRIMAry KEY o UNIQUE, se omite la configuración actual de SET NULL y el valor predeterminado del campo es NOT NULL.  
  
 COMPROBAR *lExpression1*  
 Especifica una regla de validación para el campo. *lExpression1* puede ser una función definida por el usuario. Siempre que se anexa un registro en blanco, se comprueba la regla de validación. Se genera un error si la regla de validación no permite un valor de campo en blanco en un registro anexado.  
  
 ERROR *cMessageText1*  
 Especifica el mensaje de error que se muestra en Visual FoxPro cuando la regla de campo genera un error. El mensaje solo se muestra cuando se cambian los datos en una ventana de exploración o en una ventana de edición.  
  
 *EEXPRESSION1* predeterminado  
 Especifica un valor predeterminado para el campo. El tipo de datos de *eExpression1* debe ser el mismo que el tipo de datos del campo.  
  
 PRIMARY KEY  
 Crea un índice principal para el campo. La etiqueta del índice principal tiene el mismo nombre que el campo.  
  
 UNIQUE  
 Crea un índice candidato para el campo. La etiqueta de índice candidata tiene el mismo nombre que el campo.  
  
> [!NOTE]  
>  Los índices candidatos (creados mediante la inclusión de la opción UNIQUE en CREATE TABLE o ALTER TABLE-SQL) no son los mismos que los índices creados con la opción UNIQUE en el comando INDEX. Un índice creado con la opción UNIQUE en el comando INDEX permite claves de índice duplicadas; los índices candidatos no permiten claves de índice duplicadas. Vea [index](../../odbc/microsoft/index-command.md) para obtener información adicional sobre su opción única.  
  
 Los valores NULL y los registros duplicados no se permiten en un campo que se usa para un índice principal o candidato. Sin embargo, Visual FoxPro no generará un error si crea un índice principal o candidato para un campo que admita valores NULL. Visual FoxPro generará un error si intenta especificar un valor null o duplicado en un campo que se usa para un índice principal o candidato.  
  
 REFERENCEs *TableName2*[tag *TagName1*]  
 Especifica la tabla primaria en la que se establece una relación persistente. Si omite la etiqueta *TagName1*, la relación se establece mediante la clave de índice principal de la tabla primaria. Si la tabla primaria no tiene un índice principal, Visual FoxPro genera un error.  
  
 Incluya la etiqueta *TagName1* para establecer una relación basada en una etiqueta de índice existente para la tabla primaria. Los nombres de etiqueta de índice pueden contener hasta 10 caracteres.  
  
 La tabla primaria no puede ser una tabla libre.  
  
 NOCPTRANS  
 Impide la traducción a una página de códigos diferente para los campos de caracteres y de memorando. Si la tabla se convierte en otra página de códigos, los campos para los que se ha especificado NOCPTRANS no se convierten. NOCPTRANS solo se puede especificar para los campos de caracteres y de memorando.  
  
 En el ejemplo siguiente se crea una tabla denominada MyTable que contiene dos campos de caracteres y dos campos memo. El segundo campo de carácter, char2, y el segundo campo de memorando, memo2, incluyen NOCPTRANS para evitar la traducción.  
  
```  
CREATE TABLE mytable (char1 C(10), char2 C(10) NOCPTRANS,;  
   memo1 M, memo2 M NOCPTRANS)  
```  
  
 CLAVE principal *eExpression2* etiqueta *TagName2*  
 Especifica un índice principal que se va a crear. *eExpression2* especifica cualquier campo o combinación de campos de la tabla. TAG *TagName2* especifica el nombre de la etiqueta de índice principal que se crea. Los nombres de etiqueta de índice pueden contener hasta 10 caracteres.  
  
 Dado que una tabla solo puede tener un índice principal, no puede incluir esta cláusula si ya ha creado un índice principal para un campo. Visual FoxPro genera un error si se incluye más de una cláusula de clave principal en CREATE TABLE.  
  
 *TagName3* de etiqueta *eExpression3* única  
 Crea un índice candidato. *eExpression3* especifica cualquier campo o combinación de campos de la tabla. Sin embargo, si ha creado un índice principal con una de las opciones de clave principal, no puede incluir el campo que se especificó para el índice principal. TAG *TagName3* especifica un nombre de etiqueta para la etiqueta de índice candidata que se crea. Los nombres de etiqueta de índice pueden contener hasta 10 caracteres.  
  
 Una tabla puede tener varios índices candidatos.  
  
 CLAVE externa *eExpression4* etiqueta *TagName4*[NODUP]  
 Crea un índice externo (no principal) y establece una relación con una tabla primaria. *eExpression4* especifica la expresión de clave de índice externa y *TagName4* especifica el nombre de la etiqueta de clave de índice externa que se crea. Los nombres de etiqueta de índice pueden contener hasta 10 caracteres. Incluya NODUP para crear un índice externo candidato.  
  
 Puede crear varios índices externos para la tabla, pero las expresiones Foreign index deben especificar distintos campos en la tabla.  
  
 REFERENCEs *TableName3*[tag *TagName5*]  
 Especifica la tabla primaria en la que se establece una relación persistente. Incluya la etiqueta *TagName5* para establecer una relación basada en una etiqueta de índice para la tabla primaria. Los nombres de etiqueta de índice pueden contener hasta 10 caracteres. De forma predeterminada, si se omite la etiqueta *TagName5,* la relación se establece mediante la clave de índice principal de la tabla primaria.  
  
 COMPROBAR *eExpression2*[error *cMessageText2*]  
 Especifica la regla de validación de la tabla. ERROR *cMessageText2* especifica el mensaje de error que Visual FoxPro muestra cuando se ejecuta la regla de validación de tabla. El mensaje solo se muestra cuando se cambian los datos en una ventana examinar o en una ventana de edición.  
  
 FROM ARRAY *ArrayName*  
 Especifica el nombre de una matriz existente cuyo contenido es el nombre, el tipo, la precisión y la escala de cada campo de la tabla. El contenido de la matriz se puede definir con la función **AFIELDS**().  
  
## <a name="remarks"></a>Observaciones  
 La nueva tabla se abre en el área de trabajo más baja disponible y se puede tener acceso a ella mediante su alias. La nueva tabla se abre exclusivamente, independientemente de la configuración actual de SET EXCLUSIVe.  
  
 Si una base de datos está abierta y no incluye la cláusula FREE, la nueva tabla se agrega a la base de datos. No se puede crear una nueva tabla con el mismo nombre que una tabla de la base de datos.  
  
 Si una base de datos está abierta, CREATE TABLE-SQL requiere el uso exclusivo de la base de datos. Para abrir una base de datos para su uso exclusivo, incluya EXCLUSIVe en OPEN DATABASE.  
  
 Si una base de datos no está abierta cuando se crea la nueva tabla, incluidas las cláusulas NAME, CHECK, DEFAULT, FOREIGN KEY, PRIMAry KEY o REFERENCEs, se genera un error.  
  
> [!NOTE]  
>  CREATE TABLE sintaxis utiliza comas para separar ciertas opciones de CREATE TABLE. Además, las cláusulas NULL, NOT NULL, CHECK, DEFAULT, PRIMAry KEY y UNIQUE deben colocarse dentro de los paréntesis que contienen las definiciones de columna.  
  
## <a name="driver-remarks"></a>Notas del controlador  
 Cuando la aplicación envía la instrucción SQL de ODBC CREATE TABLE al origen de datos, el controlador ODBC de Visual FoxPro traduce el comando en el comando de la tabla FoxProCREATE de visual con la sintaxis que se muestra en la tabla siguiente.  
  
|Sintaxis de ODBC|Sintaxis de Visual FoxPro|  
|-----------------|--------------------------|  
|CREATE TABLE *nombre de tabla base*<br /><br /> (*tipo de datos de identificador de columna*<br /><br /> [NOT NULL]<br /><br /> [,*tipo de datos de identificador de columna*<br /><br /> [NOT NULL]...)|CREATE TABLE *TableName1* [nombre *LongTableName*]<br /><br /> (*FieldName1* *FieldType*<br /><br /> [(*nFieldWidth* [, *nPrecision*])]<br /><br /> [NOT NULL])|  
  
 Cuando se crea una tabla mediante el controlador, el controlador cierra la tabla inmediatamente después de la creación para permitir el acceso a la tabla por parte de otros usuarios. Esto difiere de Visual FoxPro, que deja la tabla abierta exclusivamente en el momento de su creación. Sin embargo, si se ejecuta un procedimiento almacenado en el origen de datos que contiene una instrucción CREATE TABLE, la tabla se deja abierta.  
  
 Si el origen de datos es una base de datos (archivo. DBC), el controlador ODBC de Visual FoxPro crea una tabla denominada *LongTableName* con el mismo nombre que el nombre de la *tabla base*.  
  
### <a name="using-data-definition-language-ddl"></a>Usar el lenguaje de definición de datos (DDL)  
 No se puede incluir DDL en los lugares siguientes:  
  
-   En una instrucción SQL por lotes que requiere una transacción  
  
-   En el modo de confirmación manual, después de una instrucción que requiera una transacción, a menos que la aplicación llame primero a **SQLTransact**.  
  
 Por ejemplo, si desea crear una tabla temporal, debe crear la tabla antes de comenzar la instrucción que requiere una transacción. Si incluye la instrucción CREATE TABLE en una instrucción SQL por lotes que requiera una transacción, el controlador devolverá un mensaje de error.  
  
## <a name="see-also"></a>Consulte también  
 [ALTER TABLE-comando SQL](../../odbc/microsoft/alter-table-sql-command.md)   
 [Tipos de datos admitidos (controlador ODBC de Visual FoxPro)](../../odbc/microsoft/supported-data-types-visual-foxpro-odbc-driver.md)   
 [Comando INSERT-SQL](../../odbc/microsoft/insert-sql-command.md)   
 [Seleccione - comando SQL](../../odbc/microsoft/select-sql-command.md)
