---
description: Tipos de datos de campo de Visual FoxPro
title: Tipos de datos de campo de Visual FoxPro | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- field data types [ODBC]
- Visual FoxPro ODBC driver [ODBC], data types
ms.assetid: 50b733dc-679a-4b10-bc5d-98bb474dead2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2032ee25039364186f33486e4662b84c36c06947
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207473"
---
# <a name="visual-foxpro-field-data-types"></a>Tipos de datos de campo de Visual FoxPro
En la tabla siguiente se muestran los valores para el argumento *FieldType* en alter table y CREATE TABLE e indica si son necesarios los argumentos *nFieldWidth* y *nPrecision* .  
  
|*FieldType*|*NFieldWidth*|*nPrecision*|Descripción|  
|-----------------|-------------------|------------------|-----------------|  
|B|-|d|Double|  
|C|No|-|Campo de carácter de ancho *n*|  
|D|-|-|Date|  
|F|No|d|Campo numérico flotante de ancho *n* con posiciones decimales de *d*|  
|G|-|-|General|  
|I|-|-|Entero|  
|L|-|-|Lógicos|  
|M|-|-|Memo|  
|No|No|d|Campo numérico de ancho *n* con posiciones decimales de *d*|  
|T|-|-|DateTime|  
|Y|-|-|Moneda|
