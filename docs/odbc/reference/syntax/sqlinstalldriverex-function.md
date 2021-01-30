---
description: Función SQLInstallDriverEx
title: Función SQLInstallDriverEx | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLInstallDriverEx
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallDriverEx
helpviewer_keywords:
- SQLInstallDriverEx function [ODBC]
ms.assetid: 1dd74544-f4e9-46e1-9b5f-c11d84fdab4c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3ebdfa732ba9c5a4a539009600749563daff4a35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189712"
---
# <a name="sqlinstalldriverex-function"></a>Función SQLInstallDriverEx
**Conformidad**  
 Versión introducida: ODBC 3,0  
  
 **Resumen**  
 **SQLInstallDriverEx** agrega información sobre el controlador a la entrada Odbcinst.ini en la información del sistema e incrementa el *UsageCount* del controlador en 1. Sin embargo, si ya existe una versión del controlador pero el valor de *UsageCount* para el controlador no existe, el nuevo valor de *UsageCount* se establece en 2.  
  
 Esta función no copia realmente ningún archivo. Es responsabilidad del programa de llamada copiar los archivos del controlador en el directorio de destino correctamente.  
  
 También se puede tener acceso a la funcionalidad de **SQLInstallDriverEx** con [ODBCCONF.EXE](../../../odbc/odbcconf-exe.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLInstallDriverEx(  
     LPCSTR    lpszDriver,  
     LPCSTR    lpszPathIn,  
     LPSTR     lpszPathOut,  
     WORD      cbPathOutMax,  
     WORD *    pcbPathOut,  
     WORD      fRequest,  
     LPDWORD   lpdwUsageCount);  
```  
  
## <a name="arguments"></a>Argumentos  
 *lpszDriver*  
 Entradas La descripción del controlador (normalmente, el nombre del DBMS asociado) presentada a los usuarios en lugar del nombre del controlador físico. El argumento *lpszDriver* debe contener una lista de pares de palabra clave-valor terminada en null que describan el controlador. Para obtener más información sobre los pares de clave-valor, vea [subclaves de especificación de controladores](../../../odbc/reference/install/driver-specification-subkeys.md). Para obtener más información sobre la cadena terminada en null, consulte [función ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md).  
  
 *lpszPathIn*  
 Entradas Ruta de acceso completa del directorio de destino de la instalación o un puntero nulo. Si *lpszPathIn* es un puntero nulo, los controladores se instalarán en el directorio del sistema.  
  
 *lpszPathOut*  
 Genere Ruta de acceso del directorio de destino en el que se debe instalar el controlador. Si el controlador no se ha instalado previamente, *lpszPathOut* debe ser el mismo que *lpszPathIn*. Si el controlador se instaló previamente, *lpszPathOut* es la ruta de acceso de la instalación anterior.  
  
 *cbPathOutMax*  
 Entradas Longitud de *lpszPathOut*.  
  
 *pcbPathOut*  
 Genere Número total de bytes (sin incluir el carácter de terminación nula) disponible para devolver en *lpszPathOut*. Si el número de bytes disponibles para devolver es mayor o igual que *cbPathOutMax*, la ruta de acceso de salida de *lpszPathOut* se trunca a *cbPathOutMax* menos el carácter de terminación null. El argumento *pcbPathOut* puede ser un puntero nulo.  
  
 *fRequest*  
 Entradas Tipo de solicitud. El argumento *fRequest* debe contener uno de los siguientes valores:  
  
 ODBC_INSTALL_INQUIRY: Consulte dónde se puede instalar un controlador.  
  
 ODBC_INSTALL_COMPLETE: complete la solicitud de instalación.  
  
 *lpdwUsageCount*  
 Genere Recuento de uso del controlador después de llamar a esta función.  
  
 Las aplicaciones no deben establecer el recuento de uso. ODBC mantendrá este recuento.  
  
## <a name="returns"></a>Devoluciones  
 La función devuelve TRUE si es correcto, FALSE si se produce un error.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLInstallDriverEx** devuelve false, se puede obtener un valor de *\* pfErrorCode* asociado mediante una llamada a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Error general del instalador|Se produjo un error en el que no había ningún error específico del instalador.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longitud de búfer no válida|El argumento *lpszPathOut* no era lo suficientemente grande como para contener la ruta de acceso de salida. El búfer contiene la ruta de acceso truncada.<br /><br /> El argumento *cbPathOutMax* era 0 y *fRequest* se ODBC_INSTALL_COMPLETE.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Tipo de solicitud no válido|El argumento *fRequest* no era uno de los siguientes:<br /><br /> ODBC_INSTALL_INQUIRY ODBC_INSTALL_COMPLETE|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Pares palabra clave-valor no válidos|El argumento *lpszDriver* contenía un error de sintaxis.|  
|ODBC_ERROR_INVALID_PATH|Ruta de instalación no válida|El argumento *lpszPathIn* contenía una ruta de acceso no válida.|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|No se pudo cargar la biblioteca de instalación del controlador o el traductor|No se pudo cargar la biblioteca de instalación del controlador.|  
|ODBC_ERROR_INVALID_PARAM_SEQUENCE|Secuencia de parámetros no válida|El argumento *lpszDriver* no contenía una lista de pares palabra clave-valor.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|No se pudo aumentar o reducir el recuento de uso de componentes|El instalador no pudo incrementar el recuento de uso del controlador.|  
  
## <a name="comments"></a>Comentarios  
 El argumento *lpszDriver* es una lista de atributos en forma de pares palabra clave-valor. Cada par se termina con un byte nulo y la lista completa finaliza con un byte nulo. (Es decir, dos bytes nulos marcan el final de la lista). El formato de esta lista es el siguiente:  
  
 _driver-DESC_ **\\** 0Driver **=** _driver-dll-nombredearchivo_ **\\** 0 [Setup **=** _setup-dll-FILENAME_ <b>\\</b> 0]  
  
 [_driver-ATTR-palabraclave1_ **=** _valor1_ <b>\\</b> 0] [_driver-ATTR-palabraclave2_ **=** _valor2_ <b>\\</b> 0]... <b>\\</b> 0,1  
  
 donde \ 0 es un byte nulo y *driver-ATTR-keywordn* es cualquier palabra clave del atributo driver. Las palabras clave deben aparecer en el orden especificado. Por ejemplo, supongamos que un controlador para archivos de texto con formato tiene archivos dll de instalación y controladores independientes y puede usar archivos con las extensiones. txt y. csv. El argumento *lpszDriver* para este controlador podría ser el siguiente:  
  
```  
Text\0Driver=TEXT.DLL\0Setup=TXTSETUP.DLL\0FileUsage=1\0  
FileExtns=*.txt,*.csv\0\0  
```  
  
 Supongamos que un controlador para SQL Server no tiene un archivo DLL de instalación independiente y no tiene ninguna palabra clave de atributo de controlador. El argumento *lpszDriver* para este controlador podría ser el siguiente:  
  
```  
SQL Server\0Driver=SQLSRVR.DLL\0\0  
```  
  
 Después de que **SQLInstallDriverEx** recupera información sobre el controlador del argumento *lpszDriver* , agrega la descripción del controlador a la sección [ODBC drivers] de la entrada Odbcinst.ini de la información del sistema. A continuación, crea una sección titulada con la descripción del controlador y agrega las rutas de acceso completas de la DLL del controlador y del archivo DLL de instalación. Por último, devuelve la ruta de acceso del directorio de destino de la instalación, pero no copia los archivos del controlador en él. En realidad, el programa de llamada debe copiar los archivos del controlador en el directorio de destino.  
  
 **SQLInstallDriverEx** incrementa en 1 el recuento de uso de componentes para el controlador instalado. Si ya existe una versión del controlador pero el recuento de uso de componentes para el controlador no existe, el nuevo valor de recuento de uso de componentes se establece en 2.  
  
 El programa de instalación de la aplicación es responsable de copiar físicamente el archivo del controlador y mantener el recuento de uso de archivos. Si el archivo del controlador no se ha instalado previamente, el programa de instalación de la aplicación debe copiar el archivo en la ruta de acceso *lpszPathIn* y crear el recuento de uso de archivos. Si el archivo se ha instalado previamente, el programa de instalación simplemente incrementa el recuento de uso de archivos y devuelve la ruta de acceso de la instalación anterior en el argumento *lpszPathOut* .  
  
> [!NOTE]  
>  Para obtener más información acerca de los recuentos de uso de componentes y recuentos de uso de archivos, consulte [recuento de uso](../../../odbc/reference/install/usage-counting.md)  
  
 Si la aplicación instaló previamente una versión anterior del archivo del controlador, el controlador debe desinstalarse y volver a instalarse, de modo que el contador de uso del componente de controlador sea válido. Se debe llamar primero a **SQLConfigDriver** (con un *fRequest* de ODBC_REMOVE_DRIVER) y, a continuación, se debe llamar a **SQLRemoveDriver** para reducir el recuento de uso de componentes. A continuación, se debe llamar a **SQLInstallDriverEx** para volver a instalar el controlador, incrementando el recuento de uso de componentes. El programa de instalación de la aplicación debe reemplazar el archivo anterior por el nuevo archivo. El recuento de uso de archivos seguirá siendo el mismo y cualquier otra aplicación que use el archivo de versión anterior usará ahora la versión más reciente.  
  
> [!NOTE]  
>  Si el controlador se instaló previamente y se llama a **SQLInstallDriverEx** para instalar el controlador en otro directorio, la función devolverá True, pero *lpszPathOut* incluirá el directorio en el que ya está instalado el controlador. No incluirá el directorio especificado en el argumento *lpszDriver* .  
  
 La longitud de la ruta de acceso en *lpszPathOut* en **SQLInstallDriverEx** permite un proceso de instalación en dos fases, por lo que una aplicación puede determinar qué *CbPathOutMax* debe ser llamando a **SQLInstallDriverEx** con un *fRequest* del modo ODBC_INSTALL_INQUIRY. Esto devolverá el número total de bytes disponibles en el búfer de *pcbPathOut* . A continuación, se puede llamar a **SQLInstallDriverEx** con un *fRequest* de ODBC_INSTALL_COMPLETE y el argumento *cbPathOutMax* establecido en el valor del búfer *pcbPathOut* , más el carácter de terminación null.  
  
 Si decide no usar el modelo de dos fases para **SQLInstallDriverEx**, debe establecer *cbPathOutMax*, que define el tamaño del almacenamiento de la ruta de acceso del directorio de destino, en el valor _MAX_PATH, como se define en stdlib. h, para evitar el truncamiento.  
  
 Cuando *fRequest* se ODBC_INSTALL_COMPLETE, **SQLInstallDriverEx** no permite que *lpszPathOut* sea NULL (o que *cbPathOutMax* sea 0). Si *fRequest* es ODBC_INSTALL_COMPLETE, se devuelve false cuando el número de bytes disponibles para devolver es mayor o igual que *cbPathOutMax*, con el resultado de que se produce el truncamiento.  
  
 Después de llamar a **SQLInstallDriverEx** y de que el programa de instalación de la aplicación haya copiado el archivo del controlador (si es necesario), el archivo dll de instalación del controlador debe llamar a **SQLConfigDriver** para establecer la configuración del controlador.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Instalación del Administrador de controladores|[SQLInstallDriverManager](../../../odbc/reference/syntax/sqlinstalldrivermanager-function.md)|
