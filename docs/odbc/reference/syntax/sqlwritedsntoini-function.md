---
description: Función SQLWriteDSNToIni
title: Función SQLWriteDSNToIni | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLWriteDSNToIni
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLWriteDSNToIni
helpviewer_keywords:
- SQLWriteDSNToIni [ODBC]
ms.assetid: dc7018b2-18d4-4657-96d0-086479a47474
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 58e09c90ef785d1c851a1b95ef9a861277f47db7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191701"
---
# <a name="sqlwritedsntoini-function"></a>Función SQLWriteDSNToIni
**Conformidad**  
 Versión introducida: ODBC 1,0  
  
 **Resumen**  
 **SQLWriteDSNToIni** agrega un origen de datos a la información del sistema.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLWriteDSNToIni(  
     LPCSTR   lpszDSN,  
     LPCSTR   lpszDriver);  
```  
  
## <a name="arguments"></a>Argumentos  
 *lpszDSN*  
 Entradas Nombre del origen de datos que se va a agregar.  
  
 *lpszDriver*  
 Entradas Descripción del controlador (normalmente, el nombre del DBMS asociado) que se presenta a los usuarios en lugar del nombre del controlador físico.  
  
## <a name="returns"></a>Devoluciones  
 La función devuelve TRUE si es correcto, FALSE si se produce un error.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLWriteDSNToIni** devuelve false, se puede obtener un valor de *\* pfErrorCode* asociado mediante una llamada a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Error general del instalador|Se produjo un error en el que no había ningún error específico del instalador.|  
|ODBC_ERROR_INVALID_DSN|DSN no válido|El argumento *lpszDSN* contenía una cadena que no era válida para un DSN.|  
|ODBC_ERROR_INVALID_NAME|Nombre de traductor o controlador no válido|El argumento *lpszDriver* no era válido.|  
|ODBC_ERROR_REQUEST_FAILED|Error en la solicitud|El instalador no pudo crear un DSN en el registro.|  
|ODBC_ERROR_OUT_OF_MEM|No hay memoria suficiente|El instalador no pudo realizar la función debido a una falta de memoria.|  
  
## <a name="comments"></a>Comentarios  
 **SQLWriteDSNToIni** agrega el origen de datos a la sección [ODBC Data Sources] de la información del sistema. A continuación, crea una sección de especificación para el origen de datos y agrega una palabra clave única (**driver**) con el nombre de la dll del controlador como su valor. Si la sección especificación de origen de datos ya existe, **SQLWriteDSNToIni** quita la sección anterior antes de crear la nueva.  
  
 El autor de la llamada de esta función debe agregar las palabras clave y los valores específicos del controlador a la sección especificación de origen de datos de la información del sistema.  
  
 Si el nombre del origen de datos es predeterminado, **SQLWriteDSNToIni** también crea la sección especificación predeterminada del controlador en la información del sistema.  
  
 Solo se debe llamar a esta función desde un archivo DLL de instalación.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Agregar, modificar o quitar un origen de datos|[ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md)(en el archivo dll de instalación)|  
|Agregar, modificar o quitar un origen de datos|[SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md)|  
|Quitar un nombre de origen de datos de la información del sistema|[SQLRemoveDSNFromIni](../../../odbc/reference/syntax/sqlremovedsnfromini-function.md)|
