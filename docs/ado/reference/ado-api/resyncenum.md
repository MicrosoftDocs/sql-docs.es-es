---
description: ResyncEnum
title: ResyncEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ResyncEnum
helpviewer_keywords:
- ResyncEnum enumeration [ADO]
ms.assetid: d3df2c90-e570-4c40-a79a-25b3448a009c
author: rothja
ms.author: jroth
ms.openlocfilehash: c20965a348227583580bb75a807e792f74a9e967
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166626"
---
# <a name="resyncenum"></a>ResyncEnum
Especifica si se sobrescriben los valores subyacentes mediante una llamada a [Resync](./resync-method.md).  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adResyncAllValues**|2|Predeterminada. Sobrescribe los datos y las actualizaciones pendientes se cancelan.|  
|**adResyncUnderlyingValues**|1|No sobrescribe los datos y las actualizaciones pendientes no se cancelan.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Paquete: **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums. Resync. ALLVALUES|  
|AdoEnums. Resync. UNDERLYINGVALUES|  
  
## <a name="applies-to"></a>Se aplica a  
 [Método Resync](./resync-method.md)