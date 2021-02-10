---
description: Propiedades Recordset y SourceRecordset (RDS)
title: Propiedades Recordset y SourceRecordset (RDS) | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Recordset property [ADO]
ms.assetid: a29e3fb9-306d-497a-9a59-1856a914e5e9
author: rothja
ms.author: jroth
ms.openlocfilehash: 531a832c5519b7a049c96de15971c74bc9a607a9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049205"
---
# <a name="recordset-sourcerecordset-properties-rds"></a>Propiedades Recordset y SourceRecordset (RDS)
Indica el objeto de **conjunto de registros** devuelto desde un objeto comercial personalizado.  
  
 **Se aplica a:** [objeto DataControl (RDS)](./datacontrol-object-rds.md)  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
DataControl.SourceRecordset = Recordset  
Recordset = DataControl.Recordset   
```  
  
#### <a name="parameters"></a>Parámetros  
 *DataControl*  
 Variable de objeto que representa un objeto [RDS. Objeto DataControl](./datacontrol-object-rds.md) .  
  
 *DataRecordsets*  
 Variable de objeto que representa un objeto de **conjunto de registros** .  
  
## <a name="remarks"></a>Observaciones  
 Puede establecer la propiedad **SourceRecordset** en un [conjunto de registros](../ado-api/recordset-object-ado.md) devuelto desde un objeto comercial personalizado.  
  
 Estas propiedades permiten que una aplicación controle el proceso de enlace por medio de un proceso personalizado. Reciben un conjunto de filas ajustado en un **conjunto de registros** para que pueda interactuar directamente con el conjunto de **registros**, realizando acciones como establecer propiedades o recorrer en iteración el conjunto de **registros**.  
  
 Puede establecer la propiedad **SourceRecordset** o leer la propiedad del **conjunto de registros** en tiempo de ejecución en el código de scripting.  
  
 **SourceRecordset** es una propiedad de solo escritura, a diferencia de la propiedad **Recordset** , que es una propiedad de solo lectura.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto DataControl (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades Recordset y SourceRecordset (VBScript)](./recordset-and-sourcerecordset-properties-example-vbscript.md)   
 [Método CreateRecordset (RDS)](./createrecordset-method-rds.md)   
 [Método Query (RDS)](./query-method-rds.md)