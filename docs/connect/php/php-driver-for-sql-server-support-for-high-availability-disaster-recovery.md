---
description: Compatibilidad con recuperación ante desastres y alta disponibilidad en los controladores de Microsoft para PHP en SQL Server
title: Compatibilidad con recuperación ante desastres de alta disponibilidad para los controladores de Microsoft para PHP para SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 07/31/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 73a80821-d345-4fea-b076-f4aabeb4af3e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4a5c5747dad97cc54859cb9b40b966cec66970fc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058410"
---
# <a name="support-for-high-availability-disaster-recovery"></a>Compatibilidad con recuperación ante desastres de alta disponibilidad
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

En este tema se describe la compatibilidad de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], agregada en la versión 3.0, con recuperación ante desastres de alta disponibilidad.

A partir de la versión 3.0 de los controladores de Microsoft para PHP para SQL Server, puede especificar el agente de escucha del grupo de disponibilidad de un grupo de disponibilidad de recuperación ante desastres de alta disponibilidad o una instancia de clúster de conmutación por error como el servidor en la cadena de conexión.

La propiedad de conexión **MultiSubnetFailover** indica que la aplicación se está implementando en un grupo de disponibilidad o una instancia de clúster de conmutación por error y que el controlador intentará conectarse a la base de datos en la instancia principal de SQL Server mediante un intento de conexión a todas las direcciones IP. Al conectarse a un agente de escucha de grupo de disponibilidad de SQL Server o una instancia de clúster de conmutación por error de SQL Server, especifique siempre **MultiSubnetFailover=True**. Si la aplicación está conectada a una base de datos AlwaysOn que conmuta por error, se interrumpirá la conexión original y la aplicación deberá abrir una nueva para seguir trabajando tras la conmutación por error.

Puede encontrar detalles completos sobre los grupos de disponibilidad Always On en la página de documentación de [recuperación ante desastres de alta disponibilidad](../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md).

## <a name="transparent-network-ip-resolution-tnir"></a>Resolución de IP de red transparente (TNIR)

La resolución de IP de red transparente (TNIR) es una revisión de la característica **MultiSubnetFailover** existente. Afecta la secuencia de conexión del controlador cuando la primera dirección IP resuelta del nombre de host no responde y hay varias direcciones IP asociadas con el nombre de host. La opción de conexión correspondiente es **TransparentNetworkIPResolution**. Junto con **MultiSubnetFailover**, proporciona las cuatro secuencias de conexión siguientes: 

- TNIR habilitado y **MultiSubnetFailover** deshabilitado: se intenta una IP, seguida de todas las direcciones IP en paralelo.
- TNIR habilitado y **MultiSubnetFailover** habilitado: todas las direcciones IP se intentan en paralelo.
- TNIR deshabilitado y **MultiSubnetFailover** deshabilitado: todas las direcciones IP se intentan una tras otra.
- TNIR deshabilitado y **MultiSubnetFailover** habilitado: todas las direcciones IP se intentan en paralelo.

TNIR está habilitado de forma predeterminada y **MultiSubnetFailover**, deshabilitado de forma predeterminada.

Este es un ejemplo de cómo habilitar TNIR y **MultiSubnetFailover** mediante el controlador PDO_SQLSRV:

```
<?php
$serverName = "yourservername";
$username = "yourusername";
$password = "yourpassword";
$connectionString = "sqlsrv:Server=$serverName; TransparentNetworkIPResolution=Enabled; MultiSubnetFailover=yes";
try {
    $conn = new PDO($connectionString, $username, $password, array(PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION));
    // your code 
    // more of your code
    // when done, close the connection
    unset($conn);
} catch(PDOException $e) {
    print_r($e->errorInfo);
}
?>
```

## <a name="upgrading-to-use-multi-subnet-clusters-from-database-mirroring"></a>Actualizar para utilizar clústeres de varias subredes a partir de la creación de reflejo de la base de datos  
Se producirá un error de conexión si las palabras clave de conexión **MultiSubnetFailover** y **Failover_Partner** se encuentran en la cadena de conexión. También se producirá un error si se usa **MultiSubnetFailover** y SQL Server devuelve una respuesta del asociado de conmutación por error que indica que forma parte de un par de creación de reflejo de la base de datos.  
  
Al actualizar una aplicación PHP que actualmente usa la creación de reflejo de la base de datos en un escenario de varias subredes, quite la propiedad de conexión **Failover_Partner** y reemplácela por **MultiSubnetFailover** establecida en **True**, así como también reemplace el nombre del servidor en la cadena de conexión por un agente de escucha de grupo de disponibilidad. Si una cadena de conexión usa **Failover_Partner** y **MultiSubnetFailover=true**, el controlador generará un error. En cambio, si una cadena de conexión usa **Failover_Partner** y **MultiSubnetFailover=false** (o **ApplicationIntent=ReadWrite**), la aplicación usará la creación de reflejo de la base de datos.  
  
Si la creación de reflejo de la base de datos se usa en la base de datos principal del grupo de disponibilidad y **MultiSubnetFailover=true** se usa en la cadena de conexión que se conecta a una base de datos principal, en lugar de a un agente de escucha de grupo de disponibilidad, el controlador devolverá un error.  

[!INCLUDE[specify-application-intent_read-only-routing](~/includes/paragraph-content/specify-application-intent-read-only-routing.md)]


## <a name="see-also"></a>Consulte también  
[Conexión al servidor](../../connect/php/connecting-to-the-server.md)  
