---
description: Objetos RDS
title: Objetos RDS | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
helpviewer_keywords:
- objects [ADO], RDS
- RDS objects [ADO]
ms.assetid: f2ac8b3b-f968-41c4-a504-7aee3538b7c7
author: rothja
ms.author: jroth
ms.openlocfilehash: 1f53cd2ef72b6ff540a8663f6400c8dcdf2fb822
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052986"
---
# <a name="rds-objects"></a>Objetos RDS
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
|Object|Descripción|  
|-|-|  
|[DataControl (RDS)](./datacontrol-object-rds.md)|Enlaza un **conjunto de registros** de consulta de datos a uno o varios controles (por ejemplo, un cuadro de texto, un control de cuadrícula o un cuadro combinado) para mostrar los datos del **conjunto de registros** en una página web.<br /><br /> El objeto **DataControl** es seguro para el scripting.|  
|[DataFactory (RDSServer)](./datafactory-object-rdsserver.md)|Implementa métodos que proporcionan acceso a los datos de lectura y escritura a los orígenes de datos especificados para las aplicaciones del lado cliente.<br /><br /> El objeto **DataFactory** no es seguro para el scripting.|  
|[DataSpace (RDS)](./dataspace-object-rds.md)|Crea proxies del lado cliente para objetos comerciales personalizados ubicados en el nivel intermedio.<br /><br /> El objeto **DataSpace** es seguro para el scripting.|  
|[Interfaz IRDSService (RDS)](./irdsservice-interface-rds.md)|Expone el método [InvokeService (RDS)](./invokeservice-rds.md) , que se usa para devolver un puntero a la interfaz solicitada en una versión más capaz del objeto.|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de RDS](./rds-api-reference.md)