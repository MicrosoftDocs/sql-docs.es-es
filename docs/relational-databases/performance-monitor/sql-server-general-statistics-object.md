---
title: General Statistics (objeto de SQL Server) | Microsoft Docs
description: 'Obtenga más información sobre el objeto SQLServer: General Statistics, que proporciona contadores para supervisar la actividad general de todo el servidor, como el número de conexiones actuales.'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:General Statistics
- General Statistics object
ms.assetid: c738e549-d7e7-4211-9ec3-064ac140af7c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 124352d2c252f927d0b07a34f1077fe0c9dde210
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505694"
---
# <a name="sql-server-general-statistics-object"></a>General Statistics (objeto de SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  El objeto **SQLServer:General Statistics** de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona contadores para supervisar la actividad general en el servidor, como el número de conexiones actuales y el número de usuarios que se conectan y se desconectan de equipos en los que se ejecuta una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] por segundo. Esto resulta útil cuando se trabaja en sistemas de procesamiento de transacciones en línea (OLTP) grandes, con un gran número de clientes que se conectan y se desconectan de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Esta tabla describe los contadores de **General Statistics** de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Contadores de Estadísticas generales de SQL Server|Descripción|  
|--------------------------------------------|-----------------|  
|**Tablas temporales activas**|Número de tablas o variables de tabla temporales en uso.|  
|**Restablecimientos de conexión/s**|Número total de inicios de sesión comenzados desde el grupo de conexiones.|  
|**Notificaciones de eventos que se quitarán con retraso**|Número de notificaciones de eventos que esperan a que un subproceso del sistema las quite.|  
|**Solicitudes HTTP autenticadas**|Número de solicitudes HTTP autenticadas iniciadas por segundo.|  
|**Conexiones lógicas**|Número de conexiones lógicas con el sistema.<br /><br /> La principal finalidad de las conexiones lógicas es dar servicio a varias solicitudes de conjuntos de resultados activos múltiples (MARS). En las solicitudes MARS, cada vez que una aplicación establece una conexión con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puede haber varias conexiones lógicas que correspondan a una conexión física.<br /><br /> Cuando no se usa MARS, la relación entre conexiones físicas y lógicas es 1:1. Por tanto, cada vez que una aplicación establece una conexión con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], las conexiones lógicas aumentarán en 1.|  
|**Inicios de sesión/seg.**|Número total de inicios de sesión por segundo. No se incluyen las conexiones agrupadas.|  
|**Cierres de sesión/seg.**|Número total de cierres de sesión iniciados por segundo.|  
|**Interbloqueos de Mars**|Número de interbloqueos de Mars detectados.|  
|**Velocidad de esperas no atómicas**|Número de esperas no atómicas por segundo.|  
|**Procesos bloqueados**|Número de los procesos bloqueados actualmente.|  
|**Solicitudes SOAP vacías**|Número de solicitudes SOAP vacías iniciadas por segundo.|  
|**Llamadas de métodos SOAP**|Número de llamadas de métodos SOAP iniciadas por segundo.|  
|**Solicitudes de inicio de sesión SOAP**|Número de solicitudes de inicio de sesión SOAP iniciadas por segundo.|  
|**Solicitudes de finalización de sesión SOAP**|Número de solicitudes de finalización de sesión SOAP iniciadas por segundo.|  
|**Solicitudes SOAP SQL**|Número de solicitudes SOAP SQL iniciadas por segundo.|  
|**Solicitudes SOAP WSDL**|Número de solicitudes de lenguaje de descripción de servicios web SOAP iniciadas por segundo.|  
|**Esperas de bloqueo de proveedor de E/S de seguimiento de SQL**|Número de esperas de bloqueo del proveedor de E/S de archivos por segundo.| 
|**Velocidad de creación de tablas temporales**|Número de tablas o variables de tabla temporales creadas por segundo.|  
|**Tablas temporales que destruir**|Número de tablas y variables de tablas temporales en espera de que destruya el subproceso de sistema de limpieza.|  
|**Identificador de unidad de recuperación de tempdb**|El número de identificador de unidad de recuperación tempdb duplicado que se generó.|
|**Identificador de conjunto de filas de tempdb**|El número de identificador de conjunto de filas tempdb duplicado que se generó.| 
|**Cola de notificación de eventos de seguimiento**|Número de instancias de notificación de eventos de seguimiento que esperan en la cola interna para ser enviados a través de Service Broker.|  
|**Transactions**|Número de altas de transacciones (locales, DTC y enlazadas juntas).|  
|**Conexiones de usuario**|Cuenta el número de usuarios actualmente conectados a SQL Server.|  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar el uso de recursos&#40;Monitor de sistema&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
