---
description: Usar tokens en pasos de trabajo
title: Usar tokens en pasos de trabajo
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- job steps [SQL Server Agent]
- security [SQL Server Agent], enabling alert job step tokens
- SQL Server Agent jobs, job steps
- tokens [SQL Server]
- escape macros [SQL Server Agent]
ms.assetid: 105bbb66-0ade-4b46-b8e4-f849e5fc4d43
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 7463f708819d772ef8d9e94a8f8b58ca4a7e4f7a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344687"
---
# <a name="use-tokens-in-job-steps"></a>Usar tokens en pasos de trabajo
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El Agente le permite usar tokens en scripts de pasos de trabajo de [!INCLUDE[tsql](../../includes/tsql-md.md)] . La utilización de tokens al escribir pasos de trabajo ofrece la misma flexibilidad que las variables al escribir programas de software. Una vez que se inserta un token en un script de pasos de trabajo, el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sustituye el token en tiempo de ejecución, antes de que el subsistema de [!INCLUDE[tsql](../../includes/tsql-md.md)] ejecute el paso de trabajo.  
  
  
## <a name="understanding-using-tokens"></a>Descripción del uso de tokens  
  
> [!IMPORTANT]  
> Todos los usuarios de Windows que tengan permisos de escritura en el Registro de eventos de Windows pueden tener acceso a los pasos de trabajo activados por alertas del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o de WMI. Para evitar este riesgo de seguridad, se deshabilitan de manera predeterminada los tokens del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que pueden utilizarse en trabajos activados por alertas. Estos tokens son: **A-DBN**, **A-SVR**, **A-ERR**, **A-SEV**, **A-MSG** y **WMI(** _propiedad_ **)** . Tenga en cuenta que en esta versión el uso de los tokens se ha ampliado a todas las alertas.  
>   
> Si necesita usar estos tokens, asegúrese primero de que solo los miembros de los grupos de seguridad de Windows de confianza, como el grupo Administradores, tienen permisos de escritura en el registro de eventos del equipo donde reside [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . A continuación, para habilitar estos tokens, haga clic con el botón derecho en **Agente SQL Server** en el Explorador de objetos, elija **Propiedades** y, en la página **Sistema de alerta** , active la casilla **Reemplazar tokens para todas las respuestas de trabajos a alertas** .  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] La sustitución del token del Agente es sencilla y eficaz: el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reemplaza el token por un valor literal de cadena exacto. Todos los tokens distinguen mayúsculas de minúsculas. Los pasos de trabajo deben tenerlo en cuenta y citar correctamente los tokens utilizados o convertir la cadena de reemplazo al tipo de datos correcto.  
  
Por ejemplo, se puede usar la siguiente instrucción para imprimir el nombre de la base de datos en un paso de trabajo:  
  
`PRINT N'Current database name is $(ESCAPE_SQUOTE(A-DBN))' ;`  
  
En este ejemplo, la macro **ESCAPE_SQUOTE** se inserta con el token **A-DBN** . En tiempo de ejecución, el nombre de la base de datos apropiada sustituirá al token **A-DBN** . La macro de escape convierte cualquier comilla simple que se pueda pasar de forma inadvertida en la cadena de reemplazo del token. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El Agente reemplazará una comilla simple con dos comillas simples en la cadena final.  
  
Por ejemplo, si la cadena que se pasa para reemplazar al token es `AdventureWorks2012'SELECT @@VERSION --`, el comando que ejecuta el paso de trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] será:  
  
`PRINT N'Current database name is AdventureWorks2012''SELECT @@VERSION --' ;`  
  
En este caso, la instrucción insertada, `SELECT @@VERSION`, no se ejecuta. En su lugar, la comilla simple adicional hace que el servidor analice la instrucción insertada como una cadena. Si la cadena de reemplazo del token no contiene una comilla simple, no se convierte ninguno de los caracteres y el paso de trabajo que contiene el token se ejecutará como se espera.  
  
Para depurar el uso de tokens en pasos de trabajo, utilice instrucciones de impresión como, por ejemplo, `PRINT N'$(ESCAPE_SQUOTE(SQLDIR))'` y guarde la salida del paso de trabajo en un archivo o una tabla. Use la página **Avanzadas** del cuadro de diálogo **Propiedades de paso de trabajo** para especificar un archivo o tabla de salida del paso de trabajo.  
  
## <a name="sql-server-agent-tokens-and-macros"></a>Tokens y macros del Agente SQL Server  
En las siguientes tablas se indican y describen los tokens y macros compatibles con el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="sql-server-agent-tokens"></a>Tokens del Agente SQL Server  
  
|Token|Descripción|  
|---------|---------------|  
|**(A-DBN)**|nombre de base de datos. Si el trabajo se ejecuta a causa de una alerta, el valor del nombre de la base de datos sustituye automáticamente a este token en el paso de trabajo.|  
|**(A-SVR)**|Nombre de servidor. Si el trabajo se ejecuta a causa de una alerta, el valor del nombre del servidor sustituye automáticamente a este token en el paso de trabajo.|  
|**(A-ERR)**|Número de error. Si el trabajo se ejecuta a causa de una alerta, el valor del número de error sustituye automáticamente a este token en el paso de trabajo.|  
|**(A-SEV)**|Gravedad del error. Si el trabajo se ejecuta a causa de una alerta, el valor de la gravedad del error sustituye automáticamente a este token en el paso de trabajo.|  
|**(A-MSG)**|Texto del mensaje. Si el trabajo se ejecuta a causa de una alerta, el valor del texto del mensaje sustituye automáticamente a este token en el paso de trabajo.|  
|**(JOBNAME)**|Nombre del trabajo. Este token solo está disponible en SQL Server 2016 y versiones posteriores.|  
|**(STEPNAME)**|Nombre del paso. Este token solo está disponible en SQL Server 2016 y versiones posteriores.|  
|**(DATE)**|Fecha actual (en formato AAAAMMDD).|  
|**(INST)**|Nombre de instancia. Para una instancia predeterminada, este token tendrá el nombre de instancia predeterminado: MSSQLSERVER.|  
|**(JOBID)**|Id. del trabajo.|  
|**(MACH)**|Nombre del equipo.|  
|**(MSSA)**|Nombre del servicio SQLServerAgent maestro.|  
|**(OSCMD)**|Prefijo del programa usado para ejecutar los pasos de trabajo de **CmdExec** .|  
|**(SQLDIR)**|Directorio en el que se instala [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . El directorio predeterminado es C:\Archivos de programa\Microsoft SQL Server\MSSQL.|  
|**(SQLLOGDIR)**|Token de reemplazo para la ruta de la carpeta de registro de errores de SQL Server; por ejemplo, $(ESCAPE_SQUOTE(SQLLOGDIR)). Este token solo está disponible en SQL Server 2014 y versiones posteriores.|  
|**(STEPCT)**|Contador del número de veces que se ha ejecutado este paso (sin incluir los reintentos). Lo puede utilizar el comando del paso para forzar la terminación de un bucle de múltiples pasos.|  
|**(STEPID)**|Id. del paso.|  
|**(SRVR)**|Nombre del equipo en el que se ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es una instancia con nombre, incluye el nombre.|  
|**(TIME)**|Hora actual (en formato HHMMSS).|  
|**(STRTTM)**|La hora (en formato HHMMSS) en la que el trabajo empezó a ejecutarse.|  
|**(STRTDT)**|La fecha (en formato AAAAMMDD) en la que el trabajo empezó a ejecutarse.|  
|**(WMI(** _propiedad_ **))**|Para los trabajos que se ejecutan en respuesta a las alertas de WMI, es el valor de la propiedad especificada por *property*. Por ejemplo, `$(WMI(DatabaseName))` proporciona el valor de la propiedad **DatabaseName** del evento WMI que provocó la ejecución de la alerta.|  
  
### <a name="sql-server-agent-escape-macros"></a>Macros de escape del Agente SQL Server  
  
|Macros de escape|Descripción|  
|-----------------|---------------|  
|**$(ESCAPE_SQUOTE(** _token\_name_ **))**|Convierte las comillas simples (') en la cadena de reemplazo del token. Sustituye una comilla simple por dos comillas simples.|  
|**$(ESCAPE_DQUOTE(** _token\_name_ **))**|Convierte las comillas dobles ('') en la cadena de reemplazo del token. Sustituye una comilla doble por dos comillas dobles.|  
|**$(ESCAPE_RBRACKET(** _token\_name_ **))**|Convierte los corchetes de cierre (]) en la cadena de reemplazo del token. Sustituye un corchete de cierre por dos corchetes de cierre.|  
|**$(ESCAPE_NONE(** _token\_name_ **))**|Sustituye el token sin convertir ninguno de los caracteres de la cadena. Se incluye esta macro para la compatibilidad con versiones anteriores en entornos donde las cadenas de reemplazo de tokens se esperan únicamente de usuarios de confianza. Para obtener más información, vea "Actualizar pasos de trabajo para usar macros" más adelante en este tema.|  
  
## <a name="updating-job-steps-to-use-macros"></a>Actualizar pasos de trabajo para usar macros  
La tabla siguiente describe cómo el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] administra el reemplazo de los tokens. Para habilitar o deshabilitar la alerta de sustitución de los tokens, haga clic con el botón derecho en **Agente SQL Server** en el Explorador de objetos, seleccione **Propiedades** y, en la página **Sistema de alerta** , active o desactive la casilla **Reemplazar tokens para todas las respuestas de trabajos a alertas** .  
  
|Sintaxis de los tokens|Alerta de sustitución de tokens activada|Alerta de sustitución de tokens desactivada|  
|----------------|------------------------------|-------------------------------|  
|Macro ESCAPE utilizada|Todos los tokens de los trabajos se han sustituido correctamente.|Los tokens activados por alertas no se han sustituido. Estos tokens son: **A-DBN**, **A-SVR**, **A-ERR**, **A-SEV**, **A-MSG** y **WMI(** _propiedad_ **)** . Otros tokens estáticos se han sustituido correctamente.|  
|Macro ESCAPE no utilizada|Los trabajos que contienen tokens producen un error.|Los trabajos que contienen tokens producen un error.|  
  
## <a name="token-syntax-update-examples"></a>Ejemplos de actualización de la sintaxis del token  
  
### <a name="a-using-tokens-in-non-nested-strings"></a>A. Usar tokens en cadenas no anidadas  
En el siguiente ejemplo se muestra cómo actualizar un script simple no anidado con la macro de escape apropiada. Antes de ejecutar el script de actualización, el siguiente script de paso de trabajo usa un token de paso de trabajo para imprimir el nombre de la base de datos apropiada:  
  
`PRINT N'Current database name is $(A-DBN)' ;`  
  
Después de ejecutar el script de actualización, se inserta una macro `ESCAPE_NONE` antes del token `A-DBN` . Debido a que se utilizan comillas simples para delimitar la cadena de impresión, se debe actualizar el paso de trabajo mediante la inserción de la macro `ESCAPE_SQUOTE` , del siguiente modo:  
  
`PRINT N'Current database name is $(ESCAPE_SQUOTE(A-DBN))' ;`  
  
### <a name="b-using-tokens-in-nested-strings"></a>B. Usar tokens en cadenas anidadas  
En scripts de pasos de trabajo donde se utilizan tokens en cadenas o instrucciones anidadas, éstas deben volver a escribirse como varias instrucciones antes de insertarse en las macros de escape apropiadas.  
  
Por ejemplo, considere el siguiente paso de trabajo, que utiliza el token `A-MSG` y no se ha actualizado con una macro de escape:  
  
`PRINT N'Print ''$(A-MSG)''' ;`  
  
Después de ejecutar el script de actualización, se inserta una macro `ESCAPE_NONE` con el token. No obstante, en este caso, se debería volver a escribir el script sin usar anidamiento e insertar la macro `ESCAPE_SQUOTE` para convertir los delimitadores que se puedan haber pasado en la cadena de reemplazo del token:  
  
```sql
DECLARE @msgString nvarchar(max);
SET @msgString = '$(ESCAPE_SQUOTE(A-MSG))';
SET @msgString = QUOTENAME(@msgString,'''');
PRINT N'Print ' + @msgString;
```
  
Además, tenga en cuenta que en este ejemplo la función QUOTENAME establece el carácter de cita.  
  
### <a name="c-using-tokens-with-the-escape_none-macro"></a>C. Usar tokens con la macro ESCAPE_NONE  
El siguiente ejemplo es parte de un script que recupera el `job_id` de la tabla `sysjobs` y utiliza el token `JOBID` para rellenar la variable `@JobID` , que se declaró previamente en el script como un tipo de datos binario. Tenga en cuenta que, debido a que no se requieren delimitadores para los tipos de datos binarios, la macro `ESCAPE_NONE` se usa con el token `JOBID` . Una vez que se ejecute el script de actualización, no debería ser necesario actualizar este paso de trabajo.  
  
```sql
SELECT * FROM msdb.dbo.sysjobs  
WHERE @JobID = CONVERT(uniqueidentifier, $(ESCAPE_NONE(JOBID)));
```
  
## <a name="see-also"></a>Consulte también  
[Implementar trabajos](../../ssms/agent/implement-jobs.md)  
[Administrar pasos de trabajo](../../ssms/agent/manage-job-steps.md)  
