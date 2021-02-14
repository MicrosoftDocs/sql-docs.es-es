---
description: RAISERROR (Transact-SQL)
title: RAISERROR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RAISERROR
- RAISERROR_TSQL
- RAISEERROR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmessages system table
- errors [SQL Server], RAISERROR statement
- user-defined error messages [SQL Server]
- system flags
- generating errors [SQL Server]
- TRY block [SQL Server]
- recording errors
- ad hoc messages
- RAISERROR statement
- CATCH block
- messages [SQL Server], RAISERROR statement
ms.assetid: 483588bd-021b-4eae-b4ee-216268003e79
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e9892fe80ff3f29dca2ac46c99249fa603ab5528
ms.sourcegitcommit: 6f4fb9cfd0cad06127a6328adc745e2ba7c191d1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2021
ms.locfileid: "99570489"
---
# <a name="raiserror-transact-sql"></a>RAISERROR (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

> [!NOTE]
> La instrucción **RAISERROR** no respeta **SET XACT_ABORT**. Las nuevas aplicaciones deben usar **THROW** en vez de **RAISERROR**.

  Genera un mensaje de error e inicia el procesamiento de errores de la sesión. RAISERROR puede hacer referencia a un mensaje definido por el usuario almacenado en la vista de catálogo sys.messages o puede generar un mensaje dinámicamente. El mensaje se devuelve como un mensaje de error de servidor a la aplicación que realiza la llamada o a un bloque CATCH asociado de una construcción TRY…CATCH. Las nuevas aplicaciones deben usar [THROW](../../t-sql/language-elements/throw-transact-sql.md) en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
RAISERROR ( { msg_id | msg_str | @local_variable }  
    { ,severity ,state }  
    [ ,argument [ ,...n ] ] )  
    [ WITH option [ ,...n ] ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
RAISERROR ( { msg_str | @local_variable }  
    { ,severity ,state }  
    [ ,argument [ ,...n ] ] )  
    [ WITH option [ ,...n ] ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *msg_id*  
 Es un número de mensaje de error definido por el usuario almacenado en la vista de catálogo sys.messages por medio de sp_addmessage. Los números de error para los mensajes de error definidos por el usuario deberían ser mayores que 50000. Si no se especifica *msg_id*, RAISERROR genera un mensaje de error con el número 50000.  
  
 *msg_str*  
 Es un mensaje definido por el usuario con un formato similar al de la función **printf** de la biblioteca de C estándar. El mensaje de error puede tener 2.047 caracteres como máximo. Si el mensaje contiene más de 2.048 caracteres, solamente aparecerán los 2.044 primeros y se agregarán puntos suspensivos para indicar que el mensaje se ha truncado. Tenga en cuenta que los parámetros de sustitución utilizan más caracteres de lo que muestra la salida debido al comportamiento del almacenamiento interno. Por ejemplo, el parámetro de sustitución de *%d* con el valor 2 asignado genera en realidad un carácter en la cadena del mensaje, pero internamente ocupa tres caracteres adicionales de almacenamiento. Este requisito de almacenamiento reduce el número de caracteres disponibles para la salida del mensaje.  
  
 Si se especifica *msg_str*, RAISERROR genera un mensaje de error con el número 50000.  
  
 *msg_str* es una cadena de caracteres que incluye especificaciones de conversión opcionales. Cada especificación de conversión define cómo se aplica formato a un valor de la lista de argumentos y cómo se coloca en un campo en la posición indicada en la especificación de conversión de *msg_str*. Las especificaciones de conversión tienen el formato siguiente:  
  
 % [[*flag*] [*width*] [. *precision*] [{h | l}]] *type*  
  
 Los parámetros que se pueden usar en *msg_str* son:  
  
 *flag*  
  
 Es un código que determina el espaciado y la justificación del valor sustituido.  
  
|Código|Prefijo o justificación|Descripción|  
|----------|-----------------------------|-----------------|  
|- (menos)|Justificado a la izquierda|Justifica a la izquierda el valor del argumento en el ancho de campo dado.|  
|+ (más)|Prefijo de signo|Coloca el signo más (+) o menos (-) delante del valor del argumento, si el valor es de un tipo con signo.|  
|0 (cero)|Relleno con ceros|Coloca ceros iniciales en la salida hasta alcanzar el ancho mínimo. Cuando aparecen 0 y el signo menos (-), el 0 no se tiene en cuenta.|  
|# (número)|Prefijo 0x del tipo hexadecimal de x o X|Cuando se utiliza con el formato o, x o X, la marca del signo de número (#) se coloca delante de los valores distintos de cero con 0, 0x o 0X, respectivamente. Cuando d, i, o u llevan delante la marca del signo de número (#), se omite la marca.|  
|' ' (espacio en blanco)|Relleno con espacios|Coloca delante del valor de salida espacios en blanco si el valor tiene signo y es positivo. Se omite cuando se incluye con la marca del signo más (+).|  
  
 *width*  
  
 Es un entero que define el ancho mínimo del campo en el que se coloca el valor del argumento. Si la longitud del valor del argumento es igual o mayor que *width*, el valor se imprime sin relleno. Si el valor es menor que *width*, se rellena hasta obtener la longitud especificada en *width*.  
  
 El asterisco (*) significa que el ancho se especifica mediante el argumento asociado de la lista de argumentos, que debe tener un valor entero.  
  
 *precisión*  
  
 Es el número máximo de caracteres del valor del argumento que se utilizan para valores de cadena. Por ejemplo, si una cadena tiene cinco caracteres y la precisión es 3, solo se utilizan los tres primeros caracteres del valor de cadena.  
  
 Para los valores enteros, *precision* es el número mínimo de dígitos impresos.  
  
 El asterisco (*) significa que la precisión se especifica mediante el argumento asociado de la lista de argumentos, que debe tener un valor entero.  
  
 {h | l} *type*  
  
 Se usa con los tipos de caracteres d, i, o, s, x, X, u, y crea valores **shortint** (h) o **longint** (l).  
  
|Especificación de tipo|Representa|  
|------------------------|----------------|  
|d o i|Entero con signo|  
|o|Octal sin signo|  
|s|String|  
|u|Entero sin signo|  
|x o X|Hexadecimal sin signo|  
  
> [!NOTE]  
>  Estas especificaciones de tipo se basan en las definidas originalmente para la función **printf** de la biblioteca de C estándar. Las especificaciones de tipo usadas en las cadenas de mensajes RAISERROR se asignan a tipos de datos de [!INCLUDE[tsql](../../includes/tsql-md.md)], mientras que las especificaciones usadas en **printf** se asignan a tipos de datos del lenguaje C. RAISERROR no admite las especificaciones de tipo usadas en **printf** cuando [!INCLUDE[tsql](../../includes/tsql-md.md)] no tiene un tipo de datos similar al tipo de datos de C asociado. Por ejemplo, RAISERROR no admite la especificación *%* para punteros porque [!INCLUDE[tsql](../../includes/tsql-md.md)] no tiene un tipo de datos de puntero.  
  
> [!NOTE]  
>  Para convertir un valor al tipo de datos [!INCLUDE[tsql](../../includes/tsql-md.md)] **bigint**, especifique **%I64d**.  
  
 *\@local_variable*  
 Es una variable de un tipo de datos de caracteres válido que contiene una cadena formateada de la misma forma que *msg_str*. *\@local_variable* debe ser **char** o **varchar**, o bien se debe poder convertir implícitamente a estos tipos de datos.  
  
 *severity*  
 Es el [nivel de gravedad](../../relational-databases/errors-events/database-engine-error-severities.md) definido por el usuario asociado a este mensaje. Cuando se usa *msg_id* para generar un mensaje definido por el usuario creado con sp_addmessage, la gravedad especificada en RAISERROR reemplaza la gravedad especificada en sp_addmessage.  
  
 Todos los usuarios pueden especificar los niveles de gravedad del 0 al 18. Solo los miembros del rol fijo de servidor sysadmin o los usuarios con permisos ALTER TRACE pueden especificar los niveles de gravedad del 19 al 25. Para los niveles de gravedad del 19 al 25, se necesita la opción WITH LOG. Los niveles de gravedad menores que 0 se interpretan como 0. Los niveles de gravedad mayores que 25 se interpretan como 25.  
  
> [!CAUTION]  
>  Los niveles de gravedad del 20 al 25 se consideran muy negativos. Si se encuentra un nivel de gravedad de este tipo, la conexión de cliente termina tras recibir el mensaje, y el error se incluye en el registro de errores y en el registro de la aplicación.  
  
 Puede especificar -1 para devolver el valor de gravedad asociado al error como se muestra en el ejemplo siguiente.  
  
```sql  
RAISERROR (15600,-1,-1, 'mysp_CreateCustomer');  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Msg 15600, Level 15, State 1, Line 1   
 An invalid parameter or option was specified for procedure 'mysp_CreateCustomer'.
 ```  
  
 *state*  
 Entero entre 0 y 255. Los valores negativos son 1 de forma predeterminada. No deben usarsevalores mayores que 255. 
  
 Si se genera el mismo error definido por el usuario en varias ubicaciones, el uso de un único número de estado para cada ubicación puede ayudar a averiguar qué sección del código está generando los errores.  
  
 *argument*  
 Son los parámetros usados en la sustitución de las variables definidas en *msg_str* o el mensaje correspondiente a *msg_id*. Puede haber 0 o más parámetros de sustitución, pero el número total no puede superar 20. Cada parámetro de sustitución puede ser una variable local o cualquiera de estos tipos de datos: **tinyint**, **smallint**, **int**, **char**, **varchar**, **nchar**, **nvarchar**, **binary** o **varbinary**. No se admiten otros tipos de datos.  
  
 *Opción*  
 Es una opción personalizada del error. Puede tener uno de los valores de la tabla siguiente.  
  
|Value|Descripción|  
|-----------|-----------------|  
|REGISTRO|Guarda el error en el registro de errores y en el registro de aplicación de la instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los errores guardados en el registro de errores tienen un límite máximo de 440 bytes. Solo los miembros del rol fijo de servidor sysadmin o los usuarios con permisos ALTER TRACE pueden especificar WITH LOG.<br /><br /> [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)]|  
|NOWAIT|Envía inmediatamente los mensajes al cliente.<br /><br /> [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)]|  
|SETERROR|Establece los valores de @@ERROR y ERROR_NUMBER en *msg_id* o 50000, independientemente del nivel de gravedad.<br /><br /> [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)]|  
  
## <a name="remarks"></a>Observaciones  
 Los errores generados por RAISERROR funcionan igual que los generados por el código del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Las funciones del sistema ERROR_LINE, ERROR_MESSAGE, ERROR_NUMBER, ERROR_PROCEDURE, ERROR_SEVERITY, ERROR_STATE y @@ERROR informan de los valores especificados por RAISERROR. Cuando se ejecuta RAISERROR con un nivel de gravedad 11 o superior en un bloque TRY, transfiere el control al bloque CATCH asociado. El error se devuelve al autor de la llamada si RAISERROR se ejecuta:  
  
-   Fuera del ámbito de cualquier bloque TRY.  
  
-   Con un nivel de gravedad 10 o inferior en un bloque TRY.  
  
-   Con un nivel de gravedad 20 o superior que finaliza la conexión con la base de datos.  
  
 Los bloques CATCH pueden utilizar RAISERROR para volver a iniciar el error que invocó el bloque CATCH mediante funciones del sistema como ERROR_NUMBER y ERROR_MESSAGE con el fin de recuperar la información del error original. @@ERROR está establecido de forma predeterminada en 0 para los mensajes con una gravedad de 1 a 10.  
  
 Cuando *msg_id* especifica un mensaje definido por el usuario disponible en la vista de catálogo sys.messages, RAISERROR procesa el mensaje de la columna de texto usando las mismas reglas que se aplican al texto de un mensaje definido por el usuario especificado con *msg_str*. El texto del mensaje definido por el usuario puede contener especificaciones de conversión. En ese caso, RAISERROR asigna los valores de los argumentos a las especificaciones de conversión. Use sp_addmessage y sp_dropmessage para agregar y quitar, respectivamente, mensajes de error definidos por el usuario.  
  
 RAISERROR se puede utilizar como alternativa a PRINT para devolver mensajes a las aplicaciones que realizan llamadas. La sustitución de caracteres que RAISERROR admite es similar a la de la función **printf** de la biblioteca de C estándar, mientras que no sucede lo mismo con la instrucción PRINT de [!INCLUDE[tsql](../../includes/tsql-md.md)]. La instrucción PRINT no se ve afectada por los bloques TRY, mientras que, si RAISERROR se ejecuta con un nivel de gravedad de 11 a 19 en un bloque TRY, transfiere el control al bloque CATCH asociado. Especifique un nivel de gravedad 10 o inferior para que RAISERROR devuelva un mensaje desde un bloque TRY sin invocar el bloque CATCH.  
  
 Normalmente, los argumentos reemplazan las especificaciones de conversión secuencialmente: el primer argumento reemplaza la primera especificación de conversión, el segundo argumento reemplaza la segunda especificación de conversión, y así sucesivamente. Por ejemplo, en la siguiente instrucción `RAISERROR`, el primer argumento `N'number'` reemplaza la primera especificación de conversión `%s` y el segundo argumento `5` reemplaza la segunda especificación de conversión `%d.`  
  
```sql  
RAISERROR (N'This is message %s %d.', -- Message text.  
           10, -- Severity,  
           1, -- State,  
           N'number', -- First argument.  
           5); -- Second argument.  
-- The message text returned is: This is message number 5.  
GO  
```  
  
 Si se especifica un asterisco (*) para el ancho o la precisión de una especificación de conversión, el valor que se utilizará para el ancho o la precisión se especifica como un valor de argumento entero. En este caso, una especificación de conversión puede utilizar hasta tres argumentos, uno para el valor de ancho, uno para el valor de precisión y uno para el valor de sustitución.  
  
 Por ejemplo, las dos instrucciones `RAISERROR` siguientes devuelven la misma cadena. Una especifica los valores de ancho y precisión en la lista de argumentos y la otra en la especificación de conversión.  
  
```sql  
RAISERROR (N'<\<%*.*s>>', -- Message text.  
           10, -- Severity,  
           1, -- State,  
           7, -- First argument used for width.  
           3, -- Second argument used for precision.  
           N'abcde'); -- Third argument supplies the string.  
-- The message text returned is: <<    abc>>.  
GO  
RAISERROR (N'<\<%7.3s>>', -- Message text.  
           10, -- Severity,  
           1, -- State,  
           N'abcde'); -- First argument supplies the string.  
-- The message text returned is: <<    abc>>.  
GO  
```  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-error-information-from-a-catch-block"></a>A. Devolver información de error desde un bloque CATCH  
 En el siguiente ejemplo de código se muestra cómo utilizar `RAISERROR` dentro de un bloque `TRY` para provocar que la ejecución salte al bloque `CATCH` asociado. También se muestra cómo utilizar `RAISERROR` para devolver información acerca del error que invocó al bloque `CATCH`.  
  
> [!NOTE]  
>  RAISERROR solo genera errores con un estado comprendido entre 1 y 127. Como [!INCLUDE[ssDE](../../includes/ssde-md.md)] puede generar errores con un estado 0, se recomienda que compruebe el estado del error que devuelve ERROR_STATE antes de pasarlo como un valor al parámetro de estado RAISERROR.  
  
```sql  
BEGIN TRY  
    -- RAISERROR with severity 11-19 will cause execution to   
    -- jump to the CATCH block.  
    RAISERROR ('Error raised in TRY block.', -- Message text.  
               16, -- Severity.  
               1 -- State.  
               );  
END TRY  
BEGIN CATCH  
    DECLARE @ErrorMessage NVARCHAR(4000);  
    DECLARE @ErrorSeverity INT;  
    DECLARE @ErrorState INT;  
  
    SELECT   
        @ErrorMessage = ERROR_MESSAGE(),  
        @ErrorSeverity = ERROR_SEVERITY(),  
        @ErrorState = ERROR_STATE();  
  
    -- Use RAISERROR inside the CATCH block to return error  
    -- information about the original error that caused  
    -- execution to jump to the CATCH block.  
    RAISERROR (@ErrorMessage, -- Message text.  
               @ErrorSeverity, -- Severity.  
               @ErrorState -- State.  
               );  
END CATCH;  
```  
  
### <a name="b-creating-an-ad-hoc-message-in-sysmessages"></a>B. Crear un mensaje ad hoc en sys.messages  
 En el siguiente ejemplo se indica cómo mostrar un mensaje almacenado en la vista de catálogo sys.messages. El mensaje se agregó a la vista de catálogo sys.messages por medio del procedimiento almacenado del sistema `sp_addmessage` con el número de mensaje `50005`.  
  
```sql  
sp_addmessage @msgnum = 50005,  
              @severity = 10,  
              @msgtext = N'<\<%7.3s>>';  
GO  
RAISERROR (50005, -- Message id.  
           10, -- Severity,  
           1, -- State,  
           N'abcde'); -- First argument supplies the string.  
-- The message text returned is: <<    abc>>.  
GO  
sp_dropmessage @msgnum = 50005;  
GO  
```  
  
### <a name="c-using-a-local-variable-to-supply-the-message-text"></a>C. Utilizar una variable local para suministrar el texto del mensaje  
 En el siguiente ejemplo de código se muestra cómo utilizar una variable local para suministrar el texto del mensaje para una instrucción `RAISERROR`.  
  
```sql  
DECLARE @StringVariable NVARCHAR(50);  
SET @StringVariable = N'<\<%7.3s>>';  
  
RAISERROR (@StringVariable, -- Message text.  
           10, -- Severity,  
           1, -- State,  
           N'abcde'); -- First argument supplies the string.  
-- The message text returned is: <<    abc>>.  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones integradas &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [PRINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/print-transact-sql.md)   
 [sp_addmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)   
 [sp_dropmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmessage-transact-sql.md)   
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [xp_logevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/xp-logevent-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)   
 [ERROR_LINE &#40;Transact-SQL&#41;](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/error-state-transact-sql.md)   
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)  
  
  

