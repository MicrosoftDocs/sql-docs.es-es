---
description: Asignar almacenamiento
title: Asignación de almacenamiento | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- column-wise binding [ODBC]
- SQL Server Native Client ODBC driver, result sets
- ODBC applications, result sets
- SQLBindCol function
- storing data [ODBC]
- result sets [ODBC], storage
- SQL Server Native Client ODBC driver, data storage
- row-wise binding
- binding result sets [SQL Server Native Client]
- array binding
ms.assetid: 11c81955-5300-495f-925f-9256f2587b58
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f13375aa2ae2d3a56b32b2a36112413d4259f7ba
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467706"
---
# <a name="assigning-storage"></a>Asignar almacenamiento
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Una aplicación puede asignar almacenamiento para los resultados antes o después de ejecutar una instrucción SQL. Si una aplicación prepara o ejecuta primero la instrucción SQL, puede realizar consultas sobre el conjunto de resultados antes de asignar almacenamiento para los resultados. Por ejemplo, si no se conoce el conjunto de resultados, la aplicación debe recuperar el número de columnas para poder asignar almacenamiento al mismo.  
  
 Para asociar el almacenamiento de una columna de datos, una aplicación llama a [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md)y lo pasa:  
  
-   El tipo al que deben convertirse los datos.  
  
-   La dirección de un búfer de salida para los datos.  
  
     La aplicación debe asignar este búfer, que debe ser lo suficientemente grande como para albergar los datos en el formato al que se conviertan.  
  
-   La longitud del búfer de salida.  
  
     Este valor se omite si los datos devueltos tienen un ancho fijo en C, como un entero, un número real o una estructura de fecha.  
  
-   La dirección de un búfer de almacenamiento donde devolver el número de bytes de los datos disponibles.  
  
 Una aplicación también puede enlazar las columnas del conjunto de resultados a matrices de variables de programa para que las filas del conjunto de resultados puedan capturarse en bloques. Existen dos tipos distintos de enlaces de matriz:  
  
-   El enlace de modo de columna finaliza cuando cada columna se enlaza a su propia matriz de variables.  
  
     El enlace de modo de columna se especifica llamando a [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) con el *atributo* establecido en SQL_ATTR_ROW_BIND_TYPE y *ValuePtr* establecido en SQL_BIND_BY_COLUMN. Todas las matrices deben tener el mismo número de elementos.  
  
-   El enlace de modo de fila finaliza cuando todos los parámetros de la instrucción SQL se enlazan como una unidad a una matriz de estructuras que contienen variables individuales para los parámetros.  
  
     El enlace de modo de fila se especifica mediante una llamada a **SQLSetStmtAttr** con el *atributo* establecido en SQL_ATTR_ROW_BIND_TYPE y *ValuePtr* establecido en el tamaño de la estructura que contiene las variables que recibirán las columnas del conjunto de resultados.  
  
 La aplicación también establece SQL_ATTR_ROW_ARRAY_SIZE en el número de elementos de las matrices de columnas o filas y establece SQL_ATTR_ROW_STATUS_PTR y SQL_ATTR_ROWS_FETCHED_PTR.  
  
## <a name="see-also"></a>Consulte también  
 [Procesar los resultados &#40;ODBC&#41;](../../relational-databases/native-client-odbc-results/processing-results-odbc.md)  
  
  
