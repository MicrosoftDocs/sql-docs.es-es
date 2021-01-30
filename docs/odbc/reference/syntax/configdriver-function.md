---
description: Función ConfigDriver
title: Función ConfigDriver | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ConfigDriver
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- ConfigDriver
helpviewer_keywords:
- ConfigDriver [ODBC]
ms.assetid: 9473f48f-bcae-4784-89c1-7839bad4ed13
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5abe589b84eff738aeaf41139082781254fb0488
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174740"
---
# <a name="configdriver-function"></a>Función ConfigDriver
**Conformidad**  
 Versión introducida: ODBC 2,5  
  
 **Resumen**  
 **ConfigDriver** permite a un programa de instalación realizar las funciones de instalación y desinstalación sin necesidad de que el programa llame a **ConfigDSN**. Esta función realizará funciones específicas del controlador, como la creación de información del sistema específica del controlador y la realización de conversiones de DSN durante la instalación, así como la limpieza de las modificaciones de la información del sistema durante la desinstalación. Esta función se expone mediante el archivo DLL de instalación del controlador o un archivo DLL de instalación independiente.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL ConfigDriver(  
      HWND    hwndParent,  
      WORD    fRequest,  
      LPCSTR  lpszDriver,  
      LPCSTR  lpszArgs,  
      LPSTR   lpszMsg,  
      WORD    cbMsgMax,  
      WORD *  pcbMsgOut);  
```  
  
## <a name="arguments"></a>Argumentos  
 *hwndParent*  
 Entradas Identificador de la ventana primaria. Si el identificador es null, la función no mostrará ningún cuadro de diálogo.  
  
 *fRequest*  
 Entradas Tipo de solicitud. El argumento *fRequest* debe contener uno de los siguientes valores:  
  
 ODBC_INSTALL_DRIVER: instalar un nuevo controlador.  
  
 ODBC_REMOVE_DRIVER: Quite un controlador.  
  
 Esta opción también puede ser específica del controlador, en cuyo caso el argumento *fRequest* de la primera opción debe comenzar desde ODBC_CONFIG_DRIVER_MAX + 1. El argumento *fRequest* de cualquier opción adicional también debe empezar con un valor mayor que ODBC_CONFIG_DRIVER_MAX + 1.  
  
 *lpszDriver*  
 Entradas El nombre del controlador tal como está registrado en la clave Odbcinst.ini de la información del sistema.  
  
 *lpszArgs*  
 Entradas Una cadena terminada en null que contiene argumentos para un *fRequest* específico del controlador.  
  
 *lpszMsg*  
 Genere Una cadena terminada en null que contiene un mensaje de salida de la configuración del controlador.  
  
 *cbMsgMax*  
 Entradas Longitud de *lpszMsg*.  
  
 *pcbMsgOut*  
 Genere Número total de bytes disponibles que se van a devolver en *lpszMsg*.  
  
 Si el número de bytes disponibles para devolver es mayor o igual que *cbMsgMax*, el mensaje de salida de *lpszMsg* se trunca en *cbMsgMax* menos el carácter de terminación null. El argumento *pcbMsgOut* puede ser un puntero nulo.  
  
## <a name="returns"></a>Devoluciones  
 La función devuelve TRUE si es correcto, FALSE si se produce un error.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **ConfigDriver** devuelve false, se envía un valor de *\* pfErrorCode* asociado al búfer de error del instalador mediante una llamada a **SQLPostInstallerError** y se puede obtener llamando a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_INVALID_HWND|Identificador de ventana no válido|El argumento *hwndParent* no era válido.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Tipo de solicitud no válido|El argumento *fRequest* no era uno de los siguientes:<br /><br /> ODBC_INSTALL_DRIVER ODBC_REMOVE_DRIVER<br /><br /> La opción específica del controlador era menor o igual que ODBC_CONFIG_DRIVER_MAX.|  
|ODBC_ERROR_INVALID_NAME|Nombre de traductor o controlador no válido|El argumento *lpszDriver* no era válido. No se encontró en el registro.|  
|ODBC_ERROR_REQUEST_FAILED|Error en la *solicitud*|No se pudo realizar la operación solicitada por el argumento *fRequest* .|  
|ODBC_ERROR_DRIVER_SPECIFIC|Error específico del controlador o del traductor|Error específico del controlador para el que no hay ningún error del instalador de ODBC definido. El argumento *SzError* en una llamada a la función **SQLPostInstallerError** debe contener el mensaje de error específico del controlador.|  
  
## <a name="comments"></a>Comentarios  
  
### <a name="driver-specific-options"></a>Opciones de Driver-Specific  
 Una aplicación puede solicitar características específicas del controlador expuestas por el controlador mediante el argumento *fRequest* . El valor de *fRequest* para la primera opción será ODBC_CONFIG_DRIVER_MAX más 1, y las opciones adicionales se incrementarán en 1 a partir de ese valor. Los argumentos requeridos por el controlador para esa función deben proporcionarse en una cadena terminada en NULL pasada en el argumento *lpszArgs* . Los controladores que proporcionan esta funcionalidad deben mantener una tabla de opciones específicas del controlador. Las opciones deben estar completamente documentadas en la documentación del controlador. Los escritores de aplicaciones que usan opciones específicas del controlador deben tener en cuenta que esto hará que la aplicación sea menos interoperable.  
  
### <a name="messages"></a>Messages  
 Una rutina de instalación del controlador puede enviar un mensaje de texto a una aplicación como una cadena terminada en null en el búfer de *lpszMsg* . El mensaje se truncará en *cbMsgMax* menos el carácter de terminación null de la función **ConfigDriver** si es mayor o igual que *cbMsgMax* caracteres.
