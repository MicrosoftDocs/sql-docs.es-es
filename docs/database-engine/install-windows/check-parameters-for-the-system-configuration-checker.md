---
title: Comprobación de los parámetros del Comprobador de configuración del sistema
description: Durante la instalación de SQL Server, el Comprobador de configuración del sistema para las condiciones que impiden una instalación correcta de SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- installing SQL Server, system configuration checks
- failed system configuration checks [SQL Server]
- verifying configuration before installation
- SCC [SQL Server]
- system configuration checker
- scanning computer before installation [SQL Server]
- checking configuration before installation
- status information [SQL Server], system configuration checker
- parameters [SQL Server], system configuration checker
- configuration checkers [SQL Server]
- Setup [SQL Server], system configuration checker
ms.assetid: 8e712c15-6bfa-4d71-b303-9526101e5594
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 541247d27df04953ca91daa4dcd99386170f9ea0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347855"
---
# <a name="check-parameters-for-the-system-configuration-checker"></a>Comprobar los parámetros del Comprobador de configuración del sistema

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Durante la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , el Comprobador de configuración del sistema (SCC) examina el equipo en el que se instalará [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . El SCC comprueba las condicione que impiden una instalación correcta de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Antes de que el programa de instalación inicie el Asistente para la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , el SCC recupera el estado de cada elemento. A continuación, compara el resultado con las condiciones necesarias y proporciona instrucciones para evitar problemas de bloqueo.  
  
El comprobador de la configuración del sistema genera un informe que contiene una descripción breve de cada regla ejecutada y el estado de ejecución. El informe de comprobación de la configuración del sistema se encuentra en %programfiles%\Microsoft SQL Server\140\Setup Bootstrap\Log\\\<YYYYMMDD_HHMM>\\\..    
  
Vea los vínculos siguientes para obtener más información:

- [Requisitos de hardware y software para instalar SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)   
- [Consideraciones de seguridad para una instalación de SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)   
- [Actualizaciones de ediciones y versiones admitidas](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)  
  
  
