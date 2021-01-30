---
description: 'Apéndice D: Tipo de datos'
title: 'Apéndice D: tipos de datos | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- C data types [ODBC], defined
- SQL data types [ODBC], defined
- data types [ODBC]
- data types [ODBC], about data types
ms.assetid: 981d49c3-3531-4543-aa75-5bd9e4f67000
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b2ff95858f654bef654b18105b4fda52c77cca0c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212552"
---
# <a name="appendix-d-data-types"></a>Apéndice D: Tipo de datos
ODBC define dos conjuntos de tipos de datos: tipos de datos de SQL y tipos de datos de C. Los tipos de datos de SQL indican el tipo de datos de los datos almacenados en el origen de datos. Los tipos de datos de C indican el tipo de datos de los datos almacenados en los búferes de la aplicación.  
  
 Cada DBMS define los tipos de datos de SQL de acuerdo con el estándar SQL-92. Para cada tipo de datos de SQL especificado en el estándar SQL-92, ODBC define un identificador de tipo, que es un valor **#define** que se pasa como argumento en funciones ODBC o se devuelve en los metadatos de un conjunto de resultados. Los únicos tipos de datos SQL-92 no admitidos por ODBC son BIT (el tipo de SQL_BIT ODBC tiene características diferentes), BIT_VARYING, TIME_WITH_TIMEZONE, TIMESTAMP_WITH_TIMEZONE y NATIONAL_CHARACTER. Los controladores son responsables de asignar tipos de datos SQL específicos de orígenes de datos a los identificadores de tipo de datos SQL de ODBC y a los identificadores de tipo de datos SQL específicos del controlador. El tipo de datos SQL se especifica en el campo SQL_DESC_CONCISE_TYPE de un descriptor de implementación.  
  
 ODBC define los tipos de datos de C y sus identificadores de tipo ODBC correspondientes. Una aplicación especifica el tipo de datos C del búfer que recibirá los datos del conjunto de resultados pasando el identificador de tipo C adecuado en el argumento *TargetType* en una llamada a **SQLBindCol** o **SQLGetData**. Especifica el tipo C del búfer que contiene un parámetro de instrucción pasando el identificador de tipo C adecuado en el argumento *ValueType* en una llamada a **SQLBindParameter**. El tipo de datos C se especifica en el campo SQL_DESC_CONCISE_TYPE de un descriptor de la aplicación.  
  
> [!NOTE]  
>  No hay tipos de datos de C específicos del controlador.  
  
 Cada tipo de datos de SQL corresponde a un tipo de datos C de ODBC. Antes de devolver datos del origen de datos, el controlador lo convierte al tipo de datos de C especificado. Antes de enviar datos al origen de datos, el controlador lo convierte a partir del tipo de datos de C especificado.  
  
 Este apéndice contiene los siguientes temas.  
  
-   [Usar identificadores de tipo de datos](../../../odbc/reference/appendixes/using-data-type-identifiers.md)  
  
-   [Tipos de datos SQL](../../../odbc/reference/appendixes/sql-data-types.md)  
  
-   [Tipos de datos C](../../../odbc/reference/appendixes/c-data-types.md)  
  
-   [Identificadores y descriptores de tipo de datos](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md)  
  
-   [Identificadores de pseudotipo](../../../odbc/reference/appendixes/pseudo-type-identifiers.md)  
  
-   [Transferencia de datos en su forma binaria](../../../odbc/reference/appendixes/transferring-data-in-its-binary-form.md)  
  
-   [Directrices para los tipos de datos numéricos y de intervalo](../../../odbc/reference/appendixes/guidelines-for-interval-and-numeric-data-types.md)  
  
-   [Restricciones del calendario gregoriano](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md)  
  
-   [Tamaño de columna, dígitos decimales, longitud de transferencia y tamaño de presentación](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md)  
  
-   [Convertir datos de SQL a tipos de datos C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md)  
  
-   [Convertir datos de C en tipos de datos SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md)  
  
 Para obtener una explicación de los tipos de datos ODBC, vea [tipos de datos en ODBC](../../../odbc/reference/develop-app/data-types-in-odbc.md). Para obtener información acerca de los tipos de datos de SQL específicos del controlador, consulte la documentación del controlador.
