---
title: Especificación de la dirección URL de una réplica de disponibilidad
description: Obtenga información sobre cómo especificar la dirección URL del punto de conexión al agregar o modificar una réplica dentro de un grupo de disponibilidad AlwaysOn en SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- endpoints [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], endpoint
- Endpoint URLs (HADR)
ms.assetid: d7520c13-a8ee-4ddc-9e9a-54cd3d27ef1c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0bee045cec3e11e7ae4f511b7e7b630aee208cd9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349258"
---
# <a name="specify-endpoint-url---adding-or-modifying-availability-replica"></a>Especificar la dirección URL del punto de conexión - Agregar o modificar una réplica de disponibilidad
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Para hospedar una réplica de disponibilidad de un grupo de disponibilidad, una instancia de servidor debe poseer un extremo de creación de reflejo de la base de datos. La instancia de servidor utiliza este extremo para escuchar los mensajes de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] procedentes de las réplicas de disponibilidad hospedadas por otras instancias del servidor. Para definir una réplica de disponibilidad para un grupo de disponibilidad, debe especificar la dirección URL del extremo de la instancia del servidor que hospedará la réplica. La *dirección URL del punto de conexión* identifica el protocolo de transporte del TCP del punto de conexión de creación de reflejo de la base de datos, la dirección del sistema de la instancia del servidor y el número de puerto asociado con el punto de conexión.  
  
> [!NOTE]  
>  El término “dirección URL del extremo” es sinónimo del término “dirección de red del servidor” que usa la interfaz de usuario y la documentación de creación de reflejo de la base de datos.  
  
  
##  <a name="syntax-for-an-endpoint-url"></a><a name="SyntaxOfURL"></a> Sintaxis de una dirección URL del extremo  
 La sintaxis de una dirección URL del extremo tiene el siguiente formato:  
  
 TCP <strong>://</strong> *\<system-address>* <strong>:</strong> *\<port>*  
  
 , donde  
  
-   *\<system-address>* es una cadena que identifica de forma inequívoca el sistema del equipo de destino. Generalmente, la dirección del servidor es un nombre del sistema (si los sistemas están en el mismo dominio), un nombre de dominio completo o una dirección IP:  
  
    -   Dado que todos los nodos del clúster de conmutación por error de Windows Server (WSFC) están en el mismo dominio, puede usar el nombre del equipo; por ejemplo, `SYSTEM46`.  
  
    -   Para utilizar una dirección IP, ésta debe ser única en el entorno. Recomendamos que utilice una dirección IP solo si es estática. La dirección IP puede ser IP Versión 4 (IPv4) o IP Versión 6 (IPv6). Las direcciones IPv6 se deben incluir entre corchetes, por ejemplo: **[** _<dirección_IPv6>_ **]** .  
  
         Para conocer la dirección IP de un sistema, en el símbolo del sistema de Windows, escriba el comando **ipconfig** .  
  
    -   El nombre de dominio completo siempre funciona. Esta es una cadena de dirección definida localmente que adopta diferentes formatos en distintos lugares. Con frecuencia, aunque no siempre, el nombre de dominio completo es un nombre compuesto que incluye el nombre del equipo y una serie de segmentos de dominio separados por puntos con el siguiente formato:  
  
         _nombre_equipo_ **.** _segmento_dominio_[... **.** _segmento_dominio_]  
  
         donde *nombre_equipo* es el nombre de red del equipo que ejecuta la instancia de servidor y *segmento_dominio*[... **.** _segmento_dominio_] es la información restante de dominio del servidor; por ejemplo: `localinfo.corp.Adventure-Works.com`.  
  
         El contenido y el número de segmentos de dominio se determinan en la empresa u organización. Para obtener más información, vea [Buscar el nombre de dominio completo](#Finding_FQDN), más adelante en este tema.  
  
-   *\<port>* es el número de puerto que usa el punto de conexión de creación de reflejo de la instancia de servidor asociada.  
  
     Un extremo de creación de reflejo de la base de datos puede utilizar cualquier puerto disponible en el equipo. Cada número de puerto debe estar asociado con un único extremo y cada extremo está asociado con una sola instancia de servidor; en consecuencia, diferentes instancias del mismo servidor escuchan en diferentes extremos con distintos puertos. Por consiguiente, el puerto que especifique en la dirección URL del extremo al especificar una réplica de disponibilidad siempre dirigirá los mensajes entrantes a la instancia de servidor cuyo extremo esté asociado con dicho puerto.  
  
     En la dirección URL del extremo, solo el número de puerto identifica la instancia del servidor que está asociada con el extremo de creación de reflejo en el equipo de destino. En la siguiente ilustración se muestran las direcciones de extremo de dos instancias de servidor en un solo equipo. La instancia predeterminada usa el puerto `7022` y la instancia con nombre usa el puerto `7033`. Las direcciones URL del extremo para estas dos instancias de servidor son, respectivamente, `TCP://MYSYSTEM.Adventure-works.MyDomain.com:7022` y `TCP://MYSYSTEM.Adventure-works.MyDomain.com:7033`. Observe que la dirección no contiene el nombre de la instancia de servidor.  
  
     ![Direcciones de red del servidor de una instancia predeterminada](../../../database-engine/availability-groups/windows/media/dbm-2-instances-ports-1-system.gif "Direcciones de red del servidor de una instancia predeterminada")  
  
     Para identificar el puerto asociado actualmente al extremo de creación de reflejo de la base de datos de una instancia de servidor, utilice la siguiente instrucción [!INCLUDE[tsql](../../../includes/tsql-md.md)] :  
  
    ```  
    SELECT type_desc, port FROM sys.TCP_endpoints  
    ```  
  
     Busque la fila cuyo valor **type_desc** sea "DATABASE_MIRRORING" y use el número de puerto correspondiente.  
  
### <a name="examples"></a>Ejemplos  
  
#### <a name="a-using-a-system-name"></a>A. Usar un nombre de sistema  
 La siguiente dirección URL del extremo especifica un nombre de sistema, `SYSTEM46`, y el puerto `7022`.  
  
 `TCP://SYSTEM46:7022`  
  
#### <a name="b-using-a-fully-qualified-domain-name"></a>B. Usar un nombre de dominio completo  
 La siguiente dirección URL del extremo especifica un nombre de dominio completo, `DBSERVER8.manufacturing.Adventure-Works.com`, y el puerto `7024`.  
  
 `TCP://DBSERVER8.manufacturing.Adventure-Works.com:7024`  
  
#### <a name="c-using-ipv4"></a>C. Usar IPv4  
 La siguiente dirección URL del extremo especifica una dirección IPv4, `10.193.9.134`, y el puerto `7023`.  
  
 `TCP://10.193.9.134:7023`  
  
#### <a name="d-using-ipv6"></a>D. Usar IPv6  
 La siguiente dirección URL del extremo contiene una dirección IPv6, `2001:4898:23:1002:20f:1fff:feff:b3a3`, y el puerto `7022`.  
  
 `TCP://[2001:4898:23:1002:20f:1fff:feff:b3a3]:7022`  
  
##  <a name="finding-the-fully-qualified-domain-name-of-a-system"></a><a name="Finding_FQDN"></a> Buscar el nombre de dominio completo de un sistema  
 Para buscar el nombre de dominio completo de un sistema, en el símbolo del sistema de Windows de ese sistema, escriba:  
  
 **IPCONFIG /ALL**  
  
 Para formar el nombre de dominio completo, concatene los valores de *<host_name>* y *<Primary_Dns_Suffix>* de la siguiente manera:  
  
 _<host_name>_ **.** _<sufijo_DNS_primario>_  
  
 Por ejemplo, la configuración IP  
  
 `Host Name  .  .  .  .  .  .  : MYSERVER`  
  
 `Primary Dns Suffix  .  .  .  : mydomain.Adventure-Works.com`  
  
 equivale al siguiente nombre de dominio completo:  
  
 `MYSERVER.mydomain.Adventure-Works.com`  
  
> [!NOTE]  
>  Si necesita más información acerca de un nombre de dominio completo, póngase en contacto con el administrador del sistema.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
 **Para configurar un extremo de creación del reflejo de la base de datos**  
  
-   [Crear un punto de conexión de creación de reflejo de la base de datos para grupos de disponibilidad AlwaysOn &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [Crear un punto de conexión de creación de reflejo de la base de datos para la autenticación de Windows &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Usar certificados para un punto de conexión de creación de reflejo de la base de datos &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
    -   [Permitir que un punto de conexión de creación de reflejo de la base de datos utilice certificados para las conexiones salientes &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
    -   [Permitir que un punto de conexión de creación de reflejo de la base de datos use certificados para las conexiones entrantes &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Especificar una dirección de red de servidor &#40;creación de reflejo de la base de datos&#41;](../../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
  
-   Especificar la dirección URL del extremo al agregar o modificar una réplica de disponibilidad (SQL Server)  
  
-   [Solucionar problemas de configuración de grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
 **Para ver información acerca del extremo de creación de reflejo de la base de datos**  
  
-   [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
 **Para agregar una réplica de disponibilidad**  
  
-   [Agregar una réplica secundaria a un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Combinar una réplica secundaria con un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado  
  
-   [Guía de soluciones AlwaysOn de Microsoft SQL Server para lograr alta disponibilidad y recuperación ante desastres](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
## <a name="see-also"></a>Consulte también  
 [Creación y configuración de grupos de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-endpoint-transact-sql.md)  
  
