---
title: Causas de los errores de conectividad entre réplicas de disponibilidad
description: En este tema se describen las posibles razones de un error de conexión entre las réplicas que participan en un grupo de disponibilidad Always On.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- troubleshooting [SQL Server], HADR
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], troubleshooting
ms.assetid: cd613898-82d9-482f-a255-0230a6c7d6fe
author: cawrites
ms.author: chadam
ms.openlocfilehash: 478724d2081e95deb10bd2f0e44a6cd330fe94ce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350776"
---
# <a name="determine-possible-reason-for-connectivity-failures-between-availability-replicas"></a>"Determinación de la posible causa de los errores de conectividad entre réplicas de disponibilidad
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
Los problemas físicos, del sistema operativo o de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pueden producir un error en una sesión entre dos réplicas de disponibilidad. Una réplica de disponibilidad no comprueba con regularidad los componentes de los que depende Sqlservr.exe para comprobar si están funcionando de forma correcta o si se ha producido un error. Sin embargo, en algunos tipos de errores, el componente afectado informa a Sqlservr.exe. Cuando otro componente informa del error, éste se denomina *error de hardware*. Para detectar otros errores que podrían pasar desapercibidos, [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] implementa su propio mecanismo de tiempo de espera de la sesión. Especifica el período de espera de la sesión en segundos. El período de espera es el tiempo máximo durante el que una instancia de servidor espera hasta recibir un mensaje PING de otra instancia, antes de considerar que la otra instancia está desconectada. Cuando se agota el tiempo de espera de sesión entre dos réplicas de disponibilidad, las réplicas de disponibilidad suponen que se ha producido un error y declaran *un error de software*.  
  
> [!IMPORTANT]  
>  Los errores de bases de datos que no son de la base de datos principal no son detectables. Además, no es probable que se detecte un error en el disco de datos, a menos que la base de datos se reinicie por el error de un disco de datos.  
  
 La rapidez en la detección de errores y, por tanto, el tiempo de reacción ante un error, depende de si el error es de hardware o software. En el caso de algunos errores de hardware, como los errores de red, se informa de inmediato. Sin embargo, en ciertos casos, es posible que períodos de tiempo de espera de componentes específicos retrasen el informe de algunos errores de hardware. En los errores de software, la longitud del período de tiempo de espera de la sesión determina la rapidez en la detección del error. De manera predeterminada, este valor es 10 segundos. Éste es el valor mínimo recomendado.  
  
## <a name="failures-due-to-hard-errors"></a>Problemas debidos a errores de hardware  
 Las posibles causas de errores de hardware incluyen (sin limitarse a) las siguientes condiciones:  
  
-   Una conexión interrumpida o un cable roto  
  
-   Una tarjeta de red en mal estado  
  
-   Un cambio de enrutador  
  
-   Cambios en el firewall  
  
-   Reconfiguración de extremos  
  
-   Pérdida de la unidad en que reside el registro de transacciones  
  
-   Errores de proceso o errores del sistema operativo  
  
 Por ejemplo, cuando la unidad de registro de la base de datos principal deja de responder y se produce un error, el sistema operativo informa a Sqlservr.exe de que se ha producido un error grave.  
  
 Algunos componentes, como los componentes de red y ciertos subsistemas de E/S, tienen sus propios tiempos de espera para determinar errores. Esos tiempos de espera son independientes de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], que los desconoce y no sabe nada de su comportamiento. En estos casos, la demora del tiempo de espera aumenta el tiempo entre que se produce un error y el momento en que la réplica de disponibilidad recibe el error de hardware resultante.  
  
> [!NOTE]  
>  La única comprobación de errores activa realizada para las réplicas de disponibilidad se produce en los casos en que tiene lugar un error de software Para obtener más información, vea "Problemas debidos a errores de software", más adelante en este tema.  
  
 Para ayudarle a interpretar las situaciones de error que tienen lugar en su red, pregunte al ingeniero de red qué mensajes de error se envían a un puerto cuando tienen lugar los siguientes eventos en una conexión TCP:  
  
-   DNS no funciona  
  
-   Los cables están desconectados  
  
-   [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows tiene un firewall que bloquea un puerto específico.  
  
-   Se ha producido un error en la aplicación que está supervisando un puerto  
  
-   Se cambia el nombre de un servidor basado en Windows.  
  
-   Se reinicia un servidor basado en Windows.  
  
> [!NOTE]  
>  [!INCLUDE[ssHADRc](../../../includes/sshadrc-md.md)] no protege frente a problemas específicos de los clientes que intentan obtener acceso a los servidores. Por ejemplo, considere un caso en que un adaptador de red pública controla las conexiones de cliente a la réplica principal, como una tarjeta de interfaz de red privada controla el tráfico entre las instancias de servidor que están hospedando las réplicas de un grupo de disponibilidad. En este caso, el error del adaptador de red pública evitaría que los clientes tuvieran acceso a las bases de datos.  
  
## <a name="failures-due-to-soft-errors"></a>Problemas debidos a errores de software  
 Las condiciones que pueden provocar el agotamiento del tiempo de espera de la sesión incluyen (sin limitarse a) lo siguiente:  
  
-   Errores de red, como tiempos de espera de vínculos TCP, paquetes que se han dañado o se han quitado o paquetes que están en un orden incorrecto.  
  
-   Un sistema operativo, un servidor o una base de datos que no responde.  
  
-   Se ha agotado el tiempo de espera de un servidor de Windows.  
  
-   Insuficientes recursos, tales como una sobrecarga de disco o CPU, llenado completo del registro de transacciones, falta de memoria o subprocesos en el sistema. En estos casos, debe incrementar el período de tiempo de espera, reducir la carga de trabajo o cambiar el hardware para que pueda ocuparse de la carga de trabajo.  
  
### <a name="the-session-timeout-mechanism"></a>El mecanismo de tiempo de espera de la sesión  
 Dado que los errores de software no pueden detectarse directamente en una instancia del servidor, los errores de software podrían ocasionar que una réplica de disponibilidad espere indefinidamente una respuesta de la otra réplica de disponibilidad de la sesión. Para evitar esto, [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] implementa su propio mecanismo de tiempo de espera de sesión, que hace que las réplicas de disponibilidad conectadas envíen un ping en cada conexión abierta a intervalos de tiempo fijos. La recepción de un ping durante el período de tiempo de espera indica que la conexión todavía está abierta y que las instancias de servidor se comunican a través de ella. Al recibir un ping, la réplica restablece el contador de tiempo de espera de dicha conexión. Para obtener más información sobre la relación del modo de disponibilidad y los tiempos de espera, vea [Modos de disponibilidad &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
 La réplica principal y secundaria hacen ping entre sí para indicar que siguen activas, y un tiempo de espera de sesión limitado impide que una réplica espere indefinidamente a recibir el ping de la otra réplica. El límite de tiempo de espera de la sesión es una propiedad de la réplica configurable por el usuario que tiene un valor predeterminado de 10 segundos. La recepción de un ping durante el período de tiempo de espera indica que la conexión todavía está abierta y que las instancias de servidor se comunican a través de ella. Al recibir un ping, la réplica de disponibilidad restablece el contador de tiempo de espera de la sesión de esa conexión.  
  
 Si no se recibe ningún ping de la otra réplica durante el tiempo de espera de la sesión, el tiempo de espera se agota. La conexión se cierra y la réplica con el tiempo de espera agotado entra en el estado DISCONNECTED. Aunque una replicación desconectada esté configurada en modo de confirmación sincrónica, las transacciones no esperarán a que la réplica vuelva a conectarse y sincronizarse.  
  
## <a name="responding-to-an-error"></a>Responder a un error  
 Independientemente del tipo de error, una instancia de servidor que detecta un error responde apropiadamente según el rol de la instancia, el modo de disponibilidad de la sesión y el estado de las demás conexiones de la sesión. Para obtener más información sobre lo que pasa cuando se pierde un asociado, vea [Modos de disponibilidad &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
## <a name="related-tasks"></a>Related Tasks  
 **Para cambiar el valor de tiempo de espera (solo en modo de disponibilidad de confirmación sincrónica)**  
  
-   [Cambiar el tiempo de espera de la sesión en una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
 **Para visualizar el valor del tiempo de espera actual**  
  
-   Consulte **session_timeout** en [sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md).  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
