---
description: Interbloqueos con nivel de aislamiento de lectura repetible
title: Interbloqueos con el nivel de aislamiento repetible de lectura | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- deadlocks in RDS [ADO]
- read repeatable in RDS [ADO]
ms.assetid: 29f3683f-12f3-4304-8a54-fe133c25a423
author: rothja
ms.author: jroth
ms.openlocfilehash: 05b43a72ede823a3a755edfdaa48be1e767b3e74
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036505"
---
# <a name="deadlocks-with-read-repeatable-isolation-level"></a>Interbloqueos con nivel de aislamiento de lectura repetible
Si un objeto comercial personalizado usa un nivel de aislamiento de lectura repetible para tener acceso a un SQL Server, y dos clientes que envían una consulta y actualizan simultáneamente el objeto comercial reciben un interbloqueo. El servicio de datos remoto está diseñado para permitir que uno de los procesos agote el tiempo de espera para liberar el interbloqueo, pero se producirá un error en la actualización para ese cliente.  
  
 Use la propiedad dinámica tiempo de espera del **comando** del [servicio de cursor](../appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md) para modificar la longitud del tiempo de espera.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="see-also"></a>Consulte también  
 [Aspectos básicos de RDS](./rds-fundamentals.md)