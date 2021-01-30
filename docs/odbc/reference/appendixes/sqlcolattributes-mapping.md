---
description: Asignación de SQLColAttributes
title: Asignación de SQLColAttributes | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLColAttributes
- SQLColAttribute function [ODBC], mapping
ms.assetid: 30e25719-176b-4c48-97d4-920766b22412
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7c8ff9d861663e1bb9e52ad5e083fa4fed809139
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202895"
---
# <a name="sqlcolattributes-mapping"></a>Asignación de SQLColAttributes
Cuando una aplicación llama a **SQLColAttributes** a través de un controlador ODBC *3. x* , la llamada a **SQLColAttributes** se asigna a **SQLColAttribute** de la siguiente manera:  
  
> [!NOTE]
>  El prefijo usado en los valores de *FieldIdentifier* de ODBC *3. x* se ha cambiado del que se usa en ODBC *2. x*. El nuevo prefijo es "SQL_DESC"; el prefijo anterior era "SQL_COLUMN".  
  
1.  Si la aplicación es una aplicación ODBC *2. x* , *fDescType* se SQL_COLUMN_TYPE y el tipo devuelto es un tipo de fecha y hora conciso, el administrador de controladores asigna los valores devueltos para los códigos de fecha, hora y marca de tiempo.  
  
2.  Si *fDescType* es SQL_COLUMN_NAME, SQL_COLUMN_NULLABLE o SQL_COLUMN_COUNT, el administrador de controladores llama a **SQLColAttribute** en el controlador con el argumento *FieldIdentifier* asignado a SQL_DESC_NAME, SQL_DESC_NULLABLE o SQL_DESC_COUNT, según corresponda *.* Todos los demás valores de *fDescType* se pasan al controlador.  
  
 Un controlador ODBC *3. x* debe admitir todas las *FieldIdentifiers* de ODBC *3. x* enumeradas para **SQLColAttribute**.  
  
 Un controlador ODBC *3. x* debe admitir SQL_COLUMN_PRECISION y SQL_DESC_PRECISION, SQL_COLUMN_SCALE y SQL_DESC_SCALE, y SQL_COLUMN_LENGTH y SQL_DESC_LENGTH. Estos valores son diferentes porque la precisión, la escala y la longitud se definen de forma diferente en ODBC *3. x* que en ODBC *2. x*. Para obtener más información, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación en el](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) Apéndice D: tipos de datos.
