---
description: Función SQLWritePrivateProfileString
title: Función SQLWritePrivateProfileString | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLWritePrivateProfileString
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLWritePrivateProfileString
helpviewer_keywords:
- SQLWritePrivateProfileString [ODBC]
ms.assetid: 526f36a4-92ed-4874-9725-82d27c0b86f9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8f001e3a4ad2b6e3e4aad3f34e8a87be1c7b2b12
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191643"
---
# <a name="sqlwriteprivateprofilestring-function"></a>Función SQLWritePrivateProfileString
**Conformidad**  
 Versión introducida: ODBC 2,0  
  
 **Resumen**  
 **SQLWritePrivateProfileString** escribe un nombre de valor y los datos en la subclave Odbc.ini de la información del sistema.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLWritePrivateProfileString(  
     LPCSTR     lpszSection,  
     LPCSTR     lpszEntry,  
     LPCSTR     lpszString,  
     LPCSTR     lpszFilename);  
```  
  
## <a name="arguments"></a>Argumentos  
 *lpszSection*  
 Entradas Apunta a una cadena terminada en null que contiene el nombre de la sección en la que se copiará la cadena. Si la sección no existe, se crea. El nombre de la sección es independiente de las mayúsculas y minúsculas; la cadena puede ser cualquier combinación de mayúsculas y minúsculas.  
  
 *lpszEntry*  
 Entradas Apunta a una cadena terminada en null que contiene el nombre de la clave que se va a asociar a una cadena. Si la clave no existe en la sección especificada, se crea. Si este argumento es NULL, se elimina toda la sección, incluidas todas las entradas de la sección.  
  
 *lpszString*  
 Entradas Apunta a una cadena terminada en null que se va a escribir en el archivo. Si este argumento es NULL, se elimina la clave a la que apunta el argumento *lpszEntry* .  
  
 *lpszFilename*  
 Genere Apunta a una cadena terminada en null que nombra el archivo de inicialización.  
  
## <a name="returns"></a>Devoluciones  
 La función devuelve TRUE si es correcto, FALSE si se produce un error.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLWritePrivateProfileString** devuelve false, se puede obtener un valor de *\* pfErrorCode* asociado mediante una llamada a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Error general del instalador|Se produjo un error en el que no había ningún error específico del instalador.|  
|ODBC_ERROR_REQUEST_FAILED|Error en la solicitud|No se pudo escribir la información del sistema solicitada.|  
|ODBC_ERROR_OUT_OF_MEM|No hay memoria suficiente|El instalador no pudo realizar la función debido a una falta de memoria.|  
  
## <a name="comments"></a>Comentarios  
 **SQLWritePrivateProfileString** se proporciona como una forma sencilla de portar controladores y dll de instalación de controladores de Microsoft® Windows® a Microsoft windows NT®/Windows 2000. Las llamadas a **WritePrivateProfileString** que escriben una cadena de perfil en el archivo Odbc.ini deben reemplazarse por llamadas a **SQLWritePrivateProfileString**. **SQLWritePrivateProfileString** llama a las funciones de la API de® Win32 para agregar el nombre de valor y los datos especificados a la subclave Odbc.ini de la información del sistema.  
  
 Un modo de configuración indica dónde se encuentra en la información del sistema la entrada Odbc.ini que enumera los valores de DSN. Si el DSN es un DSN de usuario (la variable de estado es USERDSN_ONLY), la función escribe en la entrada Odbc.ini de HKEY_CURRENT_USER. Si el DSN es un DSN del sistema (SYSTEMDSN_ONLY), la función escribe en la entrada Odbc.ini de HKEY_LOCAL_MACHINE. Si la variable de estado es BOTHDSN, se intenta HKEY_CURRENT_USER y, si se produce un error, se usa HKEY_LOCAL_MACHINE.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Obtener un valor de la información del sistema|[SQLGetPrivateProfileString](../../../odbc/reference/syntax/sqlgetprivateprofilestring-function.md)|
