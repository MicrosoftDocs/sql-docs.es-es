---
description: AffectEnum
title: AffectEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- AffectEnum
helpviewer_keywords:
- AffectEnum enumeration [ADO]
ms.assetid: 1ab921a0-6c57-43b4-9291-701b2599f3e8
author: rothja
ms.author: jroth
ms.openlocfilehash: 0a4f722e1ac4557ff5e2fb89d63c3d40f490c5b7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164861"
---
# <a name="affectenum"></a>AffectEnum
Especifica qué registros se ven afectados por una operación.  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adAffectAll**|3|Si no hay ningún [filtro](./filter-property.md) aplicado al conjunto de **registros**, afecta a todos los registros.<br /><br /> Si la propiedad **Filter** está establecida en un criterio de cadena (por ejemplo, "author = ' Smith '"), la operación afectará a los registros visibles en el capítulo actual.<br /><br /> Si la propiedad **Filter** está establecida en un miembro de [FilterGroupEnum](./filtergroupenum.md) o una matriz de marcadores, la operación afectará a todas las filas del conjunto de **registros**. **Nota: adAffectAll** está oculto en el examinador de objetos Visual Basic.|  
|**adAffectAllChapters**|4|Afecta a todos los registros de todos los capítulos del mismo nivel del **conjunto de registros**, incluidos los que no están visibles a través de cualquier **filtro** aplicado actualmente.|  
|**adAffectCurrent**|1|Afecta solo al registro actual.|  
|**adAffectGroup**|2|Afecta únicamente a los registros que cumplen la configuración de propiedad de [filtro](./filter-property.md) actual. Debe establecer la propiedad **Filter** en un valor **FilterGroupEnum** o una matriz de **marcadores** para usar esta opción.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Paquete: **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums. Reverse. ALL|  
|AdoEnums. afecte a. ALLCHAPTERS|  
|AdoEnums. afectar a. CURRENT|  
|AdoEnums. afectar al grupo|  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Método CancelBatch (ADO)](./cancelbatch-method-ado.md)  
        [Delete (método) (conjunto de registros ADO)](./delete-method-ado-recordset.md)  
    :::column-end:::
    :::column:::
        [Método Resync](./resync-method.md)  
        [Método UpdateBatch](./updatebatch-method.md)  
    :::column-end:::
:::row-end:::