---
description: ALTER ROUTE (Transact-SQL)
title: ALTER ROUTE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_ROUTE_TSQL
- ALTER ROUTE
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER ROUTE statement
- dropping routes
- modifying routes
- removing routes
- routes [Service Broker], modifying
ms.assetid: 8dfb7b16-3dac-4e1e-8c97-adf2aad07830
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: d1fc273bdb22d58974ec5fdc562cf3502348b196
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204037"
---
# <a name="alter-route-transact-sql"></a>ALTER ROUTE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifica la información de ruta de una ruta existente en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER ROUTE route_name  
WITH    
  [ SERVICE_NAME = 'service_name' [ , ] ]  
  [ BROKER_INSTANCE = 'broker_instance' [ , ] ]  
  [ LIFETIME = route_lifetime [ , ] ]  
  [ ADDRESS =  'next_hop_address' [ , ] ]  
  [ MIRROR_ADDRESS = 'next_hop_mirror_address' ]  
[ ; ]  
  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *route_name*  
 Es el nombre de la ruta que se va a cambiar. No se pueden especificar nombres de servidor, base de datos o esquema.  
  
 WITH  
 Presenta las cláusulas que definen la ruta que se va a modificar.  
  
 SERVICE_NAME **='** _service\_name_ **'**  
 Especifica el nombre del servicio remoto señalado por esta ruta. El *service_name* debe coincidir exactamente con el nombre que usa el servicio remoto. [!INCLUDE[ssSB](../../includes/sssb-md.md)] usa una comparación byte a byte para buscar una coincidencia con *service_name*. En otras palabras, en la comparación se distinguen mayúsculas y minúsculas, y no se considera la intercalación actual. Una ruta con el nombre de servicio **'SQL/ServiceBroker/BrokerConfiguration'** es una ruta a un servicio de notificación de configuración del agente. Es posible que una ruta a este servicio no especifique una instancia de agente.  
  
 Si se omite la cláusula SERVICE_NAME, el nombre de servicio de la ruta no varía.  
  
 BROKER_INSTANCE **='** _broker\_instance_ **'**  
 Especifica la base de datos que hospeda el servicio de destino. El parámetro *instancia_de_broker* debe ser el identificador de la instancia de broker para la base de datos remota, que se puede obtener al ejecutar la consulta siguiente en la base de datos seleccionada:  
  
```sql  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID();  
```  
  
 Si se omite la cláusula BROKER_INSTANCE, la instancia de broker para la ruta no varía.  
  
> [!NOTE]  
>  Esta opción no está disponible en las bases de datos independientes.  
  
 LIFETIME **=** _route\_lifetime_  
 Especifica el tiempo, en segundos, durante el que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retiene la ruta en la tabla de enrutamiento. Transcurrido este tiempo, la ruta expira y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ya no la tiene en cuenta al elegir una ruta para una conversación nueva. Si se omite esta cláusula, la vigencia de la ruta no varía.  
  
 ADDRESS **='** _next\_hop\_address_'  

 Para Azure SQL Managed Instance, `ADDRESS` debe ser local.

 Especifica la dirección de red para esta ruta. En *dirección_de_próximo_salto* se especifica una dirección TCP/IP en el formato siguiente:  
  
 **TCP://** { *dns_name* | *netbios_name* |*ip_address* } **:** *port_number*  
  
 El *port_number* especificado debe coincidir con el número de puerto del extremo de [!INCLUDE[ssSB](../../includes/sssb-md.md)] de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el equipo especificado. Se puede obtener ejecutando la consulta siguiente en la base de datos seleccionada:  
  
```sql  
SELECT tcpe.port  
FROM sys.tcp_endpoints AS tcpe  
INNER JOIN sys.service_broker_endpoints AS ssbe  
   ON ssbe.endpoint_id = tcpe.endpoint_id  
WHERE ssbe.name = N'MyServiceBrokerEndpoint';  
```  
  
 Cuando una ruta especifica **"LOCAL"** para *dirección_de_próximo_salto*, el mensaje se entrega a un servicio en la instancia actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Cuando una ruta especifica **"TRANSPORT"** para *dirección_de_próximo_salto*, la dirección de red se determina en función de la dirección de red del nombre del servicio. Una ruta que especifica **"TRANSPORT"** puede especificar un nombre de servicio o una instancia de agente.  
  
 Cuando *dirección_de_próximo_salto* es el servidor principal de una base de datos reflejada, también debe especificar MIRROR_ADDRESS para el servidor reflejado. En caso contrario, esta ruta no realiza la conmutación por error automáticamente al servidor reflejado.  
  
> [!NOTE]  
>  Esta opción no está disponible en las bases de datos independientes.  
  
 MIRROR_ADDRESS **='** _next\_hop\_mirror\_address_ **'**  
 Especifica la dirección de red del servidor reflejado de un par reflejado cuyo servidor principal se encuentra en *dirección_de_reflejo_de_próximo_salto*. En *dirección_de_reflejo_de_próximo_salto* se especifica una dirección TCP/IP en el formato siguiente:  
  
 **TCP://** { *dns_name* | *netbios_name* | *ip_address* } **:** *port_number*  
  
 El *port_number* especificado debe coincidir con el número de puerto del extremo de [!INCLUDE[ssSB](../../includes/sssb-md.md)] de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el equipo especificado. Se puede obtener ejecutando la consulta siguiente en la base de datos seleccionada:  
  
```sql  
SELECT tcpe.port  
FROM sys.tcp_endpoints AS tcpe  
INNER JOIN sys.service_broker_endpoints AS ssbe  
   ON ssbe.endpoint_id = tcpe.endpoint_id  
WHERE ssbe.name = N'MyServiceBrokerEndpoint';  
```  
  
 Si se especifica MIRROR_ADDRESS, la ruta debe especificar la cláusula SERVICE_NAME y la cláusula BROKER_INSTANCE. Es posible que una ruta que especifica **"LOCAL"** o **"TRANSPORT"** para *dirección_de_próximo_salto* no especifique una dirección de reflejo.  
  
> [!NOTE]  
>  Esta opción no está disponible en las bases de datos independientes.  
  
## <a name="remarks"></a>Observaciones  
 La tabla de enrutamiento en la que se almacenan las rutas es una tabla de metadatos que se puede leer mediante la vista de catálogo **sys.routes**. La tabla de enrutamiento solo se puede actualizar mediante las instrucciones CREATE ROUTE, ALTER ROUTE y DROP ROUTE.  
  
 Las cláusulas que no se especifican en el comando ALTER ROUTE no varían. Por consiguiente, no se puede aplicar ALTER a una ruta para especificar que dicha ruta no agota el tiempo de espera, que coincide con un nombre de servicio o que coincide con una instancia de agente. Para cambiar estas características de la ruta, debe quitar la ruta existente y crear otra ruta con la información nueva.  
  
 Cuando una ruta especifica **"TRANSPORT"** para *dirección_de_próximo_salto*, la dirección de red se determina en función del nombre del servicio. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede procesar de forma correcta nombres de servicio que comienzan con una dirección de red en un formato válido para *dirección_de_próximo_salto*. Los servicios con nombres que contienen direcciones de red válidas se enrutan a la dirección de red del nombre de servicio.  
  
 La tabla de enrutamiento puede contener un número indeterminado de rutas que especifican el mismo servicio, dirección de red e identificador de instancia de broker. En este caso, [!INCLUDE[ssSB](../../includes/sssb-md.md)] elige una ruta mediante un procedimiento diseñado para buscar la coincidencia más exacta entre la información especificada en la conversación y la información de la tabla de enrutamiento.  
  
 Para modificar AUTHORIZATION para un servicio, utilice la instrucción ALTER AUTHORIZATION.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, se concede permiso para modificar una ruta al propietario de la ruta, a los miembros de los roles fijos de base de datos **db_ddladmin** o **db_owner**, y a los miembros del rol fijo de servidor **sysadmin**.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-changing-the-service-for-a-route"></a>A. Cambiar el servicio para una ruta  
 En el ejemplo siguiente se modifica la ruta `ExpenseRoute` para que señale al servicio remoto `//Adventure-Works.com/Expenses`.  
  
```sql  
ALTER ROUTE ExpenseRoute  
   WITH   
     SERVICE_NAME = '//Adventure-Works.com/Expenses';  
```  
  
### <a name="b-changing-the-target-database-for-a-route"></a>B. Cambiar la base de datos de destino para una ruta  
 En el ejemplo siguiente se cambia la base de datos de destino para la ruta `ExpenseRoute` a la base de datos identificada por el identificador único `D8D4D268-00A3-4C62-8F91-634B89B1E317.`  
  
```sql  
ALTER ROUTE ExpenseRoute  
   WITH   
     BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89B1E317';  
```  
  
### <a name="c-changing-the-address-for-a-route"></a>C. Cambiar la dirección para una ruta  
 En el ejemplo siguiente se cambia la dirección de red para la ruta `ExpenseRoute` al puerto TCP `1234` en el host con la dirección IP `10.2.19.72`.  
  
```sql  
ALTER ROUTE ExpenseRoute   
   WITH   
     ADDRESS = 'TCP://10.2.19.72:1234';  
```  
  
### <a name="d-changing-the-database-and-address-for-a-route"></a>D. Cambiar la base de datos y la dirección para una ruta  
 En el ejemplo siguiente se cambia la dirección de red para la ruta `ExpenseRoute` al puerto TCP `1234` en el host con el nombre DNS `www.Adventure-Works.com`. También se cambia la base de datos de destino a la base de datos identificada por el identificador único `D8D4D268-00A3-4C62-8F91-634B89B1E317`.  
  
```sql  
ALTER ROUTE ExpenseRoute  
   WITH   
     BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89B1E317',  
     ADDRESS = 'TCP://www.Adventure-Works.com:1234';  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/create-route-transact-sql.md)   
 [DROP ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-route-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
