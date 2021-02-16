---
title: Uso de XEvent Profiler de SSMS
description: XEvent Profiler muestra un visor en directo de eventos extendidos. Aprenda por qué usar este generador de perfiles, las características clave y cómo empezar a ver los eventos extendidos.
ms.date: 10/02/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: genemi
ms.technology: xevents
ms.topic: tutorial
helpviewer_keywords:
- extended events [SQL Server], system health session
- extended events [SQL Server], system_health session
- system_health session [SQL Server extended events]
- system health session [SQL Server extended events]
ms.assetid: 1e1fad43-d747-4775-ac0d-c50648e56d78
author: yualan
ms.author: alayu
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 0cfc93c0e35ccfa81b6570187056dc8e2b500741
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355270"
---
# <a name="use-the-ssms-xevent-profiler"></a>Uso de XEvent Profiler de SSMS

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
XEvent Profiler es una característica de SQL Server Management Studio (SSMS) que muestra una ventana con los eventos extendidos para visualizarlos de forma dinámica. En la información general puede consultar los motivos por los que el generador de perfiles resulta tan útil, las características principales y las instrucciones para visualizar los eventos extendidos.

## <a name="why-would-i-use-the-xevent-profiler"></a>¿Por qué XEvent Profiler es tan útil?
A diferencia de SQL Profiler, XEvent Profiler está directamente integrado en SSMS y compilado con la tecnología de eventos extendidos escalables en el motor de SQL. Esta característica permite acceder rápidamente a una visualización de streaming en vivo de los eventos de diagnóstico en el servidor SQL. Dicha visualización se puede personalizar. Además, las personalizaciones que haga se pueden compartir con otros usuarios de SSMS como archivo .viewsettings. La sesión que crea XE Profiler es menos invasiva para el servidor SQL en ejecución que otro seguimiento SQL similar, en el caso de usar SQL Profiler. El usuario también puede personalizar esta sesión con la IU de propiedades de sesión de XE existentes o mediante TSQL.

## <a name="prerequisites"></a>Prerrequisitos
Esta característica solo está disponible para la versión 17.3 de SQL Server Management Studio (SSMS) u otras posteriores. Asegúrese de utilizar la última versión, que podrá encontrar [aquí](../../ssms/download-sql-server-management-studio-ssms.md).

## <a name="getting-started"></a><a id="getting-started"></a>Introducción
Para acceder a XEvent Profiler, siga estos pasos:

1. Abra **SQL Server Management Studio**.

2. Conéctese a una instancia de Motor de base de datos de SQL Server o el localhost.

3. En el Explorador de objetos, encontrará el elemento de menú correspondiente a XE Profiler. Para expandirlo, haga clic en el signo "+".

   ![Menú de XE Profiler](media/xevents-xe-profiler-menu.png)

4. Si quiere ver todos los eventos extendidos de esta sesión, haga doble clic en **Estándar**. Si quiere ver las instrucciones SQL registradas, haga clic en **T-SQL**. En caso de que todavía no haya ninguna sesión creada, se creará una para usted.

   ![Sesión de XE Profiler](media/xevents-xe-profiler-start-session.png)

5. Ahora puede ver los eventos extendidos.

   ![Visor de XE Profiler](media/xevents-xe-profiler-start-viewer.png)

## <a name="see-also"></a>Consulte también
[Eventos extendidos](../../relational-databases/extended-events/extended-events.md)  
[Herramientas de eventos extendidos](../../relational-databases/extended-events/extended-events-tools.md)  
  
