---
description: Función SQLConfigDriver
title: Función SQLConfigDriver | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLConfigDriver
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLConfigDriver
helpviewer_keywords:
- SQLConfigDriver function [ODBC]
ms.assetid: 4f681961-ac9f-4d88-b065-5258ba112642
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: abe28e8d870216cf090e57a93e0c75766db08231
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165330"
---
# <a name="sqlconfigdriver-function"></a>Función SQLConfigDriver
**Conformidad**  
 Versión introducida: ODBC 2,5  
  
 **Resumen**  
 **SQLConfigDriver** carga el archivo dll de instalación del controlador adecuado y llama a la función **ConfigDriver** .  
  
 También se puede tener acceso a la funcionalidad de **SQLConfigDriver** con [ODBCCONF.EXE](../../../odbc/odbcconf-exe.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLConfigDriver(  
     HWND     hwndParent,  
     WORD     fRequest,  
     LPCSTR   lpszDriver,  
     LPCSTR   lpszArgs,  
     LPSTR    lpszMsg,  
     WORD     cbMsgMax,  
     WORD *   pcbMsgOut);  
```  
  
## <a name="arguments"></a>Argumentos  
 *hwndParent*  
 Entradas Identificador de la ventana primaria. Si el identificador es null, la función no mostrará ningún cuadro de diálogo.  
  
 *fRequest*  
 Entradas Tipo de solicitud. *fRequest* debe contener uno de los siguientes valores:  
  
 ODBC_CONFIG_DRIVER: cambia el tiempo de espera de agrupación de conexiones utilizado por el controlador.  
  
 ODBC_INSTALL_DRIVER: instala un nuevo controlador.  
  
 ODBC_REMOVE_DRIVER: quita un controlador existente.  
  
 Esta opción también puede ser específica del controlador, en cuyo caso el *fRequest* de la primera opción debe comenzar desde ODBC_CONFIG_DRIVER_MAX + 1. El *fRequest* de cualquier opción adicional también debe empezar con un valor mayor que ODBC_CONFIG_DRIVER_MAX + 1.  
  
 *lpszDriver*  
 Entradas El nombre del controlador tal como está registrado en la información del sistema.  
  
 *lpszArgs*  
 Entradas Una cadena terminada en null que contiene argumentos para un *fRequest* específico del controlador.  
  
 *lpszMsg*  
 Genere Una cadena terminada en null que contiene un mensaje de salida de la configuración del controlador.  
  
 *cbMsgMax*  
 Entradas Longitud de *lpszMsg.*  
  
 *pcbMsgOut*  
 Genere Número total de bytes disponibles que se van a devolver en *lpszMsg*. Si el número de bytes disponibles para devolver es mayor o igual que *cbMsgMax*, el mensaje de salida de *lpszMsg* se trunca en *cbMsgMax* menos el carácter de terminación null. El argumento *pcbMsgOut* puede ser un puntero nulo.  
  
## <a name="returns"></a>Devoluciones  
 La función devuelve TRUE si es correcto, FALSE si se produce un error.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLConfigDriver** devuelve false, se puede obtener un valor de *\* pfErrorCode* asociado mediante una llamada a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Error general del instalador|Se produjo un error en el que no había ningún error específico del instalador.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longitud de búfer no válida|El argumento *lpszMsg* no era válido.|  
|ODBC_ERROR_INVALID_HWND|Identificador de ventana no válido|El argumento *hwndParent* no era válido.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Tipo de solicitud no válido|El argumento *fRequest* no era uno de los siguientes:<br /><br /> ODBC_INSTALL_DRIVER ODBC_REMOVE_DRIVER<br /><br /> El argumento *fRequest* era una opción específica del controlador que era menor o igual que ODBC_CONFIG_DRIVER_MAX.|  
|ODBC_ERROR_INVALID_NAME|Nombre de traductor o controlador no válido|El argumento *lpszDriver* no era válido. No se encontró en el registro.|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Pares palabra clave-valor no válidos|El argumento *lpszArgs* contenía un error de sintaxis.|  
|ODBC_ERROR_REQUEST_FAILED|Error en la *solicitud*|El instalador no pudo realizar la operación solicitada por el argumento *fRequest* . Error en la llamada a **ConfigDriver** .|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|No se pudo cargar la biblioteca de instalación del controlador o el traductor|No se pudo cargar la biblioteca de instalación del controlador.|  
|ODBC_ERROR_OUT_OF_MEM|No hay memoria suficiente|El instalador no pudo realizar la función debido a una falta de memoria.|  
  
## <a name="comments"></a>Comentarios  
 **SQLConfigDriver** permite a una aplicación llamar a la rutina **ConfigDriver** de un controlador sin tener que conocer el nombre y cargar el archivo dll de instalación específico del controlador. Un programa de instalación llama a esta función una vez instalado el archivo DLL de instalación del controlador. El programa que realiza la llamada debe tener en cuenta que esta función podría no estar disponible para todos los controladores. En tal caso, el programa de llamada debe continuar sin errores.  
  
## <a name="driver-specific-options"></a>Opciones de Driver-Specific  
 Una aplicación puede solicitar características específicas del controlador expuestas por el controlador mediante el argumento *fRequest* . El valor de *fRequest* de la primera opción será ODBC_CONFIG_DRIVER_MAX + 1, y las opciones adicionales se incrementarán en 1 a partir de ese valor. Los argumentos requeridos por el controlador para esa función deben proporcionarse en una cadena terminada en NULL pasada en el argumento *lpszArgs* . Los controladores que proporcionan esta funcionalidad deben mantener una tabla de opciones específicas del controlador. Las opciones deben estar completamente documentadas en la documentación del controlador. Los escritores de aplicaciones que usan opciones específicas del controlador deben tener en cuenta que este uso hará que la aplicación sea menos interoperable.  
  
## <a name="setting-connection-pooling-timeout"></a>Establecer tiempo de espera de agrupación de conexiones  
 Las propiedades de tiempo de espera de agrupación de conexiones se pueden establecer cuando se establece la configuración del controlador. Se llama a **SQLConfigDriver** con un *fRequest* de ODBC_CONFIG_DRIVER y *lpszArgs* establecido en **CPTimeout**. **CPTimeout** determina el período de tiempo que una conexión puede permanecer en el grupo de conexiones sin usarse. Cuando expira el tiempo de espera, la conexión se cierra y se quita del grupo. El tiempo de espera predeterminado es de 60 segundos.  
  
 Cuando se llama a **SQLConfigDriver** con *fRequest* establecido en ODBC_INSTALL_DRIVER o ODBC_REMOVE_DRIVER, el administrador de controladores carga el archivo dll de instalación del controlador adecuado y llama a la función **ConfigDriver** . Cuando se llama a **SQLConfigDriver** con un *fRequest* de ODBC_CONFIG_DRIVER, todo el procesamiento se realiza en el instalador de ODBC, de modo que no es necesario cargar el archivo dll de instalación del controlador.  
  
## <a name="messages"></a>Messages  
 Una rutina de instalación del controlador puede enviar un mensaje de texto a una aplicación como cadenas terminadas en null en el búfer de *lpszMsg* . El mensaje se truncará en *cbMsgMax* menos el carácter de terminación null de la función **ConfigDriver** si es mayor o igual que *cbMsgMax* caracteres.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Agregar, modificar o quitar un controlador|[ConfigDriver](../../../odbc/reference/syntax/configdriver-function.md)(en el archivo dll de instalación)|  
|Quitar el origen de datos predeterminado|[SQLRemoveDefaultDataSource](../../../odbc/reference/syntax/sqlremovedefaultdatasource-function.md)|
