---
description: StreamWriteEnum
title: StreamWriteEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- StreamWriteEnum
helpviewer_keywords:
- StreamWriteEnum enumeration [ADO]
ms.assetid: bdbf3405-a0bd-4f02-85d4-e3fe8da3f3f7
author: rothja
ms.author: jroth
ms.openlocfilehash: f0f102ec1457b0cb327ab563ee348cf4112c749a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056550"
---
# <a name="streamwriteenum"></a>StreamWriteEnum
Especifica si un separador de línea se anexa a la cadena escrita en un objeto de [secuencia](./stream-object-ado.md) .  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adWriteChar**|0|Predeterminada. Escribe la cadena de texto especificada (especificada por el parámetro de *datos* ) en el objeto de **secuencia** .|  
|**adWriteLine**|1|Escribe una cadena de texto y un carácter separador de líneas en un objeto de **secuencia** . Si no se define la propiedad [LineSeparator](./lineseparator-property-ado.md) , se devuelve un error en tiempo de ejecución.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Estas constantes no tienen equivalentes de ADO/WFC.  
  
## <a name="applies-to"></a>Se aplica a  
 [Método WriteText](./writetext-method.md)