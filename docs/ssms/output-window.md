---
description: Ventana de salida de SQL Server Management Studio
title: Ventana de salida de SSMS
ms.custom: seo-lt-2019
ms.date: 08/09/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Output Window [SQL Server Management Studio]
- Activity Monitor [SQL Server Management Studio]
- Object Explorer [SQL Server Management Studio]
ms.assetid: a2ce1a07-b4e2-471c-87d2-b8de5e6c6864
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f123f01811bec35f8418e8e6a5935d581650c261
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353958"
---
# <a name="output-window-in-sql-server-management-studio"></a>Ventana de salida de SQL Server Management Studio
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
La Ventana de salida se puede abrir en el menú Ver o mediante la combinación de teclas Ctrl+Alt+O. Hay varios canales de salida disponibles.

En la tabla siguiente se ofrece una visión general de los tipos de mensajes asociados a cada canal de salida.

|Canal|Descripción|
|-----------|---------------|  
|**Telemetría**|La telemetría es la secuencia de [datos de uso de la función anónima](sql-server-management-studio-ssms.md) que recopila Microsoft. Estos eventos pueden ser útiles para sus propios registros de uso de SSMS. Pueden ayudarle a identificar los nodos del Explorador de objetos que ha expandido, así como qué comandos ha ejecutado durante su sesión en SSMS con la Ventana de salida abierta.|
|**Explorador de objetos**|Este canal da como resultado el texto de consulta y el tiempo transcurrido para realizar las consultas SQL necesarias para expandir nodos en el Explorador de objetos. Cada consulta registra una consulta inicial y un evento de consulta final. Cada evento tiene una marca de tiempo y el URN asociados a la entidad a la que se envía la consulta. El [URN](/previous-versions/sql/sql-server-2005/ms220608(v=sql.90)) hace referencia al objeto de administración de SQL subyacente y consta de una jerarquía de estilo XPath. Por ejemplo, el URN de una tabla llamada "Tabla1" en la base de datos "Base de datos" del servidor "Mi servidor" sería "Server[@Name='MiServidor']/Database[@Name='BaseDeDatos'/Table[/@Name='Tabla1']".  Al expandir un nodo en el Explorador de objetos, puede realizar varias de estas consultas usando parámetros diferentes. El evento de finalización de consulta contendrá el tiempo transcurrido de la consulta junto con el texto TSQL. Es posible que esta consulta de datos le resulte útil para el análisis del rendimiento del servidor en casos en los que el Explorador de objetos parezca inusualmente lento al intentar expandir un nodo determinado. **Nota**: No todos los nodos del Explorador de objetos aportan este nivel de detalle de la consulta al expandirse.|
|**Monitor de actividad**|Este canal se inicia cuando se [abre el Monitor de actividad](../relational-databases/performance-monitor/activity-monitor.md) para un servidor. Esta secuencia contiene eventos que muestran parte del texto de la consulta y la marca de tiempo de cada consulta, mensajes de error y notificaciones del monitor para informar de que están en pausa debido a problemas de conectividad. Si el Monitor de actividad parece estar inactivo o no se puede actualizar, compruebe este canal de salida para obtener más información.|