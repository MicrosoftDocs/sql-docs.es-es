---
description: Función SQLInstallTranslatorEx
title: Función SQLInstallTranslatorEx | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLInstallTranslatorEx
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallTranslatorEx
helpviewer_keywords:
- SQLInstallTranslatorEx function [ODBC]
ms.assetid: a0630602-53c1-4db0-98ce-70d160aedf8d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e055f3f3565c6240134bf43b4b4f8951059e2725
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189643"
---
# <a name="sqlinstalltranslatorex-function"></a>Función SQLInstallTranslatorEx
**Conformidad**  
 Versión introducida: ODBC 3,0  
  
 **Resumen**  
 **SQLInstallTranslatorEx** agrega información acerca de un traductor a la sección Odbcinst.ini de la clave del registro de información del sistema (HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBCINST.INI \odbc Translator).  
  
 También se puede tener acceso a la funcionalidad de **SQLInstallTranslatorEx** con [ODBCCONF.EXE](../../../odbc/odbcconf-exe.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLInstallTranslatorEx(  
     LPCSTR    lpszTranslator,  
     LPCSTR    lpszPathIn,  
     LPSTR     lpszPathOut,  
     WORD      cbPathOutMax,  
     WORD *    pcbPathOut,  
     WORD      fRequest,  
     LPDWORD   lpdwUsageCount);  
```  
  
## <a name="arguments"></a>Argumentos  
 *lpszTranslator*  
 Entradas Debe contener una lista de pares de palabra clave-valor con doble terminación que describa al traductor. Para obtener más información sobre la sintaxis de pares de palabra clave y valor, consulte [subclaves de especificación de traductor](../../../odbc/reference/install/translator-specification-subkeys.md).  
  
 Las palabras clave **Translator** y **setup** deben incluirse en la cadena *lpszTranslator* . La DLL de traducción se muestra con la palabra clave **Translator** y la dll de instalación del traductor aparece con la palabra clave **setup** . Cada par se termina con un byte nulo y la lista completa finaliza con un byte nulo. (Es decir, dos bytes NULOs marcan el final de la lista). El formato de *lpszTranslator* es el siguiente:  
  
 \0Translator =*Translator-dll-FILENAME*\ 0 [Setup =*setup-dll-FILENAME*\ 0] \ 0  
  
 *lpszPathIn*  
 Entradas Ruta de acceso completa donde se va a instalar el traductor o un puntero nulo. Si *lpszPath* es un puntero nulo, los traductores se instalarán en el directorio del sistema.  
  
 *lpszPathOut*  
 Genere La ruta de acceso del directorio de destino donde se debe instalar el traductor. Si nunca se ha instalado el traductor, *lpszPathOut* es el mismo que *lpszPathIn*. Si existe una instalación anterior del traductor, *lpszPathOut* es la ruta de acceso de la instalación anterior.  
  
 *cbPathOutMax*  
 Entradas Longitud de *lpszPathOut.*  
  
 *pcbPathOut*  
 Genere Número total de bytes disponibles que se van a devolver en *lpszPathOut*. Si el número de bytes disponibles para devolver es mayor o igual que *cbPathOutMax*, la ruta de acceso de salida de *lpszPathOut* se trunca a *pcbPathOutMax* menos el carácter de terminación null. El argumento *pcbPathOut* puede ser un puntero nulo.  
  
 *fRequest*  
 Entradas Tipo de solicitud. *fRequest* debe contener uno de los siguientes valores:  
  
 ODBC_INSTALL_INQUIRY: Consulte dónde se puede instalar un traductor.  
  
 ODBC_INSTALL_COMPLETE: complete la solicitud de instalación.  
  
 *lpdwUsageCount*  
 Genere Recuento de uso del traductor después de llamar a esta función.  
  
 Las aplicaciones no deben establecer el recuento de uso. ODBC mantendrá este recuento.  
  
## <a name="returns"></a>Devoluciones  
 La función devuelve TRUE si es correcto, FALSE si se produce un error.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLInstallTranslatorEx** devuelve false, se puede obtener un valor de *\* pfErrorCode* asociado mediante una llamada a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Error general del instalador|Se produjo un error en el que no había ningún error específico del instalador.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longitud de búfer no válida|El argumento *lpszPathOut* no era lo suficientemente grande como para contener la ruta de acceso de salida. El búfer contiene la ruta de acceso truncada.<br /><br /> El argumento *cbPathOutMax* era 0 y el argumento *fRequest* se ODBC_INSTALL_COMPLETE.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Tipo de solicitud no válido|El argumento *fRequest* no era uno de los siguientes:<br /><br /> ODBC_INSTALL_INQUIRY ODBC_INSTALL_COMPLETE|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Pares palabra clave-valor no válidos|El argumento *lpszTranslator* contenía un error de sintaxis.|  
|ODBC_ERROR_INVALID_PATH|Ruta de instalación no válida|El argumento *lpszPathIn* contenía una ruta de acceso no válida.|  
|ODBC_ERROR_INVALID_PARAM_SEQUENCE|Secuencia de parámetros no válida|El argumento *lpszTranslator* no contenía una lista de pares palabra clave-valor.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|No se pudo aumentar o reducir el recuento de uso de componentes del registro|El instalador no pudo incrementar el recuento de uso del traductor.|  
  
## <a name="comments"></a>Comentarios  
 **SQLInstallTranslatorEx** proporciona un mecanismo para instalar solo el traductor. Esta función no copia realmente ningún archivo. El programa de llamada es responsable de copiar los archivos de traductor.  
  
 **SQLInstallTranslatorEx** incrementa el recuento de uso de componentes para el traductor instalado en 1. Si ya existe una versión del traductor pero no existe el recuento de uso de componentes para el traductor, el nuevo valor de recuento de uso de componentes se establece en 2.  
  
 El programa de instalación de la aplicación es responsable de copiar físicamente el archivo de traductor y mantener el recuento de uso de archivos. Si el archivo de traductor no se ha instalado previamente, el programa de instalación de la aplicación debe copiar el archivo o los archivos y crear el recuento de uso de archivos o archivos. Si el archivo se ha instalado previamente, el programa de instalación simplemente incrementa el recuento de uso de archivos.  
  
 Si la aplicación instaló previamente una versión anterior del traductor, el traductor debe desinstalarse y volver a instalarse, para que el recuento de uso del componente de traductor sea válido. Se debe llamar a **SQLRemoveTranslator** para reducir el recuento de uso de componentes y, a continuación, se debe llamar a **SQLInstallTranslatorEx** para incrementar el recuento de uso de componentes. El programa de instalación de la aplicación debe reemplazar el archivo o los archivos antiguos por el nuevo archivo. El recuento de uso de archivos seguirá siendo el mismo y otras aplicaciones que usen el archivo de versión anterior utilizarán ahora la versión más reciente.  
  
 La longitud de la ruta de acceso en *lpszPathOut* en **SQLInstallTranslatorEx** permite un proceso de instalación en dos fases, por lo que una aplicación puede determinar qué *CbPathOutMax* debe ser llamando a **SQLInstallTranslatorEx** con un *fRequest* del modo ODBC_INSTALL_INQUIRY. Esto devolverá el número total de bytes disponibles en el búfer de *pcbPathOut* . A continuación, se puede llamar a **SQLInstallTranslatorEx** con un *fRequest* de ODBC_INSTALL_COMPLETE y el argumento *cbPathOutMax* establecido en el valor del búfer *pcbPathOut* , más el carácter de terminación null.  
  
 Si decide no usar el modelo de dos fases para **SQLInstallTranslatorEx**, debe establecer *cbPathOutMax*, que define el tamaño del almacenamiento de la ruta de acceso del directorio de destino, en el valor _MAX_PATH, como se define en stdlib. h, para evitar el truncamiento.  
  
 Cuando *fRequest* se ODBC_INSTALL_COMPLETE, **SQLInstallTranslatorEx** no permite que *lpszPathOut* sea NULL (o que *cbPathOutMax* sea 0). Si *fRequest* es ODBC_INSTALL_COMPLETE, se devuelve false cuando el número de bytes disponibles para devolver es mayor o igual que *cbPathOutMax*, con el resultado de que se produce el truncamiento.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Devolver una opción de traducción predeterminada|[ConfigTranslator](../../../odbc/reference/syntax/configtranslator-function.md)|  
|Seleccionar traductores|[SQLGetTranslator](../../../odbc/reference/syntax/sqlgettranslator-function.md)|  
|Quitar traductores|[SQLRemoveTranslator](../../../odbc/reference/syntax/sqlremovetranslator-function.md)|
