---
description: ConnectModeEnum
title: ConnectModeEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ConnectModeEnum
helpviewer_keywords:
- ConnectModeEnum enumeration [ADO]
ms.assetid: 3792c294-5161-4538-a908-22a5fc50b85f
author: rothja
ms.author: jroth
ms.openlocfilehash: 5232779e4756e8130ad5b7b5a4058e2bed96be1b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034635"
---
# <a name="connectmodeenum"></a>ConnectModeEnum
Especifica los permisos disponibles para modificar los datos de una [conexión](./connection-object-ado.md), abrir un [registro](./record-object-ado.md)o especificar valores para la propiedad [mode](./mode-property-ado.md) de los objetos **Record** y [Stream](./stream-object-ado.md) .  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adModeRead**|1|Indica permisos de solo lectura.|  
|**adModeReadWrite**|3|Indica permisos de lectura y escritura.|  
|**adModeRecursive**|0x400000|Se usa junto con los demás valores de *\* ShareDeny \** (**adModeShareDenyNone**, **adModeShareDenyWrite** o **adModeShareDenyRead**) para propagar las restricciones de uso compartido a todos los subregistros del **registro** actual. No tiene ningún efecto si el **registro** no tiene ningún elemento secundario. Si solo se usa con **adModeShareDenyNone** , se genera un error en tiempo de ejecución. Sin embargo, se puede usar con **adModeShareDenyNone** cuando se combina con otros valores. Por ejemplo, puede usar "**adModeRead** o **adModeShareDenyNone** o **adModeRecursive**".|  
|**adModeShareDenyNone**|16|Permite que otros usuarios abran una conexión con cualquier permiso. No se puede negar a los demás el acceso de lectura ni el de escritura.|  
|**adModeShareDenyRead**|4|Impide que otros abran una conexión con permisos de lectura.|  
|**adModeShareDenyWrite**|8|Impide que otros abran una conexión con permisos de escritura.|  
|**adModeShareExclusive**|12|Impide que otros abran una conexión.|  
|**adModeUnknown**|0|Predeterminada. Indica que los permisos todavía no se han establecido o no se pueden determinar.|  
|**adModeWrite**|2|Indica permisos de solo escritura.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Paquete: **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums. ConnectMode. READ|  
|AdoEnums. ConnectMode. READWRITE|  
|(No hay ningún equivalente de AdoEnums. ConnectMode. RECURSIVE)|  
|AdoEnums. ConnectMode. SHAREDENYNONE|  
|AdoEnums. ConnectMode. SHAREDENYREAD|  
|AdoEnums. ConnectMode. SHAREDENYWRITE|  
|AdoEnums. ConnectMode. SHAREEXCLUSIVE|  
|AdoEnums. ConnectMode. UNKNOWN|  
|AdoEnums. ConnectMode. WRITE|  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Propiedad Mode (ADO)](./mode-property-ado.md)  
        [Open (método) (registro de ADO)](./open-method-ado-record.md)  
    :::column-end:::
    :::column:::
        [Open (método) (Stream de ADO)](./open-method-ado-stream.md)  
        [Objeto de secuencia (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::