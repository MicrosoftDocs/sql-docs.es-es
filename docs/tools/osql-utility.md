---
title: osql (utilidad)
description: En SQL Server, la utilidad osql le permite insertar instrucciones Transact-SQL, procedimientos del sistema y archivos de script. Osql utiliza ODBC para comunicarse con el servidor.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- statements [SQL Server], command prompt
- QUIT command
- Transact-SQL statements, command prompt
- EXIT command
- operating systems [SQL Server], commands
- osql utility [SQL Server]
- stored procedures [SQL Server], command prompt
- scripts [SQL Server], command prompt
- RESET command
- GO command
- command prompt utilities [SQL Server], osql
- CTRL+C command
ms.assetid: cf530d9e-0609-4528-8975-ab8e08e40b9a
author: markingmyname
ms.author: maghan
ms.manageR: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/16/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 41c47f76dd0a4b81d1ccc337b65bf1c8c157b770
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335991"
---
# <a name="osql-utility"></a>osql (utilidad)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

La utilidad **osql** permite especificar archivos de script, procedimientos del sistema e instrucciones de [!INCLUDE[tsql](../includes/tsql-md.md)] . Esta herramienta utiliza ODBC para comunicarse con el servidor.  
  
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Evite utilizar esta característica en los nuevos trabajos de programación y planee modificar las aplicaciones que actualmente la utilizan. En su lugar, utilice **sqlcmd** . Para obtener más información, consulte [sqlcmd Utility](../tools/sqlcmd-utility.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
osql  
[-?] |  
[-L] |  
[  
  {  
     {-Ulogin_id [-Ppassword]} | -E }  
     [-Sserver_name[\instance_name]] [-Hwksta_name] [-ddb_name]  
     [-ltime_out] [-ttime_out] [-hheaders]  
     [-scol_separator] [-wcolumn_width] [-apacket_size]  
     [-e] [-I] [-D data_source_name]  
     [-ccmd_end] [-q "query"] [-Q"query"]  
     [-n] [-merror_level] [-r {0 | 1}]  
     [-iinput_file] [-ooutput_file] [-p]  
     [-b] [-u] [-R] [-O]  
]  
```  
  
## <a name="arguments"></a>Argumentos  
 **-?**  
 Muestra el resumen de la sintaxis de los modificadores de **osql** .  
  
 **-L**  
 Enumera los servidores configurados localmente y los nombres de los servidores que difunden en la red.  
  
> [!NOTE]  
>  Debido a la naturaleza de la difusión en las redes, es posible que **osql** no reciba una respuesta a tiempo de todos los servidores. Por lo tanto, la lista de servidores devuelta puede variar para cada invocación de esta opción.  
  
 **-U** _login_id_  
 Es el identificador de inicio de sesión del usuario. Los identificadores de inicio de sesión distinguen entre mayúsculas y minúsculas.  
  
 **-P** _password_  
 Es una contraseña especificada por el usuario. Si no se usa la opción **-P** , **osql** solicitará una contraseña. Si se usa la opción **-P** al final del símbolo del sistema sin una contraseña, **osql** usará la contraseña predeterminada (NULL).  
  
> [!IMPORTANT]  
>  No utilice una contraseña en blanco. Utilice una contraseña segura. Para obtener más información, consulte [Strong Passwords](../relational-databases/security/strong-passwords.md).  
  
 En las contraseñas se distingue entre mayúsculas y minúsculas.  
  
 La variable de entorno OSQLPASSWORD le permite establecer una contraseña predeterminada para la sesión actual. Por lo tanto, no tiene que codificar una contraseña en archivos por lotes.  
  
 Si no establece la contraseña con la opción **-P** , **osql** comprueba primero la variable OSQLPASSWORD. Si no hay ningún valor establecido, **osql** utiliza la contraseña predeterminada, NULL. En el ejemplo siguiente se especifica la variable OSQLPASSWORD en el símbolo del sistema y, a continuación, se tiene acceso a la utilidad **osql** :  
  
```  
C:\>SET OSQLPASSWORD=abracadabra  
C:\>osql   
```  
  
> [!IMPORTANT]  
>  Para enmascarar la contraseña, no especifique la opción **-P** junto con la opción **-U** . En su lugar, después de especificar **osql** con la opción **-U** y otros modificadores (sin especificar **-P**), pulse ENTRAR y **osql** le solicitará una contraseña. Este método garantiza que la contraseña se enmascare al especificarla.  
  
 **-E**  
 Utiliza una conexión de confianza en lugar de solicitar una contraseña.  
  
 **-S** _server\_name_[ **\\** _instance\_name_]  
 Especifica la instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a la que hay que conectarse. Especifique *server_name* para conectar con la instancia predeterminada de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en ese servidor. Especifique _nombre\_servidor_ **\\** _nombre\_instancia_ para conectar una instancia con nombre de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en ese servidor. Si no se especifica ningún servidor, **osql** se conecta a la instancia predeterminada de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en el equipo local. Esta opción es necesaria cuando se ejecuta **osql** desde un equipo remoto conectado a la red.  
  
 **-H** _wksta_name_  
 Es el nombre de una estación de trabajo. El nombre de la estación de trabajo se almacena en **sysprocesses.hostname** y se muestra mediante **sp_who**. Si no se especifica esta opción, se supone el nombre actual del equipo.  
  
 **-d** _db_name_  
 Ejecuta una instrucción USE *db_name* al iniciar **osql**.  
  
 **-l** _time_out_  
 Especifica el número de segundos que tienen que transcurrir antes de que un inicio de sesión de **osql** agote el tiempo de espera. El tiempo de espera predeterminado para el inicio de sesión de **osql** es de ocho segundos.  
  
 **-t** _time_out_  
 Especifica el número de segundos que tienen que transcurrir antes de que un comando exceda el tiempo de espera. Si no se especifica ningún valor para *time_out* , los comandos no tienen tiempo de espera.  
  
 **-h** _headers_  
 Especifica el número de filas que se van a imprimir entre los encabezados de las columnas. La opción predeterminada es imprimir los encabezados una vez para cada conjunto de resultados de la consulta. Utilice -1 para especificar que no desea imprimir los encabezados. Si usa -1, no debe dejar espacio entre el parámetro y el valor ( **-h-1**, no **-h -1**).  
  
 **-s** _col_separator_  
 Especifica el carácter separador de columnas que, de forma predeterminada, es un espacio en blanco. Para utilizar caracteres con un significado especial para el sistema operativo (por ejemplo, | ; & < >), escríbalos entre comillas dobles (").  
  
 **-w** _column_width_  
 Permite al usuario ajustar el ancho de la pantalla para la salida. El valor predeterminado es 80 caracteres. Cuando una línea de salida ha alcanzado el ancho máximo de pantalla, se divide en varias líneas.  
  
 **-a** _packet_size_  
 Permite solicitar un paquete de diferente tamaño. Los valores válidos de *packet_size* son de 512 a 65 535. El valor predeterminado de **osql** es el servidor predeterminado. Si se aumenta el tamaño del paquete, se puede mejorar el rendimiento durante la ejecución de scripts grandes en las que hay muchas instrucciones SQL entre comandos GO. [!INCLUDE[msCoName](../includes/msconame-md.md)] indican que 8192 suele ser la configuración más rápida para operaciones de copia masiva. Se puede solicitar un tamaño mayor para los paquetes, pero **osql** adopta el valor predeterminado para el servidor si no puede cumplirse la solicitud.  
  
 **-e**  
 Muestra lo escrito.  
  
 **-I**  
 Activa la opción de conexión QUOTED_IDENTIFIER.  
  
 **-D** _data_source_name_  
 Conecta a un origen de datos ODBC definido mediante el controlador ODBC de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. La conexión **osql** utiliza las opciones especificadas en el origen de datos.  
  
> [!NOTE]  
>  Esta opción no funciona con los orígenes de datos definidos para otros controladores.  
  
 **-c** _cmd_end_  
 Especifica el terminador del comando. De forma predeterminada, los comandos se terminan y se envían a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] si se escribe GO en una línea aparte. Cuando restablezca el terminador del comando, no utilice palabras reservadas de [!INCLUDE[tsql](../includes/tsql-md.md)] ni caracteres que tengan un significado especial para el sistema operativo, vayan o no precedidos por una barra diagonal inversa.  
  
 **-q "** _query_ **"**  
 Ejecuta una consulta cuando se inicia **osql** , pero no sale de **osql** cuando se completa la consulta. (La instrucción de consulta no debe incluir GO.) Si emite una consulta desde un archivo por lotes, utilice %variables o %variables% de entorno. Por ejemplo:  
  
```  
SET table=sys.objects  
osql -E -q "select name, object_id from %table%"  
```  
  
 Utilice comillas dobles para la consulta y comillas simples en los elementos que incruste en la consulta.  
  
 **-Q"** _query_ **"**  
 Ejecuta una consulta y sale inmediatamente de **osql**. Utilice comillas dobles para la consulta y comillas simples en los elementos que incruste en la consulta.  
  
 **-n**  
 Quita la numeración y el símbolo del sistema (>) de las líneas de entrada.  
  
 **-m** _error_level_  
 Personaliza la presentación de los mensajes de error. Se muestra el nivel de error, estado y número de mensaje en errores con el nivel de gravedad especificado o superior. No se mostrarán errores que tengan un nivel de gravedad inferior al nivel especificado. Use **-1** para especificar que se devuelvan todos los encabezados con los mensajes, incluso con los mensajes informativos. Si usa **-1**, no debe dejar espacio entre el parámetro y los valores ( **-m-1**, no **-m -1**).  
  
 **-r** { **0**| **1**}  
 Redirige la salida del mensaje a la pantalla (**stderr**). Si no especifica ningún parámetro, o si especifica **0**, solo se redirigirán los mensajes con nivel de gravedad 11 o superior. Si especifica **1**, toda salida de mensaje (incluida "print") se redirigirá.  
  
 **-i** _input_file_  
 Identifica el archivo que contiene un lote de instrucciones SQL o procedimientos almacenados. Se puede usar el operador de comparación menor que ( **\<** ) en lugar de **-i**.  
  
 **-o** _output_file_  
 Identifica el archivo que recibe la salida de **osql**. Se puede usar el operador de comparación mayor que ( **>** ) en lugar de **-o**.  
  
 Si *input_file* no está en formato Unicode y no se especifica **-u** , *output_file* se almacena en formato OEM. Si *input_file* está en formato Unicode o se especifica **-u** , *output_file* se almacena en formato Unicode.  
  
 **-p**  
 Imprime las estadísticas de rendimiento.  
  
 **-b**  
 Especifica que **osql** se cierre y devuelva un valor de DOS ERRORLEVEL cuando se produce un error. El valor que se devuelve a la variable DOS ERRORLEVEL es 1 cuando el mensaje de error de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tiene una gravedad superior a 11; de lo contrario, el valor devuelto es 0. [!INCLUDE[msCoName](../includes/msconame-md.md)] Los archivos por lotes de MS-DOS pueden probar el valor de DOS ERRORLEVEL y tratar el error apropiadamente.  
  
 **-u**  
 Especifica que el archivo *output_file* se almacene en formato Unicode, independientemente del formato del archivo *input_file*.  
  
 **-R**  
 Especifica que el controlador ODBC de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] debe usar la configuración del cliente cuando realice conversiones de datos de moneda, fecha y hora a datos de caracteres.  
  
 **-O**  
 Especifica que se desactiven algunas características de **osql** para emular el comportamiento de versiones anteriores de **isql**. Estas características están desactivadas:  
  
-   Procesamiento por lotes de final de archivo (EOF)  
  
-   Escala automática de ancho de la consola  
  
-   Mensajes detallados  
  
 También establece -1 como valor predeterminado de ERRORLEVEL de DOS.  
  
> [!NOTE]  
>  Las opciones **-n**, **-O** y **-D** han dejado de admitirse en **osql**.  
  
## <a name="remarks"></a>Observaciones  
 La utilidad **osql** se inicia directamente desde el sistema operativo con las opciones en mayúsculas o en minúsculas, tal como se muestran aquí. Después de iniciar **osql** acepta instrucciones SQL y las envía a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] de forma interactiva. Se da formato a los resultados y se muestran en la pantalla (**stdout**). Utilice QUIT o EXIT para salir de **osql**.  
  
 Si no especifica un nombre de usuario cuando inicia **osql**, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] comprueba las variables de entorno y las usa, por ejemplo, **osqluser=(** _user_ **)** u **osqlserver=(** _server_ **)** . Si no se establecen variables de entorno, se utilizará el nombre de usuario de la estación de trabajo. Si no especifica un servidor, se utilizará el nombre de la estación de trabajo.  
  
 Si no se usan las opciones **-U** ni **-P** , [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] intenta conectarse usando el modo de autenticación de [!INCLUDE[msCoName](../includes/msconame-md.md)] Windows. La autenticación se basa en la cuenta de [!INCLUDE[msCoName](../includes/msconame-md.md)] Windows del usuario que ejecuta **osql**.  
  
 La utilidad **osql** utiliza la API de ODBC. La utilidad utiliza la configuración predeterminada del controlador ODBC de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] para las opciones de conexión ISO de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Para obtener más información, vea Efectos de las opciones de ANSI.  
  
> [!NOTE]  
>  La utilidad **osql** no admite los tipos de datos definidos por el usuario CLR. Para procesar estos tipos de datos, debe usar la utilidad **sqlcmd** . Para obtener más información, consulte [sqlcmd Utility](../tools/sqlcmd-utility.md).  
  
## <a name="osql-commands"></a>Comandos OSQL  
 Además de las instrucciones de [!INCLUDE[tsql](../includes/tsql-md.md)] de **osql**, los siguientes comandos también están disponibles.  
  
|Get-Help|Descripción|  
|-------------|-----------------|  
|GO|Ejecuta todas las instrucciones escritas después del último GO.|  
|RESET|Borra cualquier instrucción que haya escrito.|  
|QUIT o EXIT( )|Sale de **osql**.|  
|CTRL+C|Finaliza una consulta sin salir de **osql**.|  
  
> [!NOTE]  
>  Los comandos !! y ED ya no se admiten en **osql**.  
  
 Los terminadores del comando GO (predeterminado), RESET EXIT, QUIT y CTRL+C solo se reconocen si aparecen al principio de una línea, inmediatamente después de símbolo del sistema de **osql** .  
  
 GO marca tanto el final de un lote como la ejecución de cualquier instrucción de [!INCLUDE[tsql](../includes/tsql-md.md)] almacenada en caché. Cuando presione INTRO al final de cada línea de entrada, **osql** colocará en caché las instrucciones de esa línea. Al presionar ENTRAR después de escribir GO, todas las instrucciones almacenadas en la caché se enviarán como un lote a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 La utilidad **osql** actual funciona como si existiera una instrucción GO al final de cada script que se ejecuta, por lo que se ejecutarán todas las instrucciones del script.  
  
 Finalice un comando; para ello, escriba una línea que comience con un terminador de comandos. Puede poner un entero después del terminador de comandos para especificar cuántas veces se debe ejecutar el comando. Por ejemplo, para ejecutar este comando 100 veces, escriba:  
  
```  
SELECT x = 1  
GO 100  
```  
  
 Los resultados se imprimen una vez al final de la ejecución. **osql** no acepta más de 1000 caracteres por línea. Las instrucciones grandes se deben distribuir en varias líneas.  
  
 Pueden utilizarse las funciones de recuperación de Windows para recuperar y modificar las instrucciones **osql** . El búfer de consulta existente se puede borrar si escribe RESET.  
  
 Cuando se ejecutan procedimientos almacenados, **osql** imprime una línea en blanco entre cada conjunto de resultados de un lote. Además, el mensaje "0 filas afectadas" no aparece cuando no se aplica a la instrucción ejecutada.  
  
## <a name="using-osql-interactively"></a>Utilizar osql de forma interactiva  
 Para usar **osql** de forma interactiva, escriba el comando **osql** (y cualesquiera de las opciones) junto al símbolo del sistema.  
  
 Puede leer un archivo que contenga una consulta (como Sores.qry) para ejecutarse en **osql** si escribe un comando como este:  
  
```  
osql -E -i stores.qry  
```  
  
 Puede leer un archivo que contenga una consulta (como Titles.qry) y dirigir el resultado a otro archivo si escribe un comando similar a este:  
  
```  
osql -E -i titles.qry -o titles.res  
```  
  
> [!IMPORTANT]  
>  Siempre que sea posible, use la opción **-E**(conexión de confianza).  
  
 Cuando use **osql** de forma interactiva, podrá leer un archivo del sistema operativo en el búfer de comandos con **:r**_nombre\_archivo_. De esta manera, se enviará el script SQL de *file_name* directamente al servidor como un lote único.  
  
> [!NOTE]  
>  Cuando se usa **osql**, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] trata el separador de lotes GO, si aparece en el archivo de script de SQL, como un error de sintaxis.  
  
## <a name="inserting-comments"></a>Insertar comentarios  
 Puede incluir comentarios en una instrucción Transact-SQL que vaya a enviarse a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con **osql**. Se permiten dos estilos de comentario: `--` y `/*...*/`.  
  
## <a name="using-exit-to-return-results-in-osql"></a>Utilizar EXIT para devolver resultados en osql  
 Puede utilizar el resultado de una instrucción SELECT como valor devuelto de **osql**. Si es numérica, la primera columna de la última fila del resultado se convierte en un entero de 4 bytes (long). MS-DOS pasa el byte bajo al proceso primario o al nivel de errores del sistema operativo. Windows pasa el entero de 4 bytes completo. La sintaxis es:  
  
```  
EXIT ( < query > )  
```  
  
 Por ejemplo:  
  
```  
EXIT(SELECT @@ROWCOUNT)  
```  
  
 También puede incluir el parámetro EXIT como parte de un archivo por lotes. Por ejemplo:  
  
```  
osql -E -Q "EXIT(SELECT COUNT(*) FROM '%1')"  
```  
  
 La utilidad **osql** pasa al servidor todo lo que haya entre paréntesis **()** , exactamente como se haya escrito. Si un procedimiento almacenado del sistema selecciona un conjunto y devuelve un valor, solo se devuelve la selección. La instrucción EXIT **()** sin nada entre paréntesis ejecuta todo lo que le precede en el lote y luego sale sin ningún valor devuelto.  
  
 Hay cuatro formatos de EXIT:  
  
-   EXIT  
  
> [!NOTE]  
>  No ejecuta el lote; sale de forma inmediata y no devuelve ningún valor.  
  
-   EXIT **()**  
  
> [!NOTE]  
>  Ejecuta el lote y, a continuación, sale sin devolver ningún valor.  
  
-   EXIT **(** _query_ **)**  
  
> [!NOTE]  
>  Ejecuta el lote, incluida la consulta, y, a continuación, sale tras devolver el resultado de la consulta.  
  
-   RAISERROR con un estado de 127  
  
> [!NOTE]  
>  Si utiliza RAISERROR en un script **osql** y se genera el estado 127, **osql** terminará y devolverá un id. de mensaje al cliente. Por ejemplo:  
  
```  
RAISERROR(50001, 10, 127)  
```  
  
 Este error provocará la finalización del script **osql** y se devolverá al cliente un mensaje con el id. 50001.  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]reserva los valores devueltos -1 a -99; **osql** define los valores siguientes:  
  
-   -100  
  
     Error encontrado antes de seleccionar el valor devuelto.  
  
-   -101  
  
     No se encontró ninguna fila al seleccionar el valor devuelto.  
  
-   -102  
  
     Error de conversión al seleccionar el valor devuelto.  
  
## <a name="displaying-money-and-smallmoney-data-types"></a>Mostrar tipos de datos money y smallmoney  
 **osql** muestra los tipos de datos **money** y **smallmoney** con dos decimales, aunque [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] almacene internamente el valor con cuatro decimales. Observe el ejemplo:  
  
```  
SELECT CAST(CAST(10.3496 AS money) AS decimal(6, 4))  
GO  
```  
  
 Esta instrucción da como resultado `10.3496`, que indica que el valor se almacena con todos los decimales intactos.  
  
## <a name="see-also"></a>Consulte también  
 [Comentario &#40;MDX&#41;](../mdx/comment-mdx.md)   
 [-- &#40;Comentario&#41; &#40;MDX&#41;](../mdx/comment-mdx-operator-reference.md)   
 [CAST y CONVERT &#40;Transact-SQL&#41;](../t-sql/functions/cast-and-convert-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../t-sql/language-elements/raiserror-transact-sql.md)  
  
  
