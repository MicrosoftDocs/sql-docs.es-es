---
description: Eliminar registros mediante el método Delete
title: Eliminar registros mediante el método Delete | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO, deleting records
- deleting records [ADO]
- editing data [ADO], Delete method
- Delete method [ADO]
ms.assetid: bfed5cfa-7f57-463b-9da2-0c612a079d30
author: rothja
ms.author: jroth
ms.openlocfilehash: 43695feb469483bfe1b3a7d9e629adb7d2f7d4e5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033265"
---
# <a name="deleting-records-using-the-delete-method"></a>Eliminar registros mediante el método Delete
El uso del método **Delete** marca el registro actual o un grupo de registros en un objeto de **conjunto de registros** para su eliminación. Si el objeto de **conjunto de registros** no permite la eliminación de registros, se produce un error. Si está en modo de actualización inmediata, las eliminaciones se producen inmediatamente en la base de datos. Si un registro no se puede eliminar correctamente (debido a infracciones de la integridad de la base de datos, por ejemplo), el registro permanecerá en modo de edición después de la llamada a **Update.** Esto significa que debe cancelar la actualización mediante [CancelUpdate](../../reference/ado-api/cancelupdate-method-ado.md) antes de salir del registro actual (por ejemplo, con [Close](../../reference/ado-api/close-method-ado.md), [Move](../../reference/ado-api/move-method-ado.md)o [NextRecordset](../../reference/ado-api/nextrecordset-method-ado.md)).  
  
 Si está en modo de actualización por lotes, los registros se marcan para su eliminación de la memoria caché y la eliminación real se produce cuando se llama al método **UpdateBatch** . (Para ver los registros eliminados, establezca la propiedad **Filter** en **adFilterAffectedRecords** después de llamar a **Delete** ).  
  
 Al intentar recuperar valores de campo del registro eliminado, se genera un error. Después de eliminar el registro actual, el registro eliminado permanece activo hasta que se mueve a otro registro. Una vez que salga del registro eliminado, ya no estará accesible.  
  
 Si anida eliminaciones en una transacción, puede recuperar los registros eliminados mediante el método **RollbackTrans** . Si está en modo de actualización por lotes, puede cancelar una eliminación pendiente o un grupo de eliminaciones pendientes mediante el método **CancelBatch** .  
  
 Si se produce un error al intentar eliminar registros debido a un conflicto con los datos subyacentes (por ejemplo, otro usuario ya ha eliminado un registro), el proveedor devuelve advertencias a la colección de **errores** , pero no detiene la ejecución del programa. Solo se produce un error en tiempo de ejecución si hay conflictos en todos los registros solicitados.  
  
 Si se establece la propiedad dinámica de **tabla única** y el **conjunto de registros** es el resultado de ejecutar una operación de combinación en varias tablas, el método **Delete** solo eliminará las filas de la tabla mencionada en la propiedad de **tabla única** .  
  
 El método **Delete** toma un argumento opcional que le permite especificar qué registros se ven afectados por la operación de **eliminación** . Los únicos valores válidos para este argumento son cualquiera de las siguientes constantes enumeradas de **AffectEnum** de ADO:  
  
-   **adAffectCurrent** Afecta solo al registro actual.  
  
-   **adAffectGroup** Afecta únicamente a los registros que cumplen la configuración de propiedad de **filtro** actual. La propiedad **Filter** debe establecerse en un valor **FilterGroupEnum** o en una matriz de **marcadores** para usar esta opción.  
  
 En el código siguiente se muestra un ejemplo de cómo especificar **adAffectGroup** al llamar al método **Delete** . Este ejemplo agrega algunos registros al conjunto de **registros** de ejemplo y actualiza la base de datos. A continuación, filtra el **conjunto de registros** mediante la constante enumerada del filtro **adFilterAffectedRecords** , que deja solo los registros recién agregados visibles en el **conjunto de registros.** Finalmente, llama al método **Delete** y especifica que se deben eliminar todos los registros que cumplen la configuración actual de la propiedad de **filtro** (los nuevos registros).  
  
```  
'BeginDeleteGroup  
    'add some bogus records  
    With objRs  
        For i = 0 To 8  
            .AddNew  
            .Fields("CompanyName") = "Shipper Number " & i + 1  
            .Fields("Phone") = "(425) 555-000" & (i + 1)  
            .Update  
        Next i  
  
        ' update  
        .UpdateBatch  
  
        'filter on newly added records  
        .Filter = adFilterAffectedRecords  
        Debug.Print "Deleting the " & .RecordCount & _  
                    " records you just added."  
  
        'delete the newly added bogus records  
        .Delete adAffectGroup  
        .Filter = adFilterNone  
        Debug.Print .RecordCount & " records remain."  
    End With  
'EndDeleteGroup  
```