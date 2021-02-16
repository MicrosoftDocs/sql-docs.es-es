---
title: Consulta del informe del trasvase de registros (SSMS)
description: Vea el estado del trasvase de registros de transacciones en SQL Server Management Studio. Ejecute un informe de estado en un servidor de supervisión, un servidor principal o un servidor secundario.
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- viewing log shipping reports
- displaying log shipping reports
- log shipping [SQL Server], monitoring
- log shipping [SQL Server], viewing reports
ms.assetid: 3b549f2f-3683-45e5-b8e8-8095276c41ab
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6f16ee75c10cd8e33e83cc16bf157ad981205c4a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344490"
---
# <a name="view-the-log-shipping-report-sql-server-management-studio"></a>Ver el informe de trasvase de registros (SQL Server Management Studio)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo ver el informe de estado de trasvase del registro de transacciones en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Puede ejecutar un informe de estado en un servidor de supervisión, un servidor principal o un servidor secundario. Para obtener la información más completa sobre la configuración del trasvase del registro, vea el informe en la instancia del servidor de supervisión.  
  
 El informe muestra el estado de cualquier actividad de trasvase del registro cuyo estado esté disponible desde la instancia de servidor a la que esté conectado. Si esa instancia de servidor se utiliza en varias configuraciones con distintos roles (por ejemplo, actuar como servidor de supervisión para una base de datos y como secundario para otra base de datos), los resultados mostrados contendrán la información de cada configuración desde el punto de vista de cada rol. Si el procedimiento almacenado puede conectarse a la instancia de servidor de supervisión para determinada configuración del trasvase del registro, el informe muestra un estado adicional para dicha configuración.  
  
 Para cada rol que desempeñe la instancia de servidor actual se puede ver la siguiente información:  
  
|Role|Información mostrada|  
|----------|---------------------------|  
|Supervisión|El nombre y el estado de cada servidor principal y cada servidor secundario que utiliza esta instancia de servidor como su servidor de supervisión.|  
|Principal|Para cada base de datos principal, el nombre y el estado de la instancia de servidor actual (como servidor primario), junto con el nombre de la base de datos principal. El informe muestra el estado del trabajo de copia de seguridad (que se almacena localmente en el servidor principal).<br /><br /> El informe contiene además una fila por cada uno de los servidores secundarios correspondientes. Si la configuración utiliza un servidor de supervisión y el procedimiento almacenado puede conectarse a dicho servidor, estas filas muestran el estado de copia y el estado de restauración del registro de copia de seguridad más reciente.|  
|Secundario|Para cada base de datos secundaria, el nombre y el estado de la instancia de servidor actual (como servidor secundario), junto con el nombre de la base de datos secundaria.<br /><br /> El informe muestra el estado de los trabajos de copia y restauración en el servidor secundario.<br /><br /> El informe contiene además una fila para el servidor principal correspondiente. Si la configuración utiliza un servidor de supervisión y el procedimiento almacenado puede conectarse a dicho servidor, esta fila muestra el estado del registro de la copia de seguridad más reciente.|  
  
 La información mostrada depende de si la instancia de servidor es un servidor de supervisión, un servidor principal o un servidor secundario. Si no hay información disponible, las celdas correspondientes aparecen en gris.  
  
 El informe llama a **sp_help_log_shipping_monitor** para obtener los datos. Para obtener información sobre los permisos necesarios, vea [sp_help_log_shipping_monitor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-transact-sql.md).  
  
### <a name="to-display-the-transaction-log-shipping-status-report-on-a-server-instance"></a>Para mostrar el informe de estado de trasvase del registro de transacciones en una instancia de servidor  
  
1.  Conéctese a un servidor de supervisión, un servidor principal o un servidor secundario.  
  
2.  Haga clic con el botón derecho en la instancia del servidor del Explorador de objetos, seleccione **Informes** e **Informes estándar**.  
  
3.  Haga clic en **Estado de trasvase del registro de transacciones**.  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar el trasvase de registros &#40;Transact-SQL&#41;](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)  
  
  
