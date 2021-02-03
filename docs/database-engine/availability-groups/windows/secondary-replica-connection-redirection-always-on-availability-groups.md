---
title: Redireccionamiento de conexiones de lectura y escritura a la réplica principal
description: Obtenga información sobre cómo redirigir las conexiones de lectura y escritura a la réplica principal de un grupo de disponibilidad Always On con independencia del servidor especificado en la cadena de conexión.
ms.custom: seo-lt-2019
ms.date: 01/09/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: article
helpviewer_keywords:
- connection access to availability replicas
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], readable secondary replicas
- active secondary replicas [SQL Server], read-only access to
- readable secondary replicas
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: ac4900e284d1e83ef0945e1d0082279c06414b0b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236678"
---
# <a name="secondary-to-primary-replica-readwrite-connection-redirection-always-on-availability-groups"></a>Redireccionamiento de la conexión de lectura/escritura de réplicas de secundaria a principal (grupos de disponibilidad AlwaysOn)

[!INCLUDE[appliesto](../../../includes/applies-to-version/sqlserver2019.md)]

[!INCLUDE[sssql19-md](../../../includes/sssql19-md.md)] CTP 2.0 introduce el *redireccionamiento de la conexión de lectura/escritura de réplicas de secundaria a principal* para los grupos de disponibilidad Always On. El redireccionamiento de la conexión de lectura/escritura está disponible en cualquier plataforma de sistema operativo. Permite que las conexiones de las aplicaciones cliente se dirijan a la réplica principal, independientemente del servidor de destino especificado en la cadena de las conexiones. 

Por ejemplo, la cadena de conexión puede tener como destino una réplica secundaria. En función de la configuración de la réplica del grupo de disponibilidad y de la configuración de la cadena de conexión, la conexión se puede redirigir automáticamente a la réplica principal. 

## <a name="use-cases"></a>Casos de uso

Antes de [!INCLUDE[sssql19-md](../../../includes/sssql19-md.md)], el agente de escucha del grupo de disponibilidad y el recurso de clúster correspondiente redirigían el tráfico de usuario a la réplica principal para garantizar la reconexión después de una conmutación por error. [!INCLUDE[sssql19-md](../../../includes/sssql19-md.md)] sigue siendo compatible con la funcionalidad del agente de escucha del grupo de disponibilidad e incorpora el redireccionamiento de la conexión de réplica para los casos en los que no se puede incluir un agente de escucha. Por ejemplo:

* La tecnología de clúster con la que se integran los grupos de disponibilidad de SQL Server no ofrece ninguna capacidad parecida a los agentes de escucha. 
* Una configuración de varias subredes (por ejemplo, en la nube) o una IP flotante de varias subredes con Pacemaker donde las configuraciones se vuelven complejas, propensas a sufrir errores y difíciles de resolver debido a los distintos componentes implicados.
* Escalado horizontal de lectura, o el tipo de clúster y de recuperación ante desastres es `NONE`, porque no hay ningún mecanismo sencillo para garantizar una reconexión transparente después de una conmutación por error manual.

## <a name="requirement"></a>Requisito

Para que una réplica secundaria redirija solicitudes de conexión de lectura/escritura:
* La réplica secundaria debe estar en línea. 
* La especificación de réplica `PRIMARY_ROLE` debe incluir `READ_WRITE_ROUTING_URL`.
* La cadena de conexión debe ser `ReadWrite` mediante la definición de `ApplicationIntent` como `ReadWrite`, o bien si no se establece `ApplicationIntent` y se permite que el valor predeterminado (`ReadWrite`) surta efecto.

## <a name="set-read_write_routing_url-option"></a>Establecer la opción READ_WRITE_ROUTING_URL

Para configurar el redireccionamiento de la conexión de lectura/escritura, establezca `READ_WRITE_ROUTING_URL` para la réplica principal al crear el grupo de disponibilidad. 

En [!INCLUDE[sssql19-md](../../../includes/sssql19-md.md)] se ha agregado `READ_WRITE_ROUTING_URL` a la especificación `<add_replica_option>`. Vea los siguientes temas: 

* [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)
* [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)


### <a name="primary_roleread_write_routing_url-not-set-default"></a>PRIMARY_ROLE(READ_WRITE_ROUTING_URL) no establecido (predeterminado) 

De forma predeterminada, no se establece el redireccionamiento de la conexión de réplica de lectura/escritura para una réplica. La manera en que una réplica secundaria gestiona las solicitudes de conexión depende de si está configurada para permitir las conexiones y de la opción `ApplicationIntent` de la cadena de conexión. En la siguiente tabla se muestra cómo gestiona una réplica secundaria las conexiones a partir de `SECONDARY_ROLE (ALLOW CONNECTIONS = )` y `ApplicationIntent`.

|Valor de <code>ApplicationIntent</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = NO)</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = READ_ONLY)</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = ALL)</code>|
|-----|-----|-----|-----|
|`ApplicationIntent=ReadWrite`<br/> Valor predeterminado|Error de conexiones|Error de conexiones|Conexiones correctas<br/>Lecturas correctas<br/>Error de escrituras|
|`ApplicationIntent=ReadOnly`|Error de conexiones|Conexiones correctas|Conexiones correctas

En la tabla anterior se muestra el comportamiento predeterminado, que es el mismo que en las versiones de SQL Server anteriores a [!INCLUDE[sssql19-md](../../../includes/sssql19-md.md)]. 

### <a name="primary_roleread_write_routing_url-set"></a>PRIMARY_ROLE(READ_WRITE_ROUTING_URL) establecido 

Después de establecer el redireccionamiento de la conexión de lectura/escritura, la réplica gestionará las solicitudes de conexión de otra manera. El comportamiento de la conexión sigue dependiendo de la configuración de `SECONDARY_ROLE (ALLOW CONNECTIONS = )` y `ApplicationIntent`. En la siguiente tabla se muestra cómo gestiona una réplica secundaria con `READ_WRITE_ROUTING` establecido las conexiones a partir de `SECONDARY_ROLE (ALLOW CONNECTIONS = )` y `ApplicationIntent`.

|Valor de <code>ApplicationIntent</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = NO)</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = READ_ONLY)</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = ALL)</code>|
|-----|-----|-----|-----|
|`ApplicationIntent=ReadWrite`<br/>Valor predeterminado|Error de conexiones|Error de conexiones|Las conexiones se enrutan a la réplica principal|
|`ApplicationIntent=ReadOnly`|Error de conexiones|Conexiones correctas|Conexiones correctas

En la tabla anterior se muestra que, cuando la réplica principal tiene `READ_WRITE_ROUTING_URL` establecido, la réplica secundaria redirigirá las conexiones a la réplica principal cuando `SECONDARY_ROLE (ALLOW CONNECTIONS = ALL)` y la conexión especifica `ReadWrite`.

## <a name="example"></a>Ejemplo 

En este ejemplo, un grupo de disponibilidad tiene tres réplicas:
* Una réplica principal en COMPUTER01
* Una réplica secundaria sincrónica en COMPUTER02
* Réplica secundaria asincrónica en COMPUTER03

En la siguiente imagen se representa el grupo de disponibilidad.

![Grupo de disponibilidad con réplica principal, secundaria y secundaria asincrónica](media/replica-connection-redirection-always-on-availability-groups/01_originalAG.png)

El siguiente script de Transact-SQL crea este grupo de disponibilidad. En este ejemplo, cada réplica especifica `READ_WRITE_ROUTING_URL`.
```sql
CREATE AVAILABILITY GROUP MyAg   
     WITH ( CLUSTER_TYPE =  NONE )  
   FOR   
     DATABASE  <Database1>   
   REPLICA ON   
      'COMPUTER01' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER01.<domain>.<tld>:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = MANUAL,  
         SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL,   
            READ_ONLY_ROUTING_URL = 'TCP://COMPUTER01.<domain>.<tld>:1433' ),
         PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE,   
            READ_ONLY_ROUTING_LIST = (COMPUTER02, COMPUTER03),
            READ_WRITE_ROUTING_URL = 'TCP://COMPUTER01.<domain>.<tld>:1433' )   
         SESSION_TIMEOUT = 10  
         ),   
      'COMPUTER02' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER02.<domain>.<tld>:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = MANUAL, 
         SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL,   
            READ_ONLY_ROUTING_URL = 'TCP://COMPUTER02.<domain>.<tld>:1433' ),  
         PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE,   
            READ_ONLY_ROUTING_LIST = (COMPUTER01, COMPUTER03),  
            READ_WRITE_ROUTING_URL = 'TCP://COMPUTER02.<domain>.<tld>:1433' )   
         SESSION_TIMEOUT = 10  
         ),   
      'COMPUTER03' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER03.<domain>.<tld>:5022',  
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = MANUAL,  
         SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL,   
            READ_ONLY_ROUTING_URL = 'TCP://COMPUTER03.<domain>.<tld>:1433' ),  
         PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE,   
            READ_ONLY_ROUTING_LIST = (COMPUTER01, COMPUTER02),  
            READ_WRITE_ROUTING_URL = 'TCP://COMPUTER03.<domain>.<tld>:1433' )  
         SESSION_TIMEOUT = 10  
         );
GO  
```
   - `<domain>.<tld>`
      - Dominio y dominio de nivel superior del nombre de dominio completo. Por ejemplo, `corporation.com`.


### <a name="connection-behaviors"></a>Comportamientos de conexión

En el diagrama siguiente, una aplicación cliente se conecta a COMPUTER02 con `ApplicationIntent=ReadWrite`. La conexión se redirige a la réplica principal. 

![La conexión al segundo equipo se redirige a la réplica principal.](media/replica-connection-redirection-always-on-availability-groups/02_redirectionAG.png)

La réplica secundaria redirige las llamadas de lectura/escritura a la réplica principal. Una conexión de lectura/escritura a cualquier réplica se redirigirá a la réplica principal. 

En el diagrama siguiente, la réplica principal se ha conmutado por error de forma manual a COMPUTER02. Una aplicación cliente se conecta a COMPUTER01 con `ApplicationIntent=ReadWrite`. La conexión se redirige a la réplica principal. 

![La conexión se redirige a la nueva réplica principal en el segundo equipo.](media/replica-connection-redirection-always-on-availability-groups/03_redirectionAG.png)

## <a name="see-also"></a>Consulte también

[Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 
[Acerca del acceso de conexión de cliente a réplicas de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   

[Agentes de escucha de grupo de disponibilidad, conectividad de cliente y conmutación por error de una aplicación &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)
