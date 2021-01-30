---
description: Función SQLRemoveTranslator
title: Función SQLRemoveTranslator | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLRemoveTranslator
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLRemoveTranslator
helpviewer_keywords:
- SQLRemoveTranslator function [ODBC]
ms.assetid: c6feda49-0359-4224-8de9-77125cf2397b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cc8d1c7340d548276f462512bf2b52a3deb37b79
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192481"
---
# <a name="sqlremovetranslator-function"></a>Función SQLRemoveTranslator
**Conformidad**  
 Versión introducida: ODBC 3,0  
  
 **Resumen**  
 **SQLRemoveTranslator** quita información sobre un traductor de la sección Odbcinst.ini de la información del sistema y disminuye el recuento de uso de componentes del Traductor en 1.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLRemoveTranslator(  
     LPCSTR    lpszTranslator,  
     LPDWORD   lpdwUsageCount);  
```  
  
## <a name="arguments"></a>Argumentos  
 *lpszTranslator*  
 Entradas Nombre del traductor tal como está registrado en la clave Odbcinst.ini de la información del sistema.  
  
 *lpdwUsageCount*  
 Genere Recuento de uso del traductor después de llamar a esta función.  
  
## <a name="returns"></a>Devoluciones  
 La función devuelve TRUE si es correcto, FALSE si se produce un error. Si no existe ninguna entrada en la información del sistema cuando se llama a esta función, la función devuelve FALSE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLRemoveTranslator** devuelve false, se puede obtener un valor de *\* pfErrorCode* asociado mediante una llamada a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Error general del instalador|Se produjo un error en el que no había ningún error específico del instalador.|  
|ODBC_ERROR_COMPONENT_NOT_FOUND|No se encontró el componente en el registro|El instalador no pudo quitar la información de traductor porque no existía en el registro o no se encontró en el registro.|  
|ODBC_ERROR_INVALID_NAME|Nombre de traductor o controlador no válido|El argumento *lpszTranslator* no era válido.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|No se pudo aumentar o reducir el recuento de uso de componentes|El instalador no pudo reducir el recuento de uso del controlador.|  
|ODBC_ERROR_OUT_OF_MEM|No hay memoria suficiente|El instalador no pudo realizar la función debido a una falta de memoria.|  
  
## <a name="comments"></a>Comentarios  
 **SQLRemoveTranslator** complementa la función [SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md) y actualiza el recuento de uso de componentes en la información del sistema. Solo se debe llamar a esta función desde una aplicación de instalación.  
  
 **SQLRemoveTranslator** disminuirá el recuento de uso de componentes en 1. Si el recuento de uso de componentes va a 0, se quitará la entrada de Traductor en la información del sistema. La entrada Translator se encuentra en la siguiente ubicación de la información del sistema, en el nombre del traductor:  
  
 `HKEY_LOCAL_MACHINE`  
  
 `SOFTWARE`  
  
 `ODBC`  
  
 `Odbcinst.ini`  
  
 **SQLRemoveTranslator** realmente no quita ningún archivo. El programa de llamada es responsable de la eliminación de archivos y del mantenimiento del recuento de uso de archivos. Solo después de que el recuento de uso de componentes y el recuento de uso de archivos hayan llegado a cero, se elimina un archivo físicamente. Algunos archivos de un componente se pueden eliminar y otros no se eliminan, dependiendo de si los archivos se usan en otras aplicaciones que han incrementado el recuento de uso de archivos.  
  
 También se llama a **SQLRemoveTranslator** como parte de un proceso de actualización. Si una aplicación detecta que tiene que realizar una actualización y ha instalado previamente el controlador, el controlador se debe quitar y volver a instalar. Primero se debe llamar a **SQLRemoveTranslator** para reducir el recuento de uso de componentes y, después, se debe llamar a **SQLInstallTranslatorEx** para incrementar el recuento de uso de componentes. El programa de instalación de la aplicación debe reemplazar físicamente los archivos antiguos por los nuevos archivos. El recuento de uso de archivos seguirá siendo el mismo y otras aplicaciones que usen los archivos de la versión anterior utilizarán ahora la versión más reciente.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Instalación de un traductor|[SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md)|
