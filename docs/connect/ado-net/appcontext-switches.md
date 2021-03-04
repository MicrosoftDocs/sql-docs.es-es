---
title: Modificadores de AppContext en SqlClient
description: Describe cómo usar los modificadores de AppContext disponibles en SqlClient.
ms.date: 06/15/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: v-daenge
ms.openlocfilehash: aa6de0ff33b0d6a70ef14a42f5def3f3c6a20482
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836022"
---
# <a name="appcontext-switches-in-sqlclient"></a>Modificadores de AppContext en SqlClient

[!INCLUDE [Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

La clase AppContext permite que SqlClient proporcione una nueva función a la vez que sigue siendo compatible con autores de llamada que dependen del comportamiento anterior. Los usuarios pueden no participar de un cambio de comportamiento estableciendo modificadores de AppContext específicos.

## <a name="enabling-decimal-truncation-behavior"></a>Habilitación del comportamiento de truncamiento decimal

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

A partir de Microsoft.Data.SqlClient 2.0, los datos decimales se redondearán de forma predeterminada, tal como lo hace SQL Server. Para habilitar el comportamiento anterior del truncamiento, puede establecer el modificador de AppContext **"Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal"** en `true` en el inicio de la aplicación:

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal", true);
```

## <a name="enabling-managed-networking-on-windows"></a>Habilitación de redes administradas en Windows

[!INCLUDE [appliesto-xxxx-netcore-netst-md](../../includes/appliesto-xxxx-netcore-netst-md.md)]

En Windows, SqlClient utiliza de forma predeterminada una implementación nativa de la interfaz de red SNI. Para habilitar el uso de una implementación de SNI administrada, puede establecer el modificador de AppContext **"Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows"** en `true` en el inicio de la aplicación:

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows", true);
```

Este modificador alternará el comportamiento del controlador a fin de usar una implementación de red administrada en los proyectos de .NET Core 2.1+ y .NET Standard 2.0+ en Windows, eliminando todas las dependencias de las bibliotecas nativas para la biblioteca Microsoft.Data.SqlClient. Está pensado solo para fines de prueba y depuración.

> [!NOTE]
> Hay algunas diferencias conocidas en comparación con la implementación nativa. Por ejemplo, la implementación administrada no admite la autenticación de Windows que no sea de dominio.

## <a name="disabling-transparent-network-ip-resolution"></a>Deshabilitación de la resolución de IP de red transparente

[!INCLUDE [appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

La resolución de IP de red transparente (TNIR) es una revisión de la característica MultiSubnetFailover existente. Afecta la secuencia de conexión del controlador cuando la primera dirección IP resuelta del nombre de host no responde y hay varias direcciones IP asociadas con el nombre de host. TNIR interactúa con MultiSubnetFailover para proporcionar las tres secuencias de conexión siguientes:<br />
* 0: se intenta una IP, seguida de todas las direcciones IP en paralelo.
* 1: todas las direcciones IP se intentan en paralelo.
* 2: todas las direcciones IP se intentan una tras otra.

|TransparentNetworkIPResolution|MultiSubnetFailover|Comportamiento|
|--------|--------|--------|
|True|True|1|
|True|False|0|
|False|True|1|
|False|False|2|

TransparentNetworkIPResolution está habilitado de forma predeterminada. MultiSubnetFailover está deshabilitado de forma predeterminada. Para deshabilitar TNIR, puede establecer el modificador de AppContext **"Switch.Microsoft.Data.SqlClient.DisableTNIRByDefaultInConnectionString"** en `true` en el inicio de la aplicación:

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.DisableTNIRByDefaultInConnectionString", true);
```

Para obtener más información sobre cómo establecer estas propiedades, vea la documentación de la [Propiedad SqlConnection.ConnectionString](/dotnet/api/microsoft.data.sqlclient.sqlconnection.connectionstring). 

## <a name="enable-a-minimum-timeout-during-login"></a>Habilitación de un tiempo de espera mínimo durante el inicio de sesión

[!INCLUDE [appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

Para evitar que un intento de inicio de sesión espere indefinidamente, puede establecer el modificador de AppContext **Switch.Microsoft.Data.SqlClient.UseOneSecFloorInTimeoutCalculationDuringLogin** en `true` en el inicio de la aplicación:

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.UseOneSecFloorInTimeoutCalculationDuringLogin", false);
```

## <a name="disable-blocking-behavior-of-readasync"></a>Deshabilitación del comportamiento de bloqueo de ReadAsync

[!INCLUDE [appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

De forma predeterminada, ReadAsync se ejecuta de forma sincrónica y bloquea el subproceso de llamada en .NET Framework. Para deshabilitar este comportamiento de bloqueo, puede establecer el modificador de AppContext **Switch.Microsoft.Data.SqlClient.MakeReadAsyncBlocking** en `false` en el inicio de la aplicación:

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.MakeReadAsyncBlocking", false);
```

## <a name="see-also"></a>Consulte también

[Clase AppContext](/dotnet/api/system.appcontext?view=netcore-3.1&preserve-view=true)