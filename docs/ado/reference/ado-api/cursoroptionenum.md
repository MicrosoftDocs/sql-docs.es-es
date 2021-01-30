---
description: CursorOptionEnum
title: CursorOptionEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CursorOptionEnum
helpviewer_keywords:
- CursorOptionEnum enumeration [ADO]
ms.assetid: 4e10cda7-ce81-4466-94c2-844d38191cf1
author: rothja
ms.author: jroth
ms.openlocfilehash: 00b50d99fa31e260118a93ce357c1afc4871e4ce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171357"
---
# <a name="cursoroptionenum"></a>CursorOptionEnum
Especifica qué funcionalidad debe probar el método [Supports](./supports-method.md) .  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adAddNew**|0x1000400|Admite el método [AddNew](./addnew-method-ado.md) para agregar nuevos registros.|  
|**adApproxPosition**|0x4000|Admite las propiedades [AbsolutePosition](./absoluteposition-property-ado.md) y [AbsolutePage](./absolutepage-property-ado.md) .|  
|**adBookmark**|0x2000|Admite la propiedad [Bookmark](./bookmark-property-ado.md) para obtener acceso a registros específicos.|  
|**adDelete**|0x1000800|Admite el método [Delete](./delete-method-ado-recordset.md) para eliminar registros.|  
|**adFind**|0x80000|Admite el método [Find](./find-method-ado.md) para buscar una fila en un [conjunto de registros](./recordset-object-ado.md).|  
|**adHoldRecords**|0x100|Recupera más registros o cambia la siguiente posición sin confirmar todos los cambios pendientes.|  
|**adIndex**|0x100000|Admite la propiedad de [Índice](./index-property.md) para asignar un nombre a un índice.|  
|**adMovePrevious**|0x200|Admite los métodos [MoveFirst](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md) y [MovePrevious](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md) , y los métodos [Move](./move-method-ado.md) o [GetRows](./getrows-method-ado.md) para mover la posición del registro actual hacia atrás sin necesidad de marcadores.|  
|**adNotify**|0x40000|Indica que el proveedor de datos subyacente admite notificaciones (que determina si se admiten eventos de **conjunto de registros** ).|  
|**adResync**|0x20000|Admite el método [Resync](./resync-method.md) para actualizar el cursor con los datos que están visibles en la base de datos subyacente.|  
|**adSeek**|0x200000|Admite el método [Seek](./seek-method.md) para buscar una fila en un **conjunto de registros**.|  
|**adUpdate**|0x1008000|Admite el método [Update](./update-method.md) para modificar los datos existentes.|  
|**adUpdateBatch**|0x10000|Admite la actualización por lotes (métodos[UpdateBatch](./updatebatch-method.md) y [CancelBatch](./cancelbatch-method-ado.md) ) para transmitir grupos de cambios al proveedor.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Paquete: **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums. CursorOption. ADDNEW|  
|AdoEnums. CursorOption. APPROXPOSITION|  
|AdoEnums. CursorOption. BOOKMARK|  
|AdoEnums. CursorOption. DELETE|  
|AdoEnums. CursorOption. FIND|  
|AdoEnums. CursorOption. HOLDRECORDS|  
|AdoEnums. CursorOption. INDEX|  
|AdoEnums. CursorOption. MOVEPREVIOUS|  
|AdoEnums. CursorOption. NOTIFY|  
|AdoEnums. CursorOption. Resync|  
|AdoEnums. CursorOption. SEEK|  
|AdoEnums. CursorOption. UPDATE|  
|AdoEnums. CursorOption. UPDATEBATCH|  
  
## <a name="applies-to"></a>Se aplica a  
 [Método Supports](./supports-method.md)