---
description: MSSQLSERVER_14420
title: MSSQLSERVER_14420 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 14420 (Database Engine error)
ms.assetid: 4a1d72b1-ab1b-4119-a11b-a8a05c6fdb45
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: b85c252180248a45cb8f267299bdffccfef16d80
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196976"
---
# <a name="mssqlserver_14420"></a>MSSQLSERVER_14420
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|14420|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|SQLErrorNum14420|  
|Texto del mensaje|La base de datos principal de trasvase de registros %s.%s tiene un umbral de copia de seguridad de %d minutos y no ha realizado una operación de copia de seguridad del registro durante %d minutos. Compruebe la información del registro de agente y del monitor de trasvase de registros.|  
  
## <a name="explanation"></a>Explicación  
El trasvase de registros no está sincronizado más allá del umbral de copia de seguridad. El umbral de copia de seguridad es la cantidad de minutos que pueden transcurrir entre los trabajos de copia de seguridad del trasvase de registros antes de que se genere una alerta. Este mensaje no indica necesariamente un problema en el trasvase de registros. En su lugar, puede indicar uno de los siguientes problemas:  
  
-   El trabajo de copia de seguridad no está en ejecución. Entre las posibles causas de esto se encuentran: el servicio del Agente SQL Server en la instancia del servidor principal no se está ejecutando, el trabajo está deshabilitado o la programación del trabajo ha cambiado.  
  
-   Error en el trabajo de copia de seguridad. Entre las posibles causas de esta situación se encuentran: la ruta de acceso de la carpeta de copia de seguridad no es válida, el disco está lleno o cualquier otro motivo por el que la instrucción BACKUP generara un error.  
  
## <a name="user-action"></a>Acción del usuario  
Para solucionar el problema de este mensaje:  
  
-   Asegúrese de que el servicio del Agente SQL Server está en ejecución para la instancia del servidor principal y que el trabajo de copia de seguridad de la base de datos principal está habilitado y programado para funcionar con la frecuencia adecuada.  
  
-   Es posible que exista un error en el trabajo de copia de seguridad del servidor principal. En este caso, compruebe el historial de trabajos del trabajo de copia de seguridad para buscar la causa.  
  
-   Es posible que el trabajo de copia de seguridad del trasvase de registros, que se ejecuta en la instancia del servidor principal, no pueda conectarse a la instancia del servidor de supervisión para actualizar la tabla **log_shipping_monitor_primary**. Esto puede deberse a un problema de autenticación entre la instancia del servidor de supervisión y la instancia del servidor principal.  
  
-   Es posible que el umbral de alerta de la copia de seguridad tenga un valor incorrecto. Lo ideal sería establecer este valor en el triple de la frecuencia del trabajo de copia de seguridad. Si cambia la frecuencia del trabajo de copia de seguridad después de configurar y poner en funcionamiento el trasvase de registros, debe actualizar en consecuencia el valor del umbral de alerta de la copia de seguridad.  
  
-   Cuando la instancia del servidor de supervisión se desconecta y se vuelve a conectar posteriormente, la tabla **log_shipping_monitor_primary** no se actualiza con los valores actuales antes de que se ejecute el trabajo del mensaje de alerta. Para actualizar las tablas de supervisión con los últimos datos de la base de datos principal, ejecute **sp_refresh_log_shipping_monitor** en la instancia del servidor principal.  
  
-   La fecha o la hora es incorrecta en la instancia del servidor de supervisión o primario. Esto puede generar también mensajes de alerta. Es posible que la fecha o la hora del sistema se haya modificado en uno de ellos.  
  
    > [!NOTE]  
    > El hecho de que existan diferentes zonas horarias en las dos instancias de servidor no debería causar problemas.  
  
## <a name="see-also"></a>Consulte también  
[log_shipping_monitor_primary &#40;Transact-SQL&#41;](~/relational-databases/system-tables/log-shipping-monitor-primary-transact-sql.md)  
[Acerca del trasvase de registros &#40;SQL Server&#41;](~/database-engine/log-shipping/about-log-shipping-sql-server.md)  
[sp_help_log_shipping_monitor_primary &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-primary-transact-sql.md)  
[sp_refresh_log_shipping_monitor &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-transact-sql.md)  
[Acerca del trasvase de registros &#40;SQL Server&#41;](~/database-engine/log-shipping/about-log-shipping-sql-server.md)  
  
