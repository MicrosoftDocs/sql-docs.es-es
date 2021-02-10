---
description: Propiedad ExecuteOptions (RDS)
title: Propiedad ExecuteOptions (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- ExecuteOptions property [ADO], VBScript example
ms.assetid: 62a4fd88-afc3-4f1f-b978-40710a30c4e9
author: rothja
ms.author: jroth
ms.openlocfilehash: a7ec4676ae96b1caf5e7561b90d5930a3b07d378
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053176"
---
# <a name="executeoptions-property-rds"></a>Propiedad ExecuteOptions (RDS)
Indica si la ejecución asincrónica está habilitada.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve uno de los valores siguientes.  
  
|Constante|Descripción|  
|--------------|-----------------|  
|**adcExecSync**|Ejecuta la siguiente actualización del conjunto de [registros](../ado-api/recordset-object-ado.md) sincrónicamente.|  
|**adcExecAsync**|Predeterminada. Ejecuta la siguiente actualización del conjunto de **registros** de forma asincrónica.|  
  
> [!NOTE]
>  Cada archivo ejecutable que utiliza estas constantes debe proporcionar declaraciones para ellos. Puede cortar y pegar las declaraciones de constantes que desee del archivo Adcvbs. Inc, que se encuentra en la carpeta de instalación predeterminada de la biblioteca RDS.  
  
## <a name="remarks"></a>Observaciones  
 Si **ExecuteOptions** está establecida en **adcExecAsync**, se ejecuta de forma asincrónica la siguiente llamada de **actualización** en [RDS.](./datacontrol-object-rds.md) **Conjunto de registros** del objeto DataControl.  
  
 Si intenta llamar a [RESET](./reset-method-rds.md), [Refresh](./refresh-method-rds.md), [SubmitChanges](./submitchanges-method-rds.md), [CancelUpdate](../ado-api/cancelupdate-method-ado.md)o [Recordset](./recordset-sourcerecordset-properties-rds.md) mientras otra operación asincrónica podría cambiar el [objeto RDS.](./datacontrol-object-rds.md) Se está ejecutando el **conjunto de registros** del objeto DataControl; se produce un error.  
  
 Si se produce un error durante una operación asincrónica, el **objeto RDS.** El valor [ReadyState](./readystate-property-rds.md) del objeto DataControl cambia de **adcReadyStateLoaded** a **adcReadyStateComplete**, y el valor de la propiedad **Recordset** no sigue siendo *nada*.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto DataControl (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades ExecuteOptions y FetchOptions (VBScript)](./executeoptions-and-fetchoptions-properties-example-vbscript.md)   
 [Cancel (método) (RDS)](./cancel-method-rds.md)