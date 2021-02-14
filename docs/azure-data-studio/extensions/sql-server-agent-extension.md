---
title: Extensión del Agente SQL Server
description: Obtenga información sobre cómo instalar y usar la extensión del Agente SQL Server para Azure Data Studio, una extensión para administrar trabajos y configuraciones del Agente SQL.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 6bec78ab2c79af6f6a4dca70b25345f567def48b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040275"
---
# <a name="sql-server-agent-extension-preview"></a>Extensión del Agente SQL Server (versión preliminar)

La extensión del Agente SQL Server (versión preliminar) es una extensión para administrar y solucionar los problemas de los trabajos del Agente SQL y la configuración. Esta extensión está actualmente en versión preliminar.

Estas son algunas de las acciones principales:

- Mostrar una lista de los trabajos del Agente SQL Server configurados en una instancia de SQL Server
- Ver el historial de trabajos con resultados de la ejecución de trabajos
- Control básico de trabajos para iniciar y detener trabajos

## <a name="install-the-sql-server-agent-extension"></a>Instalar la extensión del Agente SQL Server

1. Para abrir el administrador de extensiones y acceder a las extensiones disponibles, seleccione el icono de extensiones, o bien seleccione **Extensiones** en el menú **Ver**.
2. Seleccione una extensión disponible para ver sus detalles.

   ![Instalar el agente](media/sql-server-agent-extension/install-sql-agent.png)

3. Seleccione la extensión que quiera e **instálela**.
4. Seleccione **Recargar** para habilitar la extensión (solo es necesario la primera vez que se instala una extensión).
5. Para ir al panel de administración, haga clic con el botón derecho en el servidor o la base de datos y seleccione **Administrar**.
6. Las extensiones instaladas se muestran como pestañas en el panel de administración:

   ![Ver el agente](media/sql-server-agent-extension/view-sql-agent.png)

## <a name="view-jobs"></a>Ver trabajos

Al conectarse a la extensión del Agente SQL Server, lo primero que verá es una lista de todos los trabajos del agente.

   ![Ver trabajos](media/sql-server-agent-extension/job-view.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el Agente SQL Server, [vea la documentación](../../ssms/agent/sql-server-agent.md).
