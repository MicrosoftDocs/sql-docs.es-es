---
description: SQLConfigDataSource Function
title: Función SQLConfigDataSource | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLConfigDataSource
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLConfigDataSource
helpviewer_keywords:
- SQLConfigDataSource function [ODBC]
ms.assetid: f8d6e342-c010-434e-b1cd-f5371fb50a14
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ddcfac8f1f211884aa3f46db3f56a2762778ed92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165342"
---
# <a name="sqlconfigdatasource-function"></a>SQLConfigDataSource Function
**Conformidad**  
 Versión introducida: ODBC 1,0  
  
 **Resumen**  
 **SQLConfigDataSource** agrega, modifica o elimina orígenes de datos.  
  
 También se puede tener acceso a la funcionalidad de **SQLConfigDataSource** con [ODBCCONF.EXE](../../../odbc/odbcconf-exe.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLConfigDataSource(  
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
  
 ODBC_ADD_DSN: Agregue un nuevo origen de datos de usuario.  
  
 ODBC_CONFIG_DSN: configurar (modificar) un origen de datos de usuario existente.  
  
 ODBC_REMOVE_DSN: Quite un origen de datos de usuario existente.  
  
 ODBC_ADD_SYS_DSN: Agregue un nuevo origen de datos del sistema.  
  
 ODBC_CONFIG_SYS_DSN: modificar un origen de datos del sistema existente.  
  
 ODBC_REMOVE_SYS_DSN: Quite un origen de datos del sistema existente.  
  
 ODBC_REMOVE_DEFAULT_DSN: Quite la sección especificación de origen de datos predeterminada de la información del sistema. (También quita la sección especificación predeterminada del controlador de la entrada Odbcinst.ini de la información del sistema. Este *fRequest* realiza la misma función que la función **SQLRemoveDefaultDataSource** desusada). Cuando se especifica esta opción, todos los demás parámetros de la llamada a **SQLConfigDataSource** deben ser valores NULL. Si no son NULL, se omitirán.  
  
 *lpszDriver*  
 Entradas Descripción del controlador (normalmente, el nombre del DBMS asociado) que se presenta a los usuarios en lugar del nombre del controlador físico.  
  
 *lpszAttributes*  
 Entradas Una lista de atributos de doble terminación nulada en forma de pares palabra clave-valor. Para obtener más información, vea [ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md).  
  
## <a name="returns"></a>Devoluciones  
 La función devuelve TRUE si es correcto, FALSE si se produce un error. Si no existe ninguna entrada en la información del sistema cuando se llama a esta función, la función devuelve FALSE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLConfigDataSource** devuelve false, se puede obtener un valor de *\* PfErrorCode* asociado llamando a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Error general del instalador|Se produjo un error en el que no había ningún error específico del instalador.|  
|ODBC_ERROR_INVALID_HWND|Identificador de ventana no válido|El argumento *hwndParent* no era válido o era null.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Tipo de solicitud no válido|El argumento *fRequest* no era uno de los siguientes:<br /><br /> ODBC_ADD_DSN ODBC_CONFIG_DSN ODBC_REMOVE_DSN ODBC_ADD_SYS_DSN ODBC_CONFIG_SYS_DSN ODBC_REMOVE_SYS_DSN ODBC_REMOVE_DEFAULT_DSN|  
|ODBC_ERROR_INVALID_NAME|Nombre de traductor o controlador no válido|El argumento *lpszDriver* no era válido. No se encontró en el registro.|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Pares palabra clave-valor no válidos|El argumento *lpszAttributes* contenía un error de sintaxis.|  
|ODBC_ERROR_REQUEST_FAILED|Error en la *solicitud*|El instalador no pudo realizar la operación solicitada por el argumento *fRequest* . Error en la llamada a **ConfigDSN** .|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|No se pudo cargar la biblioteca de instalación del controlador o el traductor|No se pudo cargar la biblioteca de instalación del controlador.|  
|ODBC_ERROR_OUT_OF_MEM|No hay memoria suficiente|El instalador no pudo realizar la función debido a una falta de memoria.|  
  
## <a name="comments"></a>Comentarios  
 **SQLConfigDataSource** usa el valor de *lpszDriver* para leer la ruta de acceso completa del archivo dll de instalación del controlador a partir de la información del sistema. Carga el archivo DLL y llama a **ConfigDSN** con los mismos argumentos que se pasaron a él.  
  
 **SQLConfigDataSource** devuelve false si no puede encontrar o cargar el archivo dll de instalación o si el usuario cancela el cuadro de diálogo. De lo contrario, devuelve el estado recibido de **ConfigDSN**.  
  
 **SQLConfigDataSource** asigna el DSN del sistema *FREQUEST* s al DSN de usuario *fRequest* s (ODBC_ADD_SYS_DSN a ODBC_ADD_DSN, ODBC_CONFIG_SYS_DSN a ODBC_CONFIG_DSN y ODBC_REMOVE_SYS_DSN a ODBC_REMOVE_DSN). Para distinguir los DSN de usuario y del sistema, **SQLConfigDataSource** establece el modo de configuración del instalador de acuerdo con la tabla siguiente. Antes de devolver, **SQLConfigDataSource** restablece el modo de configuración a BOTHDSN. **ConfigDSN** (implementado por controladores) debe llamar a **SQLWriteDSNToIni** y **SQLWritePrivateProfileString** para admitir un DSN de sistema. Para obtener más información, consulte [función ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md).  
  
|*fRequest*|Modo de configuración|  
|----------------|------------------------|  
|ODBC_ADD_DSN|USERDSN_ONLY|  
|ODBC_CONFIG_DSN|USERDSN_ONLY|  
|ODBC_REMOVE_DSN|USERDSN_ONLY|  
|ODBC_ADD_SYS_DSN|SYSTEMDSN_ONLY|  
|ODBC_CONFIG_SYS_DSN|SYSTEMDSN_ONLY|  
|ODBC_REMOVE_SYS_DSN|SYSTEMDSN_ONLY|  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Agregar, modificar o quitar un origen de datos|[ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md) (en el archivo dll de instalación)|  
|Quitar un nombre de origen de datos de la información del sistema|[SQLRemoveDSNFromIni](../../../odbc/reference/syntax/sqlremovedsnfromini-function.md)|  
|Agregar un nombre de origen de datos a la información del sistema|[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|
