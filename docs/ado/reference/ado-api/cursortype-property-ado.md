---
description: Propiedad CursorType (ADO)
title: Propiedad CursorType (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::CursorType
helpviewer_keywords:
- CursorType property [ADO]
ms.assetid: b62c66ca-58d5-430e-9257-eb38c65e48c2
author: rothja
ms.author: jroth
ms.openlocfilehash: 7dc8d34ccc631d9ecea19fdfceb36743adbaa5e0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034455"
---
# <a name="cursortype-property-ado"></a>Propiedad CursorType (ADO)
Indica el tipo de cursor utilizado en un objeto de [conjunto de registros](./recordset-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor de [CursorTypeEnum](./cursortypeenum.md) . El valor predeterminado es **adOpenForwardOnly**.  
  
## <a name="remarks"></a>Observaciones  
 Utilice la propiedad **CursorType** para especificar el tipo de cursor que se debe utilizar al abrir el objeto de **conjunto de registros** .  
  
 Solo se admite un valor de **adOpenStatic** si la propiedad [CursorLocation](./cursorlocation-property-ado.md) está establecida en **adUseClient**. Si se establece un valor no compatible, no se producirá ningún error. en su lugar, se utilizará el **CursorType** compatible más cercano.  
  
 Si un proveedor no admite el tipo de cursor solicitado, puede devolver otro tipo de cursor. La propiedad **CursorType** cambiará para coincidir con el tipo de cursor real en uso cuando el objeto de [conjunto de registros](./recordset-object-ado.md) esté abierto. Para comprobar la funcionalidad específica del cursor devuelto, use el método [Supports](./supports-method.md) . Después de cerrar el **conjunto de registros**, la propiedad **CursorType** vuelve a su configuración original.  
  
 En el gráfico siguiente se muestra la funcionalidad del proveedor (identificada por **admite** constantes de método) que se requiere para cada tipo de cursor.  
  
|Para un conjunto de registros de este CursorType|El método Supports debe devolver true para todas estas constantes|  
|----------------------------------------|---------------------------------------------------------------------|  
|**adOpenForwardOnly**|ninguno|  
|**adOpenKeyset**|**adBookmark**, **adHoldRecords**, **adMovePrevious**, **adResync**|  
|**adOpenDynamic**|**adMovePrevious**|  
|**adOpenStatic**|**adBookmark**, **adHoldRecords**, **adMovePrevious**, **adResync**|  
  
> [!NOTE]
>  Aunque **admite**(**adUpdateBatch**) puede ser true para los cursores dinámicos y de solo avance, para las actualizaciones por lotes debe utilizar un cursor Keyset o static. Establezca la propiedad [LockType](./locktype-property-ado.md) en **adLockBatchOptimistic** y la propiedad **CursorLocation** en **adUseClient** para habilitar el servicio de cursor para OLE DB, que es necesario para las actualizaciones por lotes.  
  
 La propiedad **CursorType** es de lectura y escritura cuando el **conjunto de registros** está cerrado y es de solo lectura cuando está abierto.  
  
> [!NOTE]
>  **Uso del servicio de datos remotos** Cuando se usa en un objeto de **conjunto de registros** del lado cliente, la propiedad **CursorType** solo se puede establecer en **adOpenStatic**.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades CursorType, LockType y EditMode (VB)](./cursortype-locktype-and-editmode-properties-example-vb.md)   
 [Ejemplo de las propiedades CursorType, LockType y EditMode (VC + +)](./cursortype-locktype-and-editmode-properties-example-vc.md)   
 [Método Supports](./supports-method.md)