---
description: Requery (método)
title: Requery (método) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::Requery
- Recordset15::raw_Requery
helpviewer_keywords:
- Requery method [ADO]
ms.assetid: d81ab76f-1aa8-4ccf-92ec-b65254dc3ea1
author: rothja
ms.author: jroth
ms.openlocfilehash: 060c1c7f792a0cf74e9ca8192167c41fcb7b9399
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051626"
---
# <a name="requery-method"></a>Requery (método)
Actualiza los datos de un objeto de [conjunto de registros](./recordset-object-ado.md) volviendo a ejecutar la consulta en la que se basa el objeto.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
recordset.Requery Options  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Opciones*  
 Opcional. Máscara de máscara que contiene valores de [ExecuteOptionEnum](./executeoptionenum.md) y [CommandTypeEnum](./commandtypeenum.md) que afectan a esta operación.  
  
> [!NOTE]
>  Si *Options* se establece en **adAsyncExecute**, esta operación se ejecutará de forma asincrónica y se emitirá un evento [RecordsetChangeComplete](./willchangerecordset-and-recordsetchangecomplete-events-ado.md) cuando finalice. Los valores **ExecuteOpenEnum** de **adExecuteNoRecords** o **adExecuteStream** no se deben usar con **Requery**.  
  
## <a name="remarks"></a>Observaciones  
 Use el método **Requery** para actualizar todo el contenido de un objeto de **conjunto de registros** desde el origen de datos, para lo que debe volver a emitir el comando original y recuperar los datos por segunda vez. Llamar a este método es equivalente a llamar a los métodos [Close](./close-method-ado.md) y [Open](./open-method-ado-recordset.md) en sucesión. Si está editando el registro actual o agregando un nuevo registro, se produce un error.  
  
 Mientras el objeto de **conjunto de registros** está abierto, las propiedades que definen la naturaleza del cursor ([CursorType](./cursortype-property-ado.md), [LockType](./locktype-property-ado.md), [MaxRecords](./maxrecords-property-ado.md), etc.) son de solo lectura. Por lo tanto, el método **Requery** solo puede actualizar el cursor actual. Para cambiar cualquiera de las propiedades del cursor y ver los resultados, debe utilizar el método [Close](./close-method-ado.md) para que las propiedades se vuelvan a leer y escribir. Después, puede cambiar la configuración de las propiedades y llamar al método [Open](./open-method-ado-recordset.md) para volver a abrir el cursor.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de los métodos Execute, Requery y Clear (VB)](./execute-requery-and-clear-methods-example-vb.md)   
 [Ejemplo de métodos Execute, Requery y Clear (VBScript)](./execute-requery-and-clear-methods-example-vbscript.md)   
 [Ejemplo de los métodos Execute, Requery y Clear (VC + +)](./execute-requery-and-clear-methods-example-vc.md)   
 [Propiedad CommandText (ADO)](./commandtext-property-ado.md)