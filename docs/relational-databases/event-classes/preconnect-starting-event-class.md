---
description: PreConnect:Starting, clase de eventos
title: PreConnect:Starting, clase de eventos |Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- PreConnect:Starting Event Class
ms.assetid: d43ed0ad-3dbd-42e0-9cef-8320b8d87497
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4d091c0a6fb0041092aa742622e4c0afbd96d387
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467826"
---
# <a name="preconnectstarting-event-class"></a>PreConnect:Starting, clase de eventos
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La clase de eventos PreConnect:Starting indica cuándo se inicia la ejecución un desencadenador de LOGON o la función clasificadora del regulador de recursos.  
  
## <a name="preconnectstarting-event-class-data-columns"></a>Columnas de datos de la clase de eventos PreConnect:Starting  
  
|Nombre de columna de datos|Tipo de datos|Descripción|Identificador de columna|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|EventClass|**int**|215|27|No|  
|SPID|**int**|El Id. del proceso de servidor que dispara este evento.|12|Sí|  
|EventSubClass|**int**|1 para la función clasificadora definida por el usuario.|21|Sí|  
|StartTime|**datetime**|La hora en la que se inicia la función clasificadora definida por el usuario.|14|Sí|  
|ObjectID|**int**|El Id. del objeto clasificador definido por el usuario.|22|Sí|  
|ObjectName|**nvarchar(256)**|El nombre de dos partes de la función del clasificador definida por el usuario. Por ejemplo, dbo.classifier.|34|Sí|  
  
## <a name="see-also"></a>Consulte también  
 [Eventos extendidos](../../relational-databases/extended-events/extended-events.md)   
 [PreConnect:Completed, clase de eventos](../../relational-databases/event-classes/preconnect-completed-event-class.md)   
 [Regulador de recursos](../../relational-databases/resource-governor/resource-governor.md)  
  
  
