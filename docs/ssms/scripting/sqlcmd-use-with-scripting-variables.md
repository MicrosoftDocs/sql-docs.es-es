---
title: Usar sqlcmd con variables de script
description: Aprenda a usar variables de scripting para crear un script que se pueda usar en varios escenarios.
ms.custom: seo-lt-2019
ms.date: 08/09/2016
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- scripts [SQL Server], sqlcmd utility
- variables [SQL Server], scripts
- scripting variables [SQL Server]
- sqlcmd utility, scripts
- setvar command
ms.assetid: 793495ca-cfc9-498d-8276-c44a5d09a92c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 799bc2eca98002343d8ff755782d8f3b71d2af77
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342647"
---
# <a name="sqlcmd---use-with-scripting-variables"></a>sqlcmd - Usar sqlcmd con variables de script
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Las variables que se usan en scripts se denominan variables de scripting. Las variables de scripting posibilitan el uso de un script en varias situaciones. Por ejemplo, si desea ejecutar un script en varios servidores, en lugar de modificar el script para cada servidor, puede usar una variable de scripting para el nombre del servidor. Al cambiar el nombre del servidor proporcionado a la variable de scripting, el mismo script puede ejecutarse en diferentes servidores.  
  
 Las variables de scripting pueden definirse explícitamente mediante el comando **setvar** o implícitamente mediante la opción **sqlcmd -v**.  
  
 Además, en este tema se proporcionan ejemplos en los que se definen variables de entorno en el símbolo del sistema Cmd.exe mediante **SET**.  
  
## <a name="setting-scripting-variables-by-using-the-setvar-command"></a>Definir variables de scripting mediante el comando setvar  
 El comando **setvar** se usa para definir variables de scripting. Las variables que se definen mediante el comando **setvar** se almacenan internamente. Las variables de scripting no deben confundirse con las variables de entorno que se definen en el símbolo del sistema mediante **SET**. Si un script hace referencia a una variable que no es de entorno o que no está definida mediante **setvar**, aparece un mensaje de error y se detiene la ejecución del script. Para obtener más información, vea la opción **-b** en [sqlcmd Utility](../../tools/sqlcmd-utility.md).  
  
## <a name="variable-precedence-low-to-high"></a>Precedencia de variables (menor a mayor)  
 Si hay más de un tipo de variable con el mismo nombre, se usa la variable que tenga la prioridad más alta.  
  
1.  Variables de entorno del nivel del sistema  
  
2.  Variables de entorno del nivel del usuario  
  
3.  Shell de comandos (**SET X=Y**) establecido en el símbolo del sistema antes de iniciar **sqlcmd**  
  
4.  **sqlcmd -v** X=Y  
  
5.  **:Setvar** X Y  
  
> [!NOTE]  
>  Para ver las variables de entorno, en el **Panel de control**, abra **Sistema** y haga clic en la pestaña **Opciones avanzadas** .  
  
## <a name="implicitly-setting-scripting-variables"></a>Definir las variables de scripting implícitamente  
 Al iniciar **sqlcmd** con una opción que tiene una variable **sqlcmd** relacionada, la variable **sqlcmd** se establece implícitamente en el valor especificado mediante la opción. En el siguiente ejemplo, `sqlcmd` se inicia con la opción `-l` . Esto establece implícitamente la variable SQLLOGINTIMEOUT.  
  
```
c:\> sqlcmd -l 60
```
 
También puede usar la opción **-v** para establecer una variable de scripting que existe en un script. En el script siguiente (el nombre de archivo es `testscript.sql`), `ColumnName` es una variable de scripting.  
 
```
USE AdventureWorks2012;

SELECT x.$(ColumnName)
FROM Person.Person x
WHERE x.BusinessEntityID < 5;
```

Después, puede especificar el nombre de la columna que desea que se devuelva mediante la opción `-v` :  
 
```
sqlcmd -v ColumnName ="FirstName" -i c:\testscript.sql
```

Para devolver otra columna usando el mismo script, cambie el valor de la variable de scripting `ColumnName` .  
  
```
sqlcmd -v ColumnName ="LastName" -i c:\testscript.sql
```

## <a name="guidelines-for-scripting-variable-names-and-values"></a>Directrices para los nombres y valores de variable de script  
 Al especificar un nombre para las variables de script, tenga en cuenta lo siguiente:  
  
-   Los nombres de variable no deben contener caracteres de espacio en blanco ni comillas.  
  
-   Los nombres de variable no deben tener la misma forma que una expresión variable, como *$(var)* .  
  
-   Las variables de scripting no distinguen entre mayúsculas y minúsculas.  
  
    > [!NOTE]  
    >  Si no se asigna ningún valor a una variable de entorno **sqlcmd** , se quita la variable. Si se usa **:setvar VarName** sin ningún valor, la variable se borra.  
  
 Al especificar valores para las variables de scripting, tenga en cuenta lo siguiente:  
  
-   Los valores de variable definidos mediante **setvar** o la opción **-v** deben ir entre comillas si el valor de cadena contiene espacios.  
  
-   Si las comillas forman parte del valor de variable, es necesario usar un carácter de escape. Por ejemplo: :`setvar MyVar "spac""e"`.  
  
## <a name="guidelines-for-cmdexe-set-variable-values-and-names"></a>Directrices para los nombres y valores de la variable SET de Cmd.exe  
 Las variables que se definen mediante SET forman parte del entorno de Cmd.exe y se puede hacer referencia a ellas mediante **sqlcmd**. Tenga en cuenta las directrices siguientes:  
  
-   Los nombres de variable no deben contener caracteres de espacio en blanco ni comillas.  
  
-   Los valores de variable pueden contener caracteres de espacio en blanco o comillas.  
  
## <a name="sqlcmd-scripting-variables"></a>Variables de scripting sqlcmd  
 Las variables definidas mediante **sqlcmd** se denominan variables de scripting. En la siguiente tabla se enumeran las variables de scripting de **sqlcmd** .  
  
|        Variable         | Opción relacionada | L/E |         Valor predeterminado         |
| ----------------------- | -------------- | --- | ----------------------- |
| SQLCMDUSER\*             | -U             | R   | ""                      |
| SQLCMDPASSWORD\*         | -P             | --  | ""                      |
| SQLCMDSERVER\*           | -S             | R   | "DefaultLocalInstance"  |
| SQLCMDWORKSTATION       | -H             | R   | "ComputerName"          |
| SQLCMDDBNAME            | -d             | R   | ""                      |
| SQLCMDLOGINTIMEOUT      | -l             | L/E | "8" (segundos)           |
| SQLCMDSTATTIMEOUT       | -T             | L/E | "0" = esperar indefinidamente |
| SQLCMDHEADERS           | -H             | L/E | "0"                     |
| SQLCMDCOLSEP            | -S             | L/E | " "                     |
| SQLCMDCOLWIDTH          | -w             | L/E | "0"                     |
| SQLCMDPACKETSIZE        | -a             | R   | "4096"                  |
| SQLCMDERRORLEVEL        | -M             | L/E | "0"                     |
| SQLCMDMAXVARTYPEWIDTH   | -y             | L/E | "256"                   |
| SQLCMDMAXFIXEDTYPEWIDTH | -y             | L/E | "0" = ilimitado         |
| SQLCMDEDITOR            |                | L/E | "edit.com"              |
| SQLCMDINI               |                | R   | ""                      |

SQLCMDUSER, SQLCMDPASSWORD y SQLCMDSERVER se establecen cuando se usa **:Connect** .  

L indica que el valor solo puede establecerse una vez durante la inicialización del programa.  
  
L/E indica que el valor puede volver a definirse mediante el comando **setvar** y los comandos siguientes usarán el nuevo valor.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-the-setvar-command-in-a-script"></a>A. Usar el comando setvar en un script  
 Muchas opciones de **sqlcmd** pueden controlarse en un script mediante el comando **setvar** . En el siguiente ejemplo, se crea el script `test.sql` ; en él, la variable `SQLCMDLOGINTIMEOUT` está establecida en `60` segundos y otra variable de scripting, `server`, está establecida en `testserver`. El siguiente código está en `test.sql`.  

```
:setvar SQLCMDLOGINTIMEOUT 60
:setvar server "testserver"
:connect $(server) -l $(SQLCMDLOGINTIMEOUT)

USE AdventureWorks2012;

SELECT FirstName, LastName
FROM Person.Person;
```

Se llama al script mediante sqlcmd:

```
sqlcmd -i c:\test.sql
```
  
### <a name="b-using-the-setvar-command-interactively"></a>B. Usar el comando setvar interactivamente  
 En el ejemplo siguiente se muestra cómo establecer una variable de script de manera interactiva mediante el comando `setvar` .  

```
sqlcmd
:setvar  MYDATABASE AdventureWorks2012
USE $(MYDATABASE);
GO
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Changed database context to 'AdventureWorks2012'
1>
```
  
### <a name="c-using-command-prompt-environment-variables-within-sqlcmd"></a>C. Usar variables de entorno del símbolo del sistema en sqlcmd  
 En el ejemplo siguiente, se establecen cuatro variables de entorno `are` , a las que luego se llama desde `sqlcmd`.  

```
C:\>SET tablename=Person.Person
C:\>SET col1=FirstName
C:\>SET col2=LastName
C:\>SET title=Ms.
C:\>sqlcmd -d AdventureWorks2012
1> SELECT TOP 5 $(col1) + ' ' + $(col2) AS Name
2> FROM $(tablename)
3> WHERE Title ='$(title)'
4> GO
```
  
### <a name="d-using-user-level-environment-variables-within-sqlcmd"></a>D. Usar variables de entorno de usuario en sqlcmd  
 En el ejemplo siguiente, se establece la variable de entorno de usuario `%Temp%` en el símbolo del sistema y se pasa al archivo de entrada de `sqlcmd` . Para obtener la variable de entorno de nivel de usuario, en el **Panel de control**, haga doble clic en **Sistema**. Haga clic en la pestaña **Avanzadas** y, a continuación, en **Variables de entorno**.  
  
 El código siguiente se encuentra en el archivo de entrada `c:\testscript.txt`:

```
:OUT $(MyTempDirectory)
USE AdventureWorks2012;

SELECT FirstName
FROM AdventureWorks2012.Person.Person
WHERE BusinessEntityID` `< 5;
```

El código siguiente se escribe en el símbolo del sistema:

```
C:\ >SET MyTempDirectory=%Temp%\output.txt
C:\ >sqlcmd -i C:\testscript.txt
```

 El resultado siguiente se envía al archivo de salida C:\Documents and Settings\\<usuario\>\Local Settings\Temp\output.txt.  

```
Changed database context to 'AdventureWorks2012'.
FirstName
--------------------------------------------------
Gustavo
Catherine
Kim
Humberto

(4 rows affected)
```

### <a name="e-using-a-startup-script"></a>E. Usar un script de inicio  
 Un script de inicio **sqlcmd** se ejecuta al iniciar **sqlcmd** . En el ejemplo siguiente se establece la variable de entorno `SQLCMDINI`. Este es el contenido de `init.sql.`  

```
SET NOCOUNT ON
GO

DECLARE @nt_username nvarchar(128)
SET @nt_username = (SELECT rtrim(convert(nvarchar(128), nt_username))
FROM sys.dm_exec_sessions WHERE spid = @@SPID)
SELECT  @nt_username + ' is connected to ' +
rtrim(CONVERT(nvarchar(20), SERVERPROPERTY('servername'))) +
' (' +`  
rtrim(CONVERT(nvarchar(20), SERVERPROPERTY('productversion'))) +
')'
:setvar SQLCMDMAXFIXEDTYPEWIDTH 100
SET NOCOUNT OFF
GO

:setvar SQLCMDMAXFIXEDTYPEWIDTH
```

 De esta manera, se llama al archivo `init.sql` cuando se inicia `sqlcmd` .  
  
```
c:\> SET sqlcmdini=c:\init.sql
>1 Sqlcmd
```

 Este es el resultado.  

```
>1 < user > is connected to < server > (9.00.2047.00)
```

  
> [!NOTE]  
>  La opción **-X** deshabilita la característica de script de inicio.  
  
### <a name="f-variable-expansion"></a>F. Expansión de variables  
 En el ejemplo siguiente se muestra cómo trabajar con datos que tienen el formato de una variable de **sqlcmd** .  

```
USE AdventureWorks2012;
CREATE TABLE AdventureWorks2012.dbo.VariableTest
(
Col1 nvarchar(50)
);
GO
```

 Inserte una fila en la `Col1` de `dbo.VariableTest` que contiene el valor `$(tablename)`.  

```
INSERT INTO AdventureWorks2012.dbo.VariableTest(Col1)
VALUES('$(tablename)');
GO
```
  
 En el símbolo del sistema `sqlcmd` , si no hay ninguna variable con el valor `$(tablename)`, las instrucciones siguientes devuelven la fila.  
  
```
C:\> sqlcmd
>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = '$(tablename)';
>2 GO
>3 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = N'$(tablename)';
>4 GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
>1 Col1
>2 ------------------
>3 $(tablename)
>4
>5 (1 rows affected)
```

 Si la variable `MyVar` está establecida en `$(tablename)`.  

```
>6 :setvar MyVar $(tablename)
```

 Estas instrucciones devuelven la fila y, además, un mensaje que indica que "No se definió la variable de script 'nombreDeTabla'".  

```
>6 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = '$(tablename)';
>7 GO

>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = N'$(tablename)';
>2 GO
```

 Estas instrucciones devuelven la fila.  

```
>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = '$(MyVar)';
>2 GO
```

```
>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = N'$(MyVar)';
>2 GO
```
  
## <a name="see-also"></a>Consulte también  
 [Usar la utilidad sqlcmd](./sqlcmd-use-the-utility.md)   
 [sqlcmd (utilidad)](../../tools/sqlcmd-utility.md)   
 [Referencia de la utilidad del símbolo del sistema &#40;motor de base de datos&#41;](../../tools/command-prompt-utility-reference-database-engine.md)  
  
