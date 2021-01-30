---
description: Función SQLPostInstallerError
title: Función SQLPostInstallerError | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLPostInstallerError
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLPostInstallerError
helpviewer_keywords:
- SQLPostInstallerError function [ODBC]
ms.assetid: 4c60d827-b2d2-4f27-b220-daa9e1fcdd8d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 72f3059e53e68cce0fc40286ae7c02d0e7eb38bb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204517"
---
# <a name="sqlpostinstallererror-function"></a>Función SQLPostInstallerError
**Conformidad**  
 Versión introducida: ODBC 3,0  
  
 **Resumen**  
 **SQLPostInstallerError** proporciona un mecanismo para que la biblioteca de instalación de un controlador o un traductor informe de los errores de las funciones **ConfigDriver**, **ConfigDSN** y **ConfigTranslator** en la cola de errores del instalador. Las aplicaciones no utilizan esta API; utilizan **SQLInstallerError** para recuperar el error.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
RETCODE SQLPostInstallerError(  
     DWORD    fErrorCode,  
     LPSTR    szErrorMsg);  
```  
  
## <a name="arguments"></a>Argumentos  
 *fErrorCode*  
 Entradas Código de error del instalador.  
  
 *szErrorMsg*  
 Entradas Texto del mensaje de error.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS o SQL_ERROR.  
  
## <a name="diagnostics"></a>Diagnóstico  
 **SQLPostInstallerError** no envía valores de error por sí mismo. Si el error se ha expuesto correctamente en la cola de errores del instalador (recuperable mediante **SQLInstallerError**), se devuelve SQL_SUCCESS. Se devolverá SQL_ERROR si el valor del argumento *dwErrorCode* no es ninguno de los códigos de error del instalador especificados.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Agregar, modificar o quitar un controlador|[ConfigDriver](../../../odbc/reference/syntax/configdriver-function.md)|  
|Agregar, modificar o quitar orígenes de datos|[ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md)|  
|Configuración de una opción de traducción|[ConfigTranslator](../../../odbc/reference/syntax/configtranslator-function.md)|
