---
title: Solución de problemas de conexión al Motor de base de datos de SQL Server
description: Obtenga información sobre cómo solucionar problemas de conexión. Vea los pasos que se deben llevar a cabo cuando no se puede usar TCP/IP para conectarse a un Motor de base de datos de SQL Server en un solo servidor.
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: troubleshooting
helpviewer_keywords:
- troubleshooting, connecting to Database Engine
- connecting to Database Engine, troubleshooting
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: sqlfreshmay19
ms.date: 11/25/2019
ms.openlocfilehash: cd4dae49fb19532e9aee082e4046c4d79dbf0a82
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99238000"
---
# <a name="troubleshoot-connecting-to-the-sql-server-database-engine"></a>Solución de problemas de conexión al Motor de base de datos de SQL Server

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

En este artículo se proporciona una lista exhaustiva de técnicas de solución de problemas que se pueden usar cuando no es posible conectarse a una instancia del Motor de base de datos de SQL Server en un único servidor.

>[!NOTE]
>Para otros escenarios, consulte:
>- [Agente de escucha de grupo de disponibilidad](../availability-groups/windows/listeners-client-connectivity-application-failover.md)
>- [Instancias de clúster de conmutación por error](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)

Los problemas no están ordenados de más habituales a menos, sino de más básicos a más complejos. Se da por sentado que se está conectando a la instancia de SQL Server desde otro equipo mediante el protocolo TCP/IP, que es la situación más habitual.

Estas instrucciones resultan útiles al solucionar el error "**Conectar con el servidor**", que puede ser `Error Number: 11001 (or 53), Severity: 20, State: 0`. A continuación se presenta un ejemplo de un mensaje de error:

> `A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections.`
>
> `(provider: Named Pipes Provider, error: 40 - Could not open a connection to SQL Server) (Microsoft SQL Server, Error: 53)`
>
> `(provider: TCP Provider, error: 0 - No such host is known.) (Microsoft SQL Server, Error: 11001)`

Este error normalmente significa que el cliente no puede encontrar la instancia de SQL Server. Esto ocurre normalmente cuando existe al menos uno de los siguientes problemas:

- Hay un problema con el nombre del equipo que aloja SQL Server.
- La instancia no resuelve la dirección IP correcta.
- No se ha especificado correctamente el número de puerto TCP.

> [!TIP]
> En [Solving Connectivity errors to SQL Server](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server) (Resolver errores de conectividad de SQL Server) encontrará una página interactiva de solución de problemas de los servicios de soporte al cliente de [!INCLUDE[msCoName_md](../../includes/msconame-md.md)].

## <a name="not-included"></a>No incluido

- En este tema no se incluye información sobre los errores SSPI. Para ver los errores SSPI, consulte [Cómo solucionar el mensaje de error "No se puede generar contexto SSPI"](https://support.microsoft.com/kb/811889).
- En este tema no se incluye información sobre los errores Kerberos. Para obtener ayuda, vea [Microsoft Kerberos Configuration Manager para SQL Server](https://www.microsoft.com/download/details.aspx?id=39046).
- En este tema no se incluye información sobre la conectividad de Azure SQL Database. Para obtener ayuda, vea [Solucionar problemas de conectividad con Microsoft Azure SQL Database](/azure/sql-database/troubleshoot-connectivity-issues-microsoft-azure-sql-database).

## <a name="get-instance-name-from-configuration-manger"></a>Obtención del nombre del administrador de configuración

En el servidor que hospeda la instancia de SQL Server, compruebe el nombre de la instancia. Use el [Administrador de configuración de SQL Server](../../relational-databases/sql-server-configuration-manager.md).

El Administrador de configuración se instala automáticamente en el equipo al instalar SQL Server. Las instrucciones para iniciar el Administrador de configuración varían ligeramente según la versión de SQL Server y Windows. Para obtener detalles concretos de la versión, consulte [Administrador de configuración de SQL Server](../../relational-databases/sql-server-configuration-manager.md).

1. Inicie sesión en el equipo que hospeda la instancia de SQL Server.
2. Inicie el Administrador de configuración de SQL Server.
3. En el panel izquierdo, seleccione **Servicios de SQL Server**.
4. En el panel derecho, compruebe el nombre de la instancia del motor de base de datos.

   - `SQL SERVER (MSSQLSERVER)` indica una instancia predeterminada de SQL Server. El nombre de la instancia predeterminada es `<computer name>`.
   - `SQL SERVER (<instance name>)` indica una instancia con nombre de SQL Server. El nombre de la instancia es `<computer name>\<instance name>`.

## <a name="verify---the-instance-is-running"></a>Comprobación de que la instancia se está ejecutando

Para comprobar que la instancia se está ejecutando, en Configuration Manager mire el símbolo de la instancia de SQL Server.

- Una flecha verde indica que una instancia se está ejecutando.
- Un cuadrado rojo indica que una instancia está detenida.

Si la instancia se detiene, haga clic en ella con el botón derecho y luego haga clic en **Iniciar**. Se inicia la instancia del servidor y el indicador se convierte en una flecha verde.

## <a name="verify---sql-server-browser-service-is-running"></a><a name = "startbrowser"></a> Comprobación: ejecución del servicio SQL Server Browser

Para conectarse a una instancia con nombre, el servicio SQL Server Browser debe estar ejecutándose. En Configuration Manager, busque el servicio **SQL Server Browser** y compruebe que se está ejecutando. Si no se está ejecutando, inícielo. No es necesario el servicio SQL Server Browser para las instancias predeterminadas.

Una instancia predeterminada de SQL Server no requiere el servicio SQL Server Browser.

## <a name="testing-a-local-connection"></a>Prueba de una conexión local

Antes de solucionar un problema de conexión desde otro equipo, pruebe la capacidad de conectarse desde una aplicación cliente instalada localmente en el equipo con SQL Server. La conexión local evita problemas con redes y cortafuegos. 

En este procedimiento se usa SQL Server Management Studio. Si no tiene instalado Management Studio, consulte [Descarga de SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md). Si no puede instalar Management Studio, puede probar la conexión con la utilidad `sqlcmd.exe`. `sqlcmd.exe` se instala con el Motor de base de datos. Para obtener información sobre `sqlcmd.exe`, vea [sqlcmd (utilidad)](../../tools/sqlcmd-utility.md)).

1. Inicie sesión en el equipo donde está instalado SQL Server con un inicio de sesión que tenga permiso para acceder a SQL Server. (Durante la instalación, SQL Server necesita que se especifique al menos un inicio de sesión como administrador de SQL Server. Si no conoce ningún administrador, consulte [Conectarse a SQL Server cuando los administradores del sistema no tienen acceso](connect-to-sql-server-when-system-administrators-are-locked-out.md)).
2. En la página Iniciar, escriba **SQL Server Management Studio**, o bien, en el menú Inicio de versiones anteriores de Windows, seleccione **Todos los programas**, **Microsoft SQL Server** y luego haga clic en **SQL Server Management Studio**.
3. En el cuadro de diálogo **Conectar con el servidor** , en el cuadro de tipo **Servidor** , seleccione **Motor de base de datos**. En el cuadro **Autenticación** , seleccione **Autenticación de Windows**. En el cuadro **Nombre del servidor**, escriba uno de los siguientes tipos de conexión:

   |Conectar con|Tipo|Ejemplo|
   |:-----------------|:---------------|:-----------------|
   |Instancia predeterminada|`<computer name>`|`ACCNT27`|
   |Instancia con nombre|`<computer name\instance name>`|`ACCNT27\PAYROLL`|

   > [!NOTE]
   > Al conectarse a SQL Server desde una aplicación cliente del mismo equipo, se usa el protocolo de memoria compartida. La memoria compartida es un tipo de canalización local con nombre, así que a veces se producen errores relacionados con las canalizaciones.

   Si aparece un error en este punto, tendrá que resolverlo antes de continuar. Hay muchos aspectos que podrían plantear problemas. El inicio de sesión podría no estar autorizado a conectarse. Podría faltar la base de datos predeterminada.

   > [!NOTE]
   > Algunos mensajes de error que se pasan al cliente no proporcionan suficiente información para solucionar el problema a propósito. Se trata de una característica de seguridad para evitar proporcionar información sobre SQL Server a cualquier atacante. Para ver la información completa sobre el error, busque en el registro de errores de SQL Server. Allí se proporcionan los detalles. 

4. Si recibe el error `18456 Login failed for user`, el tema de los Libros en pantalla [MSSQLSERVER_18456](../../relational-databases/errors-events/mssqlserver-18456-database-engine-error.md) contiene información adicional sobre los códigos de error. Además, el blog de Aaron Bertrand ofrece una lista extensa de códigos de error en [Solucionar el error 18456](https://sqlblog.org/2011/01/14/troubleshooting-error-18456). Puede ver el registro de errores con SSMS (si se puede conectar), en la sección Administración del Explorador de objetos. De lo contrario, puede ver el registro de errores con el programa Bloc de notas de Windows. La ubicación predeterminada varía en función de la versión y se puede cambiar durante la instalación. La ubicación predeterminada de [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] es `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Log\ERRORLOG`. 

5. Si puede conectar con la memoria compartida, pruebe la conexión mediante TCP. Puede forzar una conexión TCP si especifica `tcp:` antes del nombre. Por ejemplo:

   |Conectar con:|Escriba:|Ejemplo:|
   |-----------------|---------------|-----------------|
   |Instancia predeterminada|`tcp:<computer name>`|`tcp:ACCNT27`|
   |Instancia con nombre|`tcp:<computer name/instance name>`|`tcp:ACCNT27\PAYROLL`|

6. Si puede conectar con la memoria compartida pero no con TCP, debe solucionar el problema de TCP. Lo más probable es que TCP no esté habilitado. Para habilitar TCP, consulte los pasos anteriores de [Habilitar protocolos](#enableprotocols).

7. Si su objetivo es conectar con una cuenta que no sea la de un administrador, una vez que pueda conectarse como administrador, vuelva a probar la conexión con el inicio de sesión de autenticación de Windows o el inicio de sesión de autenticación de SQL Server que la aplicación cliente está usando.

## <a name="get-the-ip-address-of-the-server"></a>Obtención de la dirección IP del servidor

Obtenga la dirección IP del equipo que hospeda la instancia de SQL Server.

1. En el menú Iniciar, haga clic en **Ejecutar**. En la ventana **Ejecutar**, escriba **cmd** y haga clic en **Aceptar**.
2. En la ventana de símbolo del sistema, escriba `ipconfig` y pulse ENTRAR. Anote la dirección **IPv4** y la dirección **IPv6** .

  >SQL Server puede conectarse mediante el protocolo IP versión 4 o el protocolo IP versión 6. La red podría permitir uno o ambos. La mayoría de los usuarios comienza por solucionar los problemas de la dirección **IPv4** . Es más corta y más fácil de escribir.

## <a name="get-the-sql-server-instance-tcp-port"></a><a name = "getTCP"></a>Obtención del puerto TCP de la instancia de SQL Server

En la mayoría de los casos, se conecta al Motor de base de datos desde otro equipo mediante el protocolo TCP.

1. Con SQL Server Management Studio en el equipo que ejecuta SQL Server, conéctese a la instancia de SQL Server. En el Explorador de objetos, expanda **Administración**, **Registros de SQL Server** y luego haga doble clic en el registro actual.
2. En el Visor de registros, haga clic en el botón **Filtrar** de la barra de herramientas. En el cuadro **El mensaje contiene texto**, escriba `server is listening on`, haga clic en **Aplicar filtro** y después en **Aceptar**.
3. Debería aparecer un mensaje similar a `Server is listening on [ 'any' <ipv4> 1433]`. 

Este mensaje indica que esta instancia de SQL Server está escuchando en todas las direcciones IP de este equipo (para IP versión 4) y está escuchando al puerto TCP 1433. (El puerto TCP 1433 suele ser el que usa el motor de base de datos o una instancia predeterminada de SQL Server. Solo una instancia de SQL Server puede usar un puerto, por lo que si hay más de una instancia de SQL Server instalada, algunas instancias deben usar otros números de puerto). Anote el número de puerto usado por la instancia de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] a la que está intentando conectarse.

  > [!NOTE]
  > Probablemente aparezca `IP address 127.0.0.1`. Se denomina la dirección del adaptador de bucle invertido. Solo se pueden usar los procesos en el mismo equipo para conectarse. Puede ser útil para solucionar problemas, pero no se puede usar para conectarse desde otro equipo.

## <a name="enable-protocols"></a><a name = "enableprotocols"></a>Habilitación de protocolos

En algunas instalaciones de SQL Server, no se puede conectar al motor de base de datos desde otro equipo a menos que un administrador use el Administrador de configuración para permitirlo. Para habilitar conexiones desde otro equipo:

1. Abra el Administrador de configuración de SQL Server como se explicó anteriormente.
2. En el panel izquierdo del Administrador de configuración, expanda **Configuración de red de SQL Server** y luego seleccione la instancia de SQL Server a la que quiere conectarse. El panel derecho muestra los protocolos de conexión disponibles. Normalmente la memoria compartida está habilitada. Solo se puede usar desde el mismo equipo, por lo que en la mayoría de las instalaciones se deja habilitada. Para conectarse a SQL Server desde otro equipo, normalmente se usa TCP/IP. Si TCP/IP no está habilitado, haga clic con el botón derecho en **TCP/IP** y luego haga clic en **Habilitar**.
3. Si modificó la configuración habilitada de cualquier protocolo, reinicie el Motor de base de datos. En el panel izquierdo, seleccione **Servicios de SQL Server**. En el panel derecho, haga clic con el botón derecho en la instancia del motor de base de datos y luego haga clic en **Reiniciar**.

## <a name="testing-tcpip-connectivity"></a><a name="testTCPIP"></a>Prueba de la conectividad TCP/IP

La conexión a SQL Server mediante TCP/IP exige que Windows pueda establecerla. Use la herramienta `ping` para probar TCP.

1. En el menú Iniciar, haga clic en **Ejecutar**. En la ventana **Ejecutar** , escriba **cmd** y haga clic en **Aceptar**. 
2. En la ventana del símbolo del sistema, escriba `ping <ip address>` y luego la dirección IP del equipo con SQL Server. Por ejemplo:

    - IPv4: `ping 192.168.1.101`
    - IPv6: `ping fe80::d51d:5ab5:6f09:8f48%11`

3. Si la red está configurada correctamente, `ping` devuelve `Reply from <IP address>` seguido de información adicional. Si `ping` devuelve `Destination host unreachable` o `Request timed out`, significa que TCP/IP no está configurado correctamente. Los errores en este punto podrían indicar un problema con el equipo cliente, el equipo servidor o algo relacionado con la red, como un enrutador. Para solucionar problemas de red, consulte [Solución de problemas de TCP/IP avanzada](/windows/client-management/troubleshoot-tcpip).
4. Después, si la prueba de ping mediante la dirección IP fue correcta, compruebe que el nombre del equipo se pueda resolver en la dirección TCP/IP. En el equipo cliente, en la ventana del símbolo del sistema, escriba `ping` y luego el nombre del equipo con SQL Server. Por ejemplo, `ping newofficepc`.
5. Si el `ping` a la dirección IP dirección es ha realizado correctamente, pero el `ping` al equipo devuelve `Destination host unreachable` o `Request timed out`, es posible que tenga información antigua (obsoleta) de la resolución de nombres almacenados en caché en el equipo cliente. Escriba `ipconfig /flushdns` para borrar la caché DNS (resolución de nombres dinámica). Luego vuelva a hacer ping al equipo por nombre. Con la memoria caché DNS vacía, el equipo cliente busca la información más reciente sobre la dirección IP del equipo servidor.
6. Si la red está configurada correctamente, `ping` devuelve `Reply from <IP address>` seguido de información adicional. Si puede hacer ping correctamente al equipo servidor por dirección IP pero recibe un error como `Destination host unreachable.` o `Request timed out.` cuando hace ping por nombre de equipo, la resolución de nombres no está configurada correctamente. (Para obtener más información, vea el artículo de 2006 mencionado anteriormente: [How to Troubleshoot Basic TCP/IP Problems [Cómo solucionar problemas básicos de TCP/IP]](https://support.microsoft.com/kb/169790)). La resolución de nombres correcta no es necesaria para conectarse a SQL Server, pero si no se puede resolver el nombre del equipo en una dirección IP, se debe especificar la dirección IP para realizar las conexiones. La resolución de nombres se puede arreglar más adelante.

## <a name="open-a-port-in-the-firewall"></a>Apertura de un puerto en el firewall

De forma predeterminada, el firewall de Windows está activado y bloquea las conexiones de otro equipo. Para conectar con TCP/IP desde otro equipo, tiene que configurar el firewall en el equipo de SQL Server de modo que permita las conexiones al puerto TCP usado por el motor de base de datos. La instancia predeterminada es escuchar en el puerto TCP 1433. Si tiene instancias con nombre o ha cambiado el puerto de la instancia predeterminada, es posible que el puerto TCP [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] escuche en otro puerto. Consulte [Obtención del puerto TCP de la instancia de SQL Server](#getTCP).

Si se conecta a una instancia con nombre o a un puerto distinto al puerto TCP 1433, también debe abrir el puerto UDP 1434 del servicio SQL Server Browser. Para obtener instrucciones paso a paso sobre la apertura de un puerto de Firewall de Windows, consulte [Configurar Firewall de Windows para el acceso al motor de base de datos](configure-a-windows-firewall-for-database-engine-access.md).

## <a name="test-the-connection"></a>Comprobación de la conexión

Una vez que se pueda conectar con TCP en el mismo equipo, es hora de intentar conectar desde el equipo cliente. En teoría, podría usar cualquier aplicación cliente, pero para evitar más complejidad, instale las herramientas de administración de SQL Server en el cliente y realice el intento con SQL Server Management Studio.

1. En el equipo cliente, con SQL Server Management Studio, intente conectarse mediante la dirección IP y el número de puerto TCP en el formato dirección IP coma número de puerto. Por ejemplo, `192.168.1.101,1433`. Si esta conexión no funciona, es probable que tenga uno de los siguientes problemas:

   - El `ping` de la dirección IP no funciona, lo que indica un problema de configuración general de TCP. Vuelva a la sección [Prueba de la conectividad TCP/IP](#testTCPIP).
   - SQL Server no está escuchando en el protocolo TCP. Vuelva a la sección [Habilitar protocolos](#enableprotocols).
   - SQL Server está escuchando en un puerto distinto al especificado. Vuelva a la sección [Obtención del número de puerto TCP](#getTCP).
   - El firewall está bloqueando el puerto TCP de SQL Server. Vuelva a la sección [Apertura de un puerto del firewall](#open-a-port-in-the-firewall).

2. Una vez que se pueda conectar con el número de puerto y la dirección IP, intente conectarse con la dirección IP y sin número de puerto. En el caso de una instancia predeterminada, use solo la dirección IP. En el caso de una instancia con nombre, use la dirección IP y el nombre de instancia en el formato dirección IP barra diagonal inversa nombre de instancia, por ejemplo, `192.168.1.101\<instance name>`; si esto no funciona, es probable que tenga uno de los siguientes problemas:

   - Si se conecta a la instancia predeterminada, podría estar escuchando en un puerto distinto al puerto TCP 1433 y el cliente no estaría intentando conectarse al número de puerto correcto.
   - Si se conecta a una instancia con nombre, el número de puerto no se devuelve al cliente.

   Ambos problemas están relacionados con el servicio SQL Server Browser, que proporciona el número de puerto al cliente. Las soluciones son:

   - Inicie el servicio SQL Server Browser. Consulte las instrucciones para [iniciar el explorador en Administrador de configuración de SQL Server](#startbrowser).
   - El firewall está bloqueando el servicio SQL Server Browser. Abra el puerto UDP 1434 del firewall. Vuelva a la sección [Apertura de un puerto del firewall](#open-a-port-in-the-firewall). Asegúrese de estar abriendo un puerto UDP, no un puerto TCP.
   - Un enrutador está bloqueando la información del puerto UDP 1434. La comunicación UDP (protocolo de datagramas de usuario) no está diseñada para pasar a través de enrutadores. Esto evita que la red se llene de tráfico de baja prioridad. Puede configurar el enrutador para reenviar el tráfico UDP. También puede optar por proporcionar siempre el número de puerto al conectarse.
   - Si el equipo cliente usa Windows 7 o Windows Server 2008 (o un sistema operativo más reciente), el sistema operativo cliente podría eliminar el tráfico UDP porque la respuesta del servidor se devuelve desde una dirección IP diferente a la que se consultó. Esta es una característica de seguridad que bloquea la "asignación de origen no estricta". Para más información, vea la sección **Varias direcciones IP de servidor** del tema de los Libros en pantalla [Solucionar problemas de: tiempo de espera expirado](/previous-versions/sql/sql-server-2008-r2/ms190181(v=sql.105)). Se trata de un artículo de SQL Server 2008 R2, pero las entidades de seguridad aún son válidas. Puede configurar el cliente para usar la dirección IP correcta. También puede optar por proporcionar siempre el número de puerto al conectarse.

3. Una vez que se pueda conectar con la dirección IP (o la dirección IP y el nombre de instancia de una instancia con nombre), intente conectarse con el nombre de equipo (o el nombre de equipo y el nombre de instancia de una instancia con nombre). Coloque `tcp:` delante del nombre del equipo para forzar una conexión TCP/IP. Por ejemplo, en el caso de la instancia predeterminada en un equipo denominado `ACCNT27`, use `tcp:ACCNT27` ; en el caso de una instancia con nombre denominada `PAYROLL`en ese equipo, use `tcp:ACCNT27\PAYROLL` . Si puede conectarse con la dirección IP pero no con el nombre de equipo, entonces tiene un problema de resolución de nombres. Vuelva a la sección 4 de **Prueba de la conectividad TCP/IP**.

4. Una vez que se conecte mediante el nombre de equipo al forzar TCP, intente conectarse con el nombre de equipo pero sin forzar TCP. Por ejemplo, en el caso de una instancia predeterminada, use solo el nombre de equipo como `CCNT27` ; en el caso de una instancia con nombre, use el nombre de equipo y el nombre de instancia como `ACCNT27\PAYROLL` . Si puede conectarse al forzar TCP, pero no sin hacerlo, es probable que el cliente esté usando otro protocolo (por ejemplo, canalizaciones con nombre).

    1. En el equipo cliente, en el panel izquierdo del Administrador de configuración de SQL Server, expanda **Configuración de SQL Native Client** *versión* **Configuración** y, luego, seleccione **Protocolos de cliente**.
    2. En el panel derecho, asegúrese de que TCP/IP esté habilitado. Si TCP/IP está deshabilitado, haga clic con el botón derecho en **TCP/IP** y luego haga clic en **Habilitar**.
    3. Asegúrese de que el orden de protocolo de TCP/IP sea un número menor que el de los protocolos de canalizaciones con nombre (o VIA en versiones anteriores). Por lo general, debería dejar la memoria compartida como orden 1 y TCP/IP como orden 2. La memoria compartida solo se usa cuando el cliente y SQL Server se están ejecutando en el mismo equipo. Todos los protocolos habilitados se prueban por orden hasta que se establece la conexión, aunque la memoria compartida se omite cuando la conexión no está en el mismo equipo.