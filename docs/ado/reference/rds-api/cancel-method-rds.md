---
description: Cancel (método) (RDS)
title: Método CANCEL (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Cancel method [RDS]
ms.assetid: 560b5b3d-fba9-4275-8920-9c3e186134f7
author: rothja
ms.author: jroth
ms.openlocfilehash: 5c3c1c78d69db0ceba435c91f13172a5ee77a697
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049555"
---
# <a name="cancel-method-rds"></a>Cancel (método) (RDS)
Cancela la ejecución de una llamada de método pendiente y asincrónica.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
RDS.DataControl.Cancel  
```  
  
## <a name="remarks"></a>Observaciones  
 Cuando se llama a **Cancel**, [ReadyState](./readystate-property-rds.md) se establece automáticamente en **AdcReadyStateLoaded** y el [conjunto de registros](../ado-api/recordset-object-ado.md) estará vacío.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto DataControl (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo del método CANCEL (VBScript)](./cancel-method-example-vbscript.md)   
 [CANCEL (método) (ADO)](../ado-api/cancel-method-ado.md)   
 [CancelBatch (método) (ADO)](../ado-api/cancelbatch-method-ado.md)   
 [CancelUpdate (método) (ADO)](../ado-api/cancelupdate-method-ado.md)   
 [Método CancelUpdate (RDS)](./cancelupdate-method-rds.md)   
 [Propiedad ExecuteOptions (RDS)](./executeoptions-property-rds.md)