---
description: Función SQLRemoveDriver
title: Función SQLRemoveDriver | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLRemoveDriver
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLRemoveDriver
helpviewer_keywords:
- SQLRemoveDriver function [ODBC]
ms.assetid: 9a3b4f8b-982b-44b9-ade6-754ff026dc90
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2ee0165f815b874844f072c36325fb7afb8046c2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192506"
---
# <a name="sqlremovedriver-function"></a>Función SQLRemoveDriver
**Conformidad**  
 Versión introducida: ODBC 3,0  
  
 **Resumen**  
 **SQLRemoveDriver** cambia o quita información sobre el controlador de la entrada Odbcinst.ini en la información del sistema.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLRemoveDriver(  
     LPCSTR   lpszDriver,  
     BOOL     fRemoveDSN,  
     LPDWORD  lpdwUsageCount);  
```  
  
## <a name="arguments"></a>Argumentos  
 *lpszDriver*  
 Entradas El nombre del controlador tal como está registrado en la clave Odbcinst.ini de la información del sistema.  
  
 *fRemoveDSN*  
 Entradas Los valores válidos son:  
  
 TRUE: Quite los DSN asociados al controlador especificado en *lpszDriver*. FALSE: no se quitan los DSN asociados al controlador especificado en *lpszDriver*.  
  
 *lpdwUsageCount*  
 Genere Recuento de uso del controlador después de llamar a esta función.  
  
## <a name="returns"></a>Devoluciones  
 La función devuelve TRUE si es correcto, FALSE si se produce un error. Si no existe ninguna entrada en la información del sistema cuando se llama a esta función, la función devuelve FALSE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLRemoveDriver** devuelve false, se puede obtener un valor de *\* pfErrorCode* asociado mediante una llamada a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Error general del instalador|Se produjo un error en el que no había ningún error específico del instalador.|  
|ODBC_ERROR_COMPONENT_NOT_FOUND|No se encontró el componente en el registro|El instalador no pudo quitar la información del controlador porque no existía en el registro o no se encontró en el registro.|  
|ODBC_ERROR_INVALID_NAME|Nombre de traductor o controlador no válido|El argumento *lpszDriver* no era válido.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|No se pudo aumentar o reducir el recuento de uso de componentes|El instalador no pudo reducir el recuento de uso del controlador.|  
|ODBC_ERROR_REQUEST_FAILED|Error en la solicitud|El argumento *fRemoveDSN* era true; sin embargo, no se pudieron quitar uno o más DSN. Error en la llamada a **SQLConfigDriver** con la solicitud de ODBC_REMOVE_DRIVER.|  
|ODBC_ERROR_OUT_OF_MEM|No hay memoria suficiente|El instalador no pudo realizar la función debido a una falta de memoria.|  
  
## <a name="comments"></a>Comentarios  
 **SQLRemoveDriver** complementa la función [SQLInstallDriverEx](../../../odbc/reference/syntax/sqlinstalldriverex-function.md) y actualiza el recuento de uso de componentes en la información del sistema. Solo se debe llamar a esta función desde una aplicación de instalación.  
  
 **SQLRemoveDriver** disminuirá el valor de recuento de uso de componentes en 1. Si el recuento de uso de componentes va a 0, se producirá lo siguiente:  
  
1.  Se llamará a la función **SQLConfigDriver** con la opción ODBC_REMOVE_DRIVER. Si la opción *fRemoveDSN* está establecida en true, la función **ConfigDSN** llama a **SQLRemoveDSNFromIni** para quitar todos los orígenes de datos asociados con el controlador especificado en *lpszDriver.* Si la opción *fRemoveDSN* está establecida en false, los orígenes de datos no se eliminarán.  
  
2.  Se quitará la entrada del controlador en la información del sistema. La entrada del controlador se encuentra en la siguiente ubicación de información del sistema, en el nombre del controlador:  
  
     `HKEY_LOCAL_MACHINE`  
  
     `SOFTWARE`  
  
     `ODBC`  
  
     `Odbcinst.ini`  
  
 **SQLRemoveDriver** realmente no quita ningún archivo. El programa de llamada es responsable de la eliminación de archivos y del mantenimiento del recuento de uso de archivos. Solo después de que el recuento de uso de componentes y el recuento de uso de archivos hayan llegado a cero, se elimina un archivo físicamente. Algunos archivos de un componente se pueden eliminar y otros no se eliminan, dependiendo de si los archivos se usan en otras aplicaciones que han incrementado el recuento de uso de archivos.  
  
 También se llama a **SQLRemoveDriver** como parte de un proceso de actualización. Si una aplicación detecta que tiene que realizar una actualización y ha instalado previamente el controlador, el controlador se debe quitar y volver a instalar. Primero se debe llamar a **SQLRemoveDriver** para reducir el recuento de uso de componentes y, después, se debe llamar a **SQLInstallDriverEx** para incrementar el recuento de uso de componentes. El programa de instalación de la aplicación debe reemplazar los archivos antiguos por los nuevos archivos. El recuento de uso de archivos seguirá siendo el mismo y otras aplicaciones que usen los archivos de la versión anterior utilizarán ahora la versión más reciente.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Agregar, modificar o quitar un controlador|[ConfigDriver](../../../odbc/reference/syntax/configdriver-function.md) (en el archivo dll de instalación)|  
|Agregar, modificar o quitar un controlador|[SQLConfigDriver](../../../odbc/reference/syntax/sqlconfigdriver-function.md)|  
|Instalación de un controlador|[SQLInstallDriverEx](../../../odbc/reference/syntax/sqlinstalldriverex-function.md)|
