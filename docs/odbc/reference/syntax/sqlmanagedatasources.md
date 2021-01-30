---
description: SQLManageDataSources
title: SQLManageDataSources | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLManageDataSources
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLManageDataSources
helpviewer_keywords:
- SQLManageDataSources [ODBC]
ms.assetid: ac6d186f-b394-406c-94c4-c6331d1ca468
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4fbb34d2daafc4a135cbf51c2b0382d3965f9dfc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160805"
---
# <a name="sqlmanagedatasources"></a>SQLManageDataSources
**Conformidad**  
 Versión introducida: ODBC 2,0  
  
 **Resumen**  
 **SQLManageDataSources** muestra un cuadro de diálogo con el que los usuarios pueden configurar, agregar y eliminar orígenes de datos en la información del sistema.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLManageDataSources(  
     HWND     hwnd);  
```  
  
## <a name="arguments"></a>Argumentos  
 *identificador*  
 Entradas Identificador de la ventana primaria.  
  
## <a name="returns"></a>Devoluciones  
 **SQLManageDataSources** devuelve false si *hWnd* no es un identificador de ventana válido. De lo contrario, devuelve TRUE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLManageDataSources** devuelve false, se puede obtener un valor de *\* pfErrorCode* asociado mediante una llamada a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Error general del instalador|Se produjo un error en el que no había ningún error específico del instalador.|  
|ODBC_ERROR_REQUEST_FAILED|Error en la *solicitud*|Error en la llamada a **ConfigDSN** .|  
|ODBC_ERROR_INVALID__HWND|Identificador de ventana no válido|El argumento *hWnd* no era válido o era null.|  
|ODBC_ERROR_OUT_OF_MEM|No hay memoria suficiente|El instalador no pudo realizar la función debido a una falta de memoria.|  
  
## <a name="managing-data-sources"></a>Administrar orígenes de datos  
 **SQLManageDataSources** inicialmente muestra el cuadro de diálogo **Administrador de orígenes de datos ODBC** , tal como se muestra en la siguiente ilustración.  
  
 ![Cuadro de diálogo Administrador de ODBC de origen de datos](../../../odbc/reference/syntax/media/ch23e.gif "CH23E")  
  
 En el cuadro de diálogo se muestran los orígenes de datos enumerados en la información del sistema en tres pestañas: **DSN de usuario**, **DSN de sistema** y DSN de **archivo**. Si el usuario hace doble clic en un origen de datos o selecciona un origen de datos y hace clic en **configurar**, **SQLManageDataSources** llama a **ConfigDSN** en el archivo DLL de instalación con la opción ODBC_CONFIG_DSN.  
  
 Si el usuario hace clic en **Agregar**, **SQLManageDataSources** muestra el cuadro de diálogo **crear nuevo origen de datos** , que se muestra en la siguiente ilustración.  
  
 ![Cuadro de diálogo Crear un nuevo origen de datos](../../../odbc/reference/syntax/media/ch23f.gif "CH23F")  
  
 El cuadro de diálogo muestra una lista de los controladores instalados. Si el usuario hace doble clic en un controlador o selecciona un controlador y hace clic en **Aceptar**, **SQLManageDataSources** llama a **ConfigDSN** en el archivo dll de instalación y le pasa la opción ODBC_ADD_DSN.  
  
 Si el usuario selecciona un origen de datos y hace clic en **quitar**, **SQLManageDataSources** pregunta si el usuario desea eliminar el origen de datos. Si el usuario hace clic en **sí**, **SQLManageDataSources** llama a **ConfigDSN** en el archivo DLL de instalación con la opción ODBC_REMOVE_DSN.  
  
 El cuadro de diálogo **crear nuevo origen de datos** se utiliza para agregar o eliminar un origen de datos de usuario, un origen de datos del sistema o un origen de datos de archivo.  
  
## <a name="user-dsns"></a>DSN de usuario  
 Los DSN creados para usuarios individuales se denominarán DSN de usuario para distinguirlos de los DSN del sistema. Los DSN de usuario se registran como se indica a continuación en la información del sistema:  
  
 `HKEY_CURRENT_USERS`  
  
 `SOFTWARE`  
  
 `ODBC`  
  
 `Odbc.ini`  
  
## <a name="system-dsns"></a>DSN de sistema  
 El cuadro de diálogo **crear nuevo origen de datos** permite agregar un origen de datos del sistema al equipo local o eliminar uno, o establecer la configuración de un origen de datos del sistema.  
  
 Un origen de datos configurado con un nombre de origen de datos del sistema (DSN) puede ser utilizado por más de un usuario en el mismo equipo. También lo puede usar un servicio de todo el sistema, que puede obtener acceso al origen de datos incluso si ningún usuario ha iniciado sesión en la máquina.  
  
 Un DSN del sistema se registra en la entrada HKEY_LOCAL_MACHINE en la información del sistema en lugar de en la entrada HKEY_CURRENT_USER. No está vinculado a un usuario que inicia sesión con su nombre de usuario y contraseña particulares, pero lo puede usar cualquier usuario de ese equipo o un servicio automatizado de todo el sistema. Sin embargo, el DSN del sistema está vinculado a un equipo. No admite la capacidad de usar DSN remotos entre máquinas. Los DSN del sistema se registran como se indica a continuación en la información del sistema:  
  
 Odbc.ini DE HKEY_LOCAL_MACHINE DE SOFTWARE ODBC  
  
## <a name="file-dsns"></a>DSN de archivo  
 Un origen de datos de archivo no tiene un nombre de origen de datos, como un origen de datos de la máquina, y no está registrado en ningún usuario o equipo. La información de conexión para ese origen de datos se encuentra en un archivo. DSN que se puede copiar en cualquier equipo. Un origen de datos de archivo puede ser compartible, en cuyo caso el archivo. DSN reside en una red y se puede usar simultáneamente por varios usuarios en la red siempre que el usuario tenga instalado el controlador adecuado. Un origen de datos de archivo también puede ser no compartible, en cuyo caso solo se puede usar en un único equipo.  
  
 Para obtener más información acerca de los orígenes de datos de archivo, consulte [conectarse con orígenes de datos de archivo](../../../odbc/reference/develop-app/connecting-using-file-data-sources.md)o consulte [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
## <a name="managing-drivers"></a>Administrar controladores  
 Si el usuario hace clic en la ficha **Controladores** del cuadro de diálogo **Administrador de orígenes de datos ODBC** , **SQLManageDataSources** muestra una lista de controladores ODBC instalados en el sistema, así como información acerca de los controladores. La fecha que se muestra es la fecha de creación del controlador, tal como se muestra en la siguiente ilustración.  
  
 ![Pestaña Controladores de Administrador de orígenes de datos ODBC](../../../odbc/reference/syntax/media/ch23g.gif "ch23g")  
  
## <a name="tracing-options"></a>Opciones de traza  
 Si el usuario hace clic en la ficha **seguimiento** del cuadro de diálogo **Administrador de orígenes de datos ODBC** , **SQLManageDataSources** muestra las opciones de seguimiento, tal como se muestra en la siguiente ilustración.  
  
 ![Pestaña Trazas de Administrador de orígenes de datos ODBC](../../../odbc/reference/syntax/media/ch23h.gif "Ch23h")  
  
 Si el usuario hace clic en **iniciar el seguimiento ahora** y, a continuación, hace clic en **Aceptar**, **SQLManageDataSources** habilita el seguimiento manualmente para todas las aplicaciones que se ejecutan actualmente en el equipo.  
  
 Si el usuario especifica el nombre de un archivo de seguimiento en el cuadro de texto **ruta de acceso del archivo de registro** y, a continuación, hace clic en **Aceptar**, **SQLManageDataSources** establece **la palabra clave** de seguimiento en la sección [ODBC] de la información del sistema en el nombre especificado.  
  
> [!IMPORTANT]  
>  Se quitó la compatibilidad con Visual Studio Analyzer a partir de Windows 8 (Visual Studio Analyzer solo se incluía en versiones anteriores de Visual Studio). Para un mecanismo de solución de problemas alternativo, use el seguimiento de PUJAs.  
  
 Si el usuario hace clic en **iniciar Visual Studio Analyzer** y, a continuación, hace clic en **Aceptar**, Visual Studio Analyzer está habilitado. Permanece habilitada hasta que se hace clic en **detener Visual Studio Analyzer** .  
  
 Para obtener más información sobre el seguimiento, vea [seguimiento](../../../odbc/reference/develop-app/tracing.md). Para obtener más información sobre **las palabras clave Trace y de** **seguimiento** , vea la [subclave ODBC](../../../odbc/reference/install/odbc-subkey.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Creación de orígenes de datos|[SQLCreateDataSource](../../../odbc/reference/syntax/sqlcreatedatasource-function.md)|
