---
title: Habilitación del seguimiento de eventos en SqlClient
description: Describe cómo habilitar el seguimiento de eventos en SqlClient implementando un agente de escucha de eventos y cómo obtener acceso a los datos del evento.
ms.date: 11/23/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: v-daenge
ms.openlocfilehash: c13942dc5633deec4759ebd4c8a5661654c0952b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837092"
---
# <a name="enable-event-tracing-in-sqlclient"></a>Habilitación del seguimiento de eventos en SqlClient

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE [Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

[Seguimiento de eventos para Windows (ETW)](/windows/win32/etw/event-tracing-portal) es una eficaz herramienta de seguimiento de nivel de kernel que permite registrar eventos definidos por el controlador para la depuración y la realización de pruebas. SqlClient admite la captura de eventos ETW en diferentes niveles informativos. Para comenzar a capturar seguimientos de eventos, las aplicaciones cliente deberían escuchar eventos de la implementación de EventSource de SqlClient:

```
Microsoft.Data.SqlClient.EventSource
```

La implementación actual admite las siguientes palabras clave de evento:

| Nombre de palabra clave | Value | Descripción |
| ------------ | ----- | ----------- |
| ExecutionTrace | 1 | Activa la captura de eventos de inicio y detención antes y después de la ejecución del comando. |
| Seguimiento | 2 | Activa la captura de eventos de seguimiento de flujo de aplicación básicos. |
| Ámbito | 4 | Activa la captura de eventos de entrada y salida. |
| NotificationTrace | 8 | Activa la captura de eventos de seguimiento `SqlNotification`. |
| NotificationScope | 16 | Activa la captura de eventos de entrada y salida del ámbito `SqlNotification`. |
| PoolerTrace | 32 | Activa la captura de eventos de seguimiento de flujo de agrupación de conexiones. |
| PoolerScope | 64 | Activa la captura de eventos de seguimiento de ámbito de agrupación de conexiones. |
| AdvancedTrace | 128 | Activa la captura de eventos de seguimiento de flujo avanzados. |
| AdvancedTraceBin  | 256 | Activa la captura de eventos de seguimiento de flujo avanzados con información adicional. |
| CorrelationTrace | 512 | Activa la captura de eventos de seguimiento de flujo de correlación. |
| StateDump | 1024 | Activa la captura del volcado del estado completo de `SqlConnection`. |
| SNITrace | 2048 | Activa la captura de eventos de seguimiento de flujo de la implementación de redes administradas (solo es aplicable en .NET Core). |
| SNIScope | 4096 | Activa la captura de eventos de ámbito de la implementación de redes administradas (solo es aplicable en .NET Core). |
|||

## <a name="example"></a>Ejemplo

En el ejemplo siguiente se habilita el seguimiento de eventos para una operación de datos en la base de datos de ejemplo **AdventureWorks** y se muestran los eventos en la ventana de la consola.

[!code-csharp [SqlClientEventSource#1](~/../sqlclient/doc/samples/SqlClientEventSource.cs#1)]

## <a name="event-tracing-support-in-native-sni"></a>Compatibilidad con el seguimiento de eventos en SNI nativo

**Microsoft.Data.SqlClient** v2.1.0 amplía la compatibilidad con el seguimiento de eventos en **Microsoft.Data.SqlClient.SNI** y **Microsoft.Data.SqlClient.SNI.runtime**. Mediante el envío de EventCommand a `SqlClientEventSource`, se pueden recopilar eventos en el archivo SNI.dll nativo con las herramientas [Xperf](/windows-hardware/test/wpt/) y [PerfView](https://github.com/microsoft/perfview). Los valores válidos de EventCommand se muestran a continuación:

```cs
// Enables trace events:
EventSource.SendCommand(eventSource, (EventCommand)8192, null);

// Enables flow events:
EventSource.SendCommand(eventSource, (EventCommand)16384, null);

// Enables both trace and flow events:
EventSource.SendCommand(eventSource, (EventCommand)(8192 | 16384), null);
```

En el ejemplo siguiente se habilita el seguimiento de eventos en el archivo SNI.dll nativo cuando la aplicación tiene como destino .NET Framework. 

```cs
// Native SNI tracing example
// .NET Framework application
using System;
using System.Diagnostics.Tracing;
using Microsoft.Data.SqlClient;

public class SqlClientListener : EventListener
{
    protected override void OnEventSourceCreated(EventSource eventSource)
    {
        if (eventSource.Name.Equals("Microsoft.Data.SqlClient.EventSource"))
        {
            // Enables both trace and flow events
            EventSource.SendCommand(eventSource, (EventCommand)(8192 | 16384), null);
        }
    }
}

class Program
{
    static string connectionString = @"Data Source = localhost; Initial Catalog = AdventureWorks;Integrated Security=true;";

    static void Main(string[] args)
    {
        using (SqlClientListener listener = new SqlClientListener())
        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            connection.Open();
        }        
    }
}
```

### <a name="use-xperf-to-collect-trace-log"></a>Uso de Xperf para recopilar el registro de seguimiento

1. Inicie el seguimiento con la siguiente línea de comandos.

   ```
   xperf -start trace -f myTrace.etl -on *Microsoft.Data.SqlClient.EventSource
   ```

2. Ejecute el ejemplo de seguimiento de SNI nativo para conectarse a SQL Server.

3. Detenga el seguimiento con la siguiente línea de comandos.

   ```
   xperf -stop trace
   ```

4. Use PerfView para abrir el archivo myTrace.etl especificado en el paso 1. El registro de seguimiento de SNI se puede encontrar con los nombres de evento `Microsoft.Data.SqlClient.EventSource/SNIScope` y `Microsoft.Data.SqlClient.EventSource/SNITrace`.

   ![Uso de PerfView para ver el archivo de seguimiento de SNI](media/view-event-trace-native-sni.png)


### <a name="use-perfview-to-collect-trace-log"></a>Uso de PerfView para recopilar el registro de seguimiento

1. Inicie PerfView y ejecute `Collect > Collect` desde la barra de menús.

2. Configure el nombre del archivo de seguimiento, la ruta de acceso de salida y el nombre del proveedor.

   ![Configuración de PerfView antes de la recopilación](media/collect-event-trace-native-sni.png)

3. Inicie la recopilación.

4. Ejecute el ejemplo de seguimiento de SNI nativo para conectarse a SQL Server.

5. Detenga la recopilación de PerfView. Se tardará un tiempo en generar el archivo PerfViewData.etl según la configuración del paso 2.

6. Abra el archivo `etl` en PerfView. El registro de seguimiento de SNI se puede encontrar con los nombres de evento `Microsoft.Data.SqlClient.EventSource/SNIScope` y `Microsoft.Data.SqlClient.EventSource/SNITrace`.

## <a name="external-resources"></a>Recursos externos  

Para obtener más información, vea los recursos siguientes.  
  
|Resource|Descripción|  
|--------------|-----------------|  
|[clase EventSource](/dotnet/api/system.diagnostics.tracing.eventsource)|Se utiliza para crear eventos ETW.|
|[Clase EventListener](/dotnet/api/system.diagnostics.tracing.eventlistener)|Proporciona métodos para habilitar y deshabilitar eventos de orígenes de eventos.|
