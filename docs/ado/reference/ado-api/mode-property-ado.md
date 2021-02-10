---
description: Propiedad Mode (ADO)
title: Mode (propiedad) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::Mode
- _Stream::Mode
- _Record::Mode
helpviewer_keywords:
- Mode property [ADO]
ms.assetid: 808661eb-0d7c-4e6d-8e40-9dc3bef3d77a
author: rothja
ms.author: jroth
ms.openlocfilehash: ae638bd51ea34a81efa5fb3192592fa8f3a005c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041785"
---
# <a name="mode-property-ado"></a>Propiedad Mode (ADO)
Indica los permisos disponibles para modificar los datos de un objeto de [conexión](./connection-object-ado.md), [registro](./record-object-ado.md)o [secuencia](./stream-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor de [ConnectModeEnum](./connectmodeenum.md) . El valor predeterminado de una **conexión** es **adModeUnknown**. El valor predeterminado de un objeto de **registro** es **adModeRead**. El valor predeterminado de una **secuencia** asociada a un origen subyacente (abierto con una dirección URL como origen o como la **secuencia** predeterminada de un **registro**) es **adModeRead**. El valor predeterminado de una **secuencia** que no está asociada a un origen subyacente (con instancias en memoria) es **adModeUnknown**.  
  
## <a name="remarks"></a>Observaciones  
 Utilice la propiedad **mode** para establecer o devolver los permisos de acceso que utiliza el proveedor en la conexión actual. Solo puede establecer la propiedad **mode** cuando el objeto de **conexión** está cerrado.  
  
 Para un objeto de **secuencia** , si no se especifica el modo de acceso, se hereda del origen que se usa para abrir el objeto de **secuencia** . Por ejemplo, si se abre un **flujo** desde un objeto de **registro** , se abre de forma predeterminada en el mismo modo que el **registro**.  
  
 Esta propiedad es de lectura/escritura mientras el objeto está cerrado y es de solo lectura mientras el objeto está abierto.  
  
> [!NOTE]
>  **Uso del servicio de datos remotos** Cuando se usa en un objeto de **conexión** del lado cliente, la propiedad **mode** solo se puede establecer en **adModeUnknown**.  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Objeto de conexión (ADO)](./connection-object-ado.md)  
    :::column-end:::
    :::column:::
        [Objeto Record (ADO)](./record-object-ado.md)  
    :::column-end:::
    :::column:::
        [Objeto de secuencia (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades IsolationLevel y MODE (VB)](./isolationlevel-and-mode-properties-example-vb.md)   
 [Ejemplo de las propiedades IsolationLevel y MODE (VC + +)](./isolationlevel-and-mode-properties-example-vc.md)