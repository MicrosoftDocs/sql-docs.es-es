---
description: MSSQLSERVER_8645
title: MSSQLSERVER_8645 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8645 (Database Engine error)
ms.assetid: 63d6d6d7-3850-4061-8e96-b1fa665e3180
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8f1772c8c73fb18173916b91ea14adf3cf997f4d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205675"
---
# <a name="mssqlserver_8645"></a>MSSQLSERVER_8645
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|8645|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|MEMTIMEDOUT_ERR|  
|Texto del mensaje|Se agotó el tiempo de espera para que los recursos de memoria ejecutaran la consulta. Vuelva a ejecutar la consulta.|  
  
## <a name="explanation"></a>Explicación  
Se agotó el tiempo de espera mientras los recursos de memoria ejecutaban la consulta en el grupo de recursos 'predeterminado'.  
  
## <a name="user-action"></a>Acción del usuario  
Si no utiliza el regulador de recursos, le recomendamos que compruebe el estado y la carga generales del servidor, o la configuración del grupo de recursos o del grupo de carga de trabajo.  
  
En la siguiente lista se describen los pasos generales que ayudarán a resolver los errores de memoria:  
  
1.  Compruebe si otras aplicaciones o servicios están consumiendo memoria en este servidor. Vuelva a configurar las aplicaciones o servicios menos críticos para que consuman menos memoria.  
  
2.  Empiece a recopilar los contadores del monitor de rendimiento para **SQL Server: Administrador de búfer**, **SQL Server: Administrador de memoria**.  
  
3.  Compruebe los siguientes parámetros de configuración de memoria de SQL Server:  
  
    -   **memoria de servidor máxima**  
  
    -   **memoria de servidor mínima**  
  
    -   **memoria mínima por consulta**  
  
    Observe si hay algún valor fuera de lo normal. Corríjalos según sea necesario. Investigue el porqué de los mayores requisitos de memoria de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. La configuración predeterminada se enumera en "Establecer las opciones de configuración del servidor" en los Libros en pantalla de SQL Server.  
  
4.  Observe la salida de DBCC MEMORYSTATUS y la forma en que cambia cuando aparecen estos mensajes de error.  
  
5.  Compruebe la carga de trabajo (por ejemplo, el número de sesiones simultáneas y las consultas que se están ejecutando actualmente).  

Las siguientes acciones pueden hacer que haya más memoria disponible para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
-   Si otras aplicaciones están consumiendo recursos, intente detener su ejecución o plantéese ejecutarlas en otro servidor. Esto quitará presión externa de la memoria.  
  
-   Si ha configurado **max server memory**, aumente su valor.  
  
Ejecute los siguientes comandos DBCC para liberar varias cachés de la memoria de SQL Server.  
  
-   DBCC FREESYSTEMCACHE  
  
-   DBCC FREESESSIONCACHE  
  
-   DBCC FREEPROCCACHE  
  
Si el problema persiste, necesitará investigar más y, posiblemente, reducir la carga de trabajo.  
  
