---
title: Guía de solución de problemas de SqlClient
description: Página que proporciona soluciones a los problemas que se observan normalmente.
ms.date: 03/03/2021
dev_langs:
- csharp
- vb
ms.assetid: 6f5ff56a-a57e-49d7-8ae9-bbed697e42e3
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: 725d1522f98e721f581a7d4b7614181fd2632962
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464808"
---
# <a name="sqlclient-troubleshooting-guide"></a>Guía de solución de problemas de SqlClient

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

## <a name="exceptions-when-connecting-to-sql-server"></a>Excepciones al conectar con SQL Server

Hay varias razones por las que no se puede establecer la conexión. A continuación se muestran algunas sugerencias para la solución de problemas que se pueden usar como guía para analizar y resolver muchas de estas incidencias.

### <a name="unable-to-load-native-sni-server-name-indication-library"></a>No se puede cargar la biblioteca SNI (Indicación de nombre de servidor) nativa

#### <a name="issues-in-net-framework-applications"></a>Problemas en aplicaciones .NET Framework

Seguimiento de la pila observado:

```log
TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.SNILoadHandle' threw an exception.
DllNotFoundException: Unable to load DLL 'Microsoft.Data.SqlClient.SNI.x64.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)
```

```log
TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.SNILoadHandle' threw an exception.
DllNotFoundException: Unable to load DLL 'Microsoft.Data.SqlClient.SNI.x86.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)
```

SNI es la biblioteca nativa de C++ de la que SqlClient depende para varias operaciones de red cuando se ejecuta en Windows. En las aplicaciones .NET Framework que se compilan con el SDK de proyecto de MSBuild, los archivos DLL nativos no se administran con comandos de restauración. Por lo tanto, se incluye un archivo ".targets" en el paquete NuGet "Microsoft.Data.SqlClient.SNI" que define las operaciones "Copy" necesarias.

Se hace referencia automáticamente al archivo ".targets" incluido cuando se realiza una dependencia directa a la biblioteca "Microsoft.Data.SqlClient". En escenarios en los que se realiza una referencia transitiva (indirecta), se debe hacer referencia manualmente a este archivo ".targets" para asegurarse de que las operaciones "Copy" se puedan ejecutar cuando sea necesario.

**Solución recomendada:** asegúrese de que se hace referencia a este archivo ".targets" en el archivo ".csproj" de la aplicación para garantizar que se ejecutan las operaciones "Copy".

Estos destinos abarcan únicamente los destinos conocidos y de uso común de Microsoft. Si una herramienta o aplicación externa define destinos personalizados para copiar archivos binarios, deben definirse nuevos destinos con los mantenedores de herramientas para asegurarse de que se copian los archivos DLL de SNI nativos en los archivos binarios de Microsoft.Data.SqlClient.dll y que están disponibles cuando se ejecutan aplicaciones cliente.

#### <a name="issues-in-net-core-applications"></a>Problemas en aplicaciones .NET Core

Seguimiento de la pila observado:

```log
System.TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.TdsParser' threw an exception.
---> System.TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.SNILoadHandle' threw an exception.
---> System.DllNotFoundException: Unable to load shared library 'Microsoft.Data.SqlClient.SNI.dll' or one of its dependencies.
```

> [!NOTE]
> Es posible que este error se produzca solo en aplicaciones Windows. En el caso de que se produzca en un entorno de UNIX, debe asegurarse de que la aplicación se ha compilado correctamente para tener como destino un entorno de ejecución de UNIX y no de Windows.

SNI es la biblioteca nativa de C++ de la que SqlClient depende para varias operaciones de red cuando se ejecuta en Windows. Microsoft.Data.SqlClient no administra la carga y descarga de esta biblioteca en .NET Core.

**Solución recomendada:** asegúrese de que se conceden los permisos "Execute" en el sistema de archivos donde se cargan las bibliotecas nativas del entorno de ejecución en el proceso de .NET Core. Si esto no soluciona el problema, puede publicar una incidencia en el repositorio de [dotnet/runtime](https://github.com/dotnet/runtime) para recibir más asistencia.

#### <a name="native-sni-pdb-not-found-errors"></a>Errores de SNI nativo (PDB no encontrado)

Seguimiento de la pila observado:

```log
An assembly specified in the application dependencies manifest (sql2csv.deps.json) was not found:
  package: 'Microsoft.Data.SqlClient.SNI.runtime', version: '2.0.0'
  path: 'runtimes/win-x64/native/Microsoft.Data.SqlClient.SNI.pdb'
```

**Solución recomendada:** asegúrese de que la aplicación cliente hace referencia a la versión mínima [v2.1.0](https://www.nuget.org/packages/Microsoft.Data.SqlClient/2.1.0) del paquete Microsoft.Data.SqlClient. Al usar EF Core, agregue una referencia a esta versión del paquete de Microsoft.Data.SqlClient directamente para invalidar la dependencia.

### <a name="hostname-resolution-errors"></a>Errores de resolución de nombre de host

Seguimiento de la pila observado:

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible.
Verify that the instance name is correct and that SQL Server is configured to allow remote connections.
(provider: TCP Provider, error: 0 - No such host is known.)
```

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible.
Verify that the instance name is correct and that SQL Server is configured to allow remote connections.
(provider: TCP Provider, error: 35 - An internal exception was caught)
```

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible.
Verify that the instance name is correct and that SQL Server is configured to allow remote connections.
(provider: TCP Provider, error: 35 - An internal exception was caught)
 ---> System.Net.Internals.SocketExceptionFactory+ExtendedSocketException (00000005, 0xFFFDFFFF): Name does not resolve
```

#### <a name="possible-reasons"></a>Razones posibles

- El protocolo TCP/canalizaciones con nombre no está habilitado en SQL Server

  **Solución recomendada:** habilite el protocolo TCP/canalizaciones con nombre en la instancia de SQL Server desde la consola de Administrador de configuración de SQL Server.

- Nombre de host desconocido

  **Solución recomendada:** asegúrese de que el nombre de host se resuelve en la dirección IP del servidor desde el cliente en el que se inicia la conexión.


### <a name="login-phase-errors"></a>Errores de fase de inicio de sesión

Seguimientos de la pila observados:

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A connection was successfully established with the server, but then an error occurred during the pre-login handshake.
(provider: SSL Provider, error: 31 - Encryption(ssl/tls) handshake failed)
System.IO.EndOfStreamException: End of stream reached
```

```log
A connection was successfully established with the server, but then an error occurred during the login process.
(provider: SSL Provider, error: 0 - The target principal name is incorrect.)
```

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): Connection Timeout Expired. The timeout period elapsed during the post-login phase. The connection could have timed out while waiting for server to complete the login process and respond; Or it could have timed out while attempting to create multiple active connections.
The duration spent while attempting to connect to this server was - [Pre-Login] initialization=837; handshake=394; [Login] initialization=3; authentication=15; [Post-Login] complete=1027;
---> System.ComponentModel.Win32Exception (258): Unknown error 258
at Microsoft.Data.SqlClient.SqlInternalConnection.OnError(SqlException exception, Boolean breakConnection, Action1 wrapCloseInAction)
```

#### <a name="possible-reasons-and-solutions"></a>Posibles motivos y soluciones

- SQL Server no admite TLS 1.2

  Este error se produce normalmente en entornos cliente como contenedores de imágenes de Docker, clientes UNIX o clientes Windows, donde TLS 1.2 es el protocolo TLS mínimo admitido.

  **Solución recomendada:** instale las actualizaciones más recientes en las versiones admitidas de SQL Server <sup>1</sup> y asegúrese de que el protocolo TLS 1.2 está habilitado en el servidor.

  _<sup>1</sup> Vea el [ciclo de vida de soporte del controlador SqlClient](sqlclient-driver-support-lifecycle.md) para consultar la lista de versiones admitidas de SQL Server con diferentes versiones de Microsoft.Data.SqlClient._

  **Solución no segura:** configure los valores de TLS/SSL en el entorno de cliente/imagen de Docker para conectarse con TLS 1.0.

  ```docker
  MinProtocol = TLSv1
  CipherString = DEFAULT@SECLEVEL=1
  ```

  > [!NOTE]
  > Al conectarse con Microsoft.Data.SqlClient v2.0+ desde un entorno de Windows/Linux con TLS 1.0 o TLS 1.1, aparecerá un mensaje de advertencia de seguridad si el servidor SQL Server de destino y el cliente no pueden negociar un mínimo de la versión de TLS 1.2 al establecer la conexión: `Security Warning: The negotiated <TLS1.0 | TLS1.1> is an insecure protocol and is supported for backward compatibility only. The recommended protocol version is TLS 1.2 and later.`

- Cifrado aplicado de SQL Server

  Si el servidor de destino es una instancia de Azure SQL o un servidor SQL Server local con la propiedad "Forzar cifrado" activada, se realizará una conexión cifrada para la que el cliente debe establecer una relación de confianza con el servidor.

  **Solución recomendada:** hay dos opciones disponibles para solucionar este problema:

    1. Instale el certificado TLS/SSL del servidor SQL Server de destino en el entorno del cliente. Se validará si es necesario el cifrado.
    2. Establezca la propiedad "Trust Server Certificate = true" en la cadena de conexión.

  **Solución no segura:** deshabilite la opción "Forzar cifrado" en SQL Server.

- Certificados TLS/SSL no firmados con SHA-256 o superior.

  **Solución recomendada:** genere un nuevo certificado TLS/SSL para el servidor cuyo hash esté firmado con al menos el algoritmo hash SHA-256.

- Conjuntos de cifrado restringidos en Linux con .NET 5 o posterior

  .NET 5 presentó un cambio importante para los clientes de Linux, donde se utiliza de forma predeterminada una lista restringida de conjuntos de cifrado permitidos. Es posible que tenga que expandir la lista de conjuntos de cifrado predeterminados para aceptar clientes heredados (o para ponerse en contacto con servidores heredados), especificando un valor `CipherSuitePolicy` o cambiando el archivo de configuración de _OpenSSL_.
  
  Obtenga más información sobre los [conjuntos de cifrado TLS predeterminados para .NET en Linux ](/dotnet/core/compatibility/cryptography/5.0/default-cipher-suites-for-tls-on-linux) para la acción recomendada.

### <a name="connection-pool-exhaustion-errors"></a>Errores de agotamiento del grupo de conexiones

Seguimiento de la pila observado:

```log
System.InvalidOperationException: Timeout expired. The timeout period elapsed prior to obtaining a connection from the pool.
This may have occurred because all pooled connections were in use and max pool size was reached.
```

#### <a name="possible-reasons-and-solutions"></a>Posibles motivos y soluciones

La aplicación cliente está abriendo más conexiones de las que el grupo de conexiones puede mantener activas en un momento dado.

**Solución recomendada:** configure la propiedad de conexión "Max Pool Size" en un valor más alto y cierre las conexiones sin usar de manera oportuna.

## <a name="contact-support"></a>Ponerse en contacto con el servicio de soporte técnico

Si esta guía no soluciona los problemas de conectividad, puede ver las incidencias existentes en el repositorio [dotnet/sqlclient](https://github.com/dotnet/SqlClient) y publicar una nueva si fuera necesario.
