---
description: Función SQLGetTranslator
title: Función SQLGetTranslator | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetTranslator
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetTranslator
helpviewer_keywords:
- SQLGetTranslator function [ODBC]
ms.assetid: 33879db3-5ef9-4585-9be5-69376157e017
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5ec960312c652c625a6fb27388bd9c21a09f3206
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189732"
---
# <a name="sqlgettranslator-function"></a>Función SQLGetTranslator
**Conformidad**  
 Versión introducida: ODBC 2,0  
  
 **Resumen**  
 **SQLGetTranslator** muestra un cuadro de diálogo en el que un usuario puede seleccionar un traductor.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLGetTranslator(  
     HWND      hwndParent,  
     LPSTR     lpszName,  
     WORD      cbNameMax,  
     WORD *    pcbNameOut,  
     LPSTR     lpszPath,  
     WORD      cbPathMax,  
     WORD *    pcbPathOut,  
     DWORD *   pvOption);  
```  
  
## <a name="arguments"></a>Argumentos  
 *hwndParent*  
 Entradas Identificador de la ventana primaria.  
  
 *lpszName*  
 [Entrada/salida] Nombre del traductor de la información del sistema.  
  
 *cbNameMax*  
 Entradas Longitud máxima del búfer de *lpszName* .  
  
 *pcbNameOut*  
 [Entrada/salida] Número total de bytes (sin incluir el byte de terminación nula) pasados o devueltos en *lpszName*. Si el número de bytes disponibles para devolver es mayor o igual que *cbNameMax*, el nombre de Traductor en *lpszName* se trunca en *cbNameMax* menos el carácter de terminación de NULL. El argumento *pcbNameOut* puede ser un puntero nulo.  
  
 *lpszPath*  
 Genere Ruta de acceso completa del archivo DLL de traducción.  
  
 *cbPathMax*  
 Entradas Longitud máxima del búfer de *lpszPath* .  
  
 *pcbPathOut*  
 Genere Número total de bytes (sin incluir el byte de terminación nula) devueltos en *lpszPath*. Si el número de bytes disponibles para devolver es mayor o igual que *cbPathMax*, la ruta de acceso del archivo dll de traducción en *lpszPath* se trunca a *cbPathMax* menos el carácter de terminación null. El argumento *pcbPathOut* puede ser un puntero nulo.  
  
 *pvOption*  
 [Salida] opción de traducción de 32 bits.  
  
## <a name="returns"></a>Devoluciones  
 La función devuelve TRUE si es correcto, FALSE si se produce un error o si el usuario cancela el cuadro de diálogo.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLGetTranslator** devuelve false, se puede obtener un valor de *\* pfErrorCode* asociado mediante una llamada a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Error general del instalador|Se produjo un error en el que no había ningún error específico del instalador.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Longitud de búfer no válida|El argumento *cbNameMax* o *cbPathMax* era menor o igual que 0.|  
|ODBC_ERROR_INVALID_HWND|Identificador de ventana no válido|El argumento *hwndParent* no era válido o era null.|  
|ODBC_ERROR_INVALID_NAME|Nombre de traductor o controlador no válido|El argumento *lpszName* no era válido. No se encontró en el registro.|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|No se pudo cargar la biblioteca de instalación del controlador o el traductor|No se pudo cargar la biblioteca de traductores.|  
|ODBC_ERROR_INVALID_OPTION|Opción de transacción no válida|El argumento *pvOption* contenía un valor no válido.|  
|ODBC_ERROR_OUT_OF_MEM|No hay memoria suficiente|El instalador no pudo realizar la función debido a una falta de memoria.|  
  
## <a name="comments"></a>Comentarios  
 Si *hwndParent* es null o si *lpszName*, *lpszPath* o *pvOption* es un puntero nulo, **SQLGetTranslator** devuelve false. De lo contrario, muestra la lista de traductores instalados en el siguiente cuadro de diálogo.  
  
 ![Cuadro de diálogo Seleccionar traductor](../../../odbc/reference/syntax/media/ch23j.gif "CH23J")  
  
 Si *lpszName* contiene un nombre de traductor válido, se selecciona. De lo contrario, \<No Translator> se selecciona.  
  
 Si el usuario elige \<No Translator> , no se toca el contenido de *lpszName*, *lpszPath* y *pvOption* . **SQLGetTranslator** establece *pcbNameOut* y *PCBPATHOUT* en 0 y devuelve true.  
  
 Si el usuario elige un traductor, **SQLGetTranslator** llama a **ConfigTranslator** en el archivo dll de instalación del traductor. Si **ConfigTranslator** devuelve false, **SQLGetTranslator** vuelve a su cuadro de diálogo. Si **ConfigTranslator** devuelve true, **SQLGetTranslator** devuelve true, junto con el nombre de traductor, la ruta de acceso y la opción de traducción seleccionados.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Configuración de un traductor|[ConfigTranslator](../../../odbc/reference/syntax/configtranslator-function.md)|  
|Obtener un atributo Translation|[SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Establecer un atributo Translation|[SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|
