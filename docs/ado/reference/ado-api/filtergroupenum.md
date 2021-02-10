---
description: FilterGroupEnum
title: FilterGroupEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- FilterGroupEnum
helpviewer_keywords:
- FilterGroupEnum enumeration [ADO]
ms.assetid: b22e725e-84bd-4286-a070-290c278c3783
author: rothja
ms.author: jroth
ms.openlocfilehash: 27b8b217287f3c623fc626de1cb46dcc2eed5394
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100024601"
---
# <a name="filtergroupenum"></a>FilterGroupEnum
Especifica el grupo de registros que se van a filtrar de un [conjunto de registros](./recordset-object-ado.md).  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adFilterAffectedRecords**|2|Filtros para ver solo los registros afectados por la llamada de última [eliminación](./delete-method-ado-recordset.md), [resincronización](./resync-method.md), [UpdateBatch](./updatebatch-method.md)o [CancelBatch](./cancelbatch-method-ado.md) .|  
|**adFilterConflictingRecords**|5|Filtros para ver los registros que dieron error en la última actualización por lotes.|  
|**adFilterFetchedRecords**|3|Filtra para ver los registros en la caché actual; es decir, los resultados de la última llamada para recuperar los registros de la base de datos.|  
|**adFilterNone**|0|Quita el filtro actual y restaura todos los registros para su visualización.|  
|**adFilterPendingRecords**|1|Filtros para ver solo los registros que han cambiado pero que todavía no se han enviado al servidor. Aplicable solo para el modo de actualización por lotes.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Paquete: **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums. FilterGroup. AFFECTEDRECORDS|  
|AdoEnums. FilterGroup. CONFLICTINGRECORDS|  
|AdoEnums. FilterGroup. FETCHEDRECORDS|  
|AdoEnums. FilterGroup. NONE|  
|AdoEnums. FilterGroup. PENDINGRECORDS|  
  
## <a name="applies-to"></a>Se aplica a  
 [Propiedad Filter](./filter-property.md)