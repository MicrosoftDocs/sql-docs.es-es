---
description: Función SQLInstallDriverManager
title: Función SQLInstallDriverManager | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLInstallDriverManager
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallDriverManager
helpviewer_keywords:
- SQLInstallDriverManager function [ODBC]
ms.assetid: aebc439b-fffd-4d98-907a-0163f79aee8d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b85af0323de46972ec6d6c4bf88359209b0015d5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189702"
---
# <a name="sqlinstalldrivermanager-function"></a>Función SQLInstallDriverManager
**Conformidad**  
 Versión introducida: ODBC 1,0: desusado en Windows XP Service Pack 2, Windows Server 2003 Service Pack 1 y sistemas operativos posteriores  
  
 **Resumen**  
 **SQLInstallDriverManager** devuelve la ruta de acceso del directorio de destino para la instalación de los componentes principales de ODBC. En realidad, el programa de llamada debe copiar los archivos del administrador de controladores en el directorio de destino.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLInstallDriverManager(  
     LPSTR    lpszPath,  
     WORD     cbPathMax,  
     WORD *   pcbPathOut);  
```  
  
## <a name="arguments"></a>Argumentos  
 *lpszPath*  
 Genere Ruta de acceso del directorio de destino de la instalación.  
  
 *cbPathMax*  
 Entradas Longitud de *lpszPath*. Debe ser al menos _MAX_PATH bytes.  
  
 *pcbPathOut*  
 Genere Número total de bytes (sin incluir el byte de terminación nula) devueltos en *lpszPath*. Si el número de bytes disponibles para devolver es mayor o igual que *cbPathMax*, la ruta de acceso de *lpszPath* se trunca a *cbPathMax* menos el carácter de terminación de NULL. El argumento *pcbPathOut* puede ser un puntero nulo.  
  
## <a name="returns"></a>Devoluciones  
 La función devuelve TRUE si es correcto, FALSE si se produce un error.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLInstallDriverManager** devuelve false, se puede obtener un valor de *\* pfErrorCode* asociado mediante una llamada a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Error general del instalador|Se produjo un error en el que no había ningún error específico del instalador.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longitud de búfer no válida|El argumento *lpszPath* no era lo suficientemente grande como para contener la ruta de acceso de salida. El búfer contiene la ruta de acceso truncada.<br /><br /> El argumento *cbPathMax* era menor que _MAX_PATH.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|No se pudo aumentar o reducir el recuento de uso de componentes|El instalador no pudo incrementar el recuento de uso de componentes principales de ODBC.|  
|ODBC_ERROR_OUT_OF_MEM|No hay memoria suficiente|El instalador no pudo realizar la función debido a una falta de memoria.|  
  
## <a name="comments"></a>Comentarios  
 Se llama a **SQLInstallDriverManager** para devolver la ruta de acceso de los componentes principales de ODBC e incrementar el recuento de uso de componentes en la información del sistema. Si ya existe una versión del administrador de controladores pero no existe el recuento de uso de componentes para el controlador, el nuevo valor de recuento de uso de componentes se establece en 2.  
  
 El programa de instalación de la aplicación es responsable de copiar físicamente los archivos de componentes principales y mantener los recuentos de uso de archivos. Si no se ha instalado previamente un archivo de componente principal, el programa de instalación de la aplicación debe copiar el archivo y crear el recuento de uso de archivos. Si el archivo se ha instalado previamente, el programa de instalación simplemente incrementa el recuento de uso de archivos.  
  
 Si el programa de instalación de la aplicación instaló previamente una versión anterior del administrador de controladores, los componentes principales deben desinstalarse y volver a instalarse para que el recuento de uso de componentes principales sea válido. Primero se debe llamar a **SQLRemoveDriverManager** para reducir el recuento de uso de componentes. A continuación, se debe llamar a **SQLInstallDriverManager** para incrementar el recuento de uso de componentes. El programa de instalación de la aplicación debe reemplazar los archivos de componentes principales antiguos por los nuevos archivos. Los recuentos de uso de archivos seguirán siendo los mismos y otras aplicaciones que usen los archivos de componentes principales de la versión anterior utilizarán los archivos de la versión más reciente.  
  
 En una instalación nueva de los componentes, controladores y traductores principales de ODBC, el programa de instalación de la aplicación debe llamar a las siguientes funciones en secuencia: **SQLInstallDriverManager**, **SQLInstallDriverEx**, **SQLConfigDriver** (con un *fRequest* de ODBC_INSTALL_DRIVER) y, a continuación, **SQLInstallTranslatorEx**. En una desinstalación de los componentes principales, controladores y traductores, el programa de instalación de la aplicación debe llamar a las siguientes funciones en secuencia: **SQLRemoveTranslator**, **SQLRemoveDriver** y **SQLRemoveDriverManager**. Estas funciones deben llamarse en esta secuencia. En una actualización de todos los componentes, se debe llamar a todas las funciones de desinstalación en secuencia y, a continuación, se debe llamar a todas las funciones de instalación en orden.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Agregar, modificar o quitar un controlador|[SQLConfigDriver](../../../odbc/reference/syntax/sqlconfigdriver-function.md)|  
|Instalación de un controlador|[SQLInstallDriverEx](../../../odbc/reference/syntax/sqlinstalldriverex-function.md)|  
|Instalación de un traductor|[SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md)|  
|Quitar un controlador|[SQLRemoveDriver](../../../odbc/reference/syntax/sqlremovedriver-function.md)|  
|Quitar el administrador de controladores|[SQLRemoveDriverManager](../../../odbc/reference/syntax/sqlremovedrivermanager-function.md)|  
|Quitar un traductor|[SQLRemoveTranslator](../../../odbc/reference/syntax/sqlremovetranslator-function.md)|
