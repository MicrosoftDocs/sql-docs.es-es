---
description: Propiedad Source (ADO Recordset)
title: Propiedad Source (conjunto de registros ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::putref_Source
- Recordset15::Source
- Recordset15::PutSource
- Recordset15::get_Source
- Recordset15::GetSource
- Recordset15::PutRefSource
- Recordset15::put_Source
helpviewer_keywords:
- Source property [ADO Recordset]
ms.assetid: a05ba2c9-2821-4343-8607-4de9b764ec91
author: rothja
ms.author: jroth
ms.openlocfilehash: 04287b1fca3b3716302ab440f8f5213cff3f6302
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051376"
---
# <a name="source-property-ado-recordset"></a>Propiedad Source (ADO Recordset)
Indica el origen de datos de un objeto de [conjunto de registros](./recordset-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece un valor de **cadena** o una referencia de objeto de [comando](./command-object-ado.md) ; Devuelve solo un valor de **cadena** que indica el origen del **conjunto de registros**.  
  
## <a name="remarks"></a>Observaciones  
 Utilice la propiedad **origen** para especificar un origen de datos para un objeto de **conjunto de registros** mediante una de las siguientes opciones: una variable de objeto de **comando** , una instrucción SQL, un procedimiento almacenado o un nombre de tabla.  
  
 Si establece la propiedad de **origen** en un objeto de **comando** , la propiedad [ActiveConnection](./activeconnection-property-ado.md) del objeto de **conjunto de registros** heredará el valor de la propiedad **ActiveConnection** del objeto de **comando** especificado. Sin embargo, la lectura de la propiedad **source** no devuelve un objeto **Command** . en su lugar, devuelve la propiedad [CommandText](./commandtext-property-ado.md) del objeto **Command** en el que se establece la propiedad **source** .  
  
 Si la propiedad **source** es una instrucción SQL, un procedimiento almacenado o un nombre de tabla, puede optimizar el rendimiento pasando el argumento *Options* adecuado con la llamada al método [Open](./open-method-ado-recordset.md) .  
  
 La propiedad de **origen** es de lectura/escritura para los objetos de **conjunto de registros** cerrados y de solo lectura para los objetos de **conjunto de registros** abiertos.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de la propiedad Source (VB)](./source-property-example-vb.md)   
 [Source (propiedad, error de ADO)](./source-property-ado-error.md)   
 [Propiedad Source (Record ADO)](./source-property-ado-record.md)