---
description: RecordOpenOptionsEnum
title: RecordOpenOptionsEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- RecordOpenOptionsEnum
helpviewer_keywords:
- RecordOpenOptionsEnum enumeration [ADO]
ms.assetid: 9028aba4-90fc-4dfc-88e4-fa8a7b6fedee
author: rothja
ms.author: jroth
ms.openlocfilehash: 087b8bc6540fdfb3e504273ed2e02b71163088fa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051826"
---
# <a name="recordopenoptionsenum"></a>RecordOpenOptionsEnum
Especifica las opciones para abrir un [registro](./record-object-ado.md). Estos valores se pueden combinar mediante o.  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adDelayFetchFields**|0x8000|Indica al proveedor que los campos asociados al **registro** no deben recuperarse inicialmente, pero se pueden recuperar en el primer intento de acceso al campo. El comportamiento predeterminado, indicado por la ausencia de esta marca, consiste en recuperar todos los campos del objeto de **registro** .|  
|**adDelayFetchStream**|0x4000|Indica al proveedor que no es necesario recuperar inicialmente la secuencia predeterminada asociada al **registro** . El comportamiento predeterminado, indicado por la ausencia de esta marca, es recuperar el flujo predeterminado asociado al objeto de **registro** .|  
|**adOpenAsync**|0x1000|Indica que el objeto de **registro** está abierto en modo asincrónico.|  
|**adOpenExecuteCommand**|0x10000|Indica que la cadena de origen contiene texto de comando que se debe ejecutar. Este valor es equivalente a la opción **adCmdText** en **Recordset. Open**.|  
|**adOpenRecordUnspecified**|-1|Predeterminada. Indica que no se especifican opciones.|  
|**adOpenOutput**|0x800000|Indica que si el origen apunta a un nodo que contiene un script ejecutable (como. Página ASP), el **registro** abierto contendrá los resultados del script ejecutado. Este valor solo es válido con registros que no son de colección.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Estas constantes no tienen equivalentes de ADO/WFC.  
  
## <a name="applies-to"></a>Se aplica a  
 [Open (método) (registro de ADO)](./open-method-ado-record.md)