---
description: Resize the Job History Log
title: Resize the Job History Log
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], history
- resizing job history log
- size [SQL Server], job history log
- logs [SQL Server], jobs
- SQL Server Agent jobs, history
- historical information [SQL Server], jobs
ms.assetid: ddee1ce8-9d1b-4017-9894-bf7256aed95d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 61b6bef074842a297a46df9ba548602527924873
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339819"
---
# <a name="resize-the-job-history-log"></a>Resize the Job History Log

[!INCLUDE[applies-to-version/_ssnoversion.md](../../includes/applies-to-version/sqlserver.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

En este tema se describe cómo establecer límites de tamaño para registros de historial de trabajos del Agente [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].

- **Antes de empezar:**  

    [Seguridad](#Security)  

- **Para establecer los límites de tamaño de los registros de historial de trabajos, utilizando:**  

    [SQL Server Management Studio](#SSMS)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Antes de empezar  

### <a name="security"></a><a name="Security"></a>Seguridad

Para obtener información detallada, vea [Implementar la seguridad del Agente SQL Server](../../ssms/agent/implement-sql-server-agent-security.md).  

## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Usar SQL Server Management Studio

*Para cambiar el tamaño del registro de historial de trabajos según un tamaño sin procesar*

1. En el **Explorador de objetos** , conéctese a una instancia de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]y, después, expándala.

2. Haga clic con el botón derecho en **Agente SQL Server** y, después, seleccione **Propiedades**.

3. Seleccione la página **Historial** y, luego, configure que la opción **Limitar tamaño del registro de historial de trabajos** esté seleccionada.

4. En la casilla **Tamaño máximo del registro de historial de trabajos (filas)** , escriba el número máximo de filas que debe permitir el registro de historial de trabajos.

5. En la casilla **Máximo de filas de historial de trabajos por trabajo** , escriba el número máximo de filas que se va a permitir para un trabajo en el historial de trabajos.

**Para cambiar el tamaño del registro de historial de trabajos según la hora:**

1. En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]y, a continuación, expándala.  

2. Haga clic con el botón derecho en **Agente SQL Server** y, después, seleccione **Propiedades**.

3. Seleccione la página **Historial** y, después **Quitar historial del agente**.

4. Seleccione el número apropiado de **días**, **semanas** o **meses**.