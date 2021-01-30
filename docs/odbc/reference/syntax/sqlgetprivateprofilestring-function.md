---
description: Función SQLGetPrivateProfileString
title: Función SQLGetPrivateProfileString | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetPrivateProfileString
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetPrivateProfileString
helpviewer_keywords:
- SQLGetPrivateProfileString function [ODBC]
ms.assetid: b72ca065-4d67-48df-baac-e18379a8320a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6ec5dfa03f04b004f704ad707fb7916d40e69716
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186189"
---
# <a name="sqlgetprivateprofilestring-function"></a>Función SQLGetPrivateProfileString
**Conformidad**  
 Versión introducida: ODBC 2,0  
  
 **Resumen**  
 **SQLGetPrivateProfileString** obtiene una lista de los nombres de los valores o datos correspondientes a un valor de la información del sistema.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
int SQLGetPrivateProfileString(  
     LPCSTR   lpszSection,  
     LPCSTR   lpszEntry,  
     LPCSTR   lpszDefault,  
     LPCSTR   RetBuffer,  
     INT      cbRetBuffer,  
     LPCSTR   lpszFilename);  
```  
  
## <a name="arguments"></a>Argumentos  
 *lpszSection*  
 Entradas Apunta a una cadena terminada en null que especifica la sección que contiene el nombre de clave. Si este argumento es NULL, la función copia todos los nombres de sección del archivo en el búfer proporcionado.  
  
 *lpszEntry*  
 Entradas Apunta a la cadena terminada en null que contiene el nombre de clave cuya cadena asociada se va a recuperar. Si este argumento es NULL, todos los nombres de clave de la sección especificada por el argumento *lpszSection* se copian en el búfer especificado por el argumento *RetBuffer* .  
  
 *lpszDefault*  
 Entradas Apunta a una cadena terminada en null que especifica el valor predeterminado para la clave determinada si la clave no se encuentra en el archivo de inicialización. Este argumento no puede ser NULL.  
  
 *RetBuffer*  
 Genere Apunta al búfer que recibe la cadena recuperada.  
  
 *cbRetBuffer*  
 Entradas Especifica el tamaño, en caracteres, del búfer al que apunta el argumento *RetBuffer* .  
  
 *lpszFilename*  
 Entradas Apunta a una cadena terminada en null que nombra el archivo de inicialización. Si este argumento no contiene una ruta de acceso completa al archivo, se busca en el directorio predeterminado.  
  
## <a name="returns"></a>Devoluciones  
 **SQLGetPrivateProfileString** devuelve un valor entero que indica el número de caracteres leídos.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando se produce un error en una llamada a **SQLGetPrivateProfileString** , se puede obtener un valor de *\* pfErrorCode* asociado mediante una llamada a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Error general del instalador|Se produjo un error en el que no había ningún error específico del instalador.|  
|ODBC_ERROR_OUT_OF_MEM|No hay memoria suficiente|El instalador no pudo realizar la función debido a una falta de memoria.|  
  
## <a name="comments"></a>Comentarios  
 **SQLGetPrivateProfileString** se proporciona como una forma sencilla de portar controladores y dll de instalación de controladores de Microsoft® Windows® a Microsoft windows NT®/Windows 2000. Las llamadas a **GetPrivateProfileString** que recuperan una cadena de perfil del archivo Odbc.ini deben reemplazarse por llamadas a **SQLGetPrivateProfileString**. **SQLGetPrivateProfileString** llama a las funciones de la API de® Win32 para recuperar los nombres solicitados de los valores o los datos correspondientes a un valor de la subclave Odbc.ini de la información del sistema.  
  
 El modo de configuración de (como se establece en **SQLSetConfigMode**) indica dónde se encuentra la entrada Odbc.ini valores de DSN en la información del sistema. Si el DSN es un DSN de usuario (el modo de configuración es USERDSN_ONLY), la función Lee de la entrada Odbc.ini de HKEY_CURRENT_USER. Si el DSN es un DSN del sistema (SYSTEMDSN_ONLY), la función Lee de la entrada Odbc.ini de HKEY_LOCAL_MACHINE. Si el modo de configuración es BOTHDSN, se intenta HKEY_CURRENT_USER y, si se produce un error, se usa HKEY_LOCAL_MACHINE.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Escribir un valor en la información del sistema|[SQLWritePrivateProfileString](../../../odbc/reference/syntax/sqlwriteprivateprofilestring-function.md)|
