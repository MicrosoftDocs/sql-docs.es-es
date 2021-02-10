---
description: Procesar transacciones
title: Procesamiento de transacciones | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- transactions [ADO]
- data updates [ADO], transaction processing
- updating data [ADO], transaction processing
- nested transactions [ADO]
ms.assetid: 74ab6706-e2dc-42cb-af77-dbc58a9cf4ce
author: rothja
ms.author: jroth
ms.openlocfilehash: f534d38ea52e942a00e2e95f837b096ed6fef0f1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032387"
---
# <a name="transaction-processing"></a>Procesamiento de transacciones
Una *transacción* delimita el principio y el final de una serie de operaciones de acceso a datos ejecutadas a través de una conexión. En función de las capacidades transaccionales del origen de datos, el objeto de **conexión** también le permite crear y administrar transacciones. Por ejemplo, al usar el proveedor de OLE DB de Microsoft para SQL Server para tener acceso a una base de datos en Microsoft SQL Server, puede crear varias transacciones anidadas para los comandos que se ejecutan.  
  
 ADO garantiza que los cambios en un origen de datos que resultan de las operaciones de una transacción se producen correctamente o no.  
  
 Si cancela la transacción, o si se produce un error en una de sus operaciones, el resultado será como si no se hubiera producido ninguna de las operaciones de la transacción. El origen de datos permanecerá como estaba antes de que comenzara la transacción.  
  
 ADO proporciona los siguientes métodos para controlar las transacciones: **BeginTrans**, **CommitTrans** y **RollbackTrans**. Utilice estos métodos con un objeto de **conexión** si desea guardar o cancelar una serie de cambios realizados en los datos de origen como una sola unidad. Por ejemplo, para transferir el dinero entre cuentas, se resta una cantidad de una y se agrega la misma cantidad al otro. Si se produce un error en alguna de las actualizaciones, las cuentas ya no se equilibran. Al realizar estos cambios dentro de una transacción abierta, se garantiza que todos los cambios o ninguno de ellos pasen.  
  
> [!NOTE]
>  No todos los proveedores admiten transacciones. Compruebe que la propiedad definida por el proveedor "**Transaction DDL**" aparece en la colección de [propiedades](../../../ado/reference/ado-api/properties-collection-ado.md) del objeto de **conexión** , lo que indica que el proveedor admite transacciones. Si el proveedor no admite transacciones, al llamar a uno de estos métodos se devolverá un error.  
  
 Después de llamar al método **BeginTrans** , el proveedor ya no confirmará de forma instantánea los cambios que realice hasta que llame a **CommitTrans** o a **RollbackTrans** para finalizar la transacción.  
  
 La llamada al método **CommitTrans** guarda los cambios realizados en una transacción abierta en la conexión y finaliza la transacción. La llamada al método **RollbackTrans** invierte los cambios realizados en una transacción abierta y finaliza la transacción. Si se llama a cualquier método cuando no hay ninguna transacción abierta, se genera un error.  
  
 Dependiendo de la propiedad [attributes](../../../ado/reference/ado-api/attributes-property-ado.md) del objeto **Connection** , llamar a los métodos **CommitTrans** o **RollbackTrans** puede iniciar automáticamente una nueva transacción. Si la propiedad **attributes** está establecida en **adXactCommitRetaining**, el proveedor inicia automáticamente una nueva transacción después de una llamada a **CommitTrans** . Si la propiedad **attributes** está establecida en **adXactAbortRetaining**, el proveedor inicia automáticamente una nueva transacción después de una llamada a **RollbackTrans** .  
  
## <a name="transaction-isolation-level"></a>Nivel de aislamiento de transacción  
 Utilice la propiedad **IsolationLevel** para establecer el nivel de aislamiento de una transacción en un objeto de **conexión** . La configuración no surte efecto hasta la próxima vez que se llame al método [BeginTrans](../../../ado/reference/ado-api/begintrans-committrans-and-rollbacktrans-methods-ado.md) . Si el nivel de aislamiento solicitado no está disponible, el proveedor puede devolver el siguiente nivel mayor de aislamiento. Consulte la propiedad **IsolationLevel** en la referencia del programador de ADO para obtener más detalles sobre los valores válidos.  
  
## <a name="nested-transactions"></a>Transacciones anidadas  
 Para los proveedores que admiten transacciones anidadas, al llamar al método **BeginTrans** dentro de una transacción abierta se inicia una nueva transacción anidada. El valor devuelto indica el nivel de anidamiento: un valor devuelto de "1" indica que ha abierto una transacción de nivel superior (es decir, que la transacción no está anidada dentro de otra transacción), "2" indica que ha abierto una transacción de segundo nivel (una transacción anidada dentro de una transacción de nivel superior), etc. La llamada a **CommitTrans** o a **RollbackTrans** solo afecta a la transacción abierta más recientemente; debe cerrar o revertir la transacción actual antes de poder resolver las transacciones de nivel superior.
