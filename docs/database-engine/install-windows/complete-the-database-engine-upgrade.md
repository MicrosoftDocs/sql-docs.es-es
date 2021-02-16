---
title: Completar la actualización del motor de base de datos | Microsoft Docs
description: En este artículo se describen algunos pasos adicionales que podría tener que realizar después de finalizar la actualización del Motor de base de datos de SQL Server.
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.technology: install
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 3f08087e-e532-416c-8caa-e0ec88c57596
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 5e4d727666ad2e3c013aacdabee93517389059bd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347828"
---
# <a name="complete-the-database-engine-upgrade"></a>Completar la actualización motor de base de datos

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Una vez completada la actualización a SQL Server, hay algunos pasos más que hay que realizar. Entre ellas, figuran:  
  
Después de actualizar el [!INCLUDE[ssDE](../../includes/ssde-md.md)], complete las siguientes tareas:  
  
- **Hacer una copia de seguridad de las bases de datos:** haga una copia de seguridad completa de cada base de datos.  

- **Habilitar nuevas características:** En SQL Server 2016, 2017 y 2019, algunos cambios solo se habilitan una vez que el nivel de DATABASE_COMPATIBILITY de una base de datos se ha cambiado, como mínimo, a 130.  Para obtener más información y para ver el flujo de trabajo recomendado, vea [Cambiar el modo de compatibilidad de la base de datos y usar el almacén de consultas](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md). Si la base de datos tiene tablas optimizadas para memoria creadas en SQL Server 2014, vea [Estadísticas para las tablas optimizadas para memoria](../../relational-databases/in-memory-oltp/statistics-for-memory-optimized-tables.md).
  
- **Integration Services:**  
  
     Migre los paquetes de Integration Services al formato [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] . Para obtener más información, consulte [Upgrade Integration Services Packages](../../integration-services/install-windows/upgrade-integration-services-packages.md).  
  
- **Reporting Services:** para una nueva actualización de instalación, restaure las claves de cifrado de Reporting Services. Para obtener más información, vea [Hacer copia de seguridad y restaurar claves de cifrado de Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).  
  
- **Master Data Services:**  actualice el esquema de base de datos MDS y cree la aplicación web de SQL Server 2019. Para obtener más información, vea [Actualizar Master Data Services](../../database-engine/install-windows/upgrade-master-data-services.md).  
  
- **Data Quality Services:** actualice el esquema de base de datos DQS y compruebe la actualización del esquema de base de datos DQS. Para obtener más información, vea [Actualizar Data Quality Services](../../database-engine/install-windows/upgrade-data-quality-services.md).  
  
- **Búsqueda de texto completo:** Vuelva a rellenar los catálogos de texto completo para garantizar la coherencia semántica de los resultados de la consulta. Para obtener más información, vea [Rellenar índices de texto completo](../../relational-databases/search/populate-full-text-indexes.md).  
  
