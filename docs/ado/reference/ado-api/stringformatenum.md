---
description: StringFormatEnum
title: StringFormatEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- StringFormatEnum
helpviewer_keywords:
- StringFormatEnum enumeration [ADO]
ms.assetid: 28f7d1ec-092b-4323-a39d-d3f882c6c81a
author: rothja
ms.author: jroth
ms.openlocfilehash: 199aaab9215165e06598ca0c1be783b0a123a1c5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166394"
---
# <a name="stringformatenum"></a>StringFormatEnum
Especifica el formato al recuperar un [conjunto de registros](./recordset-object-ado.md) como una cadena.  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adClipString**|2|Delimita filas por *RowDelimiter*, columnas por *ColumnDelimiter* y valores NULL por *NullExpr*. Estos tres parámetros del método [GetString](./getstring-method-ado.md) solo son válidos con un *StringFormat* de **adClipString**.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Paquete: **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums. StringFormat. CLIPSTRING|  
  
## <a name="applies-to"></a>Se aplica a  
 [GetString (método) (ADO)](./getstring-method-ado.md)