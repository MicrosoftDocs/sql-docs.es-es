---
description: Función SQLSetConfigMode
title: Función SQLSetConfigMode | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetConfigMode
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetConfigMode
helpviewer_keywords:
- SQLSetConfigMode function [ODBC]
ms.assetid: 09eb88ea-b6f6-4eca-b19d-0951cebc6c0a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a136a476b56f108bef77395f0ee4b13ae23b10a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192457"
---
# <a name="sqlsetconfigmode-function"></a>Función SQLSetConfigMode
**Conformidad**  
 Versión introducida: ODBC 3,0  
  
 **Resumen**  
 **SQLSetConfigMode** establece el modo de configuración que indica dónde se encuentra la entrada Odbc.ini que enumera los valores de DSN en la información del sistema.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLSetConfigMode(  
     UWORD     wConfigMode);  
```  
  
## <a name="arguments"></a>Argumentos  
 *wConfigMode*  
 Entradas El modo de configuración del instalador (vea "Comentarios"). El valor de *wConfigMode* puede ser:  
  
 ODBC_USER_DSN  
  
 ODBC_SYSTEM_DSN  
  
 ODBC_BOTH_DSN  
  
## <a name="returns"></a>Devoluciones  
 La función devuelve TRUE si es correcto, FALSE si se produce un error.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLSetConfigMode** devuelve false, se puede obtener un valor de *\* pfErrorCode* asociado mediante una llamada a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_INVALID_PARAM_SEQUENCE|Secuencia de parámetros no válida|El argumento *wConfigMode* no contenía ODBC_USER_DSN, ODBC_SYSTEM_DSN o ODBC_BOTH_DSN.|  
  
## <a name="comments"></a>Comentarios  
 Esta función se usa para establecer dónde se encuentra la entrada Odbc.ini que enumera los valores de DSN en la información del sistema. Si *wConfigMode* es ODBC_USER_DSN, el DSN es un DSN de usuario y la función Lee de la entrada Odbc.ini de HKEY_CURRENT_USER. Si se ODBC_SYSTEM_DSN, el DSN es un DSN del sistema y la función Lee de la entrada Odbc.ini de HKEY_LOCAL_MACHINE. Si se ODBC_BOTH_DSN, se intenta HKEY_CURRENT_USER y, si se produce un error, se usa HKEY_LOCAL_MACHINE.  
  
 Esta función no afecta a **SQLCreateDataSource** ni a **SQLDriverConnect**. El modo de configuración debe establecerse cuando un controlador Lee del registro llamando a **SQLGetPrivateProfileString** o escribe en el registro llamando a **SQLWritePrivateProfileString**. Las llamadas a **SQLGetPrivateProfileString** y **SQLWritePrivateProfileString** usan el modo de configuración para saber en qué parte del registro se va a operar.  
  
> [!CAUTION]  
>  Solo se debe llamar a **SQLSetConfigMode** cuando sea necesario; Si el modo no se ha establecido correctamente, puede que el instalador de ODBC no funcione correctamente.  
  
 **SQLSetConfigMode** realiza una modificación directa del registro del modo de configuración. Esto se diferencia del proceso de modificación del modo de configuración mediante una llamada a **SQLConfigDataSource**. Una llamada a **SQLConfigDataSource** establece el modo de configuración para distinguir los DSN del usuario y del sistema al modificar un DSN. Antes de devolver, **SQLConfigDataSource** restablece el modo de configuración en BOTHDSN.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Crear un origen de datos|[SQLCreateDataSource](../../../odbc/reference/syntax/sqlcreatedatasource-function.md)|  
|Conectar con un origen de datos mediante una cadena de conexión o un cuadro de diálogo|[SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|Recuperación del modo de configuración|[SQLGetConfigMode](../../../odbc/reference/syntax/sqlgetconfigmode-function.md)|
