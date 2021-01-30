---
description: Métodos BeginTrans, CommitTrans y RollbackTrans (ADO)
title: Métodos BeginTrans, CommitTrans y RollbackTrans (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::raw_RollbackTrans
- Connection15::CommitTrans
- Connection15::raw_CommitTrans
- Connection15::raw_BeginTrans
- Connection15::BeginTrans
- Connection15::RollbackTrans
helpviewer_keywords:
- BeginTrans method [ADO]
- CommitTrans method [ADO]
- RollbackTrans method [ADO]
ms.assetid: d4683472-4120-4236-8640-fa9ae289e23e
author: rothja
ms.author: jroth
ms.openlocfilehash: de30ee4629d66371e41180e0caf171a91237d65c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167862"
---
# <a name="begintrans-committrans-and-rollbacktrans-methods-ado"></a>Métodos BeginTrans, CommitTrans y RollbackTrans (ADO)
Estos métodos de transacción administran el procesamiento de transacciones dentro de un objeto de [conexión](./connection-object-ado.md) de la manera siguiente:  
  
-   **BeginTrans** Inicia una nueva transacción.  
  
-   **CommitTrans** Guarda los cambios y finaliza la transacción actual. También puede iniciar una nueva transacción.  
  
-   **RollbackTrans** Cancela los cambios realizados durante la transacción actual y finaliza la transacción. También puede iniciar una nueva transacción.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
level = object.BeginTrans()  
object.BeginTrans  
object.CommitTrans  
object.RollbackTrans  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Se puede llamar a **BeginTrans** como una función que devuelve una variable de **tipo Long** que indica el nivel de anidamiento de la transacción.  
  
#### <a name="parameters"></a>Parámetros  
 *object*  
 Objeto de **conexión** .  
  
## <a name="connection"></a>Conexión  
 Utilice estos métodos con un objeto de **conexión** si desea guardar o cancelar una serie de cambios realizados en los datos de origen como una sola unidad. Por ejemplo, para transferir el dinero entre cuentas, se resta una cantidad de una y se agrega la misma cantidad al otro. Si se produce un error en alguna de las actualizaciones, las cuentas ya no se equilibran. Al realizar estos cambios dentro de una transacción abierta, se garantiza que todos los cambios o ninguno de ellos pasen.  
  
> [!NOTE]
>  No todos los proveedores admiten transacciones. Compruebe que la propiedad definida por el proveedor "**Transaction DDL**" aparece en la colección de [propiedades](./properties-collection-ado.md) del objeto de **conexión** , lo que indica que el proveedor admite transacciones. Si el proveedor no admite transacciones, al llamar a uno de estos métodos se devolverá un error.  
  
 Después de llamar al método **BeginTrans** , el proveedor ya no confirmará de forma instantánea los cambios que realice hasta que llame a **CommitTrans** o a **RollbackTrans** para finalizar la transacción.  
  
 Para los proveedores que admiten transacciones anidadas, al llamar al método **BeginTrans** dentro de una transacción abierta se inicia una nueva transacción anidada. El valor devuelto indica el nivel de anidamiento: un valor devuelto de "1" indica que ha abierto una transacción de nivel superior (es decir, que la transacción no está anidada dentro de otra transacción), "2" indica que ha abierto una transacción de segundo nivel (una transacción anidada dentro de una transacción de nivel superior), etc. La llamada a **CommitTrans** o a **RollbackTrans** solo afecta a la transacción abierta más recientemente; debe cerrar o revertir la transacción actual antes de poder resolver las transacciones de nivel superior.  
  
 La llamada al método **CommitTrans** guarda los cambios realizados en una transacción abierta en la conexión y finaliza la transacción. La llamada al método **RollbackTrans** invierte los cambios realizados en una transacción abierta y finaliza la transacción. Si se llama a cualquier método cuando no hay ninguna transacción abierta, se genera un error.  
  
 Dependiendo de la propiedad [attributes](./attributes-property-ado.md) del objeto **Connection** , llamar a los métodos **CommitTrans** o **RollbackTrans** puede iniciar automáticamente una nueva transacción. Si la propiedad **attributes** está establecida en **adXactCommitRetaining**, el proveedor inicia automáticamente una nueva transacción después de una llamada a **CommitTrans** . Si la propiedad **attributes** está establecida en **adXactAbortRetaining**, el proveedor inicia automáticamente una nueva transacción después de una llamada a **RollbackTrans** .  
  
## <a name="remote-data-service"></a>Servicio de datos remotos  
 Los métodos **BeginTrans**, **CommitTrans** y **RollbackTrans** no están disponibles en un objeto de **conexión** del lado cliente.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conexión (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de los métodos BeginTrans, CommitTrans y RollbackTrans (VB)](./begintrans-committrans-and-rollbacktrans-methods-example-vb.md)   
 [Ejemplo de los métodos BeginTrans, CommitTrans y RollbackTrans (VC + +)](./begintrans-committrans-and-rollbacktrans-methods-example-vc.md)   
 [Propiedad Attributes (ADO)](./attributes-property-ado.md)