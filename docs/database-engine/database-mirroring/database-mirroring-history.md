---
title: Historial de creación de reflejo de la base de datos | Microsoft Docs
description: Obtenga información sobre cómo ver el historial del estado de creación de reflejo de una base de datos reflejada en una instancia del servidor especificada en SQL Server Management Studio.
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.databasemirroringhistory.f1
ms.assetid: 1d6e4b10-4a23-47d7-9918-c417992f09d3
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 4dee4a7fd6632cfb8ef2f4a6950f20979c912b54
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353143"
---
# <a name="database-mirroring-history"></a>Historial de creación de reflejo de la base de datos
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Utilice este cuadro de diálogo para ver el historial del estado de creación de reflejo de una base de datos reflejada en una instancia del servidor especificada.  
  
 **Para utilizar SQL Server Management Studio a fin de supervisar la creación de reflejo de la base de datos**  
  
-   [Iniciar el Monitor de creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="options"></a>Opciones  
 **Instancia del servidor**  
 Nombre de la instancia del servidor desde la que se obtiene el historial.  
  
 **Base de datos**  
 Nombre de la base de datos cuyo historial se está notificando.  
  
 **Filtrar lista por**  
 Muestra los valores de filtro. Si se elige un nuevo valor, se actualiza la cuadrícula automáticamente.  
  
 Los valores de filtro posibles son:  
  
-   Últimas dos horas  
  
-   Últimas cuatro horas  
  
-   Últimas ocho horas  
  
-   Último día  
  
-   Últimos dos días  
  
-   Últimos 100 registros  
  
-   Últimos 500 registros  
  
-   Últimos 1000 registros  
  
-   Todos los registros  
  
 **Actualizar**  
 Haga clic en esta opción para actualizar la lista del historial.  
  
> [!NOTE]  
>  Este cuadro de diálogo no actualiza automáticamente la lista del historial. Para actualizar la lista, haga clic en **Actualizar** o elija otra opción de filtro. Solo los miembros del rol fijo de servidor **sysadmin** pueden actualizar el historial de creación de reflejo.  
  
 **Historial**  
 Muestra la lista del historial. Si se hace clic en el encabezado de columna, se ordena la cuadrícula por esa columna. La lista contiene las columnas siguientes:  
  
|Nombre de la columna|Descripción|  
|-----------------|-----------------|  
|**Hora de registro**|Marca de tiempo de la fila de historial.|  
|**Rol**|Rol de creación de reflejo actual de la instancia del servidor para esta base de datos, ya sea principal o reflejado.|  
|**Estado de creación de reflejo**|Estado de la base de datos:<br /><br /> Escenario desconectado<br /><br /> Conmutación por error pendiente<br /><br /> Suspended<br /><br /> Sincronizado<br /><br /> Sincronizando<br /><br /> Desconocido|  
|**Conexión del testigo**|Estado de la conexión del testigo en la sesión de creación de reflejo de la base de datos, ya sea Conectado o Desconectado. Si no hay testigo, el valor es NULL.|  
|**Registro sin enviar**|Tamaño, en kilobytes (KB), del registro sin enviar en la cola de envío de la instancia del servidor principal.|  
|**Hora de envío**|Cantidad aproximada de tiempo que la instancia del servidor principal necesitará para enviar el registro (actualmente en la cola de envío) a la instancia del servidor reflejado ( *tasa de envío*). Puesto que la velocidad de las transacciones entrantes puede variar de forma significativa, el registro de la hora de envío es estimativo. Sin embargo, la tasa de envío puede resultar útil para estimar de forma aproximativa el tiempo requerido para una conmutación por error manual.|  
|**Send Rate**|Velocidad con la que las transacciones se envían a la instancia del servidor reflejado, expresada en KB por segundo.|  
|**Nueva tasa de transacción**|Velocidad con la que las transacciones entrantes se insertan en el registro del servidor principal, expresada en KB por segundo. Para determinar si la creación de reflejo se retrasa, se mantiene o se recupera, compare este valor con el valor **Hora de envío** .|  
|**Transacción sin enviar más antigua**|Antigüedad de la transacción sin enviar más antigua de la cola de envío. La antigüedad de esta transacción indica la cantidad de minutos de transacciones que no se han enviado aún a la instancia del servidor reflejado. Este valor ayuda a medir la posibilidad de pérdida de datos con respecto a la hora.|  
|**Registro sin restaurar**|Cantidad de registro a la espera en la cola rehecha, expresada en KB.|  
|**Hora de restauración**|Número aproximado de minutos necesarios para que el registro que se encuentra actualmente en la cola de puesta al día se aplique a la base de datos reflejada.|  
|**Tasa de restauración**|Velocidad con la que las transacciones se restauran en la base de datos reflejada, expresada en KB por segundo.|  
|**Sobrecarga de confirmación del servidor reflejado**|Retraso medio por transacción en milisegundos (solo en modos asincrónicos). Este retardo es la cantidad de sobrecarga en la que se incurre mientras la instancia del servidor principal espera a la instancia del servidor reflejado para escribir la entrada de registro de la transacción en la cola de puesta al día.|  
  
## <a name="see-also"></a>Consulte también  
 [Iniciar el Monitor de creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [Supervisar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [Iniciar el Asistente para la configuración de seguridad de la creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
