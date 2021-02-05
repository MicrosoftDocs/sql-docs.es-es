---
description: Método compareTo (DateTimeOffset)
title: Método compareTo (DateTimeOffset) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: e4cf2ea4-0fe9-40ce-ba79-f2a2b616997e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 02e36e2e64a6a628fd088fab448b30ea0c238450
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176242"
---
# <a name="compareto-method-datetimeoffset"></a>Método compareTo (DateTimeOffset)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Compara este objeto **DateTimeOffset** con otro objeto **DateTimeOffset** según la hora en GMT.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public int compareTo(DateTimeOffset other)  
```  
  
#### <a name="parameters"></a>Parámetros  
 Un valor [DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md) que desea comparar con la instancia actual.  
  
## <a name="return-value"></a>Valor devuelto  
 La tabla siguiente describe los valores devueltos de este método:  
  
|Valor devuelto|Descripción|  
|------------------|-----------------|  
|0|Ambos objetos **DateTimeOffset** representan el mismo punto cronológico.|  
|número negativo|Este objeto **DateTimeOffset** representa un punto cronológico anterior a *other*.|  
|número positivo|Este objeto **DateTimeOffset** representa un punto cronológico posterior a *other*.|  
  
## <a name="remarks"></a>Observaciones  
 Cuando dos objetos **DateTimeOffset** tienen la misma hora de GMT, no se produce ninguna ordenación adicional de los objetos basados en el desplazamiento.  
  
## <a name="see-also"></a>Consulte también  
 [Clase DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md)   
 [Miembros de DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-members.md)  
  
  
