---
description: ODBCCONF.EXE
title: ODBCCONF.EXE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- odbcconf.exe
ms.assetid: 3bf2be83-61f9-4183-836b-85204ac7116a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e62fec06a10316bb3b773cf599f11c23ba376cfd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075051"
---
# <a name="odbcconfexe"></a>ODBCCONF.EXE
ODBCCONF.exe es una herramienta de línea de comandos que permite configurar los controladores ODBC y los nombres de los orígenes de datos.  
  
> [!NOTE]  
>  ODBCCONF.exe se quitará en una versión futura de los componentes de Windows Data Access. Evite usar esta característica y piense en modificar las aplicaciones que actualmente la utilizan. Puede usar comandos de PowerShell para administrar controladores y orígenes de datos. Para obtener más información sobre estos comandos de PowerShell, vea [cmdlets de Windows Data Access Components](/powershell/module/wdac).  
  
## <a name="syntax"></a>Sintaxis  
  
```console  
ODBCCONF [switches] action  
```  
  
## <a name="arguments"></a>Argumentos  
 *centrales*  
 Cero o más opciones de modificador. Para obtener la lista de modificadores disponibles, vea la sección Comentarios, más adelante en este tema.  
  
 *action*  
 Una acción que se va a realizar. Para ver la lista de opciones disponibles, vea la sección Comentarios.  
  
## <a name="remarks"></a>Observaciones  
 Están disponibles los siguientes modificadores:  
  
|Switch|Descripción|  
|------------|-----------------|  
|/A {*Action*}|Especifique una acción.<br /><br /> /A es opcional si solo se especifica una acción.|  
|/?|Muestra el uso de ODBCCONF.EXE.|  
|/C|El procesamiento continúa si se produce un error en una acción.|  
|/E|Borre el archivo de respuesta especificado con/F cuando finalice el procesamiento.|  
|/F|Use un archivo de respuesta, como `odbcconf /F my.rsp` .<br /><br /> My. rsp podría tener el siguiente aspecto: `REGSVR c:\my.dll`<br /><br /> /A no se utiliza en un archivo de respuesta.|  
|/H|Mostrar uso (ayuda). Este modificador es el mismo que/?.|  
|/L [*modo*] *nombre de archivo*|Enviar la salida del programa a un archivo en uno de estos tres modos: normal (n), verbose (v) y debug (d). El modo de depuración registra los archivos dll cargados por odbcconf.exe.<br /><br /> Si especifica/L sin un modo, el archivo de registro estará vacío.<br /><br /> Por ejemplo, **/lv log.txt**.|  
|/R|La acción se realizará después de un reinicio.|  
|/S|Modo silencioso. No mostrar mensajes de error.|  
  
 Están disponibles las siguientes acciones:  
  
|Acción|Descripción|  
|------------|-----------------|  
|*DRIVER_NAME CONFIGDRIVER * * parámetros de configuración específicos del controlador*|Carga el archivo DLL de instalación del controlador adecuado y llama a la función **ConfigDriver** .<br /><br /> Equivalente a la [función SQLConfigDriver](../odbc/reference/syntax/sqlconfigdriver-function.md).<br /><br /> Por ejemplo:<br /><br /> /A {CONFIGDRIVER "nombre de controlador" "CPTimeout = 60"}<br /><br /> /A {CONFIGDRIVER "nombre de controlador" "DriverODBCVer = 03.80"}|  
|CONFIGDSN *driver_name* DSN =*nombre* &#124; *atributos*|Agrega o modifica un origen de datos del sistema.<br /><br /> Equivalente a la [función SQLConfigDataSource](../odbc/reference/syntax/sqlconfigdatasource-function.md).<br /><br /> Por ejemplo:<br /><br /> /A {CONFIGDSN "SQL Server" "DSN = nombre &#124; Server = SRV"}|  
|CONFIGSYSDSN *driver_name* DSN =*nombre* &#124; *atributos*|Agrega o modifica un origen de datos del sistema.<br /><br /> Equivalente a la [función SQLConfigDataSource](../odbc/reference/syntax/sqlconfigdatasource-function.md).<br /><br /> Por ejemplo:<br /><br /> /A {CONFIGSYSDSN "SQL Server" "DSN = nombre &#124; Server = SRV"}|  
|INSTALLDRIVER|Equivalente a la [función SQLInstallDriverEx](../odbc/reference/syntax/sqlinstalldriverex-function.md).<br /><br /> Para obtener información sobre la sintaxis de pares de palabra clave y valor que se pasa a INSTALLDRIVER, vea [subclaves de especificación de controladores](../odbc/reference/install/driver-specification-subkeys.md).<br /><br /> Por ejemplo:<br /><br /> /A {INSTALLDRIVER "driver &#124; driver =c:\your.dll &#124; Setup =c:\your.dll &#124; APILevel = 2 &#124; ConnectFunctions = YYY &#124; DriverODBCVer = 03.50 &#124; FileUsage = 0 &#124; SQLLevel = 1"}|  
|Configuración de INSTALLTRANSLATOR *Translator * * ruta de acceso del controlador*|Agrega información acerca de un traductor a la clave del registro **HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBCINST.INI \Odbc Translator** .<br /><br /> Equivalente a la [función SQLInstallTranslatorEx](../odbc/reference/syntax/sqlinstalltranslatorex-function.md).<br /><br /> Para obtener información sobre la sintaxis de pares de palabra clave y valor que se pasa a INSTALLDRIVER, consulte [subclaves](../odbc/reference/install/translator-specification-subkeys.md)de la especificación de traductor.<br /><br /> Por ejemplo:<br /><br /> /A {INSTALLTRANSLATOR "mi traductor &#124; Translator =c:\my.dll &#124; Setup =c:\my.dll"}|  
|*Dll* de REGSVR|Registra un archivo DLL.<br /><br /> Equivalente a regsvr32.exe.<br /><br /> Por ejemplo:<br /><br /> /A {REGSVR c:\my.dll}|  
|SETFILEDSNDIR|Cuando HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBC.INI DSN\DefaultDSNDir de archivo \ODBC no existe, la acción SETFILEDSNDIR lo creará y le asignará el valor en HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\CommonFilesDir, anexado con orígenes de \ODBC\Data.<br /><br /> El valor en HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBC.INI archivo \ODBC DSN\DefaultDSNDir especifica la ubicación predeterminada que usa el administrador de orígenes de datos ODBC al crear un origen de datos basado en archivos.<br /><br /> Por ejemplo:<br /><br /> /A {SETFILEDSNDIR}|  
  
## <a name="see-also"></a>Consulte también  
 [Microsoft Open Database Connectivity (ODBC)](../odbc/microsoft-open-database-connectivity-odbc.md)
