---
title: Posibles errores durante la creación de reflejo de la base de datos | Microsoft Docs
description: Obtenga información sobre los errores en las sesiones de creación de reflejos de bases de datos que provocan problemas físicos, del sistema operativo o de SQL Server, y cómo responder a estos errores.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- time-out period [SQL Server database mirroring]
- soft errors [SQL Server]
- database mirroring [SQL Server], troubleshooting
- timeout errors [SQL Server]
- troubleshooting [SQL Server], database mirroring
- hard errors
- failed database mirroring sessions [SQL Server]
ms.assetid: d7031f58-5f49-4e6d-9a62-9b420f2bb17e
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 2cd8b70c401a0cfc3b12f76d289b395f3d9874ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343740"
---
# <a name="possible-failures-during-database-mirroring"></a>Posibles errores durante la creación de reflejo de la base de datos
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Los problemas físicos, del sistema operativo o de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pueden provocar un error en una sesión de creación de reflejo de la base de datos. La creación de reflejo de la base de datos no comprueba regularmente los componentes de los que depende Sqlservr.exe para comprobar si están funcionando de forma correcta o si se ha producido un error. Sin embargo, en algunos tipos de errores, el componente afectado informa a Sqlservr.exe. Cuando otro componente informa del error, éste se denomina *error de hardware*. Para detectar otros errores que pudieran pasar desapercibidos, la creación de reflejo de la base de datos implementa su propio mecanismo de tiempo de espera. Si se agota el tiempo de espera de la creación de reflejo, la creación de reflejo de la base de datos supone que se ha producido un error y declara un *error de software*. Sin embargo, algunos errores que se producen en el nivel de instancia de SQL Server no ocasionan que se agote el tiempo de espera de la creación de reflejo y pueden no ser detectados.  
  
> [!IMPORTANT]  
>  Los errores en las bases de datos que no son reflejadas no se detectan en una sesión de creación de reflejo de la base de datos. Además, no es probable que se detecte un error en el disco de datos, a menos que la base de datos se reinicie por el error de un disco de datos.  
  
 La rapidez en la detección de errores y, por tanto, el tiempo de reacción ante un error de la sesión de creación de reflejo, depende de si el error es de hardware o software. En el caso de algunos errores de hardware, como los errores de red, se informa de inmediato. Sin embargo, en ciertos casos, es posible que períodos de tiempo de espera de componentes específicos retrasen el informe de algunos errores de hardware. En los errores de software, la longitud del período de tiempo de espera de la creación de reflejo determina la rapidez en la detección del error. De manera predeterminada, este valor es 10 segundos. Éste es el valor mínimo recomendado.  
  
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
  
 Algunos componentes, como los componentes de red y ciertos subsistemas de E/S, tienen sus propios tiempos de espera para determinar errores. Esos tiempos de espera son independientes de la creación de reflejo de la base de datos, que los desconoce y no sabe nada de su comportamiento. En estos casos, la demora del tiempo de espera aumenta el tiempo entre que se produce un error y el momento en que la base de datos de creación de reflejo recibe el error de hardware resultante.  
  
> [!NOTE]  
>  La única comprobación de errores activa que realiza el proceso de creación de reflejo de la base de datos se produce en los casos en que tiene lugar un error de software Para obtener más información, vea "Problemas debidos a errores de software", más adelante en este tema.  
  
 Para ayudarle a interpretar las situaciones de error que tienen lugar en su red, pregunte al ingeniero de red qué mensajes de error se envían a un puerto cuando tienen lugar los siguientes eventos en una conexión TCP:  
  
-   DNS no funciona  
  
-   Los cables están desconectados  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows tiene un firewall que bloquea un puerto específico.  
  
-   Se ha producido un error en la aplicación que está supervisando un puerto  
  
-   Se cambia el nombre de un servidor basado en Windows.  
  
-   Se reinicia un servidor basado en Windows.  
  
> [!NOTE]  
>  La creación de reflejos no protege frente a problemas específicos de los clientes que intentan obtener acceso a los servidores. Por ejemplo, imagine un caso en el que un adaptador de red pública controla las conexiones de los clientes a la instancia de servidor principal, mientras una tarjeta de interfaz de red privada controla todo el tráfico de creación de reflejo entre las instancias de servidor. En este caso, un error en el adaptador de red pública impediría que los clientes tuvieran acceso a la base de datos; si bien, el reflejo de la base de datos se seguirá creando.  
  
## <a name="failures-due-to-soft-errors"></a>Problemas debidos a errores de software  
 Las condiciones que pueden provocar el agotamiento del tiempo de espera en la creación de reflejo incluyen (sin limitarse a) lo siguiente:  
  
-   Errores de red, como tiempos de espera de vínculos TCP, paquetes que se han dañado o se han quitado o paquetes que están en un orden incorrecto.  
  
-   Un sistema operativo, un servidor o una base de datos que no responde.  
  
-   Se ha agotado el tiempo de espera de un servidor de Windows.  
  
-   Insuficientes recursos, tales como una sobrecarga de disco o CPU, llenado completo del registro de transacciones, falta de memoria o subprocesos en el sistema. En estos casos, debe incrementar el período de tiempo de espera, reducir la carga de trabajo o cambiar el hardware para que pueda ocuparse de la carga de trabajo.  
  
### <a name="the-mirroring-time-out-mechanism"></a>El mecanismo de tiempo de espera de la creación de reflejo  
 Debido a que los errores de software no son detectables directamente por una instancia de servidor, un error de software podría provocar que una instancia de servidor esperara de manera indefinida. Para evitarlo, la creación de reflejo de la base de datos implementa su propio mecanismo de tiempo de espera basado en que cada instancia de servidor de una sesión de creación de reflejo hace ping en cada conexión abierta con un intervalo fijo.  
  
 Para mantener una conexión abierta, una instancia de servidor debe recibir un ping en dicha conexión dentro del período de tiempo de espera definido, además del tiempo de espera necesario para enviar otro ping. La recepción de un ping durante el período de tiempo de espera indica que la conexión todavía está abierta y que las instancias de servidor se comunican a través de ella. Al recibir un ping, una instancia de servidor restablece su contador de tiempo de espera en dicha conexión.  
  
 Si no se recibe ningún ping en una conexión durante el período de tiempo de espera, una instancia de servidor considera que la conexión ha agotado el tiempo de espera. La instancia de servidor cierra la conexión que ha superado el tiempo de espera y controla el evento de tiempo de espera según el estado y el modo de funcionamiento de la sesión.  
  
 Incluso aunque el otro servidor funcione correctamente, un tiempo de espera agotado se considera un error. Si el valor de tiempo de espera establecido para una sesión es demasiado corto para el nivel de respuesta normal de cualquier servidor asociado, puede darse un error falso. Un error falso se produce cuando una instancia de servidor se pone en contacto correctamente con otra cuyo tiempo de respuesta es tan lento que sus comandos ping no se reciben antes de que expire el período de tiempo de espera.  
  
 En sesiones con el modo de alto rendimiento, el tiempo de espera siempre es de 10 segundos. Normalmente, esto es suficiente para evitar errores falsos. En sesiones con el modo de alta seguridad, el período de tiempo de espera predeterminado es de 10 segundos, pero puede cambiar su duración. Para evitar errores falsos, es recomendable que el período del tiempo de espera de la creación de reflejo sea siempre de 10 segundos o más.  
  
 **Para cambiar el valor del tiempo de espera (solo modo de alta seguridad)**  
  
-   Use la instrucción [ALTER DATABASE \<database> SET PARTNER TIMEOUT \<integer>](../../t-sql/statements/alter-database-transact-sql.md).  
  
 **Para visualizar el valor del tiempo de espera actual**  
  
-   Consulte **mirroring_connection_timeout** en [sys.database_mirroring](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md).  
  
## <a name="responding-to-an-error"></a>Responder a un error  
 Independientemente del tipo de error, una instancia de servidor que detecta un error responde apropiadamente según el rol de la instancia, el modo de funcionamiento de la sesión y el estado de las demás conexiones de la sesión. Para obtener información acerca de lo que ocurre en caso de pérdida de un asociado, vea [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).  
  
## <a name="see-also"></a>Consulte también  
 [Calcular la interrupción del servicio durante la conmutación de roles &#40;creación de reflejo de la base de datos&#41;](../../database-engine/database-mirroring/estimate-the-interruption-of-service-during-role-switching-database-mirroring.md)   
 [Modos de funcionamiento de la creación de reflejo de la base de datos](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)   
 [Conmutación de roles durante una sesión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)   
 [Creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
