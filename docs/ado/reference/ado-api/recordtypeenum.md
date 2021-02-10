---
description: RecordTypeEnum
title: RecordTypeEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- RecordTypeEnum
helpviewer_keywords:
- RecordTypeEnum enumeration [ADO]
ms.assetid: f557e537-015d-4ba7-8a41-a6f00b366a91
author: rothja
ms.author: jroth
ms.openlocfilehash: 5ddaae2256f595abe3778477e020d8f6774aacec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051696"
---
# <a name="recordtypeenum"></a>RecordTypeEnum
Especifica el tipo de objeto de [registro](./record-object-ado.md) .  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adSimpleRecord**|0|Indica un registro *simple* (no contiene nodos secundarios).|  
|**adCollectionRecord**|1|Indica un registro de *colección* (contiene nodos secundarios).|  
|**adRecordUnknown**|-1|Indica que se desconoce el tipo de este **registro** .|  
|**adStructDoc**|2|Indica un tipo especial de registro de *colección* que representa documentos estructurados com.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Estas constantes no tienen equivalentes de ADO/WFC.  
  
## <a name="applies-to"></a>Se aplica a  
 [Propiedad RecordType (ADO)](./recordtype-property-ado.md)