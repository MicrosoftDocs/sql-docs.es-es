---
title: Modificar scripts SQLCMD con el Editor de consultas
description: Los scripts SQLCMD se usan cuando es necesario procesar comandos del sistema de Windows e instrucciones de Transact-SQL en el mismo script. Aprenda a escribir y editar scripts SQLCMD con el Editor de consultas del motor de base de datos.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- scripts [SQL Server], SQLCMD scripts
- SQLCMD scripts
- modifying scripts
- Query Editor [Database Engine], SQLCMD scripts
- scripts [SQL Server], SQL Server Management Studio
ms.assetid: f77b866d-c330-47c9-9e74-0b8d8dff4b31
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 60c88bce82f6eb13d1441ded4cb815cecd0842ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354669"
---
# <a name="edit-sqlcmd-scripts-with-query-editor"></a>Modificar scripts SQLCMD con el Editor de consultas

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Con el Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , es posible escribir y modificar consultas como scripts SQLCMD. Los scripts SQLCMD se usan cuando es necesario procesar comandos del sistema de Windows e instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] en el mismo script.  
  
## <a name="sqlcmd-mode"></a>Modo SQLCMD  
 Para utilizar el Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] para escribir o modificar scripts SQLCMD, es necesario habilitar el modo de scripting SQLCMD. De forma predeterminada, el modo SQLCMD no está habilitado en el Editor de consultas. Puede habilitar el modo de scripting si hace clic en el icono **Modo SQLCMD** de la barra de herramientas o si selecciona **Modo SQLCMD** en el menú **Consulta** .  
  
> [!NOTE]  
>  Al habilitar el modo SQLCMD, se desactiva IntelliSense y el depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] en el Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
 Los scripts SQLCMD del Editor de consultas pueden utilizar las mismas características que todos los scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] . Entre estas características figuran las siguientes:  
  
-   Código de colores  
  
-   Ejecución de scripts  
  
-   Control de código fuente  
  
-   Análisis de scripts  
  
-   Showplan  
  
## <a name="enable-sqlcmd-scripting-in-query-editor"></a>Habilitar scripting SQLCMD en el Editor de consultas  
 Para activar el modo de scripting SQLCMD para una ventana activa del Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] , utilice el procedimiento siguiente.  
  
#### <a name="to-switch-a-database-engine-query-editor-window-to-sqlcmd-mode"></a>Para cambiar una ventana del Editor de consultas de Database Engine al modo SQLCMD  
  
1.  En el Explorador de objetos, haga clic con el botón derecho en el servidor y, después, haga clic en **Nueva consulta** para abrir una ventana nueva del Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
2.  En el menú **Consulta** , haga clic en **Modo SQLCMD**.  
  
     El Editor de consultas ejecuta instrucciones **sqlcmd** en el contexto del Editor de consultas.  
  
3.  En la barra de herramientas del **Editor SQL** , en la lista **Bases de datos disponibles** , seleccione [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
4.  En la ventana Editor de consultas, escriba las siguientes dos instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] y, a continuación, la instrucción `!!DIR` **sqlcmd**:  
  
    ```  
    SELECT DISTINCT Type FROM Sales.SpecialOffer;  
    GO  
    !!DIR  
    GO  
    SELECT ProductCategoryID, Name FROM Production.ProductCategory;  
    GO  
    ```  
  
5.  Presione F5 para ejecutar toda la sección de instrucciones mixtas [!INCLUDE[tsql](../../includes/tsql-md.md)] y MS-DOS.  
  
     Observe los paneles de resultados SQL de la primera y tercera instrucciones.  
  
6.  En el panel **Resultados** , haga clic en la pestaña **Mensajes** para ver los mensajes de las tres instrucciones:  
  
    -   (6 filas afectadas)  
  
    -   \<The directory information>  
  
    -   (4 filas afectadas)  
  
> [!IMPORTANT]  
>  Cuando se ejecuta desde la línea de comandos, la utilidad **sqlcmd** permite una interacción total con el sistema operativo. Al usar el Editor de consultas en **Modo SQLCMD**, debe tener cuidado de no ejecutar instrucciones interactivas. El Editor de consultas no puede responder a comandos del sistema operativo.  
  
 Para obtener más información acerca de cómo ejecutar SQLCMD, vea [sqlcmd Utility](../../tools/sqlcmd-utility.md)o realice el tutorial de SQLCMD.  
  
## <a name="enable-sqlcmd-scripting-by-default"></a>Habilitar scripting SQLCMD de forma predeterminada  
 Para activar scripting SQLCMD de forma predeterminada, seleccione **Opciones** en el menú **Herramientas**, expanda **Ejecución de la consulta** y **SQL Server**, haga clic en la página **General** y, a continuación, active la casilla **De forma predeterminada, abrir nuevas consultas en modo SQLCMD** .  
  
## <a name="writing-and-editing-sqlcmd-scripts"></a>Escribir y modificar scripts SQLCMD  
 Tras habilitar el modo de scripting, puede escribir comandos SQLCMD e instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] . Se aplican las reglas siguientes:  
  
-   Los comandos SQLCMD deben ser la primera instrucción de una línea.  
  
-   Solo se admite un comando SQLCMD en cada línea.  
  
-   Los comandos SQLCMD pueden ir precedidos de comentarios o espacios en blanco.  
  
-   Los comandos SQLCMD dentro de caracteres de comentario no se ejecutan.  
  
-   Los caracteres de comentario de una única línea son dos guiones (`--)` y deben aparecer al comienzo de una línea.  
  
-   Los comandos del sistema operativo deben ir precedidos de dos signos de exclamación de cierre (`!!`). El comando con dos signos de exclamación de cierre hace que la instrucción que los sigue se ejecute mediante el procesador de comandos `cmd.exe` . El texto situado tras `!!` se pasa como un parámetro a `cmd.exe`, de modo que la línea de comandos final se ejecutará como `"%SystemRoot%\system32\cmd.exe /c <text after !!>"`.  
  
-   Para hacer una distinción clara entre los comandos SQLCMD y [!INCLUDE[tsql](../../includes/tsql-md.md)], todos los comandos SQLCMD deben ir precedidos de dos puntos (`:`).  
  
-   El comando `GO` puede ir precedido de `!!:`  
  
-   El Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] es compatible con las variables de entorno y las variables definidas como parte de un script SQLCMD, aunque no es compatible con variables SQLCMD no integradas ni variables **osql** . El procesamiento de SQLCMD por [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] distingue entre mayúsculas y minúsculas para las variables. Por ejemplo, PRINT '$(COMPUTERNAME)' genera el resultado correcto, pero PRINT '$(ComputerName)' devuelve un error.  
  
> [!CAUTION]
>  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] usa [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]SqlClient para la ejecución en los modos normal y SQLCMD. Cuando se ejecuta desde la línea de comandos, SQLCMD utiliza el proveedor OLE DB. Puesto que se pueden aplicar diferentes opciones predeterminadas, es posible observar diferentes comportamientos al ejecutar la misma consulta en el modo SQLCMD de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y en la herramienta SQLCMD.  
  
## <a name="supported-sqlcmd-syntax"></a>Sintaxis SQLCMD compatible  
 El Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] admite las siguientes palabras clave de script SQLCMD:  
  
 `[!!:]GO[count]`  
  
 `!! <command>`  
  
 `:exit(statement)`  
  
 `:Quit`  
  
 `:r <filename>`  
  
 `:setvar <var> <value>`  
  
 `:connect server[\instance] [-l login_timeout] [-U user [-P password]]`  
  
 `:on error [ignore|exit]`  
  
 `:error <filename>|stderr|stdout`  
  
 `:out <filename>|stderr|stdout`  
  
> [!NOTE]  
>  Tanto para `:error` como para `:out`, `stderr` y `stdout` envían los resultados a la pestaña de mensajes.  
  
 Los comandos SQLCMD que no aparecen en la lista anterior no son compatibles con el Editor de consultas. Cuando se ejecuta un script que contiene palabras clave de SQLCMD no compatibles, el Editor de consultas enviará un mensaje "Omitiendo el comando *\<ignored command*>" al destino, por cada palabra clave no compatible. El script se ejecutará correctamente, aunque los comandos no compatibles se omitirán.  
  
> [!CAUTION]  
>  Puesto que no se está iniciando SQLCMD desde la línea de comandos, existen algunas limitaciones al ejecutar el Editor de consultas en modo SQLCMD. No es posible enviar parámetros de línea de comandos como variables y, dado que el Editor de consultas no puede responder a las solicitudes del sistema operativo, hay que tener cuidado de no ejecutar instrucciones interactivas.  
  
## <a name="color-coding-in-sqlcmd-scripts"></a>Código de colores en los scripts SQLCMD  
 Con scripting SQLCMD habilitado, los scripts utilizarán códigos de colores. El código de colores de las palabras clave de [!INCLUDE[tsql](../../includes/tsql-md.md)] permanece igual. Los comandos SQLCMD se presentan con un fondo sombreado.  
  
## <a name="example"></a>Ejemplo  
 En el siguiente ejemplo se usa una instrucción **sqlcmd** para crear un archivo de salida denominado testoutput.txt y se ejecutan dos instrucciones SELECT de [!INCLUDE[tsql](../../includes/tsql-md.md)] SELECT junto con un comando del sistema operativo (para imprimir el directorio actual). El archivo resultante contiene la salida del mensaje de la instrucción `DIR` seguida de la salida de resultados de las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
```  
:out C:\testoutput.txt  
SELECT @@VERSION As 'Server Version'  
!!DIR  
!!:GO  
SELECT @@SERVERNAME AS 'Server Name'  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Utilidad sqlcmd](../../tools/sqlcmd-utility.md)  
  
  
