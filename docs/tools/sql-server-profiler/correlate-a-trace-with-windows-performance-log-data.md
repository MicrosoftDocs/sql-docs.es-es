---
title: Correlacionar un seguimiento con los datos del registro de rendimiento de Windows
titleSuffix: SQL Server Profiler
description: Obtenga información sobre cómo poner en correlación los registros de rendimiento de Windows con un seguimiento para que pueda ver los datos de los objetos y contadores del Monitor de sistema en SQL Server Profiler.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 1e4412c8-d27c-4aae-9b35-214128d1d00a
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 07/12/2017
ms.openlocfilehash: 9279078aee9a27b647a9f350756e317ae06cbda5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349413"
---
# <a name="correlate-a-trace-with-windows-performance-log-data"></a>Correlacionar un seguimiento con los datos del registro de rendimiento de Windows

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

El [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]permite abrir un registro de rendimiento de Microsoft Windows, elegir los contadores que desea correlacionar con un seguimiento y ver los contadores de rendimiento seleccionados junto con el seguimiento en la interfaz gráfica de usuario del [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] . Al seleccionar un evento en la ventana de seguimiento, una barra vertical roja en el panel de la ventana de datos del Monitor de sistema del [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] indica los datos del registro de rendimiento que se correlacionan con el evento de seguimiento seleccionado.  
  
 Para correlacionar un seguimiento con contadores de rendimiento, abra un archivo o una tabla de seguimiento que contenga las columnas de datos **StartTime** y **EndTime** y luego haga clic en **Importar datos de rendimiento** del menú [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] **Archivo** de . Puede abrir un registro de rendimiento y seleccionar los objetos y contadores del Monitor de sistema que desea correlacionar con el seguimiento.  
  
### <a name="to-correlate-a-trace-with-performance-log-data"></a>Para establecer correlaciones de un seguimiento con datos del registro de rendimiento  
  
1.  En [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], abra un archivo de seguimiento o una tabla de seguimiento guardados. No puede establecer correlaciones de un seguimiento en ejecución que aún está recopilando datos de eventos. Para lograr una correlación precisa con los datos del Monitor de sistema, el seguimiento debe contener las columnas de datos **StartTime** y **EndTime** .  
  
2.  En el menú **Archivo** de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], haga clic en **Importar datos de rendimiento**.  
  
3.  En el cuadro de diálogo **Abrir** , seleccione un archivo que contenga un registro de rendimiento. Los datos del registro de rendimiento deben haber sido capturados durante el mismo período de tiempo en que se capturaron los datos de seguimiento.  
  
4.  En el cuadro de diálogo **Límite de contadores de rendimiento** , active las casillas que corresponden a los objetos y contadores del Monitor del sistema que desea mostrar junto con el seguimiento. Haga clic en **Aceptar**.  
  
5.  Seleccione un evento en la ventana de eventos de seguimiento o navegue por varias filas adyacentes en la ventana de eventos de seguimiento utilizando las teclas de flecha. La barra vertical roja de la ventana **Datos del Monitor de sistema** indica los datos del registro de rendimiento correlacionados con el evento de seguimiento seleccionado.  
  
6.  Haga clic en un punto de interés en el gráfico Monitor del sistema. Se selecciona la fila correspondiente del seguimiento más cercana en el tiempo. Para ver más de cerca un intervalo de tiempo, presione el mouse (ratón) y arrastre el puntero en el gráfico Monitor del sistema.  
  
### <a name="to-create-performance-logs-that-can-be-shared-among-different-versions-of-windows"></a>Para crear registros de rendimiento que puedan compartirse entre diferentes versiones de Windows  
  
1.  En el Panel de control, abra **Herramientas administrativas** y haga doble clic en **Rendimiento**.  
  
2.  En el cuadro de diálogo **Rendimiento** , expanda **Registros y alertas de rendimiento**, haga clic con el botón derecho en **Registros de contador** y, después, haga clic en **Nueva configuración de registro**.  
  
3.  Escriba un nombre para el registro de contador y, a continuación, haga clic en **Aceptar**.  
  
4.  En la pestaña **General** , haga clic en **Agregar contadores**.  
  
5.  En la lista **Objeto de rendimiento** , seleccione un objeto de rendimiento que desee supervisar. Los nombres de los objetos de rendimiento de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para instancias predeterminadas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comienzan con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y las instancias con nombre comienzan con MSSQL$*instanceName*.  
  
6.  Agregue tantos contadores como sean necesarios para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y otros valores importantes, como el tiempo de procesador y el tiempo de disco.  
  
7.  Cuando haya terminado de agregar los contadores, haga clic en **Cerrar**.  
  
8.  Establezca los valores del intervalo **Tomar datos de muestra cada** . Comience por un intervalo de muestreo moderado, como 5 minutos, y después ajuste el intervalo si es necesario.  
  
9. En la pestaña **Archivos de registro** , elija **Archivo de texto (delimitado por comas)** en la lista **Tipo del archivo de registro** . Los archivos de registro de texto delimitado por comas se pueden compartir entre diferentes versiones de Windows y se pueden ver más tarde en herramientas de elaboración de informes como Microsoft Excel.  
  
10. En la pestaña **Programación** , especifique una programación de supervisión.  
  
11. Haga clic en **Aceptar** para crear el registro de rendimiento.  
