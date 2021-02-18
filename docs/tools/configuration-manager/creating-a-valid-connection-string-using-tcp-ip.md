---
title: Crear una cadena de conexión válida con TCP/IP
description: Aprenda a crear una cadena de conexión válida al usar TCP/IP para conectarse a una instancia de SQL Server. Vea ejemplos de cadenas válidas.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- connection strings [Database Engine]
- ports [SQL Server], connecting to
- TCP/IP [SQL Server], connection strings
- connection strings [Database Engine], TCP/IP
- aliases [SQL Server], TCP/IP
ms.assetid: ee5dbc2c-1fc6-42bd-bdf5-efa792557934
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: b40390a942ffd554955daa3762cc4da99781a131
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349808"
---
# <a name="creating-a-valid-connection-string-using-tcp-ip"></a>Crear una cadena de conexión válida con TCP/IP
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Para crear una cadena de conexión válida con TCP/IP, debe:  
  
-   Especifique un **Nombre de alias**.  
  
-   En el cuadro **Servidor**, escriba un nombre de servidor al que se pueda conectar con la herramienta **PING** , o bien una dirección IP a la que se pueda conectar con la herramienta **PING** . Para una instancia con nombre, incluya el nombre de la instancia.  
  
-   Especifique **TCP/IP** como el **Protocolo**.  
  
-   Opcionalmente, especifique un nombre de puerto en **Nº de puerto**. El valor predeterminado es 1433, que es el número de puerto de la instancia predeterminada de [!INCLUDE[ssDE](../../includes/ssde-md.md)] en un servidor. Para conectarse a una instancia con nombre o una instancia predeterminada que no escuche en el puerto 1433, debe proporcionar un número de puerto o iniciar el servicio Explorador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para más información sobre la configuración del servicio Explorador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vea [Servicio SQL Server Browser](../../tools/configuration-manager/sql-server-browser-service.md).  
  
 En el momento de la conexión, el componente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client lee los valores de servidor, protocolo y puerto del Registro para el nombre de alias especificado, y crea una cadena de conexión con el formato `tcp:<servername>[\<instancename>],<port>` o `tcp:<IPAddress>[\<instancename>],<port>`.  
  
> [!NOTE]
>  De forma predeterminada, el Firewall de Windows de [!INCLUDE[msCoName](../../includes/msconame-md.md)] cierra el puerto 1433. Dado que [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se comunica a través del puerto 1433, debe volver a abrir el puerto si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se configura para escuchar las conexiones de cliente entrantes que usan TCP/IP. Para obtener más información acerca de cómo configurar un firewall, vea “Cómo configurar un firewall para el acceso a SQL Server” en los Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o revise la documentación del firewall.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client son totalmente compatibles con el Protocolo de Internet versión 4 (IPv4) y con el Protocolo de Internet versión 6 (IPv6). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] acepta los formatos de IPv4 e IPv6 para direcciones IP. Para obtener más información sobre IPv6, vea el tema sobre la conexión mediante IPv6 en los Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="connecting-to-the-local-server"></a>Conectarse al servidor local  
 Al conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuando se ejecuta en el mismo equipo que el cliente, puede utilizar `(local)` como el nombre del servidor. Esta posibilidad no se recomienda ya que genera ambigüedad, pero puede ser útil cuando se sabe que el cliente se ejecuta en el equipo de destino. Por ejemplo, al crear una aplicación para usuarios desconectados móviles, como puede ser el personal de ventas, en la que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecutará en equipos portátiles y se almacenarán datos de proyectos, un cliente que se conecte a `(local)` siempre se conectará al servidor [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se ejecuta en el equipo portátil. En lugar de `localhost` , es posible usar la palabra **o un punto (** . `(local)`).  
  
## <a name="verifying-your-connection-protocol"></a>Comprobar el protocolo de conexión  
 La siguiente consulta devolverá el protocolo utilizado para la conexión actual.  
  
```  
SELECT net_transport   
FROM sys.dm_exec_connections   
WHERE session_id = @@SPID;  
  
```  
  
## <a name="examples"></a>Ejemplos  
 Conectarse por el nombre de servidor:  
  
```  
Alias Name         <serveralias>  
Port No            <blank>  
Protocol           TCP/IP  
Server             <servername>  
  
```  
  
 Conectarse por el nombre de servidor a una instancia con nombre:  
  
```  
Alias Name         <serveralias>  
Port No            <blank>  
Protocol           TCP/IP  
Server             <servername>\<instancename>  
  
```  
  
 Conectarse por el nombre de servidor a un puerto especificado:  
  
```  
Alias Name         <serveralias>  
Port No            <port>  
Protocol           TCP/IP  
Server             <servername>  
  
```  
  
 Conectarse por la dirección IP:  
  
```  
Alias Name         <serveralias>  
Port No            <blank>  
Protocol           TCP/IP  
Server             <IPAddress>  
  
```  
  
 Conectarse mediante la dirección IP a una instancia con nombre:  
  
```  
Alias Name         <serveralias>  
Port No            <blank>  
Protocol           TCP/IP  
Server             <IPAddress>\<instancename>  
  
```  
  
 Conectarse mediante la dirección IP a un puerto especificado:  
  
```  
Alias Name         <serveralias>  
Port No            <port number>  
Protocol           TCP/IP  
Server             <IPAddress>  
  
```  
  
 Conectarse al equipo local utilizando `(local)`:  
  
```  
Alias Name         <serveralias>  
Port No            <blank>  
Protocol           TCP/IP  
Server             (local)  
  
```  
  
 Conectarse al equipo local utilizando `localhost`:  
  
```  
Alias Name         <serveralias>  
Port No            <blank>  
Protocol           TCP/IP  
Server             localhost  
  
```  
  
 Conectarse a una instancia con nombre en el equipo local utilizando `localhost`:  
  
```  
Alias Name         <serveralias>  
Port No            <blank>  
Protocol           TCP/IP  
Server             localhost\<instancename>  
  
```  
  
 Conectarse al equipo local utilizando un punto:  
  
```  
Alias Name         <serveralias>  
Port No            <blank>  
Protocol           TCP/IP  
Server             .  
  
```  
  
 Conectarse a una instancia con nombre en el equipo local usando un punto:  
  
```  
Alias Name         <serveralias>  
Port No            <blank>  
Protocol           TCP/IP  
Server             .\<instancename>  
  
```  
  
> [!NOTE]  
>   Para más información sobre la especificación del protocolo de red como un parámetro **sqlcmd** , consulte "Conectarse al motor de base de datos con sqlcmd" en los Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a>Consulte también  
 [Crear una cadena de conexión válida con el protocolo de memoria compartida](../../tools/configuration-manager/creating-a-valid-connection-string-using-shared-memory-protocol.md)   
 [Crear una cadena de conexión válida con canalizaciones con nombre](/previous-versions/sql/sql-server-2016/ms189307(v=sql.130))   
 [Elegir un protocolo de red](/previous-versions/sql/sql-server-2016/ms187892(v=sql.130))  
  
