---
title: Establecer opciones globales de seguimiento
titleSuffix: SQL Server Profiler
description: Obtenga información sobre cómo establecer opciones globales de seguimiento y sobre las opciones que una instancia específica de SQL Server Profiler puede aplicar a todos los seguimientos.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 2854608a-c3c7-4eb8-b567-034bfec4b1a9
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 0394038ecd114f30c1a32005e126a4aa20b548a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345808"
---
# <a name="set-global-trace-options-sql-server-profiler"></a>Establecer opciones globales de seguimiento (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

En este tema se describe cómo establecer las opciones que se aplican a todos los seguimientos creados con una instancia determinada del [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
### <a name="to-set-global-trace-options"></a>Para establecer opciones globales de seguimiento  
  
1.  En el menú **Herramientas** , haga clic en **Opciones**.  
  
2.  En el cuadro de diálogo **Opciones generales** haga clic en **Elegir fuente** para modificar las opciones de visualización y luego haga clic en **Aceptar**.  
  
3.  Opcionalmente puede seleccionar **Iniciar la traza inmediatamente tras realizar la conexión**.  
  
4.  Opcionalmente también puede seleccionar **Actualizar definición de seguimiento cuando cambie la versión del proveedor**. Ésta es la opción recomendada y está seleccionada de forma predeterminada. Cuando se selecciona esta opción, la definición del seguimiento se actualiza automáticamente a la versión actual del servidor donde se realiza la traza.  
  
5.  Opcionalmente, puede especificar cómo administra el servidor los archivos de sustitución incremental:  
  
    -   Seleccione **Cargar todos los archivos de sustitución incremental en secuencia sin preguntar** para cargar automáticamente los archivos de sustitución incremental durante la reproducción.  
  
    -   Seleccione **Preguntar antes de cargar archivos de sustitución incremental** para controlar los archivos de sustitución incremental durante la reproducción.  
  
    -   Seleccione **No cargar nunca los archivos siguientes de sustitución incremental** para reproducir solamente un archivo cada vez.  
  
6.  Opcionalmente, puede establecer las opciones de reproducción:  
  
    -   **Número predeterminado de subprocesos de reproducción** controla el número de subprocesos de procesador que se van a utilizar durante la reproducción. Un número elevado de subprocesos permite completar la reproducción con más rapidez, pero degrada el rendimiento del servidor durante la reproducción. El valor recomendado es **4**. En la siguiente tabla se enumeran las opciones disponibles:  
  
        |Value|Descripción|  
        |-----------|-----------------|  
        |**2**|Valor mínimo. Utilice dos subprocesos para la reproducción.|  
        |**4**|Valor predeterminado.|  
        |**255**|Valor máximo. El uso de un valor máximo bloqueará el rendimiento de otros procesos.|  
  
    -   **Intervalo de espera del monitor de mantenimiento predeterminado (s)** establece el tiempo máximo (en segundos) durante el cual un subproceso de reproducción puede bloquear otro proceso. En la siguiente tabla se describen los valores.  
  
        |Value|Descripción|  
        |-----------|-----------------|  
        |**0**|Valor mínimo. El valor **0** indica que el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] nunca detendrá un proceso de bloqueo.|  
        |**3600**|Valor predeterminado. Permite los procesos de bloqueo inferiores a **3.600** segundos, o una hora.|  
        |**86400**|Valor máximo. Permite los procesos de bloqueo inferiores a **86.400** segundos, o un día.|  
  
    -   **Intervalo de sondeo del monitor de mantenimiento predeterminado (s)** establece la frecuencia de sondeo de subprocesos de reproducción para bloquear procesos. En la siguiente tabla se describen los valores.  
  
        |Value|Descripción|  
        |-----------|-----------------|  
        |**1**|Valor mínimo. El valor **1** indica que el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] sondeará los procesos de bloqueo una vez por segundo.|  
        |**60**|Valor predeterminado. Sondea los procesos de bloqueo una vez por minuto.|  
        |**86400**|Valor máximo. Sondea los procesos de bloqueo una vez cada **86.400** segundos, o día.|  
  
## <a name="see-also"></a>Consulte también  
 [Establecer los valores predeterminados de presentación de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/set-trace-display-defaults-sql-server-profiler.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
