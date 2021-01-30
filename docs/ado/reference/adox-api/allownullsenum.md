---
description: AllowNullsEnum
title: AllowNullsEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- AllowNullsEnum
helpviewer_keywords:
- AllowNullsEnum enumeration [ADOX]
ms.assetid: 6acf3689-1a7f-4379-9d7f-df452ccbac27
author: rothja
ms.author: jroth
ms.openlocfilehash: 872d3dfd6658a7190e3f18fcc23a1b7fb34da1f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164320"
---
# <a name="allownullsenum"></a>AllowNullsEnum
Especifica si los registros con valores NULL se indizan.  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adIndexNullsAllow**|0|El índice permite entradas en las que las columnas de clave sean null. Si se especifica un valor null en una columna de clave, la entrada se inserta en el índice.|  
|**adIndexNullsDisallow**|1|Predeterminada. El índice no permite entradas en las que las columnas de clave sean null. Si se especifica un valor null en una columna de clave, se producirá un error.|  
|**adIndexNullsIgnore**|2|El índice no inserta entradas que contengan claves nulas. Si se especifica un valor null en una columna de clave, la entrada se omite y no se produce ningún error.|  
|**adIndexNullsIgnoreAny**|4|El índice no inserta entradas en las que alguna columna de clave tenga un valor null. En el caso de un índice que tenga una clave de varias columnas, si se especifica un valor null en alguna columna, se omitirá la entrada y no se producirá ningún error.|  
  
## <a name="applies-to"></a>Se aplica a  
 [IndexNulls (propiedad, ADOX)](./indexnulls-property-adox.md)