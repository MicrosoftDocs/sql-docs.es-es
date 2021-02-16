---
title: Instalar actualizaciones de servicio de SQL Server | Microsoft Docs
description: En este artículo se proporciona información sobre cómo instalar actualizaciones para SQL Server durante una nueva instalación o después de instalar SQL Server.
ms.custom: ''
ms.date: 09/05/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 7d6c962b-c8d0-49f7-a2ac-00ad8dca930a
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 6c9ff0ec46dd63f50bdacf0e3feec5df1fbd90f3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348375"
---
# <a name="install-sql-server-servicing-updates"></a>Instalar actualizaciones de servicio de SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

En este artículo se proporciona información sobre el procedimiento para instalar actualizaciones de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)]. Esta sección proporciona información acerca de lo siguiente:
  
- Instalar actualizaciones para [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] durante una nueva instalación  
  
- Instalar actualizaciones para [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] una vez instalado.  
  
Instale las últimas actualizaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puntualmente para asegurarse de que los sistemas están al día con las actualizaciones de seguridad más recientes.  
  
## <a name="installing-updates-for-noversion-during-a-new-installation"></a>Instalar actualizaciones para [!INCLUDE[noVersion](../../includes/ssNoVersion-md.md)] durante una nueva instalación  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] integra las últimas actualizaciones del producto con la instalación del producto principal, de modo que el producto principal y las actualizaciones aplicables se instalen al mismo tiempo. La actualización del producto puede buscar actualizaciones aplicables en:  
  
- [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update  
  
- Windows Server Update Services (WSUS)  
  
- Una carpeta local  
  
- Un recurso compartido de red  
  
Una vez que el programa de instalación encuentra las versiones más recientes de las actualizaciones aplicables, las descarga y las integra con el proceso de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] actual. La actualización del producto puede incluir una actualización acumulativa, un Service Pack o un Service Pack más la actualización acumulativa.  
  
## <a name="installing-updates-for-ssnoversion-after-it-has-already-been-installed"></a>Instalar actualizaciones para [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] una vez instalado  
En una instancia instalada de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)], se recomienda aplicar las actualizaciones de seguridad y actualizaciones críticas más recientes, incluidas las versiones de distribución general (GDR), Services Packs (SP) y actualizaciones acumulativas. Para más información, vea el [anuncio de marzo de 2016 sobre el modelo de servicio incremental (ISM) de SQL Server](/archive/blogs/sqlreleaseservices/announcing-updates-to-the-sql-server-incremental-servicing-model-ism).

> [!NOTE]
> A partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] se está adoptando un ciclo de vida de servicio estándar simplificado y predecible y los Service Packs (SP) ya no están disponibles. Solo hay actualizaciones acumulativas (CU) y versiones de distribución generales (GDR) en caso necesario.
> Para más información, vea el [anuncio de septiembre de 2017 sobre el modelo de servicio moderno (MSM) de SQL Server](/archive/blogs/sqlreleaseservices/announcing-the-modern-servicing-model-for-sql-server).
  
Las actualizaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] están disponibles a través de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update (MU), Windows Server Update Services (WSUS) y del Centro de descarga de Microsoft. Las actualizaciones críticas y de seguridad para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] están disponibles a través de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update y para poder verlas necesita suscribirse a Microsoft Update a través del applet Windows Update del Panel de control.  
  
Cuando reciba una actualización a través de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update, se actualizan todas las características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la versión más reciente en modo desatendido. Si necesita más flexibilidad o no tiene acceso a Internet o a WSUS, debe obtener las actualizaciones en el Centro de descarga de [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
## <a name="see-also"></a>Consulte también  
[Instalación de SQL Server desde el asistente para instalación](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)        
[Agregar características a una instancia de SQL Server &#40;programa de instalación&#41;](./add-features-to-an-instance-of-sql-server-setup.md)         
[Reparar una instalación de SQL Server con errores](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)