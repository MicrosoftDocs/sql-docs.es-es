---
title: Inyección de código SQL | Microsoft Docs
description: Obtenga información sobre cómo funcionan los ataques por inyección de código SQL. Mitigue estos ataques validando la entrada y revisando el código para la inyección de código SQL en SQL Server.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: security, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- SQL Injection
ms.assetid: eb507065-ac58-4f18-8601-e5b7f44213ab
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 074033472f37787c0bcf1da6f261a5f835f0cbec
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838989"
---
# <a name="sql-injection"></a>Inyección de código SQL
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  La inyección de código SQL es un ataque en el que se inserta código malintencionado en cadenas que posteriormente se pasan a una instancia de SQL Server para su análisis y ejecución. Todos los procedimientos que generan instrucciones SQL deben revisarse en busca de vulnerabilidades de inyección de código, ya que SQL Server ejecutará todas las consultas recibidas que sean válidas desde el punto de vista sintáctico. Un atacante cualificado y con determinación puede manipular incluso os datos con parámetros.  
  
## <a name="how-sql-injection-works"></a>Funcionamiento de la inyección de código SQL  
 La forma principal de inyección de código SQL consiste en la inserción directa de código en variables especificadas por el usuario que se concatenan con comandos SQL y se ejecutan. Existe un ataque menos directo que inyecta código dañino en cadenas que están destinadas a almacenarse en una tabla o como metadatos. Cuando las cadenas almacenadas se concatenan posteriormente en un comando SQL dinámico, se ejecuta el código dañino.  
  
 El proceso de inyección consiste en finalizar prematuramente una cadena de texto y anexar un nuevo comando. Como el comando insertado puede contener cadenas adicionales que se hayan anexado al mismo antes de su ejecución, el atacante pone fin a la cadena inyectada con una marca de comentario "--". El texto situado a continuación se omite en tiempo de ejecución.  
  
 En el siguiente script se muestra una sencilla inyección de código SQL. El script crea una consulta SQL concatenando cadenas no modificables con una cadena especificada por el usuario:  
  
```csharp
var Shipcity;  
ShipCity = Request.form ("ShipCity");  
var sql = "select * from OrdersTable where ShipCity = '" + ShipCity + "'";  
```  
  
 Se le pide al usuario que escriba el nombre de una ciudad. Si el usuario especifica `Redmond`, la consulta ensamblada por el script presenta un aspecto similar al siguiente:  
  
```sql
SELECT * FROM OrdersTable WHERE ShipCity = 'Redmond'  
```  
  
 Sin embargo, suponga que el usuario especificase lo siguiente:  
  
```sql
Redmond'; drop table OrdersTable--  
```  
  
 En este caso, la siguiente consulta la ensambla el script:  
  
```sql
SELECT * FROM OrdersTable WHERE ShipCity = 'Redmond';drop table OrdersTable--'  
```  
  
 El punto y coma (;) denota el final de una consulta y el principio de otra. El guión doble (--) indica que el resto de la línea actual es un comentario y debe omitirse. Si el código modificado es sintácticamente correcto, el servidor lo ejecutará. Cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] procese esta instrucción, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] seleccionará en primer lugar todos los registros de `OrdersTable` donde `ShipCity` sea `Redmond`. A continuación, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] quitará `OrdersTable`.  
  
 Siempre y cuando el código SQL inyectado sea sintácticamente correcto, no será posible detectar alteraciones mediante programación. Por ello, debe validar todos los datos especificados por el usuario y revisar cuidadosamente el código que ejecute comandos SQL construidos en el servidor que utilice. Las prácticas recomendadas de codificación se describen en las siguientes secciones de este tema.  
  
## <a name="validate-all-input"></a>Validar todos los datos especificados por el usuario  
 Valide siempre los datos especificados por el usuario mediante comprobaciones de tipo, longitud, formato e intervalo. A la hora de implementar medidas de precaución frente a la especificación de datos dañinos, tenga en cuenta la arquitectura y los escenarios de implementación de la aplicación. Recuerde que los programas diseñados para ejecutarse en un entorno seguro pueden copiarse en un entorno no seguro. Las sugerencias que se muestran a continuación deben considerarse prácticas recomendadas:  
  
-   No haga suposiciones sobre el tamaño, tipo o contenido de los datos que recibirá la aplicación. Por ejemplo, debe hacer la siguiente evaluación:  
  
    -   Cómo se comportará la aplicación si un usuario (malicioso o no) especifica un archivo MPEG de 10 megabytes cuando la aplicación espera un código postal.  
  
    -   Cómo se comportará la aplicación si se inserta una instrucción `DROP TABLE` en un campo de texto  
  
-   Compruebe el tamaño y el tipo de los datos especificados y aplique unos límites adecuados. Esto puede impedir que se produzcan saturaciones deliberadas del búfer.  
  
-   Compruebe el contenido de las variables de cadena y acepte únicamente valores esperados. Rechace las especificaciones que contengan datos binarios, secuencias de escape y caracteres de comentario. Esto puede impedir la inyección de scripts y puede servir de protección frente a explotaciones de saturación del búfer.  
  
-   Cuando trabaje con documentos XML, valide todos los datos con respecto a su esquema a medida que se vayan indicando.  
  
-   No cree nunca instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] directamente a partir de datos indicados por el usuario.  
  
-   Utilice procedimientos almacenados para validar los datos indicados por el usuario.  
  
-   En entornos de varios niveles, todos los datos deben validarse antes de que se admitan en la zona de confianza. Los datos que no superen el proceso de validación deben rechazarse, y debe devolverse un error al nivel anterior.  
  
-   Implemente varias capas de validación. Las precauciones que tome contra usuarios malintencionados ocasionales pueden resultar ineficaces contra piratas informáticos con determinación. Lo más recomendable es validar los datos especificados por el usuario en la interfaz de usuario y, después, en todos los puntos posteriores en que atraviesen un límite de confianza.   
    Por ejemplo, la validación de datos en una aplicación del lado cliente puede evitar la inyección de scripts. Sin embargo, si en el siguiente nivel se asume que ya se ha validado la entrada, cualquier usuario malintencionado que sea capaz de eludir un cliente puede disfrutar de un acceso sin restricciones a un sistema.  
  
-   No concatene nunca datos especificados por el usuario que no se hayan validado. La concatenación de cadenas es el punto de entrada principal de una inyección de scripts.  
  
-   Las cadenas siguientes no se aceptan en campos desde los que se puedan construir nombres de archivo: AUX, CLOCK$, de COM1 hasta COM8, CON, CONFIG$, de LPT1 hasta LPT8, NUL y PRN.  
  
 Si es posible, rechace los datos que contengan los siguientes caracteres.  
  
|Carácter de entrada|Significado en Transact-SQL|  
|---------------------|------------------------------|  
|**;**|Delimitador de consultas.|  
|**'**|Delimitador de cadenas de datos de caracteres.|  
|**--**|Delimitador del comentario de una sola línea. El servidor no evalúa el texto siguiente **--** hasta el final de esa línea.|  
|**/\**_ ... _*\*/**|Delimitadores de comentarios. El servidor no evalúa el texto incluido entre **/\* *_ y _* \*/** .|  
|**xp_**|Se usa al principio del nombre de procedimientos almacenados extendidos de catálogo, como `xp_cmdshell`.|  
  
### <a name="use-type-safe-sql-parameters"></a>Usar parámetros SQL con seguridad de tipos  
 La colección **Parameters** de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona comprobación de tipos y validación de longitud. Si usa la colección **Parameters** , la entrada se considerará como un valor literal en lugar de código ejecutable. Un beneficio adicional de usar la colección **Parameters** es que puede exigir comprobaciones de tipos y de longitud. Los valores que no estén comprendidos en el intervalo desencadenarán una excepción. En el siguiente fragmento de código se muestra cómo usar la colección **Parameters** :  
  
```csharp
SqlDataAdapter myCommand = new SqlDataAdapter("AuthorLogin", conn);  
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure;  
SqlParameter parm = myCommand.SelectCommand.Parameters.Add("@au_id",  
     SqlDbType.VarChar, 11);  
parm.Value = Login.Text;  
```  
  
 En este ejemplo, el parámetro `@au_id` se considera un valor literal en lugar de código ejecutable. Se comprueba el tipo y la longitud de este valor. Si el valor de `@au_id` no cumple las restricciones especificadas de tipo y longitud, se producirá una excepción.  
  
### <a name="use-parameterized-input-with-stored-procedures"></a>Usar una entrada con parámetros con los procedimientos almacenados  
 Los procedimientos almacenados pueden ser susceptibles de una inyección de código SQL si utilizan una entrada sin filtrar. Por ejemplo, el código que se muestra a continuación es vulnerable:  
  
```csharp
SqlDataAdapter myCommand =   
new SqlDataAdapter("LoginStoredProcedure '" +   
                               Login.Text + "'", conn);  
```  
  
 Si utiliza procedimientos almacenados, deberá utilizar parámetros como entrada para dichos procedimientos.  
  
### <a name="use-the-parameters-collection-with-dynamic-sql"></a>Usar la colección Parameters con SQL dinámico  
 Si no puede usar procedimientos almacenados, de todos modos puede seguir usando parámetros, tal y como se muestra en el ejemplo de código siguiente.  
  
```csharp
SqlDataAdapter myCommand = new SqlDataAdapter(  
"SELECT au_lname, au_fname FROM Authors WHERE au_id = @au_id", conn);  
SQLParameter parm = myCommand.SelectCommand.Parameters.Add("@au_id",   
                        SqlDbType.VarChar, 11);  
Parm.Value = Login.Text;  
```  
  
### <a name="filtering-input"></a>Filtrar la entrada  
 Filtrar la entrada también puede ser útil para protegerse frente a inyecciones de código SQL mediante la eliminación de los caracteres de escape. Sin embargo, debido al gran número de caracteres que pueden presentar problemas, no se trata de una defensa confiable. El siguiente ejemplo busca el delimitador de cadenas de caracteres.  
  
```csharp
private string SafeSqlLiteral(string inputSQL)  
{  
  return inputSQL.Replace("'", "''");  
}  
```  
  
### <a name="like-clauses"></a>Cláusulas LIKE  
 Tenga en cuenta que si usa una cláusula `LIKE` , los caracteres comodín seguirán necesitando usar caracteres de escape:  
  
```csharp
s = s.Replace("[", "[[]");  
s = s.Replace("%", "[%]");  
s = s.Replace("_", "[_]");  
```  
  
## <a name="reviewing-code-for-sql-injection"></a>Revisar el código para la inyección de código SQL  
 Debe revisar todo el código que llama a `EXECUTE`, `EXEC`o `sp_executesql`. Puede utilizar consultas similares a las siguientes para ayudarle a identificar los procedimientos que contienen estas instrucciones. Esta consulta comprueba si hay 1, 2, 3 o 4 espacios después de las palabras `EXECUTE` o `EXEC`.  
  
```sql
SELECT object_Name(id) FROM syscomments  
WHERE UPPER(text) LIKE '%EXECUTE (%'  
OR UPPER(text) LIKE '%EXECUTE  (%'  
OR UPPER(text) LIKE '%EXECUTE   (%'  
OR UPPER(text) LIKE '%EXECUTE    (%'  
OR UPPER(text) LIKE '%EXEC (%'  
OR UPPER(text) LIKE '%EXEC  (%'  
OR UPPER(text) LIKE '%EXEC   (%'  
OR UPPER(text) LIKE '%EXEC    (%'  
OR UPPER(text) LIKE '%SP_EXECUTESQL%';  
```  
  
### <a name="wrapping-parameters-with-quotename-and-replace"></a>Incluir parámetros en QUOTENAME() y REPLACE()  
 En cada procedimiento almacenado seleccionado, compruebe que todas las variables que se usan en Transact-SQL dinámico se administran correctamente. Los datos procedentes de los parámetros de entrada del procedimiento almacenado o que se han leído desde una tabla deben estar incluidos en QUOTENAME() o REPLACE(). Recuerde que el valor de @variable que se pasa a QUOTENAME() es de tipo sysname y tiene una longitud máxima de 128 caracteres.  
  
|\@variable|Contenedor recomendado|  
|---------------|-------------------------|  
|Nombre de un elemento protegible|`QUOTENAME(@variable)`|  
|Cadena de ≤128 caracteres|`QUOTENAME(@variable, '''')`|  
|Cadena de > 128 caracteres|`REPLACE(@variable,'''', '''''')`|  
  
 Cuando se utiliza esta técnica, se pueden revisar las instrucciones SET como se indica a continuación:  
  
```sql
-- Before:  
SET @temp = N'SELECT * FROM authors WHERE au_lname ='''   
 + @au_lname + N'''';  
  
-- After:  
SET @temp = N'SELECT * FROM authors WHERE au_lname = '''   
 + REPLACE(@au_lname,'''','''''') + N'''';  
```  
  
### <a name="injection-enabled-by-data-truncation"></a>Inyección habilitada mediante truncamiento de datos  
 Cualquier [!INCLUDE[tsql](../../includes/tsql-md.md)] dinámico que esté asignado a una variable se truncará si es mayor que el búfer asignado para dicha variable. Un atacante que pueda forzar el truncamiento de instrucciones pasando cadenas largas de forma inesperada a un procedimiento almacenado puede manipular el resultado. Por ejemplo, el procedimiento almacenado creado por el siguiente script es vulnerable a la inyección habilitada mediante truncamiento.  
  
```sql
CREATE PROCEDURE sp_MySetPassword  
@loginname sysname,  
@old sysname,  
@new sysname  
AS  
-- Declare variable.  
-- Note that the buffer here is only 200 characters long.   
DECLARE @command varchar(200)  
-- Construct the dynamic Transact-SQL.  
-- In the following statement, we need a total of 154 characters   
-- to set the password of 'sa'.   
-- 26 for UPDATE statement, 16 for WHERE clause, 4 for 'sa', and 2 for  
-- quotation marks surrounded by QUOTENAME(@loginname):  
-- 200 - 26 - 16 - 4 - 2 = 154.  
-- But because @new is declared as a sysname, this variable can only hold  
-- 128 characters.   
-- We can overcome this by passing some single quotation marks in @new.  
SET @command= 'update Users set password='   
    + QUOTENAME(@new, '''') + ' where username='   
    + QUOTENAME(@loginname, '''') + ' AND password = '   
    + QUOTENAME(@old, '''')  
  
-- Execute the command.  
EXEC (@command)  
GO  
```  
  
 Al pasar 154 caracteres a un búfer de 128 caracteres, un atacante puede establecer una nueva contraseña para sa sin conocer la antigua.  
  
```sql
EXEC sp_MySetPassword 'sa', 'dummy',   
'123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012'''''''''''''''''''''''''''''''''''''''''''''''''''   
```  
  
 Por este motivo, debe usar un búfer de gran tamaño para una variable de comandos o ejecutar directamente [!INCLUDE[tsql](../../includes/tsql-md.md)] dinámico dentro de la instrucción `EXECUTE` .  
  
### <a name="truncation-when-quotenamevariable--and-replace-are-used"></a>Truncamiento cuando se usan QUOTENAME(@variable, '''') y REPLACE()  
 Las cadenas que devuelven QUOTENAME() y REPLACE() se truncarán sin avisar si exceden el espacio asignado. El procedimiento almacenado que se crea en el siguiente ejemplo muestra lo que puede suceder.  
  
```sql
CREATE PROCEDURE sp_MySetPassword  
    @loginname sysname,  
    @old sysname,  
    @new sysname  
AS  
  
-- Declare variables.  
    DECLARE @login sysname  
    DECLARE @newpassword sysname  
    DECLARE @oldpassword sysname  
    DECLARE @command varchar(2000)  
  
-- In the following statements, the data stored in temp variables  
-- will be truncated because the buffer size of @login, @oldpassword,  
-- and @newpassword is only 128 characters, but QUOTENAME() can return  
-- up to 258 characters.  
    SET @login = QUOTENAME(@loginname, '''')  
    SET @oldpassword = QUOTENAME(@old, '''')  
    SET @newpassword = QUOTENAME(@new, '''')  
  
-- Construct the dynamic Transact-SQL.  
-- If @new contains 128 characters, then @newpassword will be '123... n  
-- where n is the 127th character.   
-- Because the string returned by QUOTENAME() will be truncated,   
-- it can be made to look like the following statement:  
-- UPDATE Users SET password ='1234. . .[127] WHERE username=' -- other stuff here  
    SET @command = 'UPDATE Users set password = ' + @newpassword   
     + ' where username =' + @login + ' AND password = ' + @oldpassword;  
  
-- Execute the command.  
EXEC (@command);  
GO  
```  
  
 Por ello, la instrucción siguiente establecerá las contraseñas de todos los usuarios en el valor que se pasó en el código anterior.  
  
```sql
EXEC sp_MyProc '--', 'dummy', '12345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678'  
```  
  
 Puede forzar el truncamiento de cadenas al exceder el espacio en el búfer asignado cuando utiliza REPLACE(). El procedimiento almacenado que se crea en el siguiente ejemplo muestra lo que puede suceder.  
  
```sql
CREATE PROCEDURE sp_MySetPassword  
    @loginname sysname,  
    @old sysname,  
    @new sysname  
AS  
  
-- Declare variables.  
    DECLARE @login sysname  
    DECLARE @newpassword sysname  
    DECLARE @oldpassword sysname  
    DECLARE @command varchar(2000)  
  
-- In the following statements, data will be truncated because   
-- the buffers allocated for @login, @oldpassword and @newpassword   
-- can hold only 128 characters, but QUOTENAME() can return   
-- up to 258 characters.   
    SET @login = REPLACE(@loginname, '''', '''''')  
    SET @oldpassword = REPLACE(@old, '''', '''''')  
    SET @newpassword = REPLACE(@new, '''', '''''')  
  
-- Construct the dynamic Transact-SQL.  
-- If @new contains 128 characters, @newpassword will be '123...n   
-- where n is the 127th character.   
-- Because the string returned by QUOTENAME() will be truncated, it  
-- can be made to look like the following statement:  
-- UPDATE Users SET password='1234...[127] WHERE username=' -- other stuff here   
    SET @command= 'update Users set password = ''' + @newpassword + ''' where username='''   
     + @login + ''' AND password = ''' + @oldpassword + '''';  
  
-- Execute the command.  
EXEC (@command);  
GO  
```  
  
 Como en el caso de QUOTENAME(), se puede evitar el truncamiento de cadenas por REPLACE() al declarar variables temporales que sean lo suficientemente grandes para todos los casos. Si es posible, debe llamar a QUOTENAME() o REPLACE() directamente dentro de [!INCLUDE[tsql](../../includes/tsql-md.md)]dinámico. De lo contrario, puede calcular el tamaño de búfer necesario del siguiente modo. Para `@outbuffer = QUOTENAME(@input)`, el tamaño de `@outbuffer` debe ser `2*(len(@input)+1)`. Cuando usa `REPLACE()` y comillas dobles, como en el ejemplo anterior, bastará con un búfer de `2*len(@input)` .  
  
 En el siguiente cálculo se cubren todos los casos:  
  
```sql
WHILE LEN(@find_string) > 0, required buffer size =  
ROUND(LEN(@input)/LEN(@find_string),0) * LEN(@new_string)   
 + (LEN(@input) % LEN(@find_string))  
```  
  
### <a name="truncation-when-quotenamevariable--is-used"></a>Truncamiento cuando se usa QUOTENAME(@variable, ']')  
 El truncamiento se puede producir cuando el nombre de un elemento protegible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se pasa a las instrucciones que utilizan el formato `QUOTENAME(@variable, ']')`. Esto se muestra en el ejemplo siguiente.  
  
```sql
CREATE PROCEDURE sp_MyProc  
    @schemaname sysname,  
    @tablename sysname,  
AS  
  
-- Declare a variable as sysname. The variable will be 128 characters.  
-- But @objectname actually must allow for 2*258+1 characters.   
DECLARE @objectname sysname  
SET @objectname = QUOTENAME(@schemaname)+'.'+ QUOTENAME(@tablename)   
-- Do some operations.  
GO  
```  
  
 Cuando se concatenan valores de tipo sysname, se deben usar variables temporales que sean lo suficientemente grandes como para contener el máximo de 128 caracteres por valor. Si es posible, se debe llamar a `QUOTENAME()` directamente dentro del [!INCLUDE[tsql](../../includes/tsql-md.md)]dinámico. De lo contrario, puede calcular el tamaño de búfer necesario como se explica en la sección anterior.  
  
## <a name="see-also"></a>Consulte también  
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)   
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)   
 [sp_executesql &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md)   
 [Proteger SQL Server](../../relational-databases/security/securing-sql-server.md)  
  
  
