---
description: Función SQLGetInstalledDrivers
title: Función SQLGetInstalledDrivers | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetInstalledDrivers
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetInstalledDrivers
helpviewer_keywords:
- SQLGetInstalledDrivers function [ODBC]
ms.assetid: a1983a2e-0edf-422e-bd1b-ec5db40a34bc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5f770254874b09158632ebaee071964c00265a46
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180949"
---
# <a name="sqlgetinstalleddrivers-function"></a>Función SQLGetInstalledDrivers
**Conformidad**  
 Versión introducida: ODBC 1,0  
  
 **Resumen**  
 **SQLGetInstalledDrivers** lee la sección [ODBC drivers] de la información del sistema y devuelve una lista de descripciones de los controladores instalados.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLGetInstalledDrivers(  
     LPSTR   lpszBuf,  
     WORD    cbBufMax,  
     WORD *  pcbBufOut);  
```  
  
## <a name="arguments"></a>Argumentos  
 *lpszBuf*  
 Genere Lista de descripciones de los controladores instalados. Para obtener información sobre la estructura de la lista, vea "Comentarios".  
  
 *cbBufMax*  
 Entradas Longitud de *lpszBuf*.  
  
 *pcbBufOut*  
 Genere Número total de bytes (sin incluir el byte de terminación nula) devueltos en *lpszBuf*. Si el número de bytes disponibles para devolver es mayor o igual que *cbBufMax*, la lista de descripciones de los controladores de *lpszBuf* se trunca en *cbBufMax* menos el carácter de terminación null. El argumento *pcbBufOut* puede ser un puntero nulo.  
  
## <a name="returns"></a>Devoluciones  
 La función devuelve TRUE si es correcto, FALSE si se produce un error.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLGetInstalledDrivers** devuelve false, se puede obtener un valor de *\* pfErrorCode* asociado mediante una llamada a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Error general del instalador|Se produjo un error en el que no había ningún error específico del instalador.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longitud de búfer no válida|El argumento *lpszBuf* era null o no era válido o el argumento *cbBufMax* era menor o igual que 0.|  
|ODBC_ERROR_COMPONENT_NOT_FOUND|No se encontró el componente en el registro|El instalador no encontró la sección [ODBC drivers] en el registro.|  
|ODBC_ERROR_OUT_OF_MEM|No hay memoria suficiente|El instalador no pudo realizar la función debido a una falta de memoria.|  
  
## <a name="comments"></a>Comentarios  
 Cada una de las descripciones de los controladores termina con un byte nulo y la lista completa finaliza con un byte nulo. (Es decir, dos bytes nulos marcan el final de la lista). Si el búfer asignado no es lo suficientemente grande como para contener toda la lista, la lista se trunca sin errores. Se devuelve un error si se pasa un puntero nulo como *lpszBuf*.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Devolver descripciones y atributos de controladores|[SQLDrivers](../../../odbc/reference/syntax/sqldrivers-function.md)|
