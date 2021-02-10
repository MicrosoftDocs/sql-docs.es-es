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
ms.openlocfilehash: db02c56f55b577c9f74a42f43abc9a1957311c0d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040755"
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