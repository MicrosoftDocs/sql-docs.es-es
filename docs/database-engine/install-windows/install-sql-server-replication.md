---
title: Instalar la replicación de SQL Server | Microsoft Docs
description: Para instalar los componentes de replicación, use el Asistente para la instalación de SQL Server o una ventana de símbolo del sistema.
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- components [SQL Server replication]
- command line installations [SQL Server replication]
- installing replication
- replication [SQL Server], installing
- command prompt [SQL Server replication]
ms.assetid: c50ad078-060b-4a8d-ad45-9e31a8d85729
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: 55a8a744c384df0536cef1073e05b2fc61a7a1ac
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125929"
---
# <a name="install-sql-server-replication"></a>Instalar la replicación de SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Los componentes de replicación se pueden instalar mediante el Asistente para la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o en un símbolo del sistema. Instale la replicación al instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]o al modificar una instancia existente.  
  
Después de instalar los componentes de replicación, debe configurar el servidor antes de poder utilizar la replicación. Para obtener más información, vea [Configurar la distribución](../../relational-databases/replication/configure-distribution.md) en los Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
>[!IMPORTANT]  
>Si instala componentes de replicación al modificar una instancia existente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], debe detener y reiniciar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] una vez completada la instalación. De esta forma contribuye a asegurarse de que el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reconozca los subsistemas del agente de replicación y pueda llamar a los agentes de replicación mediante pasos de trabajos.  
  
## <a name="installing-replication-by-using-setup"></a>Instalar la replicación mediante el programa de instalación  
**Para instalar la replicación al instalar una nueva instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**  
  
- Para instalar componentes de replicación, incluido Replication Management Objects (RMO), seleccione **Replicación de SQL Server** en la página **Selección de características** del Asistente para la instalación.  
  
## <a name="installing-replication-from-the-command-prompt"></a>Instalar la replicación desde el símbolo del sistema  
 **Para instalar la replicación al instalar una nueva instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**  
  
- Consulte [Instalar SQL Server desde el símbolo del sistema](./install-sql-server-from-the-command-prompt.md).  
  
## <a name="see-also"></a>Consulte también  
 [Instalar SQL Server](../../database-engine/install-windows/install-sql-server.md)   
 [Instalar SQL Server desde el símbolo del sistema](./install-sql-server-from-the-command-prompt.md)   
 [Características compatibles con las ediciones de SQL Server](../../sql-server/editions-and-components-of-sql-server-2017.md)  
  
