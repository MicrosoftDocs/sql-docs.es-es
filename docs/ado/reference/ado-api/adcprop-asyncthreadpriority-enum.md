---
description: ADCPROP_ASYNCTHREADPRIORITY_ENUM
title: ADCPROP_ASYNCTHREADPRIORITY_ENUM | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADCPROP_ASYNCTHREADPRIORITY_ENUM
helpviewer_keywords:
- ADCPROP_ASYNCTHREADPRIORITY_ENUM [ADO]
ms.assetid: f0965617-17d8-41e0-98d0-f824274735a6
author: rothja
ms.author: jroth
ms.openlocfilehash: a0e9a8fff8888268587b1a2d88fa64ee5a66dba6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161778"
---
# <a name="adcprop_asyncthreadpriority_enum"></a>ADCPROP_ASYNCTHREADPRIORITY_ENUM
Para un objeto de [conjunto de registros](./recordset-object-ado.md) RDS, especifica la prioridad de ejecución del subproceso asincrónico que recupera los datos.  
  
 Use estas constantes con la propiedad dinámica "**prioridad del subproceso en segundo plano**" del **conjunto de registros** , a la que se hace referencia en el índice de propiedades dinámicas de ADO a OLE DB y que se documenta en el [servicio de cursores de Microsoft para obtener OLE DB](../../guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md) documentación.  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adPriorityAboveNormal**|4|Establece la prioridad entre normal y superior.|  
|**adPriorityBelowNormal**|2|Establece la prioridad entre el nivel más bajo y el normal.|  
|**adPriorityHighest**|5|Establece la prioridad del valor más alto posible.|  
|**AdPriorityLowest**|1|Establece la prioridad de la más baja posible.|  
|**adPriorityNormal**|3|Establece la prioridad en normal.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Paquete: **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums. AdcPropAsyncThreadPriority. ABOVENORMAL|  
|AdoEnums. AdcPropAsyncThreadPriority. BELOWNORMAL|  
|AdoEnums. AdcPropAsyncThreadPriority. HIGHEST|  
|AdoEnums. AdcPropAsyncThreadPriority. más bajo|  
|AdoEnums. AdcPropAsyncThreadPriority. NORMAL|