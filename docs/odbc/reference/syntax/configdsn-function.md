---
description: Función ConfigDSN
title: Función ConfigDSN | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ConfigDSN
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- ConfigDSN
helpviewer_keywords:
- ConfigDSN [ODBC]
ms.assetid: 01ced74e-c575-4a25-83f5-bd7d918123f8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 42e85b0c77ccc71946836e3103953fc85206b6e2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174727"
---
# <a name="configdsn-function"></a>Función ConfigDSN
**Conformidad**  
 Versión introducida: ODBC 1,0  
  
 **Resumen**  
 **ConfigDSN** agrega, modifica o elimina orígenes de datos de la información del sistema. Puede pedir al usuario la información de conexión. Puede estar en el archivo DLL del controlador o en un archivo DLL de instalación independiente.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL ConfigDSN(  
     HWND     hwndParent,  
     WORD     fRequest,  
     LPCSTR   lpszDriver,  
     LPCSTR   lpszAttributes);  
```  
  
## <a name="arguments"></a>Argumentos  
 *hwndParent*  
 Entradas Identificador de la ventana primaria. Si el identificador es null, la función no mostrará ningún cuadro de diálogo.  
  
 *fRequest*  
 Entradas Tipo de solicitud. El argumento *fRequest* debe contener uno de los siguientes valores:  
  
 ODBC_ADD_DSN: agregar un nuevo origen de datos.  
  
 ODBC_CONFIG_DSN: configurar (modificar) un origen de datos existente.  
  
 ODBC_REMOVE_DSN: Quite un origen de datos existente.  
  
 *lpszDriver*  
 Entradas Descripción del controlador (normalmente, el nombre del DBMS asociado) que se presenta a los usuarios en lugar del nombre del controlador físico.  
  
 *lpszAttributes*  
 Entradas Una lista de atributos de doble terminación nulada en forma de pares palabra clave-valor. Para obtener más información, vea "Comentarios".  
  
## <a name="returns"></a>Devoluciones  
 La función devuelve TRUE si es correcto, FALSE si se produce un error.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **ConfigDSN** devuelve false, se envía un valor de *\* pfErrorCode* asociado al búfer de error del instalador mediante una llamada a **SQLPostInstallerError** y se puede obtener llamando a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_INVALID_HWND|Identificador de ventana no válido|El argumento *hwndParent* no era válido.|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Pares palabra clave-valor no válidos|El argumento *lpszAttributes* contenía un error de sintaxis.|  
|ODBC_ERROR_INVALID_NAME|Nombre de traductor o controlador no válido|El argumento *lpszDriver* no era válido. No se encontró en el registro.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Tipo de solicitud no válido|El argumento *fRequest* no era uno de los siguientes:<br /><br /> ODBC_ADD_DSN ODBC_CONFIG_DSN ODBC_REMOVE_DSN|  
|ODBC_ERROR_REQUEST_FAILED|Error en la *solicitud*|No se pudo realizar la operación solicitada por el argumento *fRequest* .|  
|ODBC_ERROR_DRIVER_SPECIFIC|Error específico del controlador o del traductor|Error específico del controlador para el que no hay ningún error del instalador de ODBC definido. El argumento *SzError* en una llamada a la función **SQLPostInstallerError** debe contener el mensaje de error específico del controlador.|  
  
## <a name="comments"></a>Comentarios  
 **ConfigDSN** recibe información de conexión del archivo DLL del instalador como una lista de atributos en forma de pares palabra clave-valor. Cada par se termina con un byte nulo y la lista completa finaliza con un byte nulo. (Es decir, dos bytes nulos marcan el final de la lista). No se permiten espacios alrededor del signo igual en el par palabra clave-valor. **ConfigDSN** puede aceptar palabras clave que no son palabras clave válidas para **SQLBrowseConnect** y **SQLDriverConnect**. **ConfigDSN** no admite necesariamente todas las palabras clave que son palabras clave válidas para **SQLBrowseConnect** y **SQLDriverConnect**. (**ConfigDSN** no acepta la palabra clave **driver** ). Las palabras clave utilizadas por la función **ConfigDSN** deben admitir todas las opciones necesarias para volver a crear el origen de datos mediante la característica de instalación automática del instalador. Cuando los usos de los valores de **ConfigDSN** y los valores de cadena de conexión son los mismos, deben usarse las mismas palabras clave.  
  
 Como en **SQLBrowseConnect** y **SQLDriverConnect**, las palabras clave y sus valores no deben contener **[] {} (),;? \* =! @** Characters, y el valor de la palabra clave **DSN** no puede constar solo de espacios en blanco. Debido a la gramática del registro, las palabras clave y los nombres de los orígenes de datos no pueden contener el carácter de barra diagonal inversa ( \\ ).  
  
 **ConfigDSN** debe llamar a **SQLValidDSN** para comprobar la longitud del nombre del origen de datos y comprobar que no se incluyen caracteres no válidos en el nombre. Si el nombre del origen de datos es mayor que SQL_MAX_DSN_LENGTH o incluye caracteres no válidos, **SQLValidDSN** devuelve un error y **ConfigDSN** devuelve un error. **SQLWriteDSNToIni** también comprueba la longitud del nombre del origen de datos.  
  
 Por ejemplo, para configurar un origen de datos que requiera un identificador de usuario, una contraseña y un nombre de base de datos, una aplicación de instalación podría pasar los siguientes pares de palabra clave-valor:  
  
```  
DSN=Personnel Data\0UID=Smith\0PWD=Sesame\0DATABASE=Personnel\0\0  
```  
  
 Para obtener más información acerca de estas palabras clave, consulte [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) y la documentación de cada controlador.  
  
 Para mostrar un cuadro de diálogo, *hwndParent* no debe ser null.  
  
## <a name="adding-a-data-source"></a>Agregar un origen de datos  
 Si se pasa un nombre de origen de datos a **ConfigDSN** en *lpszAttributes*, **ConfigDSN** comprueba que el nombre es válido. Si el nombre del origen de datos coincide con un nombre de origen de datos existente y *hwndParent* es null, **ConfigDSN** sobrescribe el nombre existente. Si coincide con un nombre existente y *hwndParent* no es null, **ConfigDSN** solicita al usuario que sobrescriba el nombre existente.  
  
 Si *lpszAttributes* contiene suficiente información para conectarse a un origen de datos, **ConfigDSN** puede Agregar el origen de datos o mostrar un cuadro de diálogo con el que el usuario puede cambiar la información de conexión. Si *lpszAttributes* no contiene suficiente información para conectarse a un origen de datos, **ConfigDSN** debe determinar la información necesaria; Si *hwndParent* no es null, se muestra un cuadro de diálogo para recuperar la información del usuario.  
  
 Si **ConfigDSN** muestra un cuadro de diálogo, debe mostrar cualquier información de conexión que se le haya pasado en *lpszAttributes*. En concreto, si se le ha pasado un nombre de origen de datos, **ConfigDSN** muestra ese nombre pero no permite que el usuario lo cambie. **ConfigDSN** puede proporcionar valores predeterminados para la información de conexión que no se le ha pasado en *lpszAttributes*.  
  
 Si **ConfigDSN** no puede obtener información de conexión completa para un origen de datos, devuelve false.  
  
 Si **ConfigDSN** puede obtener información de conexión completa para un origen de datos, llama a **SQLWriteDSNToIni** en el archivo DLL del instalador para agregar la nueva especificación de origen de datos al archivo Odbc.ini (o registro). **SQLWriteDSNToIni** agrega el nombre del origen de datos a la sección [orígenes de datos ODBC], crea la sección especificación de origen de datos y agrega la palabra clave **driver** con la descripción del controlador como su valor. **ConfigDSN** llama a **SQLWritePrivateProfileString** en el archivo DLL del instalador para agregar las palabras clave y los valores adicionales usados por el controlador.  
  
## <a name="modifying-a-data-source"></a>Modificar un origen de datos  
 Para modificar un origen de datos, se debe pasar un nombre de origen de datos a **ConfigDSN** en *lpszAttributes*. **ConfigDSN** comprueba que el nombre del origen de datos se encuentra en el archivo Odbc.ini (o registro).  
  
 Si *hwndParent* es null, **ConfigDSN** usa la información de *lpszAttributes* para modificar la información del archivo Odbc.ini (o registro). Si *hwndParent* no es null, **ConfigDSN** muestra un cuadro de diálogo con la información de *lpszAttributes*; para obtener información que no está en *lpszAttributes*, usa información de la información del sistema. El usuario puede modificar la información antes de que **ConfigDSN** la almacene en la información del sistema.  
  
 Si se cambió el nombre del origen de datos, **ConfigDSN** llama primero a **SQLRemoveDSNFromIni** en el archivo DLL del instalador para quitar la especificación del origen de datos existente del archivo Odbc.ini (o registro). Después, sigue los pasos de la sección anterior para agregar la nueva especificación de origen de datos. Si no se ha cambiado el nombre del origen de datos, **ConfigDSN** llama a **SQLWritePrivateProfileString** en el archivo DLL del instalador para realizar cualquier otro cambio. Es posible que **ConfigDSN** no elimine o cambie el valor de la palabra clave **driver** .  
  
## <a name="deleting-a-data-source"></a>Eliminar un origen de datos  
 Para eliminar un origen de datos, se debe pasar un nombre de origen de datos a **ConfigDSN** en *lpszAttributes*. **ConfigDSN** comprueba que el nombre del origen de datos se encuentra en el archivo Odbc.ini (o registro). A continuación, llama a **SQLRemoveDSNFromIni** en el archivo DLL del instalador para quitar el origen de datos.  
  
## <a name="note"></a>Nota
 Si escribe una versión Unicode de esta rutina, debe llamarse **ConfigDSNW**, con argumentos LPCWSTR en lugar de LPCSTR.
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Agregar, modificar o quitar un origen de datos|[SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md)|  
|Obtener un valor del archivo Odbc.ini o del registro|[SQLGetPrivateProfileString](../../../odbc/reference/syntax/sqlgetprivateprofilestring-function.md)|  
|Quitar el origen de datos predeterminado|[SQLRemoveDefaultDataSource](../../../odbc/reference/syntax/sqlremovedefaultdatasource-function.md)|  
|Quitar un nombre de origen de datos de Odbc.ini (o registro)|[SQLRemoveDSNFromIni](../../../odbc/reference/syntax/sqlremovedsnfromini-function.md)|  
|Agregar un nombre de origen de datos a Odbc.ini (o registro)|[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|  
|Escribir un valor en el archivo Odbc.ini o en el registro|[SQLWritePrivateProfileString](../../../odbc/reference/syntax/sqlwriteprivateprofilestring-function.md)|
