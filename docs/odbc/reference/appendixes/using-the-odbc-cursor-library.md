---
description: Uso de la biblioteca de cursores ODBC
title: Usar la biblioteca de cursores ODBC | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], using cursor library
- cursor library [ODBC], using cursor library
ms.assetid: 9653f2f8-ccfc-4220-99ef-601dc0fa641c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1658d3c628506f1b5c53a5c9271a10f96fa2ef44
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158186"
---
# <a name="using-the-odbc-cursor-library"></a>Uso de la biblioteca de cursores ODBC
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite usar esta característica en los nuevos trabajos de desarrollo y planee modificar las aplicaciones que actualmente la utilizan. Microsoft recomienda el uso de la funcionalidad de cursor del controlador.  
  
 Para usar la biblioteca de cursores ODBC, una aplicación:  
  
1.  Llama a **SQLSetConnectAttr** con un *atributo* de SQL_ATTR_ODBC_CURSORS para especificar cómo se debe usar la biblioteca de cursores con una conexión determinada. La biblioteca de cursores se puede usar siempre (SQL_CUR_USE_ODBC), solo se usa si el controlador no admite cursores desplazables (SQL_CUR_USE_IF_NEEDED) o nunca se usa (SQL_CUR_USE_DRIVER).  
  
2.  Llama a **SQLConnect**, **SQLDriverConnect** o **SQLBrowseConnect** para conectarse al origen de datos.  
  
3.  Llama a **SQLSetStmtAttr** para especificar el tipo de cursor (SQL_ATTR_CURSOR_TYPE), la simultaneidad (SQL_ATTR_CONCURRENCY) y el tamaño del conjunto de filas (SQL_ATTR_ROW_ARRAY_SIZE). La biblioteca de cursores admite cursores de solo avance y estáticos. Los cursores de solo avance deben ser de solo lectura, mientras que los cursores estáticos pueden ser de solo lectura o pueden usar el control de simultaneidad optimista comparando valores.  
  
4.  Asigna uno o más búferes de conjunto de filas y llama a **SQLBindCol** una o más veces para enlazar estos búferes a las columnas del conjunto de resultados.  
  
5.  Genera un conjunto de resultados mediante la ejecución de una instrucción **Select** o un procedimiento, o mediante una llamada a una función de catálogo. Si la aplicación va a ejecutar instrucciones Update posicionadas, debe ejecutar una instrucción **Select for update** para generar el conjunto de resultados.  
  
6.  Llama a **SQLFetch** o **SQLFetchScroll** una o más veces para desplazarse por el conjunto de resultados.  
  
 La aplicación puede cambiar los valores de datos en los búferes del conjunto de filas. Para actualizar los búferes de conjunto de filas con datos de la memoria caché de la biblioteca de cursores, una aplicación llama a **SQLFetchScroll** con el argumento *FetchOrientation* establecido en SQL_FETCH_RELATIVE y el argumento *FetchOffset* establecido en 0.  
  
 Para recuperar datos de una columna sin enlazar, la aplicación llama a **SQLSetPos** para colocar el cursor en la fila deseada. A continuación, llama a **SQLGetData** para recuperar los datos.  
  
 Para determinar el número de filas que se han recuperado del origen de datos, la aplicación llama a **SQLRowCount**.
