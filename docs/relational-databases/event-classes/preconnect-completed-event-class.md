---
description: PreConnect:Completed, clase de eventos
title: PreConnect:Completed, clase de eventos |Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- PreConnect:Completed Event Class
ms.assetid: 7ed2f620-6511-4985-9961-d2927c2b1759
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b432f36c72b2118c6fd6a04e7f56a9a1d40ff24b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180841"
---
# <a name="preconnectcompleted-event-class"></a>PreConnect:Completed, clase de eventos
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La clase de eventos PreConnect:Completed indica cuándo finaliza la ejecución de un desencadenador de LOGON o la función clasificadora de Regulador de recursos.  
  
## <a name="preconnectcompleted-event-class-data-columns"></a>Columnas de datos de la clase de eventos PreConnect:Completed  
  
|Nombre de columna de datos|Tipo de datos|Descripción|Identificador de columna|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|EventClass|**int**|216|27|No|  
|SPID|**int**|El Id. del proceso de servidor que dispara este evento.|12|Sí|  
|EventSubClass|**int**|1 para la función clasificadora definida por el usuario.|21|Sí|  
|StartTime|**datetime**|La hora en la que se inicia la función clasificadora definida por el usuario.|14|Sí|  
|EndTime|**datetime**|La hora en la que se inicia la función clasificadora definida por el usuario.|15|Sí|  
|Duration|**bigint**|La cantidad de tiempo, en microsegundos, que ha utilizado la función clasificadora.|13|Sí|  
|ObjectID|**int**|El Id. del objeto clasificador definido por el usuario.|22|Sí|  
|CPU|**int**|Uso de la CPU en milisegundos.|18|Sí|  
|Lecturas|**int**|El número de lecturas lógicas.|16|Sí|  
|Escrituras|**int**|El número de escrituras lógicas.|17|Sí|  
|GroupID|**int**|El Id. del grupo de cargas de trabajo clasificado.|66|Sí|  
|Error|**int**|El último número de error en caso de que la función clasificadora definida por el usuario no se ejecute.|31|Sí|  
|State|**int**|El estado del último error.|30|Sí|  
|TargetUserName|**sysname**|El valor devuelto (nombre de grupo de cargas de trabajo) para la función clasificadora definida por el usuario en caso de que el sistema no encuentre el grupo activo correspondiente. De lo contrario, esta columna se establece en NULL.|39|Sí|  
|ObjectName|**nvarchar(256)**|El nombre de dos partes de la función del clasificador definida por el usuario. Por ejemplo, dbo.classifier.|34|Sí|  
  
## <a name="see-also"></a>Consulte también  
 [Eventos extendidos](../../relational-databases/extended-events/extended-events.md)   
 [PreConnect:Starting, clase de eventos](../../relational-databases/event-classes/preconnect-starting-event-class.md)   
 [Regulador de recursos](../../relational-databases/resource-governor/resource-governor.md)  
  
  
