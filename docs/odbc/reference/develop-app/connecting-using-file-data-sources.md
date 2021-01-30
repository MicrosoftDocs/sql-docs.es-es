---
description: Conectar con orígenes de datos de archivo
title: Conectarse con orígenes de datos de archivo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- connecting to driver [ODBC], file data sources
- SQLDriverConnect function [ODBC], connecting using file data sources
- connecting to data source [ODBC], SQLDriverConnect
- connecting to driver [ODBC], SQLDriverConnect
- connecting to data source [ODBC], file data sources
- file data sources [ODBC]
ms.assetid: 3003f8c2-8be6-41cc-8d9c-612e9bd0f3ae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 285b2ba9d553d43a3b1ca7fdda8c08434201acdc
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077013"
---
# <a name="connecting-using-file-data-sources"></a>Conectar con orígenes de datos de archivo
La información de conexión para un origen de datos de archivo se almacena en un archivo. DSN. Como resultado, un solo usuario puede usar la cadena de conexión varias veces o compartir entre varios usuarios si tienen instalado el controlador adecuado. El archivo contiene un nombre de controlador (u otro nombre de origen de datos en el caso de un origen de datos de archivo no compartible) y, opcionalmente, una cadena de conexión que puede usar **SQLDriverConnect**. El administrador de controladores crea la cadena de conexión para la llamada a **SQLDriverConnect** desde las palabras clave del archivo. DSN.  
  
 Un origen de datos de archivo permite a una aplicación especificar opciones de conexión sin tener que crear una cadena de conexión para su uso con **SQLDriverConnect**. Normalmente, el origen de datos de archivo se crea especificando la palabra clave **SaveFile** , que hace que el administrador de controladores guarde la cadena de conexión de salida creada mediante una llamada a **SQLDriverConnect** al archivo. DSN. Esa cadena de conexión se puede usar repetidamente llamando a **SQLDriverConnect** con la palabra clave **FILEDSN** . Esto simplifica el proceso de conexión y proporciona un origen persistente de la cadena de conexión.  
  
 Los orígenes de datos de archivo también se pueden crear mediante una llamada a **SQLCreateDataSource** en el archivo DLL del instalador. La información se puede escribir en el archivo. DSN llamando a **SQLWriteFileDSN** y leer desde el archivo. DSN llamando a **SQLReadFileDSN**; ambas funciones también están en el archivo DLL del instalador. Para obtener información acerca de la DLL del instalador, consulte [configuración de orígenes de datos](../../../odbc/reference/install/configuring-data-sources.md).  
  
 Las palabras clave usadas para la información de conexión se encuentran en la sección [ODBC] de un archivo. DSN. La información mínima que un archivo. DSN compartible tendría en la sección [ODBC] es la palabra clave DRIVER:  
  
```  
DRIVER = SQL Server  
```  
  
 El archivo Shareable. DSN normalmente contiene una cadena de conexión, como se indica a continuación:  
  
```  
DRIVER = SQL Server  
UID = Larry  
DATABASE = MyDB  
```  
  
 Cuando el origen de datos de archivo no es compartible, el archivo. DSN solo contiene una palabra clave **DSN** . Cuando el administrador de controladores envía la información de un origen de datos de archivos que no se pueden compartir, se conecta según sea necesario al origen de datos indicado por la palabra clave **DSN** . Un archivo. DSN que no se puede compartir contiene la palabra clave siguiente:  
  
```  
DSN = MyDataSource  
```  
  
 La cadena de conexión utilizada para un origen de datos de archivo es la Unión de las palabras clave especificadas en el archivo. DSN y las palabras clave especificadas en la cadena de conexión en la llamada a **SQLDriverConnect**. Si alguna de las palabras clave del archivo. DSN entra en conflicto con las palabras clave de la cadena de conexión, el administrador de controladores decide qué valor de palabra clave se debe usar. Para obtener más información, consulte [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
## <a name="see-also"></a>Consulte también  
 [https://support.microsoft.com/kb/165866](https://mskb.pkisolutions.com/kb/165866)
