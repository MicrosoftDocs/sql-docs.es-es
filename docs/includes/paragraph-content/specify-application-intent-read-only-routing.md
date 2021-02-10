---
title: archivo de inclusión
description: archivo de inclusión
services: sql-database
author: MightyPen
ms.service: sql-database
ms.topic: include
ms.date: 04/05/2018
ms.author: genemi
ms.custom: include file
ms.openlocfilehash: 7086a69625b6ba365e5efb8ce3422799442cb05d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058409"
---
## <a name="specifying-application-intent"></a>Especificar el intento de la aplicación

Puede especificarse la palabra clave **ApplicationIntent** en la cadena de conexión. Los valores asignables son **ReadWrite** o **ReadOnly**. El valor predeterminado es **ReadWrite**.

Cuando **ApplicationIntent=ReadOnly**, el cliente solicita una carga de trabajo de lectura al conectarse. El servidor aplicará la intención en el momento de la conexión y durante una instrucción de base de datos **USE**.

La palabra clave **ApplicationIntent** no funciona con bases de datos de solo lectura heredadas.  


#### <a name="targets-of-readonly"></a>Destinos de ReadOnly

Cuando una conexión elige **ReadOnly**, la conexión se asigna a cualquiera de las siguientes configuraciones especiales que pueden existir para la base de datos:

- [Always On](~/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)
    - Una base de datos puede permitir o denegar la lectura de las cargas de trabajo en la base de datos de destino Always On. Esta opción se controla mediante la cláusula **ALLOW_CONNECTIONS** de las instrucciones Transact-SQL **PRIMARY_ROLE** y **SECONDARY_ROLE**.

- [Replicación geográfica](/azure/sql-database/sql-database-geo-replication-overview):

- [Escalado horizontal de lectura](/azure/sql-database/sql-database-read-scale-out)

Si ninguno de esos destinos especiales está disponible, se lee desde la base de datos normal.

&nbsp;

La palabra clave **ApplicationIntent** habilita el *enrutamiento de solo lectura*.


## <a name="read-only-routing"></a>Enrutamiento de solo lectura

El enrutamiento de solo lectura es una característica que puede asegurar la disponibilidad de una réplica de solo lectura de una base de datos. Para habilitar el enrutamiento de solo lectura, se aplica lo siguiente:

- Debe conectarse siempre a un agente de escucha de grupo de disponibilidad Always On.

- La palabra clave de cadena de conexión de **ApplicationIntent** debe establecerse en **ReadOnly**.

- El administrador de bases de datos debe configurar el grupo de disponibilidad para habilitar el enrutamiento de solo lectura.

Es posible que varias conexiones, cada una con enrutamiento de solo lectura, no se conecten todas a la misma réplica de solo lectura. Los cambios en la sincronización de la base de datos o los cambios en la configuración de enrutamiento del servidor pueden producir conexiones de cliente para réplicas de solo lectura diferentes. Puede asegurarse de que todas las solicitudes de solo lectura se conecten a la misma réplica de solo lectura. Para ello, *no* pase un cliente de escucha del grupo de disponibilidad a la palabra clave **Server** de la cadena de conexión. En su lugar, especifique el nombre de la instancia de solo lectura.

El enrutamiento de solo lectura puede tardar más que la conexión a la principal. La espera más larga se debe a que el enrutamiento de solo lectura se conecta primero a la principal y, luego, busca la mejor instancia secundaria legible que esté disponible. Debido a estos múltiples pasos, debe aumentar el tiempo de espera de inicio de sesión a 30 segundos como mínimo.