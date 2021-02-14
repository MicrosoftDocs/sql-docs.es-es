---
title: Notas de la versión de SQL Server 2019 | Microsoft Docs
description: Obtenga información sobre las limitaciones de SQL Server 2019 (15.x), los problemas conocidos, los recursos de ayuda y otras notas de la versión.
ms.date: 11/04/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: article
ms.assetid: 13942af8-5a40-4cef-80f5-918386767a47
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = sql-server-ver15
ms.openlocfilehash: e8a9f06dc33acab6862027cd2fdb55bb4aca4dfc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080436"
---
# <a name="sql-server-2019-release-notes"></a>Notas de la versión de [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)]
[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este artículo se describen las limitaciones y los problemas conocidos de [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)]. Para obtener información relacionada, consulte:

> [Novedades de [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)]](../sql-server/what-s-new-in-sql-server-ver15.md)

## [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]

[!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] es la versión pública más reciente de [!INCLUDE[SQL Server 2019](../includes/ssnoversion-md.md)].

Los detalles completos sobre las licencias se encuentran en la carpeta `License Terms` del soporte de instalación.

## <a name="documentation"></a>Documentación

- **Problema e impacto en el cliente**: la documentación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se puede filtrar por versión. Use el control situado en la parte superior izquierda de cada página de documentación para filtrar según sus requisitos.

## <a name="build-number"></a>Número de compilación

El número de compilación RTM de SQL Server 2019 es `15.0.2000.5`.

[!INCLUDE [sql-server-servicing-updates-version-15](../includes/sql-server-servicing-updates-version-15.md)]

## <a name="sql-server-installation-may-fail-if-ssms-18x-is-installed"></a>La instalación de SQL Server puede presentar errores si está instalado SSMS 18.x

- **Problema e impacto en el cliente**: la instalación de [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] presenta errores cuando las instalaciones siguientes se hacen en este orden:
  1. SQL Server Management Studio (SSMS) (versión 18.0, 18.1, 18.2 o 18.3) está instalado en el servidor.
  1. La instalación de [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] se intenta desde medios extraíbles. Por ejemplo, el soporte de instalación es un DVD.

- **Solución alternativa**:
  1. Desinstale cualquier versión de SSMS anterior a SSMS 18.3.1.
  1. Instale una versión más reciente de SSMS (18.3.1 o posterior). Para obtener la versión más reciente, consulte [Descargar SSMS](../ssms/download-sql-server-management-studio-ssms.md).
  1. Instale [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] de la manera habitual.

- **Se aplica a**: [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)]

## <a name="utf-8-collations"></a>Intercalaciones UTF-8

- **Problema e impacto en el cliente**: No se pueden usar las intercalaciones habilitadas para UTF-8 con las siguientes características:
  - [Tablas OLTP en memoria](../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md)
  - [Always Encrypted con enclaves seguros](../relational-databases/security/encryption/always-encrypted-enclaves.md) (cuando no se usa enclaves seguros, [Always Encrypted](../relational-databases/security/encryption/always-encrypted-database-engine.md) puede usar UTF-8)

  > [!WARNING]
  > Se produce un error al crear un [bacpac](../relational-databases/data-tier-applications/data-tier-applications.md#bacpac) de una base de datos que contiene columnas de tabla definidas como [CHAR o VARCHAR](../t-sql/data-types/char-and-varchar-transact-sql.md) que usan más de 4000 bytes.
  
  > [!NOTE]
  > Actualmente, no hay ninguna compatibilidad de interfaz de usuario para elegir intercalaciones habilitadas para UTF-8 en Azure Data Studio o SQL Server Data Tools (SSDT). La versión 18, la más reciente de [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] (SSMS), permite elegir las intercalaciones habilitadas para UTF-8 en la interfaz de usuario.

- **Se aplica a**: [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] RTM

## <a name="master-data-service-notification-email-contains-broken-link"></a>El correo electrónico de notificación de Master Data Services contiene un vínculo roto

- **Problema e impacto en el cliente**: El correo electrónico de notificación de Master Data Services (MDS) contiene un vínculo roto. El vínculo navega a una página que devuelve un error similar al mensaje siguiente:

   `The view 'Index' or its master was not found or no view engine supports the searched locations.`

- **Solución alternativa**: Abra el portal de MDS y vaya al recurso manualmente.

- **Se aplica a**: [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] RTM

## <a name="see-also"></a>Consulte también

- [Requisitos de hardware y software para instalar SQL Server](../sql-server/install/hardware-and-software-requirements-for-installing-sql-server-ver15.md)

## <a name="machine-learning-services"></a>Machine Learning Services

En el caso de problemas de SQL Server Machine Learning Services, consulte [Problemas conocidos de SQL Server Machine Learning Services](../machine-learning/troubleshooting/known-issues-for-sql-server-machine-learning-services.md).

[!INCLUDE[get-help-options-msft-only](../includes/paragraph-content/get-help-options.md)]
