---
description: Función SQLDataSourceToDriver
title: Función SQLDataSourceToDriver | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDataSourceToDriver
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDataSourceToDriver
helpviewer_keywords:
- SQLDataSourceToDriver function [ODBC]
ms.assetid: 0d87fcac-30a0-4303-ad8f-a5b53f4b428d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 20e4a4cf601990f014655f3c067e9304cc5f4388
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206090"
---
# <a name="sqldatasourcetodriver-function"></a>Función SQLDataSourceToDriver
**SQLDataSourceToDriver** supportstranslations para controladores ODBC. Las aplicaciones habilitadas para ODBC no llaman a esta función; las aplicaciones solicitan la traducción a través de **SQLSetConnectAttr**. El controlador asociado con el *ConnectionHandle* especificado en **SQLSetConnectAttr** llama al archivo dll especificado para realizar traducciones de todos los datos que fluyen desde el origen de datos al controlador. Se puede especificar un archivo DLL de traducción predeterminado en el archivo de inicialización de ODBC.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLDataSourceToDriver(  
     UDWORD     fOption,  
     SWORD      fSqlType,  
     PTR        rgbValueIn,  
     SDWORD     cbValueIn,  
     PTR        rgbValueOut,  
     SDWORD     cbValueOutMax,  
     SDWORD *   pcbValueOut,  
     UCHAR *    szErrorMsg,  
     SWORD      cbErrorMsgMax,  
     SWORD *    pcbErrorMsg);  
```  
  
## <a name="arguments"></a>Argumentos  
 *fOption*  
 Entradas Valor de la opción.  
  
 *fSqlType*  
 Entradas El tipo de datos SQL. Este argumento indica al controlador cómo convertir *rgbValueIn* en un formato aceptable por la aplicación. Para obtener una lista de tipos de datos SQL válidos, vea la sección [tipos de datos SQL](../../../odbc/reference/appendixes/sql-data-types.md) en el Apéndice D: tipos de datos.  
  
 *rgbValueIn*  
 Entradas Valor que se va a traducir.  
  
 *cbValueIn*  
 Entradas Longitud de *rgbValueIn*.  
  
 *rgbValueOut*  
 Genere Resultado de la conversión.  
  
> [!NOTE]  
>  La DLL de traducción no termina en NULL este valor.  
  
 *cbValueOutMax*  
 Entradas Longitud de *rgbValueOut*.  
  
 *pcbValueOut*  
 Genere El número total de bytes (sin incluir el byte de terminación nula) disponible para devolver en *rgbValueOut*.  
  
 En el caso de los datos de caracteres o binarios, si es mayor o igual que *cbValueOutMax*, los datos de *rgbValueOut* se truncan en *cbValueOutMax* bytes.  
  
 Para el resto de tipos de datos, se omite el valor de *cbValueOutMax* y el archivo dll de traducción supone que el tamaño de *rgbValueOut* es el tamaño del tipo de datos de C predeterminado del tipo de datos de SQL especificado con *fSqlType*.  
  
 El argumento *pcbValueOut* puede ser un puntero nulo.  
  
 *szErrorMsg*  
 Genere Puntero al almacenamiento para un mensaje de error. Se trata de una cadena vacía a menos que se haya producido un error de traducción.  
  
 *cbErrorMsgMax*  
 Entradas Longitud de *szErrorMsg*.  
  
 *pcbErrorMsg*  
 Genere Puntero al número total de bytes (sin incluir el byte de terminación nula) disponible para devolver en *szErrorMsg*. Si es mayor o igual que *cbErrorMsg*, los datos de *szErrorMsg* se truncan en *cbErrorMsgMax* menos el carácter de terminación null. El argumento *pcbErrorMsg* puede ser un puntero nulo.  
  
## <a name="returns"></a>Devoluciones  
 TRUE si la conversión se realizó correctamente, FALSE si se produjo un error en la traducción.  
  
## <a name="comments"></a>Comentarios  
 El controlador llama a **SQLDataSourceToDriver** para traducir ALLDATA (los datos del conjunto de resultados, los nombres de tabla, los recuentos de filas, los mensajes de error, etc.) que pasan desde el origen de datos al controlador. Es posible que la DLL de traducción no traduzca algunos datos, según el tipo de datos y el propósito del archivo DLL de traducción. por ejemplo, un archivo DLL que traduce los datos de caracteres de una página de códigos a otra omite todos los datos numéricos y binarios.  
  
 El valor de *fOption* se establece en el valor de *vParam* especificado mediante una llamada a **SQLSetConnectAttr** con el atributo SQL_ATTR_TRANSLATE_OPTION. Es un valor de 32 bits que tiene un significado específico para un archivo DLL de traducción determinado. Por ejemplo, podría especificar una traducción determinada del juego de caracteres.  
  
 Si se especifica el mismo búfer para *rgbValueIn* y *rgbValueOut*, se llevará a cabo la traducción de los datos en el búfer.  
  
 Aunque *cbValueIn*, *cbValueOutMax* y *pcbValueOut* son del tipo SDWORD, **SQLDataSourceToDriver** no admite necesariamente punteros enormes.  
  
 Si **SQLDataSourceToDriver** devuelve false, es posible que se haya producido un truncamiento de datos durante la traducción. Si *pcbValueOut* (el número de bytes disponibles para devolver en el búfer de salida) es mayor que *cbValueOutMax* (la longitud del búfer de salida), se produce un truncamiento. El controlador debe determinar si el truncamiento es aceptable. Si no se ha producido el truncamiento, el **SQLDataSourceToDriver** devolvió false debido a otro error. En cualquier caso, se devuelve un mensaje de error específico en *szErrorMsg*.  
  
 Para obtener más información sobre la traducción de datos, vea [archivos dll de traducción](../../../odbc/reference/develop-app/translation-dlls.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Trasladar los datos que se envían al origen de datos|[SQLDriverToDataSource](../../../odbc/reference/syntax/sqldrivertodatasource-function.md)|  
|Devolver el valor de un atributo de conexión|[SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Establecer un atributo de conexión|[SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|
