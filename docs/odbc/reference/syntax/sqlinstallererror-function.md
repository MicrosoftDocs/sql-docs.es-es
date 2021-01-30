---
description: Función SQLInstallerError
title: Función SQLInstallerError | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLInstallerError
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallerError
helpviewer_keywords:
- SQLInstallerError [ODBC]
ms.assetid: e6474b79-4d55-458f-81ce-abfafe357f83
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dcafadc902504ddfd32fa8b7369780bd5c296a0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189680"
---
# <a name="sqlinstallererror-function"></a>Función SQLInstallerError
**Conformidad**  
 Versión introducida: ODBC 3,0  
  
 **Resumen**  
 **SQLInstallerError** devuelve información de error o de estado para las funciones del instalador de ODBC.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
RETCODE SQLInstallerError(  
     WORD      iError,  
     DWORD *   pfErrorCode,  
     LPSTR     lpszErrorMsg,  
     WORD      cbErrorMsgMax,  
     WORD *    pcbErrorMsg);  
```  
  
## <a name="arguments"></a>Argumentos  
 *iError*  
 Entradas Número de registro de errores. Los números válidos van de 1 a 8.  
  
 *pfErrorCode*  
 Genere Código de error del instalador. (Para obtener más información, vea "Comentarios").  
  
 *lpszErrorMsg*  
 Genere Puntero al almacenamiento para el texto del mensaje de error.  
  
 *cbErrorMsgMax*  
 Entradas Longitud máxima del búfer de *szErrorMsg* . Debe ser menor o igual que SQL_MAX_MESSAGE_LENGTH menos el carácter de terminación null.  
  
 *cbErrorMsgMax*  
 Entradas Longitud máxima del búfer de *szErrorMsg* . Debe ser menor o igual que SQL_MAX_MESSAGE_LENGTH menos el carácter de terminación null.  
  
 *pcbErrorMsg*  
 Genere Puntero al número total de bytes (sin incluir el carácter de terminación null) disponible para devolver en *lpszErrorMsg*. Si el número de bytes disponibles para devolver es mayor o igual que *cbErrorMsgMax*, el texto del mensaje de error en *lpszErrorMsg* se trunca a *cbErrorMsgMax* menos los bytes de carácter de terminación null. El argumento *pcbErrorMsg* puede ser un puntero nulo.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA o SQL_ERROR.  
  
## <a name="diagnostics"></a>Diagnóstico  
 **SQLInstallerError** no envía valores de error por sí mismo. **SQLInstallerError** devuelve SQL_NO_DATA cuando no puede recuperar información de error (en cuyo caso *pfErrorCode* es undefined). Si **SQLInstallerError** no puede tener acceso a los valores de error por cualquier motivo que normalmente devuelva SQL_ERROR, **SQLInstallerError** devuelve SQL_ERROR pero no expone ningún valor de error. Si no conoce la longitud de la cadena de advertencia (*lpszErrorMsg*), puede establecer *lpszErrorMsg* en NULL y llamar a **SQLInstallerError**. **SQLInstallerError** devolverá la longitud de la cadena de advertencia en *cbErrorMsgMax*. Si el búfer del mensaje de error es demasiado corto, **SQLInstallerError** devuelve SQL_SUCCESS_WITH_INFO y devuelve el valor de *PfErrorCode* correcto para **SQLInstallerError**.  
  
 Para determinar si se ha producido un truncamiento en el mensaje de error, una aplicación puede comparar el valor del argumento *cbErrorMsgMax* con la longitud real del texto del mensaje escrito en el argumento *pcbErrorMsg* . Si se produce un truncamiento, se debe asignar la longitud de búfer correcta para *lpszErrorMsg* y **SQLInstallerError** se debe llamar de nuevo con el registro *IError* correspondiente.  
  
## <a name="comments"></a>Comentarios  
 Una aplicación llama a **SQLInstallerError** cuando una llamada anterior a la función del instalador de ODBC devuelve false. Las funciones instalador de ODBC y de instalación de controlador o traductor post cero o más errores solo cuando se produce un error en la función (devuelve FALSE). por lo tanto, una aplicación llama a **SQLInstallerError** solo después de que se produzca un error en una función del instalador de ODBC.  
  
 La cola de errores del instalador de ODBC se vacía cada vez que se llama a una nueva función de instalador. Por lo tanto, una aplicación no puede esperar que se recuperen errores de funciones distintas de la última llamada a la función de instalador.  
  
 Para recuperar varios errores para una llamada de función, una aplicación llama a **SQLInstallerError** varias veces.  
  
 Cuando no hay información adicional, **SQLInstallerError** devuelve SQL_NO_DATA, el argumento *pfErrorCode* es undefined, el argumento *pcbErrorMsg* es igual a 0 y el argumento *lpszErrorMsg* contiene un solo carácter de terminación null (a menos que el argumento *cbErrorMsgMax* sea igual a 0).
