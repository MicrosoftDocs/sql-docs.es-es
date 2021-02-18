---
title: Analizador (utilidad)
description: La utilidad Profiler inicia la herramienta SQL Server Profiler. Hay argumentos opcionales que le permiten controlar cómo se inicia la aplicación.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server], profiler90 utility
- profiler90 utility
- Profiler [SQL Server Profiler], starting
- SQL Server Profiler, starting
- starting SQL Server Profiler
ms.assetid: e91c30a9-0d29-4f84-bcb8-e8fb62afadda
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ab40ea190dab072d7415ded1f399cfa4a4b78568
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335951"
---
# <a name="profiler-utility"></a>Analizador (utilidad)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  La utilidad **profiler** inicia la herramienta [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] . Los argumentos opcionales que se enumeran más adelante en este tema permiten controlar cómo se inicia la aplicación.  
  
> [!NOTE]  
>  La utilidad **profiler** no está pensada para la creación de scripts en seguimientos. Para más información, consulte [SQL Server Profiler](../tools/sql-server-profiler/sql-server-profiler.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
profiler  
    [ /? ] |  
[  
{  
{ /U login_id [ /P password ] }  
| /E  
}  
{[ /S sql_server_name ] | [ /A analysis_services_server_name ] }  
[ /D database ]  
[ /T "template_name" ]  
[ /B { "trace_table_name" } ]  
{ [/F "filename" ] | [ /O "filename" ] }  
[ /L locale_ID ]  
[ /M "MM-DD-YY hh:mm:ss" ]  
[ /R ]  
[ /Z file_size ]  
]  
```  
  
## <a name="arguments"></a>Argumentos  
 **/?**  
 Muestra el resumen de sintaxis de los argumentos de **profiler** .  
  
 **/U** *login_id*  
 Es el Id. de inicio de sesión para la autenticación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Los Id. de inicio de sesión distinguen entre mayúsculas y minúsculas.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteWinAuthentication](../includes/ssnotewinauthentication-md.md)].  
  
 **/P** *password*  
 Especifica una contraseña especificada por el usuario para la autenticación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 **/E**  
 Especifica la conexión con la autenticación de Windows con las credenciales actuales del usuario.  
  
 **/S**  *sql_server_name*  
 Especifica una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Profiler se conectará automáticamente con el servidor especificado usando la información de autenticación que se especifica en los modificadores **/U** y **/P** o en el modificador **/E** . Para conectar una instancia con nombre de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], use **/S** *sql_server_name*\\*instance_name*.  
  
 **/A**  *analysis_services_server_name*  
 Especifica una instancia de Analysis Services. Profiler se conectará automáticamente con el servidor especificado usando la información de autenticación que se especifica en los modificadores **/U** y **/P** o en el modificador **/E** . Para conectar con una instancia con nombre de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], use **/A** *analysis_services_server_name\instance_name*.  
  
 **/D** *database*  
 Especifica el nombre de la base de datos que se utilizará con la conexión. Esta opción seleccionará la base de datos predeterminada para el usuario especificado si no se especifica ninguna base de datos.  
  
 **/B "** *trace_table_name* **"**  
 Especifica una tabla de seguimiento que se cargará cuando se inicie Profiler. Debe especificar la base de datos, el usuario o esquema, y la tabla.  
  
 **/T"** *template_name* **"**  
 Especifica la plantilla que se cargará para configurar el seguimiento. El nombre de la plantilla debe estar entre comillas. El nombre de la plantilla debe estar en el directorio de plantillas del sistema o en el directorio de plantillas del usuario. Si existen dos plantillas con el mismo nombre en ambos directorios, la plantilla del directorio del sistema se cargará. Si no existe ninguna plantilla con el nombre especificado se cargará la plantilla estándar. Tenga en cuenta que la extensión de archivo para la plantilla (.tdf) no debe especificarse como parte de *template_name*. Por ejemplo:  
  
```  
/T "standard"  
```  
  
 **/F"** *filename* **"**  
 Especifica la ruta y el nombre de un archivo de seguimiento que se cargará cuando se inicie Profiler. Toda la ruta y el nombre de archivo debe estar entre comillas. Esta opción no se puede usar con **/O**.  
  
 **/O "** *filename*  **"**  
 Especifica la ruta y el nombre de un archivo en el que deben escribirse los resultados del seguimiento. Toda la ruta y el nombre de archivo debe estar entre comillas. Esta opción no se puede usar con **/F.**  
  
 **/L** *locale_ID*  
 No disponible.  
  
 **/M "** *MM-DD-YY hh:mm:ss* **"**  
 Especifica la fecha y la hora de detención del seguimiento. La hora de detención debe estar entre comillas. Especifique la hora de detención de acuerdo con los parámetros de la siguiente tabla:  
  
|Parámetro|Definición|  
|---------------|----------------|  
|MM|Mes de dos dígitos|  
|DD|Día de dos dígitos|  
|YY|Año de dos dígitos|  
|hh|Hora de dos dígitos en un reloj de 24 horas|  
|MM|Minuto de dos dígitos|  
|ss|Segundo de dos dígitos|  
  
> [!NOTE]  
>  El formato "MM-DD-YY hh:mm:ss" solo se puede usar si la opción **Usar la configuración regional para mostrar valores de fecha y hora** está habilitada en [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]. Si esta opción no está habilitada, debe utilizar  el formato de fecha y hora "YYYY-MM-DD hh:mm:ss".  
  
 **/R**  
 Habilita la sustitución incremental de los archivos de seguimiento.  
  
 **/Z**  *file_size*  
 Especifica el tamaño del archivo de seguimiento en megabytes (MB). El tamaño predeterminado es 5 MB. Si está habilitada la sustitución incremental de archivos, todos los archivos de sustitución incremental estarán limitados al valor especificado en el argumento.  
  
## <a name="remarks"></a>Observaciones  
 Para iniciar un seguimiento con una plantilla específica, use las opciones **/S** y **/T** juntas. Por ejemplo, para iniciar un seguimiento mediante la plantilla Standard en MyServer\MyInstance, escriba lo siguiente en el símbolo del sistema:  
  
```  
profiler /S MyServer\MyInstance /T "Standard"  
```  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la utilidad del símbolo del sistema &#40;motor de base de datos&#41;](../tools/command-prompt-utility-reference-database-engine.md)  
  
  
