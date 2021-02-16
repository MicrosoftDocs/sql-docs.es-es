---
title: Conexión a un agente de escucha del grupo de disponibilidad
description: Contiene información sobre cómo conectarse a una escucha de grupo de disponibilidad Always On, como la forma de conectarse a la réplica principal, una réplica secundaria de solo lectura, usar TLS/SSL y Kerberos.
ms.custom: contperf-fy21q1
ms.date: 02/27/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], listeners
- read-only routing
- read-intent connections [AlwaysOn Availability Groups]
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], read-only routing
- Availability Groups [SQL Server], client connectivity
ms.assetid: 76fb3eca-6b08-4610-8d79-64019dd56c44
author: cawrites
ms.author: chadam
ms.openlocfilehash: 72005ed42951571d4903e42d128378a42b2bc007
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348446"
---
# <a name="connect-to-an-always-on-availability-group-listener"></a>Conexión a un agente de escucha del grupo de disponibilidad Always On 
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  
Una vez que haya [configurado la escucha de grupo de disponibilidad](create-or-configure-an-availability-group-listener-sql-server.md), tendrá que actualizar la cadena de conexión para conectarse a la escucha de grupo de disponibilidad de Always On. Esto redirigirá el tráfico de la aplicación automáticamente a la réplica deseada sin tener que actualizar manualmente la cadena de conexión después de cada conmutación por error. 
  

##  <a name="connect-to-the-primary-replica"></a><a name="ConnectToPrimary"></a> Conexión a la réplica principal  

Para conectarse a la réplica principal para acceso de lectura y escritura, especifique el nombre DNS de la escucha de grupo de disponibilidad en la cadena de conexión. 

Por ejemplo, para conectarse a la réplica principal en SQL Server Management Studio a través del cliente de escucha, escriba el nombre DNS del cliente de escucha en el campo Nombre del servidor: 

![Conexión con el cliente de escucha en SSMS](media/listeners-client-connectivity-application-failover/connect-to-listener-in-ssms.png)


Durante una conmutación por error, cuando la réplica principal cambia, las conexiones existentes con el cliente de escucha se desconectan y las conexiones nuevas se enrutan a la nueva réplica principal.  

Un ejemplo de una cadena de conexión básica para el proveedor ADO.NET (System.Data.SqlClient): 
  
```  
Server=tcp: AGListener,1433;Database=MyDB;Integrated Security=SSPI  
```  

Puede comprobar a qué réplica está conectado actualmente a través del cliente de escucha si ejecuta el siguiente comando de Transact-SQL (T-SQL):

```sql
SELECT @@SERVERNAME
```

Por ejemplo, cuando SQLVM1 es la réplica principal: 

![Comprobación de la conectividad de la réplica](media/listeners-client-connectivity-application-failover/replica-server-name.png)


Todavía se puede conectar directamente a la instancia de SQL Server mediante el nombre de instancia de la réplica principal o secundaria en lugar de usar la escucha de grupo de disponibilidad. Pero perderá la ventaja de que las nuevas conexiones se enruten de forma automática a la nueva réplica principal actual. Además, perderá la ventaja del enrutamiento de solo lectura, donde las conexiones especificadas con `read-intent` se enrutan de forma automática a la réplica secundaria legible. 

##  <a name="connect-to-a-read-only-replica"></a><a name="ConnectToSecondary"></a> Conexión a una réplica de solo lectura 

El _enrutamiento de solo lectura_ se refiere al enrutamiento automático de las conexiones de cliente de escucha entrantes a una réplica secundaria legible configurada para permitir cargas de trabajo de solo lectura. 

Las conexiones se enrutan de forma automática a la réplica de solo lectura si se cumple lo siguiente: 
 
-   Al menos una réplica secundaria se establece para acceso de solo lectura, y cada réplica secundaria de solo lectura y la réplica principal se [configuran para admitir el enrutamiento de solo lectura](configure-read-only-routing-for-an-availability-group-sql-server.md). 

-   La cadena de conexión hace referencia a una base de datos implicada en el grupo de disponibilidad. Una alternativa para esto sería que el inicio de sesión usado en la conexión tenga la base de datos configurada como su base de datos predeterminada. Para más información, vea [Calculating read_only_routing_url for Always On](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson) (Calcular read_only_routing_url para Always On).

-   La cadena de conexión hace referencia a un agente de escucha de grupo de disponibilidad y la intención de aplicaciones de la conexión entrante se establece en solo lectura (por ejemplo, al usar la palabra clave **Application Intent=ReadOnly** en las cadenas de conexión, los atributos de conexión o las propiedades de ODBC u OLEDB). 

El atributo de la intención de aplicación se almacena en la sesión de cliente durante el inicio de sesión y, después, la instancia de SQL Server procesará esta intención y determinará lo que debe hacer de acuerdo con la configuración del grupo de disponibilidad y el estado de lectura y escritura actual de la base de datos de destino en la réplica secundaria.  

Por ejemplo, para conectarse a una réplica de solo lectura mediante SQL Server Management Studio, seleccione **Opciones** en el cuadro de diálogo **Conectar con al servidor**, seleccione la pestaña **Parámetros de conexión adicionales** y, después, especifique `ApplicationIntent=ReadOnly` en el cuadro de texto:

![Conexión de solo lectura en SSMS](media/listeners-client-connectivity-application-failover/read-only-intent-in-ssms.png)
  
Un ejemplo de una cadena de conexión para el proveedor ADO.NET (System.Data.SqlClient) que designa la intención de solo lectura de la aplicación: 

```  
Server=tcp:AGListener;Database=AdventureWorks;Integrated Security=SSPI;ApplicationIntent=ReadOnly  
```  
  
Para obtener más información, vea [Configuración del acceso de solo lectura en una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  

## <a name="non-default-port"></a>Puerto no predeterminado

Al crear el cliente de escucha, debe designar un puerto para que lo use. Si el puerto es 1433 (el predeterminado), no es necesario especificar un número de puerto al conectarse al cliente de escucha. Pero si el puerto no es 1433, se debe especificar en la cadena de conexión en el formato `listenername,port`, por ejemplo:

![Conexión con un puerto no predeterminado](media/listeners-client-connectivity-application-failover/specify-port-in-ssms.png)

Un ejemplo de una cadena de conexión para el proveedor ADO.NET (System.Data.SqlClient) que especifica un puerto no predeterminado para el cliente de escucha: 

```  
Server=tcp:AGListener,1445;Database=AdventureWorks;Integrated Security=SSPI 
```  

##  <a name="bypass-listeners"></a><a name="BypassAGl"></a> Omisión de clientes de escucha  

Mientras los agentes de escucha del grupo de disponibilidad habilitan la compatibilidad con la redirección de conmutación por error y el enrutamiento de solo lectura, las conexiones de cliente no requieren utilizarlos. Una conexión de cliente puede hacer referencia directamente a la instancia de SQL Server en lugar de conectarse al agente de escucha del grupo de disponibilidad.  
  
Para la instancia de SQL Server, es irrelevante si una conexión inicia una sesión utilizando el agente de escucha del grupo de disponibilidad o utilizando otro punto de conexión de la instancia.  La instancia de SQL Server comprobará el estado de la base de datos de destino y permitirá o denegará la conectividad basada en la configuración del grupo de disponibilidad y el estado actual de la base de datos de la instancia.  Por ejemplo, si una aplicación cliente se conecta directamente a una instancia de puerto de SQL Server y se conecta a una base de datos de destino hospedada en un grupo de disponibilidad, y la base de datos de destino está en estado principal y en línea, la conectividad se realizará correctamente.  Si la base de datos de destino está sin conexión o en estado transitorio, la conectividad a la base de datos producirá un error.  
  
Alternativamente, en la migración de creación de reflejo de la base de datos a [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], las aplicaciones pueden especificar la cadena de conexión de creación de reflejo de la base de datos mientras solo exista una réplica secundaria y no permita conexiones de usuario. 

##  <a name="database-mirroring-connection-strings"></a><a name="DbmConnectionString"></a> Cadenas de conexión de creación de reflejo de la base de datos 
 Si un grupo de disponibilidad posee solo una réplica secundaria y está configurado con ALLOW_CONNECTIONS = READ_ONLY o con ALLOW_CONNECTIONS = NONE en la réplica secundaria, los clientes pueden conectarse a la réplica de disponibilidad principal a través de una cadena de conexión de creación de reflejo de la base de datos. Este enfoque puede ser útil cuando se migra una aplicación existente de creación de reflejo de la base de datos a un grupo de disponibilidad, siempre que se limite el grupo de disponibilidad a dos réplicas de disponibilidad (una réplica principal y una réplica secundaria). Si agrega réplicas secundarias adicionales, deberá crear un agente de escucha del grupo de disponibilidad y actualizar sus aplicaciones para que se utilice el nombre DNS del agente de escucha del grupo de disponibilidad.  
  
 Cuando se utilizan cadenas de conexión de creación de reflejo de la base de datos, el cliente puede usar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client o el proveedor de datos de .NET Framework para [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La cadena de conexión proporcionada por un cliente debe proporcionar como mínimo el nombre de una instancia del servidor, el *nombre del asociado inicial*, para identificar la instancia del servidor que hospeda inicialmente la réplica de disponibilidad con la que se desea establecer conexión. Opcionalmente, la cadena de conexión también puede proporcionar el nombre de otra instancia del servidor, *nombre del asociado de conmutación por error*, para identificar la instancia del servidor que hospeda inicialmente la réplica secundaria como nombre del asociado de conmutación por error.  
  
 Para obtener más información sobre las cadenas de conexión de creación de reflejo de la base de datos, vea [Conectar clientes a una sesión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../../database-engine/database-mirroring/connect-clients-to-a-database-mirroring-session-sql-server.md).  
  
  
##  <a name="multi-subnet-failovers"></a><a name="SupportAgMultiSubnetFailover"></a> Conmutación por error de varias subredes  
 
 Si va a usar bibliotecas cliente que admiten la opción de conexión MultiSubnetFailover en la cadena de conexión, puede optimizar la conmutación por error del grupo de disponibilidad a otra subred si establece MultiSubnetFailover en "True" o "Yes", en función de la sintaxis del proveedor que use.  
  
> [!NOTE]  
>  Se recomienda esta configuración para las conexiones de una red y de varias subredes a los agentes de escucha del grupo de disponibilidad y a los nombres de instancia de clúster de conmutación por error de SQL Server.  La habilitación de esta opción agrega optimizaciones adicionales, incluso en escenarios de una sola subred.  
  
 La opción de conexión **MultiSubnetFailover** funciona únicamente con el protocolo de red TCP y solo se admite en la conexión a un agente de escucha del grupo de disponibilidad y para cualquier nombre de red virtual que se conecta a [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
 El siguiente es un ejemplo de cadena de conexión del proveedor ADO.NET (System.Data.SqlClient) que habilita la conmutación por error de varias subredes:  
  
```  
Server=tcp:AGListener,1433;Database=AdventureWorks;Integrated Security=SSPI; MultiSubnetFailover=True  
```  
  
 La opción de conexión **MultiSubnetFailover** debe establecerse en **True** aunque el grupo de disponibilidad solo abarque una única subred.  Esto permite preconfigurar nuevos clientes que puedan abarca en el futuro subredes sin necesidad de los cambios en la cadena de conexión de cliente y también optimiza el rendimiento de conmutación por error para conmutaciones por error de las subredes.  Cuando la opción de conexión **MultiSubnetFailover** no es necesaria, proporciona la ventaja de una conmutación por error más rápida de la subred.  Esto se debe a que el controlador cliente intentará abrir un socket de TCP para cada dirección IP en paralelo asociada al grupo de disponibilidad.  El controlador cliente esperará que la primera dirección IP responda y, una vez que lo haga, la utilizará para la conexión.  
  
##  <a name="listeners--tlsssl-certificates"></a><a name="SSLcertificates"></a> Clientes de escucha y certificados TLS/SSL  

Al conectarse a un agente de escucha del grupo de disponibilidad, si las instancias participantes de SQL Server utilizan certificados TLS/SSL junto con cifrado de sesión, el controlador cliente de conexión deberá admitir el nombre alternativo del asunto del certificado TLS/SSL para forzar el cifrado.  La compatibilidad del controlador de SQL Server con el nombre alternativo del asunto del certificado está previsto para ADO.NET (SqlClient), Microsoft JDBC y SQL Native Client (SNAC).  
  
Se debe configurar un certificado X.509 para cada nodo de servidor participante en el clúster de conmutación por error con una lista de todos los agentes de escucha del grupo de disponibilidad establecidos en el nombre alternativo del asunto del certificado. 

El formato de los valores de certificado es: 

```  
CN = Server.FQDN  
SAN = Server.FQDN,Listener1.FQDN,Listener2.FQDN
```

Por ejemplo, tiene los valores siguientes: 

```
Servername: Win2019   
Instance: SQL2019   
AG: AG2019   
Listener: Listener2019   
Domain: contoso.com  (which is also the FQDN)
```

En el caso de un WSFC que tenga un solo grupo de disponibilidad, el certificado debe tener el nombre de dominio completo (FQDN) del servidor y el FQDN del agente de escucha: 

```
CN: Win2019.contoso.com
SAN: Win2019.contoso.com, Listener2019.contoso.com 
```

Con esta configuración, las conexiones se cifrarán al conectarse a la instancia (`WIN2019\SQL2019`) o al agente de escucha (`Listener2019`). 

En función de cómo esté configurada la red, hay un pequeño subconjunto de clientes que es posible que tengan que agregar también el NetBIOS al SAN. En ese caso, los valores del certificado deben ser: 

```
CN: Win2019.contoso.com
SAN: Win2019,Win2019.contoso.com,Listener2019,Listener2019.contoso.com
```

Si el WSFC tiene tres agentes de escucha del grupo de disponibilidad, por ejemplo: Listener1, Listener2, Listener3

Los valores del certificado deben ser: 

```
CN: Win2019.contoso.com
SAN: Win2019.contoso.com,Listener1.contoso.com,Listener2.contoso.com,Listener3.contoso.com
```
  
  
##  <a name="listeners-and-kerberos-spns"></a><a name="SPNs"></a> Clientes de escucha y Kerberos (SPN) 

Un administrador de dominio debe configurar un Nombre de entidad de seguridad de servicio (SPN) en Active Directory para cada escucha de grupo de disponibilidad a fin de habilitar Kerberos para las conexiones de cliente con el cliente de escucha. Al registrar el SPN, debe usar la cuenta de servicio de la instancia del servidor en el que se hospede la réplica de disponibilidad. Para que el SPN funcione en todas las réplicas, debe utilizarse la misma cuenta de servicio para todas las instancias del clúster de WSFC que hospeda el grupo de disponibilidad.  
  
 Utilice la herramienta de línea de comandos de Windows **setspn** para configurar el SPN.  Por ejemplo, para configurar un SPN para un grupo de disponibilidad denominado `AG1listener.Adventure-Works.com` hospedado en un conjunto de instancias de SQL Server configuradas para ejecutarse en la cuenta de dominio `corp\svclogin2`:  
  
```  
setspn -A MSSQLSvc/AG1listener.Adventure-Works.com:1433 corp\svclogin2  
```  
  
 Para obtener más información acerca del registro manual de un SPN para SQL Server, vea [Registrar un nombre de entidad de seguridad de servicio para las conexiones con Kerberos](../../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md).  
  


## <a name="next-steps"></a>Pasos siguientes

Una vez que se haya conectado correctamente al cliente de escucha, considere la posibilidad de descargar las [cargas de trabajo de solo lectura](overview-of-always-on-availability-groups-sql-server.md) y las [copias de seguridad](configure-backup-on-availability-replicas-sql-server.md) a la réplica secundaria para mejorar el rendimiento. También puede revisar varias [estrategias de supervisión de grupos de disponibilidad](monitoring-of-availability-groups-sql-server.md) para garantizar el estado del grupo de disponibilidad. 

Para obtener más información sobre los grupos de disponibilidad, vea [Información general de los grupos de disponibilidad Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).