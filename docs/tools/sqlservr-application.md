---
title: sqlservr (aplicación)
description: La aplicación sqlservr se inicia, se detiene, se pone en pausa y continúa una instancia de SQL Server desde un símbolo del sistema.
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server], sqlservr
- command prompt [SQL Server], pausing/resuming instance of SQL Server
- starting instance of SQL Server
- command prompt [SQL Server], continuing instance of SQL Server
- sqlservr utility
- pausing instance of SQL Server
- stopping instance of SQL Server
- resuming SQL Server
- command prompt [SQL Server], stopping instance of SQL Server
- command prompt [SQL Server], starting instance of SQL Server
- continuing instance of SQL Server
ms.assetid: 60e8ef0a-0851-41cf-a6d8-cca1e04cbcdb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 793943c492766037adbce8c077b9a391c041c88c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338581"
---
# <a name="sqlservr-application"></a>sqlservr (aplicación)

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

La aplicación **sqlservr** se inicia, se detiene, se pone en pausa y continúa una instancia de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] desde un símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```cmd
sqlservr [-s instance_name] [-c] [-d master_path] [-f] 
     [-e error_log_path] [-l master_log_path] [-m]
     [-n] [-T trace#] [-v] [-x]
```

## <a name="arguments"></a>Argumentos

**-s** *instance_name* Especifica la instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a la que hay que conectarse. Si no se especifica ninguna instancia con nombre, **sqlservr** inicia la instancia predeterminada de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

> [!IMPORTANT]
>Al iniciar una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], debe usar la aplicación **sqlservr** en el directorio correspondiente de esa instancia. Ejecute **sqlservr** desde el directorio \MSSQL\Binn para la instancia predeterminada. Ejecute **sqlservr** desde el directorio \MSSQL$*nombre_instancia*\Binn para la instancia con nombre.

 **-c** Indica que una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se inicia independientemente del Administrador de control de servicios de Windows. Esta opción se utiliza cuando se inicia [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] desde un símbolo del sistema para reducir el tiempo de inicio que necesita [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

> [!NOTE]
>Si usa esta opción, no puede detener [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mediante el Administrador de servicios de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ni el comando **net stop** y, si cierra sesión en el equipo, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se detiene.

**-d** *master_path* Indica la ruta de acceso completa para el archivo de base de datos **maestra**. No hay espacios entre **-d** y *master_path*. Si no proporciona esta opción, se usarán los parámetros del Registro existentes.

**-f** Inicia una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con la configuración mínima. Esta opción resulta útil si el valor de una opción de configuración (por ejemplo, la confirmación excesiva de memoria) ha impedido el inicio del servidor.

**-e** *error_log_path* Indica la ruta de acceso completa para el archivo de registro de errores. Si no se especifica, la ubicación predeterminada es `*\<Drive>*:\Program Files\Microsoft SQL Server\MSSQL\Log\Errorlog` para la instancia predeterminada y `*\<Drive>*:\Program Files\Microsoft SQL Server\MSSQL$*instance_name*\Log\Errorlog` para una instancia con nombre. No hay espacios entre **-e** y *error_log_path*.

**-l** *master_log_path* Indica la ruta de acceso completa para el archivo de registro de transacciones de la base de datos **maestra**. No hay espacios entre **-l** y *master_log_path*.

**-m** Indica que se inicie una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en modo de usuario único. Solo un usuario puede conectar cuando [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se inicia en modo de usuario único. No se inicia el mecanismo CHECKPOINT, que garantiza que se escriben con regularidad transacciones completadas desde la memoria caché del disco al dispositivo de base de datos. (Normalmente, esta opción se utiliza si se existen problemas en las bases de datos del sistema y necesitan repararse.) Habilita la opción **sp_configure allow updates**. De forma predeterminada, la opción **allow updates** está deshabilitada.

**-n** Permite iniciar una instancia con nombre de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Sin el parámetro **-s** establecido, la instancia predeterminada intenta iniciarse. Debe cambiar al directorio BINN apropiado para la instancia en una ventana del símbolo del sistema antes de iniciar **sqlservr.exe**. Por ejemplo, si Instance1 usara \mssql$Instance1 para sus archivos binarios, el usuario debería estar en el directorio \mssql$Instance1\binn para poder iniciar **sqlservr.exe -s instance1**. Si inicia una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con la opción **-n** , es recomendable que use también la opción **-e** ; de lo contrario los eventos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] no se registrarán.

**-T** *trace#* Indica que se debe iniciar una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con una marca de seguimiento específica (*trace#* ) vigente. Las marcas de seguimiento se utilizan para iniciar el servidor con un comportamiento distinto del habitual. Para obtener más información, vea [Marcas de seguimiento &#40;Transact-SQL&#41;](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

>[!IMPORTANT]
>Cuando especifique una marca de seguimiento, use **-T** para pasar el número de la marca de seguimiento. **acepta una t minúscula (** -t [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]), pero **-t** establece otras marcas de seguimiento internas que necesitan los ingenieros de soporte técnico de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

**-v** Muestra el número de versión del servidor.

**-x** Deshabilita el mantenimiento de estadísticas de tiempo de CPU y número de aciertos de caché. Permite el máximo rendimiento.

## <a name="remarks"></a>Observaciones
En la mayoría de los casos, el programa sqlserver.exe solo se usa para solucionar problemas o realizar las tareas principales de mantenimiento. Si se inicia [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] desde el símbolo del sistema con sqlservr.exe, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] no se inicia como servicio, de forma que [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] no se puede detener mediante los comandos **net** . Los usuarios pueden conectar con [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], pero las herramientas de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] muestran el estado del servicio, de forma que el Administrador de configuración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] indica correctamente que el servicio se ha detenido. [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] puede conectarse al servidor, pero también indica que el servicio está detenido.

## <a name="compatibility-support"></a>Soporte de compatibilidad
Los parámetros siguientes están obsoletos y no se admiten en [!INCLUDE[sssql19-md](../includes/sssql19-md.md)].

|Parámetro | Más información|
|:-----|:-----|
|**-h** | En versiones anteriores de instancias de 32 bits de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] para reservar espacio de direcciones de memoria virtual para los metadatos de Agregar memoria sin interrupción cuando AWE está habilitado. Se admite a través de [!INCLUDE[sssql14](../includes/sssql14-md.md)]. Para obtener más información, vea [Características de SQL Server en desuso y descontinuadas en SQL Server 2016](../database-engine/discontinued-database-engine-functionality-in-sql-server.md).|
|**-g** | *memoria_para_reserva*<br/><br>Se aplica a las versiones anteriores de las instancias de 32 bits de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Se admite a través de [!INCLUDE[sssql14](../includes/sssql14-md.md)]. Especifica un número entero de megabytes (MB) de memoria que [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] deja disponibles para las asignaciones de memoria en el proceso de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , pero fuera del bloque de memoria de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Para obtener más información, vea [la documentación de SQL Server 2014 sobre opciones de configuración de memoria de servidor](/previous-versions/sql/2014/database-engine/configure-windows/server-memory-server-configuration-options?view=sql-server-2014&preserve-view=true).|
| &nbsp; | &nbsp; |

## <a name="see-also"></a>Consulte también
 [Opciones de inicio del servicio de motor de base de datos](../database-engine/configure-windows/database-engine-service-startup-options.md)
