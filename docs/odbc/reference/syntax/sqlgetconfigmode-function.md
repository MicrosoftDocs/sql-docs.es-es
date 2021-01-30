---
description: Función SQLGetConfigMode
title: Función SQLGetConfigMode | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetConfigMode
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetConfigMode
helpviewer_keywords:
- SQLGetConfigMode function [ODBC]
ms.assetid: b96ab3b8-08d5-4fea-9ffe-e03043efbf2d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0ccd703f6fd8c7b80dec575d85e44f2c955ea669
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203467"
---
# <a name="sqlgetconfigmode-function"></a>Función SQLGetConfigMode
**Conformidad**  
 Versión introducida: ODBC 3,0  
  
 **Resumen**  
 **SQLGetConfigMode** recupera el modo de configuración que indica el lugar en el que se encuentra la entrada Odbc.ini que enumera los valores de DSN en la información del sistema.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLGetConfigMode(  
     UWORD *   pwConfigMode);  
```  
  
## <a name="arguments"></a>Argumentos  
 *pwConfigMode*  
 Genere Puntero al búfer que contiene el modo de configuración. (Vea "Comentarios"). El valor de *\* pwConfigMode* puede ser:  
  
 ODBC_USER_DSN  
  
 ODBC_SYSTEM_DSN  
  
 ODBC_BOTH_DSN  
  
## <a name="returns"></a>Devoluciones  
 La función devuelve TRUE si es correcto, FALSE si se produce un error.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLGetConfigMode** devuelve false, se puede obtener un valor de *\* pfErrorCode* asociado mediante una llamada a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_OUT_OF_MEM|No hay memoria suficiente|El instalador no pudo realizar la función debido a una falta de memoria.|  
  
## <a name="comments"></a>Comentarios  
 Esta función se usa para determinar dónde se encuentra la entrada Odbc.ini que enumera los valores de DSN en la información del sistema. Si *\* pwConfigMode* es ODBC_USER_DSN, el DSN es un DSN de usuario y la función Lee de la entrada Odbc.ini de HKEY_CURRENT_USER. Si se ODBC_SYSTEM_DSN, el DSN es un DSN del sistema y la función Lee de la entrada Odbc.ini de HKEY_LOCAL_MACHINE. Si se ODBC_BOTH_DSN, se intenta HKEY_CURRENT_USER y, si se produce un error, se usa HKEY_LOCAL_MACHINE.  
  
 De forma predeterminada, **SQLGetConfigMode** devuelve ODBC_BOTH_DSN. Cuando un DSN de usuario o un DSN de sistema se crean mediante una llamada a **SQLConfigDataSource**, la función establece el modo de configuración en ODBC_USER_DSN o ODBC_SYSTEM_DSN para distinguir los DSN de usuario y sistema al modificar un DSN. Antes de devolver, **SQLConfigDataSource** restablece el modo de configuración en ODBC_BOTH_DSN.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Establecimiento del modo de configuración|[SQLSetConfigMode](../../../odbc/reference/syntax/sqlsetconfigmode-function.md)|
