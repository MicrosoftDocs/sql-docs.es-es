---
description: Función SQLValidDSN
title: Función SQLValidDSN | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLValidDSN
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLValidDSN
helpviewer_keywords:
- SQLValidDSN [ODBC]
ms.assetid: 930d1d89-337a-4429-85a2-84ee10555ac9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 62692ed4b5d0ab600ceb36e87e7f796c77120c89
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191710"
---
# <a name="sqlvaliddsn-function"></a>Función SQLValidDSN
**Conformidad**  
 Versión introducida: ODBC 2,0  
  
 **Resumen**  
 **SQLValidDSN** comprueba la longitud y la validez del nombre del origen de datos antes de agregar el nombre a la información del sistema.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLValidDSN(  
     LPCSTR    lpszDSN);  
```  
  
## <a name="arguments"></a>Argumentos  
 *lpszDSN*  
 Entradas Nombre del origen de datos que se va a comprobar.  
  
## <a name="returns"></a>Devoluciones  
 La función devuelve TRUE si el nombre del origen de datos es válido. Devuelve FALSE si el nombre del origen de datos no es válido o se produjo un error en la llamada de función.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLValidDSN** devuelve false, se puede obtener un valor de *\* pfErrorCode* asociado mediante una llamada a **SQLInstallerError**. Se devuelve un *\* pfErrorCode* solo si se produjo un error en la llamada de función, no si se devolvió false porque el nombre del origen de datos no es válido. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Error general del instalador|Se produjo un error en el que no había ningún error específico del instalador.|  
|ODBC_ERROR_OUT_OF_MEM|No hay memoria suficiente|El instalador no pudo realizar la función debido a una falta de memoria.|  
  
## <a name="comments"></a>Comentarios  
 La [ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md) de un controlador llama a **SQLValidDSN** para comprobar la longitud del nombre del origen de datos y la validez de los caracteres individuales en el nombre del origen de datos. Comprueba si la longitud del nombre es mayor que SQL_MAX_DSN_LENGTH, tal y como se define en Sqlext. h. (La longitud del nombre del origen de datos también se comprueba por [SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)). **SQLValidDSN** comprueba si alguno de los siguientes caracteres no válidos se incluye en el nombre del origen de datos:  
  
 [ ] { } ( ) , ; ? * = ! \@ \  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Agregar, modificar o quitar un origen de datos|[ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md) (en el archivo dll de instalación)|  
|Agregar, modificar o quitar un origen de datos|[SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md)|  
|Escribir un nombre de origen de datos en la información del sistema|[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|
