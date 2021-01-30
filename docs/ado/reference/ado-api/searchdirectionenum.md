---
description: SearchDirectionEnum
title: SearchDirectionEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- SearchDirectionEnum
helpviewer_keywords:
- SearchDirectionEnum enumeration [ADO]
ms.assetid: 81272ae3-2165-4f4e-adfe-9ede0368cb17
author: rothja
ms.author: jroth
ms.openlocfilehash: 957e84b3a98f0248e41a83301ef2c9a9c1943b11
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166573"
---
# <a name="searchdirectionenum"></a>SearchDirectionEnum
Especifica la dirección de una búsqueda de registros dentro de un [conjunto de registros](./recordset-object-ado.md).  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adSearchBackward**|-1|Busca hacia atrás y se detiene al principio del **conjunto de registros**. Si no se encuentra ninguna coincidencia, el puntero de registro se coloca en [BOF](./bof-eof-properties-ado.md).|  
|**adSearchForward**|1|Busca hacia delante y se detiene al final del **conjunto de registros**. Si no se encuentra ninguna coincidencia, el puntero de registro se coloca en [EOF](./bof-eof-properties-ado.md).|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Paquete: **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums. SearchDirection. BACKWARD|  
|AdoEnums. SearchDirection. FORWARD|  
  
## <a name="applies-to"></a>Se aplica a  
 [Find (método) (ADO)](./find-method-ado.md)