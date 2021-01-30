---
description: Función SQLCreateDataSource
title: Función SQLCreateDataSource | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLCreateDataSource
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLCreateDataSource
helpviewer_keywords:
- SQLCreateDataSource function [ODBC]
ms.assetid: 76ee851a-dca9-40cc-8e9e-eb3f74e560ee
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: feca3e6c135e4dba441a20b1fa458f990b437fde
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206109"
---
# <a name="sqlcreatedatasource-function"></a>Función SQLCreateDataSource
**Conformidad**  
 Versión introducida: ODBC 2,0  
  
 **Resumen**  
 **SQLCreateDataSource** muestra un cuadro de diálogo con el que el usuario puede Agregar un origen de datos.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLCreateDataSource(  
     HWND    hwnd,  
     LPSTR   lpszDS);  
```  
  
## <a name="arguments"></a>Argumentos  
 *identificador*  
 Entradas Identificador de la ventana primaria.  
  
 *lpszDS*  
 Entradas Nombre del origen de datos. *lpszDS* puede ser un puntero nulo o una cadena vacía.  
  
## <a name="returns"></a>Devoluciones  
 **SQLCreateDataSource** devuelve true si se crea el origen de datos. De lo contrario, devuelve FALSE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLCreateDataSource** devuelve false, se puede obtener un valor de *\* pfErrorCode* asociado mediante una llamada a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Error general del instalador|Se produjo un error en el que no había ningún error específico del instalador.|  
|ODBC_ERROR_INVALID_HWND|Identificador de ventana no válido|El argumento *hWnd* no era válido o era null.|  
|ODBC_ERROR_INVALID_DSN|DSN no válido|El argumento *lpszDS* contenía una cadena que no era válida para un DSN.|  
|ODBC_ERROR_REQUEST_FAILED|Error en la *solicitud*|Error en la llamada a **ConfigDSN** con la opción ODBC_ADD_DSN.|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|No se pudo cargar la biblioteca de instalación del controlador o el traductor|No se pudo cargar la biblioteca de instalación del controlador.|  
|ODBC_ERROR_USER_CANCELED|Operación cancelada por el usuario|El usuario canceló la creación de un nuevo origen de datos.|  
|ODBC_ERROR_CREATE_DSN_FAILED|No se pudo crear el DSN solicitado|No se pudo conectar a la base de datos. la llamada a **SQLDriverConnect** para un DSN de archivo no devolvió una conexión correcta.<br /><br /> No se pudo escribir en el archivo.|  
|ODBC_ERROR_OUT_OF_MEM|No hay memoria suficiente|El instalador no pudo realizar la función debido a una falta de memoria.|  
  
## <a name="comments"></a>Comentarios  
 Si *hWnd* es null, **SQLCreateDataSource** devuelve false. De lo contrario, muestra el cuadro de diálogo **crear nuevo origen de datos** con una página del Asistente para elegir el tipo de origen de datos que se va a configurar, tal como se muestra en la siguiente ilustración.  
  
 ![Cuadro de diálogo Crear un nuevo origen de datos: seleccionar tipo](../../../odbc/reference/syntax/media/ch23a.gif "CH23A")  
  
 La opción predeterminada es **origen de datos de archivo**. Cuando se ha seleccionado un origen de datos y se hace clic en **siguiente** , se muestra la siguiente página del asistente que contiene una lista de controladores instalados.  
  
 ![Cuadro de diálogo Crear un nuevo origen de datos: seleccionar controlador](../../../odbc/reference/syntax/media/ch23b.gif "CH23B")  
  
 Si se hace clic en **Cancelar** , el cuadro de diálogo desaparece y **SQLCreateDataSource** devuelve false con el código de error de ODBC_ERROR_USER_CANCELED. Si se seleccionó la opción **origen de datos del usuario** o origen de **datos del sistema** , el botón **Opciones avanzadas** no estará disponible.  
  
 Cuando se haga clic en el botón **siguiente** , se producirá una de las siguientes acciones, en función del tipo de origen de datos seleccionado:  
  
-   Si se seleccionó **origen de datos de archivo** , se muestra una página del Asistente para que el usuario escriba un nombre de archivo.  
  
-   Si se seleccionó origen de **datos de usuario** o **origen de datos del sistema** , se muestra una página del asistente que muestra el tipo de origen de datos y controlador para su revisión y, cuando se hace clic en **Finalizar** , se configura el origen de datos.  
  
 Si se hace clic en **Opciones avanzadas** en la página crear nuevo origen de datos del asistente, se muestra una página del Asistente para que el usuario escriba información específica del controlador. En el cuadro de texto de este cuadro de diálogo, escriba el controlador y las palabras clave separadas por devoluciones, tal y como se muestra en la siguiente ilustración.  
  
 ![Cuadro de diálogo Configuración de creación avanzada de DSN de archivo](../../../odbc/reference/syntax/media/ch23c.gif "CH23C")  
  
 Encontrará otras palabras clave específicas del controlador en la descripción de **SQLDriverConnect**. Se permite todo excepto **DSN** .  
  
 El valor predeterminado para la opción **comprobar esta conexión** es true. Este valor predeterminado se aplica tanto si esta página del asistente está activada como si no. Si se hace clic en **Aceptar** , la cadena especificada en el cuadro de texto y el valor de la opción **comprobar esta conexión** se almacenan en caché. (Si se hace clic en el botón **cerrar** o en **Cancelar** , se perderá toda la información específica del controlador que se ha introducido recientemente, ya que la cadena especificada en el cuadro de texto y el valor de la opción **comprobar esta conexión** no se almacenan en caché).  
  
 Si se seleccionó **origen de datos de archivo** en la primera página del asistente, después de seleccionar un controlador y de especificar los valores de la palabra clave en la página avanzadas del asistente, se pide al usuario que escriba un nombre de archivo. Haga clic en **examinar** para buscar un nombre de archivo, en cuyo caso el directorio predeterminado del cuadro **examinar** se especifica mediante una combinación de la ruta de acceso especificada por CommonFileDir en HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion y "ODBC\DataSources". (Si CommonFileDir era "C:\Archivos de Programa\archivos files", el directorio predeterminado sería "C:\Archivos de Programa\archivos Files\ODBC\Data sources").  
  
 Cuando se especifica un nombre de archivo y se hace clic en **siguiente** , se comprueba la validez del nombre de archivo especificado para las reglas de nomenclatura de archivos estándar del sistema operativo. Si el nombre de archivo no es válido, un cuadro de mensaje de error notifica al usuario que se especificó un nombre de archivo no válido. Después de que el usuario confirme el cuadro de mensaje, el foco se devolverá a la página del asistente en la que se escribe el nombre de archivo. Si el nombre de archivo es válido, se muestra una página del asistente que muestra los pares palabra clave-valor seleccionados para su revisión, tal como se muestra en la siguiente ilustración.  
  
 ![Cuadro de diálogo Crear un nuevo origen de datos: revisar](../../../odbc/reference/syntax/media/ch23d.gif "CH23D")  
  
 Si se hace clic en **Finalizar** y se seleccionó **origen de datos de archivo** como tipo de origen de datos y la opción **comprobar esta conexión** es true, se llama a **SQLDriverConnect** con las palabras clave **SaveFile** y **driver** . El argumento *DriverCompletion* se establece en SQL_DRIVER_COMPLETE. El nombre de archivo de la palabra clave **SaveFile** es el nombre que se especificó o eligió, y el nombre del controlador de la palabra clave **driver** es el nombre elegido. Si se especificó una cadena de conexión específica del controlador en la página avanzadas del asistente, esa cadena se anexa después de la palabra clave **driver** .  
  
 Si **SQLDriverConnect** devuelve SQL_SUCCESS, el administrador de controladores ha creado el DSN de archivo. **SQLCreateDataSource** devuelve true. Si **SQLDriverConnect** no devuelve SQL_SUCCESS, un cuadro de mensaje de advertencia indica que no se pudo establecer una conexión con el origen de datos. Todavía se puede crear un DSN con información de conexión mínima. Este cuadro de mensaje permite al usuario cancelar o continuar con la creación del DSN de archivo.  
  
 Si el usuario decide seguir creando el DSN, este proceso continúa como si la opción **comprobar que esta conexión** se estableció en false. Si el usuario elige cancelar, se devuelve FALSE para **SQLCreateDataSource** con un código de error de ODBC_ERROR_CREATE_DSN_FAILED.  
  
 Si se seleccionó **origen de datos de archivo** como el tipo de origen de datos y la opción comprobar que **esta conexión** es falsa, se crea un DSN de archivo con la palabra clave **driver** y la cadena de conexión especificada por el usuario (si existe) en la página avanzadas del asistente. Si la creación del archivo se ha realizado correctamente, se devuelve TRUE para **SQLCreateDataSource**. Si la creación del archivo no fue correcta, un cuadro de mensaje de error notifica al usuario el error que se devolvió desde el sistema operativo. Se devuelve FALSE para **SQLCreateDataSource** con un código de error de ODBC_ERROR_CREATE_DSN_FAILED. Para obtener más información acerca de los orígenes de datos de archivo, vea [conectarse con orígenes de datos de archivo](../../../odbc/reference/develop-app/connecting-using-file-data-sources.md)o ver [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
 Si se seleccionó origen de datos de **usuario** o **del sistema** como el tipo de origen de datos, se llama a **ConfigDSN** en la biblioteca de instalación de controladores con el ODBC_ADD_DSN *fRequest*. Para obtener más información, vea [ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Administración de orígenes de datos|[SQLManageDataSources](../../../odbc/reference/syntax/sqlmanagedatasources.md)|
