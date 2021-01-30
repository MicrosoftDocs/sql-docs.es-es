---
description: Tipos de datos predeterminados de C
title: Tipos de datos de C predeterminados | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], pseudo-type identifiers
- pseudo-type identifiers [ODBC], about pseudo-type identifiers
- pseudo-type identifiers [ODBC]
ms.assetid: 229140ae-af8f-4ec8-9ccf-1e92360e0bac
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fd7f50e58fd84cec5938aacc598d12e6d733fcdc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194895"
---
# <a name="default-c-data-types"></a>Tipos de datos predeterminados de C
Si una aplicación especifica SQL_C_DEFAULT en **SQLBindCol**, **SQLGetData** o **SQLBindParameter**, el controlador supone que el tipo de datos C del búfer de entrada o de salida corresponde al tipo de datos SQL de la columna o parámetro al que está enlazado el búfer.  
  
> [!IMPORTANT]  
>  Las aplicaciones interoperables no deben utilizar SQL_C_DEFAULT. En su lugar, siempre deben especificar el tipo C del búfer que están usando. Esto se debe a que los controladores no siempre determinan correctamente el tipo C predeterminado, por las razones siguientes:  
  
-   Si el DBMS promueve un tipo de datos SQL de una columna o parámetro, el controlador no puede determinar el tipo de datos SQL original de una columna o parámetro. Por lo tanto, no puede determinar el tipo de datos de C predeterminado correspondiente.  
  
-   Si el controlador no puede determinar si una columna o un parámetro determinados están firmados, como suele ser el caso cuando lo controla el DBMS, el controlador no puede determinar si el tipo de datos de C predeterminado correspondiente debe estar firmado o sin firmar.  
  
     Dado que SQL_C_DEFAULT se proporciona solo como una comodidad de programación, la aplicación no pierde ninguna funcionalidad cuando especifica el tipo de datos real de C.  
  
 En este apéndice se incluye una tabla que muestra el tipo de datos de C predeterminado para cada tipo de [datos de SQL](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).
