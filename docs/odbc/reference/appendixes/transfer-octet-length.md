---
description: Transferir la longitud en octetos
title: Longitud del octeto de transferencia | Microsoft Docs
ms.custom: ''
ms.date: 10/28/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- transfer octet length of data types [ODBC]
- size of data types [ODBC]
- SQL data types [ODBC], column characteristics
- data types [ODBC], transfer octet length
ms.assetid: 9fdc9762-e203-4cff-9212-54f450bf18d9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b910f544be952a18ae2961e9939960820e3805bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202488"
---
# <a name="transfer-octet-length"></a>Transferir la longitud en octetos
La longitud del octeto de transferencia de una columna es el número máximo de bytes que se devuelven a la aplicación cuando los datos se transfieren a su tipo de datos C predeterminado. En el caso de los datos de caracteres, la longitud del octeto de transferencia no incluye espacio para el carácter de terminación null. La longitud del octeto de transferencia de una columna puede ser diferente del número de bytes necesarios para almacenar los datos en el origen de datos.  
  
 En la tabla siguiente se muestra la longitud de octetos de transferencia definida para cada tipo de datos SQL de ODBC.  
  
|Identificador de tipo de SQL|Length|  
|-------------------------|------------|  
|Todos los tipos de caracteres [a]|La longitud definida o máxima (para el tipo de variable) de la columna en bytes. Este es el mismo valor que el campo descriptor SQL_DESC_OCTET_LENGTH.|  
|SQL_DECIMAL<br />SQL_NUMERIC|El número de bytes necesarios para contener la representación de caracteres de estos datos si el juego de caracteres es ANSI y el doble de este número si el juego de caracteres es Unicode. Es el número máximo de dígitos más dos, porque los datos se devuelven como una cadena de caracteres y se necesitan caracteres para los dígitos, un signo y un separador decimal. Por ejemplo, la longitud de la transferencia de una columna definida como numérica (10, 3) es 12.|  
|SQL_TINYINT|1|  
|SQL_SMALLINT|2|  
|SQL_INTEGER|4|  
|SQL_BIGINT| 8 |  
|SQL_REAL|4|  
|SQL_FLOAT|8|  
|SQL_DOUBLE|8|  
|SQL_BIT|1|  
|Todos los tipos binarios [a]|Número de bytes necesarios para contener el número de caracteres definido (para tipos fijos) o máximo (para tipos de variable).|  
|SQL_TYPE_DATE<br />SQL_TYPE_TIME|6 (el tamaño de la estructura de SQL_DATE_STRUCT o SQL_TIME_STRUCT).|  
|SQL_TYPE_TIMESTAMP|16 (el tamaño de la estructura de SQL_TIMESTAMP_STRUCT).|  
|Todos los tipos de datos de intervalo|34 (el tamaño de la estructura de intervalo).|  
|SQL_GUID|16 (el tamaño de la estructura GUID).|  
| &nbsp; | &nbsp; |

 [a] si el controlador no puede determinar la longitud de la columna o el parámetro para los tipos de variable, devuelve SQL_NO_TOTAL.
