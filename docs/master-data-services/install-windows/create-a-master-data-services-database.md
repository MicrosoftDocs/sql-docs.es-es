---
title: Crea una base de datos de Master Data Services
description: Cree una base de datos de Master Data Services cuando necesite una base de datos nueva para admitir la aplicación Web de Master Data Manager y Master Data Services servicio Web.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 8373bb35-f0f9-4c3c-a53c-dfaa2ce567ac
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 464bfd4714e7cc7680b2a2800d1c2ae1711feeb1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272486"
---
# <a name="create-a-master-data-services-database"></a>Crea una base de datos de Master Data Services

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Cree una base de datos de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] cuando necesite una base de datos nueva que sea compatible con la aplicación web de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] y el servicio web de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] .  
  
## <a name="prerequisites"></a>Requisitos previos  
  
-   Para obtener información sobre los requisitos del equipo que hospeda la base de datos, vea [Requisitos de base de datos &#40;Master Data Services&#41;](../../master-data-services/install-windows/database-requirements-master-data-services.md).  
  
### <a name="to-create-a-master-data-services-database"></a>Para crear una base de datos de Master Data Services  
  
1.  Abra [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)].  
  
2.  En el panel izquierdo, haga clic en **Configuración de base de datos**.  
  
3.  En la página **Configuración de base de datos** , haga clic en **Crear base de datos**.  
  
4.  Complete el asistente **Crear base de datos** para crear y configurar la base de datos. Para obtener información sobre las opciones de interfaz de usuario (IU) en el asistente, vea [Asistente Crear base de datos &#40;Administrador de configuración de Master Data Services&#41;](../../master-data-services/create-database-wizard-master-data-services-configuration-manager.md).  
  
## <a name="next-steps"></a>Pasos siguientes  
  
-   Configure los parámetros del sistema para la aplicación web y de base de datos. Para obtener más información, vea [Configuración del sistema &#40;Master Data Services&#41;](../../master-data-services/system-settings-master-data-services.md).  
  
-   Cree una aplicación web de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] para asociarla a esta base de datos. Para obtener más información, vea [Crear una aplicación web de Master Data Manager &#40;Master Data Services&#41;](../../master-data-services/install-windows/create-a-master-data-manager-web-application-master-data-services.md).  
  
-   Configure un plan de mantenimiento para hacer copias de seguridad de la base de datos y de los registros de transacciones. Para obtener más información, vea [Requisitos de base de datos &#40;Master Data Services&#41;](../../master-data-services/install-windows/database-requirements-master-data-services.md).  
  
## <a name="see-also"></a>Consulte también  
 [Instalar Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)  
  
  
