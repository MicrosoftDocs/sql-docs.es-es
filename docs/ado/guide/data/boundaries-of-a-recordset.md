---
description: Límites de un conjunto de registros
title: Límites de un conjunto de registros | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- EOF property [ADO], boundaries of a Recordset
- Recordset object [ADO], boundaries of a Recordset
- BOF property [ADO], boundaries of a Recordset
ms.assetid: c0dd4a0f-478d-4c5e-b5d5-7535f211d064
author: rothja
ms.author: jroth
ms.openlocfilehash: f737ad11af65045a1d923f427c7487e4b751aaa8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028020"
---
# <a name="boundaries-of-a-recordset"></a>Límites de un conjunto de registros
**Recordset** admite las propiedades **BOF** y **EOF** para delimitar el principio y el final, respectivamente, del conjunto de registros. Puede pensar en **BOF** y **EOF** como registros "fantasma" que se colocan al principio y al final del **conjunto de registros**. Contando **BOF** y **EOF**, nuestro **conjunto de registros** de ejemplo tendría el siguiente aspecto:  
  
|ProductID|ProductName|UnitPrice|  
|---------------|-----------------|---------------|  
|BOF|||  
|7|Peras secas orgánicas|30,0000|  
|14|Tofu|23,2500|  
|28|Rssle Sauerkraut|45,6000|  
|51|Manzanas secas Manjimup|53,0000|  
|74|Longlife Tofu|10,0000|  
|EOF|||  
  
 Cuando un cursor se desplaza más allá del último registro, **EOF** se establece en **true**; de lo contrario, su valor es **false**. Del mismo modo, cuando el cursor se mueve antes del primer registro, **BOF** se establece en **true**; de lo contrario, su valor es **false**. Estas propiedades se utilizan normalmente para enumerar los registros del conjunto de elementos, como se muestra en el siguiente fragmento de código JScript.  
  
```  
while (objRecordset.EOF != true)   
{  
   // Work on the current record.  
   ...  
   // Advance the cursor forward to the next record.  
   objRecordset.MoveNext();  
}  
or  
while (objRecordset.BOF != true)   
{  
   // Work on the current record.  
   ...  
   // Move the cursor to the previous record.  
   objRecordset.MovePrevious();  
}  
```  
  
 Si **BOF** y **EOF** son **true**, el objeto de **conjunto de registros** está vacío. Ambas propiedades serán **false** para un objeto **Recordset** no vacío recién abierto. Puede usar las propiedades **BOF** y **EOF** conjuntamente para determinar si un objeto de **conjunto de registros** está vacío o no, tal como se muestra en el siguiente fragmento de código JScript.  
  
```  
if (objRecordset.EOF == true && objRecordset.BOF == true)  
{  
   WScript.Echo("we got an empty dataset.");  
}  
else  
{  
   WScript.Echo("we got a full dataset.");  
}  
```  
  
 Este esquema funciona con todos los tipos de cursor y es independiente de los proveedores subyacentes. Si intenta determinar el Emptiness de un objeto de **conjunto de registros** comprobando si el valor de la propiedad **RecordCount** es cero (0) o no, debe tomar precauciones para usar un cursor y un proveedor adecuados que admitan la devolución del número de registros en el resultado.  
  
 Si elimina el último registro restante en el objeto de **conjunto de registros** , el cursor se queda en un estado indeterminado. Es posible que las propiedades **BOF** y **EOF** sigan siendo **false** hasta que intente volver a colocar el registro actual, dependiendo del proveedor. Para obtener más información, vea [eliminar registros con el método Delete](./deleting-records-using-the-delete-method.md).