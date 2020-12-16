---
title: Testigos de seguimiento (Información de publicación)
description: Descripción de la pestaña "Testigos de seguimiento" de la página "Información de la publicación" que se encuentra en el Monitor de replicación dentro de SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.monitor.publicationinfo.tracertokens.f1
ms.assetid: a115ba95-17ae-45df-91bd-5a1a35f3745f
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: a3b273e574b1a21c2f495b684063c525ccc7f362
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460188"
---
# <a name="publication-information-tracer-tokens-sql-server-2005-and-later"></a>Información de la publicación, Testigos de seguimiento (SQL Server 2005 y posterior)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  La pestaña **Testigos de seguimiento** le permitirá validar las conexiones y medir la latencia de un sistema que utiliza la replicación transaccional. Se escribe un token (una pequeña cantidad de datos) en el registro de transacción de la base de datos de publicaciones, marcado como si fuese una transacción replicada, y se envía a través del sistema, de forma que permite calcular:  
  
-   Cuánto tiempo transcurre desde que se confirma una transacción en el publicador hasta que se inserta el comando correspondiente en la base de datos de distribución del distribuidor.  
  
-   Cuánto tiempo transcurre desde que se inserta un comando en la base de datos de distribución hasta que se confirma la transacción correspondiente en el suscriptor.  
  
 A partir de estos cálculos, podrá responder a diversas preguntas, como por ejemplo:  
  
-   ¿Qué suscriptores tardan más en recibir un cambio del publicador?  
  
-   De los suscriptores que esperan recibir el testigo de seguimiento, ¿cuáles, si los hay, no lo han recibido?  
  
## <a name="options"></a>Opciones  
 Para cambiar la manera que la cuadrícula muestra los datos, haga clic con el botón secundario en la cuadrícula y, a continuación, haga clic en una de las opciones siguientes:  
  
-   **Ordenar**: ordene en una o más columnas en el cuadro de diálogo **Ordenar columnas** .  
  
-   **Elegir columnas para mostrar**: seleccione las columnas que se mostrarán y el orden en el que se mostrarán en el cuadro de diálogo **Elegir columnas** .  
  
-   **Filtro**: filtre filas en la cuadrícula basándose en los valores de columna en el cuadro de diálogo **Configuración del filtro** .  
  
-   **Borrar filtro**: borre cualquier configuración de filtro para la cuadrícula.  
  
 La configuración del filtro es específica de cada cuadrícula. La selección y ordenación de las columnas se aplica a todas las cuadrículas del mismo tipo, como la cuadrícula de las publicaciones para cada Publicador.  
  
 **Insertar seguimiento**  
 Haga clic para insertar un testigo de seguimiento en el registro de transacciones del publicador.  
  
 **Hora de inserción**  
 Seleccione la hora de inserción del testigo de seguimiento para mostrar la información de latencia a partir de ese momento. De forma predeterminada, se mostrará la información a partir de la hora más reciente.  
  
> [!NOTE]  
>  La información del testigo de seguimiento se guarda durante el mismo período que otros datos del historial, que depende del período de retención de historial de la base de datos de distribución. Para obtener información sobre cómo cambiar las propiedades de la base de datos de distribución, vea [Ver y modificar las propiedades del distribuidor y del publicador](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md).  
  
 **Suscripción**  
 Nombre de cada suscripción a la publicación.  
  
 **Publicador a distribuidor**  
 Tiempo que transcurre desde que se confirma una transacción en el publicador hasta que se inserta el comando correspondiente en la base de datos de distribución del distribuidor. Si un valor muestra **Pendiente** indica que el token aún no ha llegado al distribuidor. Si persiste el estado pendiente, compruebe que se esté ejecutando el Agente de registro del LOG.  
  
 **Distribuidor a suscriptor**  
 Tiempo que transcurre desde que se inserta un comando en la base de datos de distribución hasta que se confirma la transacción correspondiente en el suscriptor. Si un valor muestra **Pendiente** indica que el token aún no ha llegado al suscriptor. Si persiste el estado pendiente, compruebe que se esté ejecutando el Agente de distribución.  
  
 **Latencia total**  
 Tiempo que transcurre desde que se confirma una transacción en el publicador hasta que se confirma la transacción correspondiente en el suscriptor. Representa la latencia de un extremo a otro del sistema de replicación del suscriptor en dicho momento. Si un valor muestra **Pendiente** indica que el token aún no ha llegado al suscriptor.  
  
## <a name="see-also"></a>Consulte también  
 [Iniciar y detener un agente de replicación &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md)   
 [Iniciar el Monitor de replicación](../../relational-databases/replication/monitor/start-the-replication-monitor.md)   
 [Medir la latencia y validar las conexiones de la replicación transaccional](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)   
 [Supervisar el rendimiento con el Monitor de replicación](../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md)   
 [Monitoring Replication](../../relational-databases/replication/monitor/monitoring-replication.md)  (Supervisar la replicación)  
 [Información general sobre los agentes de replicación](../../relational-databases/replication/agents/replication-agents-overview.md)  
  
  
