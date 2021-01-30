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
ms.openlocfilehash: 8b28b3f23d1df661e5a142d85c3a066acfbf2f50
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171042"
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