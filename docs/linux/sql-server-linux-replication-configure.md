---
title: Configuración de la replicación (SSMS)
description: Obtenga información sobre cómo configurar la replicación de SQL Server en Linux. Configure la replicación con SQL Server Management Studio (SSMS) o procedimientos almacenados de Transact-SQL.
ms.custom: seo-dt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/20/2018
ms.topic: article
ms.prod: sql
ms.prod_service: database-engine
ms.technology: linux
titleSuffix: SQL Server on Linux
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 911446480834b9ff8b51ce806729f2771d28d84f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346488"
---
# <a name="configure-sql-server-replication-on-linux"></a>Configurar Replicación de SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

[!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] presenta Replicación de SQL Server para instancias de SQL Server en Linux.

Para obtener más información sobre la replicación, vea la [documentación sobre Replicación de SQL Server](../relational-databases/replication/sql-server-replication.md).

Configure la replicación en Linux con SQL Server Management Studio (SSMS) o procedimientos almacenados de Transact-SQL.

* Para usar SSMS, siga las instrucciones que aparecen en este artículo.

  Use SSMS en un sistema operativo Windows para conectarse a instancias de SQL Server. Para obtener instrucciones e información general, vea [Empleo de SQL Server Management Studio en Windows para administrar SQL Server en Linux](./sql-server-linux-manage-ssms.md).
  
* Para obtener un ejemplo con procedimientos almacenados, siga el tutorial [Configurar Replicación de SQL Server en Linux](sql-server-linux-replication-tutorial-tsql.md).

## <a name="prerequisites"></a>Requisitos previos

Antes de configurar los publicadores, distribuidores y suscriptores, necesita completar un par de pasos de configuración para la instancia de SQL Server.

1. Habilite el Agente SQL Server para usar los agentes de replicación. En todos los servidores con Linux, ejecute los comandos siguientes en el terminal.

  ```bash
  sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true
  sudo systemctl restart mssql-server
  ```

1. Configure la instancia de SQL Server para la replicación. Para configurar la instancia de SQL Server para la replicación, ejecute `sys.sp_MSrepl_createdatatypemappings` en todas las instancias que participen en la replicación.

  ```sql
  USE msdb
  GO
  exec sys.sp_MSrepl_createdatatypemappings;
  GO
  ```

1. Cree una carpeta de instantáneas. Los agentes de SQL Server necesitan una carpeta de instantáneas con permisos de lectura y escritura. Cree la carpeta de instantáneas en el distribuidor.

  Para crear la carpeta de instantáneas y conceder acceso al usuario `mssql`, ejecute el comando siguiente:

  ```bash
  sudo mkdir /var/opt/mssql/data/ReplData/
  sudo chown mssql /var/opt/mssql/data/ReplData/
  sudo chgrp mssql /var/opt/mssql/data/ReplData/
  ```

## <a name="configure-and-monitor-replication-with-sql-server-management-studio-ssms"></a>Configurar y supervisar la replicación con SQL Server Management Studio (SSMS)

### <a name="configure-the-distributor"></a>Configurar el distribuidor
  
Para configurar el distribuidor: 

1. En SSMS, conéctese a su instancia de SQL Server en el Explorador de objetos.

1. Haga clic con el botón derecho en **Replicación** y seleccione **Configurar distribución…**

1. Siga las instrucciones del **Asistente para la configuración de distribución**.

### <a name="create-publication-and-articles"></a>Crear publicaciones y artículos

Para crear una publicación y artículos:

1. En el Explorador de objetos, haga clic en **Replicación** > **Publicaciones locales**> **Nueva publicación…**

1. Siga las instrucciones del **Asistente para nueva publicación** para configurar el tipo de replicación y los artículos que pertenecen a la publicación.

### <a name="configure-the-subscription"></a>Configurar la suscripción

Para configurar la suscripción en el Explorador de objetos, haga clic en **Replicación** > **Suscripciones locales**> **Nuevas suscripciones…**

### <a name="monitor-replication-jobs"></a>Supervisar trabajos de replicación

Use el monitor de replicación para supervisar los trabajos de replicación.

En el Explorador de objetos, haga clic con el botón derecho en **Replicación** y, después, seleccione **Iniciar monitor de replicación**.

## <a name="next-steps"></a>Pasos siguientes

[Conceptos: Replicación de SQL Server en Linux](sql-server-linux-replication.md)

[Procedimientos almacenados de replicación](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)
