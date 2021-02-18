---
title: Pausar un seguimiento
titleSuffix: SQL Server Profiler
description: Obtenga información sobre cómo pausar un seguimiento para que SQL Server Profiler deje de capturar datos de eventos y vea qué propiedades puede cambiar mientras el seguimiento está en pausa.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 432b9b0c-b5e7-47f3-a71b-310fb3bf2445
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 4c87c6d92edfaab6da1dac5d912db8a82fbfb4cf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353403"
---
# <a name="pause-a-trace-sql-server-profiler"></a>Pausar un seguimiento (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
La pausa de un seguimiento impide la captura de más datos de eventos hasta que se vuelva a iniciar.  
  
 La pausa de un seguimiento impide la captura de datos de eventos hasta que se vuelve a iniciar. Al iniciarlo de nuevo se reanudan las operaciones de seguimiento. Tras reiniciar una traza, no se pierden los datos capturados previamente. Cuando se vuelve a iniciar el seguimiento, se reanuda la captura de datos a partir de ese punto. Cuando un seguimiento está pausado, puede cambiar el nombre, los eventos, las columnas y los filtros. Sin embargo, no puede cambiar los destinos a los que envía los datos del seguimiento ni la conexión del servidor.  
  
### <a name="to-pause-a-trace"></a>Para pausar un seguimiento  
  
1.  Seleccione una ventana que contenga un seguimiento que se esté ejecutando.  
  
2.  En el menú **Archivo** , haga clic en **Pausar seguimiento**.  
  
## <a name="see-also"></a>Consulte también  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
