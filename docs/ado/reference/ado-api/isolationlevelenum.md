---
description: IsolationLevelEnum
title: IsolationLevelEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- IsolationLevelEnum
helpviewer_keywords:
- IsolationLevelEnum enumeration [ADO]
ms.assetid: 8e17a7bc-b8a3-4ae2-b6c9-ce088ad31fdf
author: rothja
ms.author: jroth
ms.openlocfilehash: 780e61f5aabea993f3af4d36b9547cca75377d99
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167197"
---
# <a name="isolationlevelenum"></a>IsolationLevelEnum
Especifica el nivel de aislamiento de transacción para un objeto de [conexión](./connection-object-ado.md) .  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adXactUnspecified**|-1|Indica que el proveedor está utilizando un nivel de aislamiento diferente al especificado, pero que no se puede determinar el nivel.|  
|**adXactChaos**|16|Indica que no se pueden sobrescribir los cambios pendientes de transacciones más aisladas.|  
|**adXactBrowse**|256|Indica que desde una transacción puede ver los cambios sin confirmar en otras transacciones.|  
|**adXactReadUncommitted**|256|Igual que **adXactBrowse**.|  
|**adXactCursorStability**|4096|Indica que desde una transacción puede ver los cambios en otras transacciones solo después de que se hayan confirmado.|  
|**adXactReadCommitted**|4096|Igual que **adXactCursorStability**.|  
|**adXactRepeatableRead**|65536|Indica que desde una transacción no se pueden ver los cambios realizados en otras transacciones, pero que la nueva consulta puede recuperar nuevos objetos de **conjunto de registros** .|  
|**adXactIsolated**|1 048 576|Indica que las transacciones se realizan aisladas de otras transacciones.|  
|**adXactSerializable**|1 048 576|Igual que **adXactIsolated**.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Paquete: **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums. IsolationLevel. no especificado|  
|AdoEnums. IsolationLevel. caos|  
|AdoEnums. IsolationLevel. BROWSE|  
|AdoEnums. IsolationLevel. READUNCOMMITTED|  
|AdoEnums. IsolationLevel. CURSORSTABILITY|  
|AdoEnums. IsolationLevel. READCOMMITTED|  
|AdoEnums. IsolationLevel. REPEATABLEREAD|  
|AdoEnums. IsolationLevel. Isolated|  
|AdoEnums. IsolationLevel. SERIALIZABLE|  
  
## <a name="applies-to"></a>Se aplica a  
 [Propiedad IsolationLevel](./isolationlevel-property.md)