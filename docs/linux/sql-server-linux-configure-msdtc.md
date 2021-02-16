---
title: Cómo configurar el Coordinador de transacciones distribuidas de Microsoft (MSDTC) en Linux
description: En este artículo, aprenderá a configurar el Coordinador de transacciones distribuidas de Microsoft (MSDTC) en Linux.
author: VanMSFT
ms.author: vanto
ms.date: 08/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: a2013fb1e32d14bed4b92aa0cc2394ad3a5c9174
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349119"
---
# <a name="how-to-configure-the-microsoft-distributed-transaction-coordinator-msdtc-on-linux"></a>Cómo configurar el Coordinador de transacciones distribuidas de Microsoft (MSDTC) en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se describe cómo configurar el Coordinador de transacciones distribuidas de Microsoft (MSDTC) en Linux.

> [!NOTE]
> El Coordinador de transacciones distribuidas en Linux se admite en SQL Server 2017 a partir de la actualización acumulativa 16.

## <a name="overview"></a>Información general

Las transacciones distribuidas se habilitan en SQL Server en Linux mediante la inclusión de la funcionalidad de asignador de puntos de conexión de MSDTC y RPC en SQL Server. Un proceso de asignador de puntos de conexión de RPC escucha las solicitudes de RPC entrantes en el puerto 135 de forma predeterminada, y facilita información de los componentes registrados a las solicitudes remotas. Las solicitudes remotas pueden usar la información devuelta por el asignador de puntos de conexión para comunicarse con los componentes de RPC registrados, como los servicios de MSDTC. Un proceso requiere privilegios de superusuario para enlazar con puertos conocidos (números de puerto inferiores a 1024) en Linux. Para evitar que SQL Server se inicie con privilegios raíz en el proceso del asignador de puntos de conexión de RPC, los administradores del sistema deben usar iptables para crear la traducción de direcciones de red con objeto de enrutar el tráfico del puerto 135 al proceso de asignador de puntos de conexión de RPC de SQL Server.

El Coordinador de transacciones distribuidas usa dos parámetros de configuración para la utilidad mssql-conf:

| Parámetro de mssql-conf | Descripción |
|---|---|
| **network.rpcport** | Puerto TCP al que se enlaza el asignador de puntos de conexión de RPC. |
| **distributedtransaction.servertcpport** | Puerto en el que escucha el servidor de MSDTC. Si no se establece, el servicio de MSDTC usa un puerto efímero aleatorio en los reinicios del servicio, y será necesario reconfigurar las excepciones de firewall para asegurarse de que el servicio de MSDTC pueda continuar con la comunicación. |

Para más información sobre estas y otras opciones de configuración relativas a MSDTC, vea [Configuración de SQL Server en Linux con la herramienta mssql-conf](sql-server-linux-configure-mssql-conf.md).

## <a name="supported-transaction-standards"></a>Estándares de transacciones admitidos

Se admiten las siguientes configuraciones de MSDTC:

| Estándar de transacciones | Orígenes de datos | Controlador ODBC | Controlador JDBC|
|---|---|---|---|
| Transacciones OLE-TX | SQL Server en Linux | Sí | No|
| Transacciones distribuidas XA | SQL Server, otras ODBC y orígenes de datos de JDBC que admiten XA | Sí (se requiere la versión 17.3 o posterior) | Yes |
| Transacciones distribuidas en el servidor vinculado | SQL Server | Sí | No

Para obtener más información, vea [Descripción de las transacciones XA](../connect/jdbc/understanding-xa-transactions.md#configuration-instructions).

## <a name="msdtc-configuration-steps"></a>Pasos de configuración de MSDTC

Hay tres pasos para configurar la comunicación y la funcionalidad de MSDTC. Si no se realizan los pasos de configuración necesarios, SQL Server no habilitará la funcionalidad de MSDTC.

- Configure **network.rpcport** y **distributedtransaction.servertcpport** mediante mssql-conf.
- Configure el firewall para permitir la comunicación en **distributedtransaction.servertcpport** y en el puerto 135.
- Configure el enrutamiento del servidor Linux para que la comunicación RPC en el puerto 135 se redirija al puerto **network.rpcport** de SQL Server.

En las siguientes secciones se proporciona información detallada sobre cada paso.

## <a name="configure-rpc-and-msdtc-ports"></a>Configuración de los puertos de RPC y MSDTC

En primer lugar, configure **network.rpcport** y **distributedtransaction.servertcpport** mediante mssql-conf. Este paso es específico de SQL Server y común a todas las distribuciones admitidas.

1. Use mssql-conf para establecer el valor de **network.rpcport**. En el siguiente ejemplo se establece en 13500.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set network.rpcport 13500
   ```

2. Establezca el valor de **distributedtransaction.servertcpport**. En el siguiente ejemplo se establece en 51999.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set distributedtransaction.servertcpport 51999
   ```

3. Reinicie SQL Server.

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="configure-the-firewall"></a>Configuración del firewall

El segundo paso consiste en configurar el firewall para permitir la comunicación en **servertcpport** y en el puerto 135.  Esto hace posible que el proceso de asignación de puntos de conexión de RPC y el proceso de MSDTC se comuniquen externamente con otros coordinadores y administradores de transacciones. Los pasos reales para llevar esto a cabo variarán en función del firewall y de la distribución de Linux. 

En el siguiente ejemplo se muestra cómo crear estas reglas en **Ubuntu**.

```bash
sudo ufw allow from any to any port 51999 proto tcp
sudo ufw allow from any to any port 135 proto tcp
```

En el siguiente ejemplo se muestra cómo podría realizarse este procedimiento en **Red Hat Enterprise Linux (RHEL)** :

```bash
sudo firewall-cmd --zone=public --add-port=51999/tcp --permanent
sudo firewall-cmd --zone=public --add-port=135/tcp --permanent
sudo firewall-cmd --reload
```

Es importante configurar el firewall antes de configurar el enrutamiento de puertos en la sección siguiente. Si el firewall se actualiza, en algunos casos se podrían borrar las reglas de enrutamiento de puertos.

## <a name="configure-port-routing"></a>Configuración del enrutamiento de puertos

Configure la tabla enrutamiento del servidor Linux para que la comunicación RPC en el puerto 135 se redirija al puerto **network.rpcport** de SQL Server. El mecanismo de configuración para el reenvío de puertos puede diferir según la distribución. En las siguientes secciones se proporcionan instrucciones relativas a Ubuntu, SUS Enterprise Linux (SLES) y Red Hat Enterprise Linux (RHEL).

### <a name="port-routing-in-ubuntu-and-sles"></a>Enrutamiento de puertos en Ubuntu y SLES

Ubuntu y SLES no usan el servicio **firewalld**, de modo que las reglas **iptable** son un mecanismo eficaz para lograr el enrutamiento de puertos. Es posible que las reglas **iptable** no se conserven durante los reinicios; es por eso que en los siguientes comandos también se proporcionan instrucciones para restaurar las reglas después de un reinicio.

1. Cree reglas de enrutamiento para el puerto 135. En el siguiente ejemplo, el puerto 135 se dirige al puerto RPC (13500), que se definió en la sección anterior. Reemplace `<ipaddress>` por la dirección IP de su servidor.

   ```bash
   sudo iptables -t nat -A PREROUTING -d <ip> -p tcp --dport 135 -m addrtype --dst-type LOCAL  \
      -j DNAT --to-destination <ip>:13500 -m comment --comment RpcEndPointMapper
   sudo iptables -t nat -A OUTPUT -d <ip> -p tcp --dport 135 -m addrtype --dst-type LOCAL \
      -j DNAT --to-destination <ip>:13500 -m comment --comment RpcEndPointMapper
   ```

   El parámetro `--comment RpcEndPointMapper` de los comandos anteriores ayuda a administrar estas reglas en comandos posteriores.

2. Vea las reglas de enrutamiento que creó con el siguiente comando:

   ```bash
   sudo iptables -S -t nat | grep "RpcEndPointMapper"
   ```

3. Guarde las reglas de enrutamiento en un archivo en el equipo.

   ```bash
   sudo iptables-save > /etc/iptables.conf
   ```

4. Para volver a cargar las reglas tras un reinicio, agregue el siguiente comando a `/etc/rc.local` (Ubuntu) o a `/etc/init.d/after.local` (SLES):

   ```bash
   iptables-restore < /etc/iptables.conf
   ```

   > [!NOTE]
   > Debe tener privilegios de superusuario (sudo) para editar los archivos **rc.local** o **after.local**.

Los comandos **iptables-save** e **iptables-restore**, junto con la configuración de inicio de `rc.local`/`after.local`, proporcionan un mecanismo básico para guardar y restaurar entradas de iptables. En función de la distribución de Linux, es posible que haya opciones más avanzadas o automatizadas. Por ejemplo, una alternativa de Ubuntu es el paquete **iptables-persistent** para que las entradas sean persistentes.

> [!IMPORTANT]
> En los pasos anteriores se da por hecho que la dirección IP es fija. Si la dirección IP de la instancia de SQL Server cambia (por una intervención manual o mediante DHCP), deberá quitar y volver a crear las reglas de enrutamiento si se crearon con iptables. Si necesita volver a crear o eliminar las reglas de enrutamiento existentes, puede usar el siguiente comando para quitar las reglas `RpcEndPointMapper` anteriores:
> 
> ```bash
> sudo iptables -S -t nat | grep "RpcEndPointMapper" | sed 's/^-A //' | while read rule; do iptables -t nat -D $rule; done
> ```

### <a name="port-routing-in-rhel"></a>Enrutamiento de puertos en RHEL

En las distribuciones que usan el servicio **firewalld** (como Red Hat Enterprise Linux), se puede usar el mismo servicio para abrir el puerto en el servidor y para el reenvío de puerto interno. Por ejemplo, en Red Hat Enterprise Linux, debe usar el servicio **firewalld** (a través de la utilidad de configuración **firewall-cmd** con `-add-forward-port` u opciones similares) para crear y administrar reglas de reenvío de puertos persistentes en lugar de usar iptables.

```bash
sudo firewall-cmd --permanent --add-forward-port=port=135:proto=tcp:toport=13500
sudo firewall-cmd --reload
```

## <a name="verify"></a>Comprobación

En este punto, SQL Server debería poder participar en las transacciones distribuidas. Para confirmar que SQL Server está escuchando, ejecute el comando **netstat** (si usa RHEL, es posible que antes tenga que instalar el paquete **net-tools**):

```bash
sudo netstat -tulpn | grep sqlservr
```

Debería ver una salida similar a esta:

```bash
tcp 0 0 0.0.0.0:1433 0.0.0.0:* LISTEN 13911/sqlservr
tcp 0 0 127.0.0.1:1434 0.0.0.0:* LISTEN 13911/sqlservr
tcp 0 0 0.0.0.0:13500 0.0.0.0:* LISTEN 13911/sqlservr
tcp 0 0 0.0.0.0:51999 0.0.0.0:* LISTEN 13911/sqlservr
tcp6 0 0 :::1433 :::* LISTEN 13911/sqlservr
tcp6 0 0 ::1:1434 :::* LISTEN 13911/sqlservr
tcp6 0 0 :::13500 :::* LISTEN 13911/sqlservr
tcp6 0 0 :::51999 :::* LISTEN 13911/sqlservr
```

Sin embargo, después de un reinicio, SQL Server no empieza a escuchar en **servertcpport** hasta la primera transacción distribuida. En este caso, no verá SQL Server escuchando en el puerto 51999 en este ejemplo hasta la primera transacción distribuida.

## <a name="configure-authentication-on-rpc-communication-for-msdtc"></a>Configuración de la autenticación en la comunicación RPC para MSDTC

MSDTC para SQL Server en Linux no usa de forma predeterminada la autenticación en la comunicación RPC. Sin embargo, cuando el equipo host se une a un dominio de Active Directory (AD), MSDTC se puede configurar para que use la comunicación RPC autenticada mediante las siguientes opciones de **mssql-conf**:

| Configuración | Descripción |
|---|---|
| **distributedtransaction.allowonlysecurerpccalls**          | Configure solo llamadas RPC seguras para transacciones distribuidas. El valor predeterminado es 0. |
| **distributedtransaction.fallbacktounsecurerpcifnecessary** | Configure solo llamadas RPC de seguridad para transacciones distribuidas. El valor predeterminado es 0. |
| **distributedtransaction.turnoffrpcsecurity**               | Habilite o deshabilite la seguridad RPC de las transacciones distribuidas. El valor predeterminado es 0. |

## <a name="additional-guidance"></a>Instrucciones adicionales

### <a name="active-directory"></a>Active Directory

Microsoft recomienda el uso del Coordinador de transacciones distribuidas con RPC habilitado si SQL Server está inscrito en una configuración de Active Directory (AD). Si SQL Server está configurado para usar la autenticación de AD, el Coordinador de transacciones distribuidas utiliza de forma predeterminada la seguridad RPC de autenticación mutua.

### <a name="windows-and-linux"></a>Windows y Linux

Si un cliente de un sistema operativo Windows necesita darse de alta en una transacción distribuida con SQL Server en Linux, deberá tener la siguiente versión mínima del sistema operativo Windows:

| Sistema operativo | Versión mínima | Compilación del sistema operativo |
|---|---|---|
| [Windows Server](/windows-server/get-started/windows-server-release-info) | 1903 | 18362.30.190401-1528 |
| [Windows 10](/windows/release-information/) | 1903 | 18362.267 |

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre SQL Server en Linux, consulte [SQL Server en Linux](sql-server-linux-overview.md).