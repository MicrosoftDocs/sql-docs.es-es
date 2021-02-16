---
title: Configure Always Encrypted using SSMS (Configurar Always Encrypted con SSMS)
description: Describe tareas para configurar y administrar bases de datos de Always Encrypted con SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 10/31/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fc8018153ca9f3ee87db9d9e19b4a696537e9e38
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345413"
---
# <a name="configure-always-encrypted-using-sql-server-management-studio"></a>Configure Always Encrypted using SQL Server Management Studio (Configurar Always Encrypted con SQL Server Management Studio)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

En este artículo se describen las tareas para configurar Always Encrypted y administrar las bases de datos que usan Always Encrypted con [SQL Server Management Studio (SSMS)](../../../ssms/download-sql-server-management-studio-ssms.md).

## <a name="security-considerations-when-using-ssms-to-configure-always-encrypted"></a>Consideraciones de seguridad al usar SSMS para configurar Always Encrypted

Cuando se usa SSMS para configurar Always Encrypted, SSMS controla las claves y la información confidencial de Always Encrypted, por lo que dichas claves e información aparecen como texto no cifrado en el proceso de SSMS. Por lo tanto, es importante ejecutar SSMS en un equipo seguro. Si la base de datos está hospedada en SQL Server, asegúrese de que SSMS se ejecuta en un equipo diferente del que hospeda la instancia de SQL Server. Dado que el objetivo principal de Always Encrypted es garantizar la seguridad de la información confidencial cifrada, incluso si el sistema de base de datos está en peligro, la ejecución de un script de PowerShell que procese las claves o la información confidencial en el equipo de SQL Server puede reducir o anular las ventajas de la característica. Para obtener más recomendaciones, vea [Security Considerations for Key Management](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management)(Consideraciones de seguridad para la administración de claves).

SSMS no admite la separación de roles entre los administradores de la base de datos (DBA) y los administradores de secretos criptográficos que tienen acceso a datos de texto no cifrado (administradores de seguridad o administradores de la aplicación). Si la organización exige la separación de roles, debe usar PowerShell para configurar Always Encrypted. Para obtener más información, consulte [Información general de administración de claves de Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md) y [Configuración de Always Encrypted con PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md). 

## <a name="always-encrypted-tasks-using-ssms"></a>Tareas de Always Encrypted mediante SSMS

- [Aprovisionamiento de claves de Always Encrypted mediante SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md)
- [Rotación de claves de Always Encrypted mediante SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md)
- [Configuración del cifrado de columna mediante el asistente para Always Encrypted](always-encrypted-wizard.md)
- [Configuración del cifrado de columnas mediante Always Encrypted con un paquete DAC](configure-always-encrypted-using-dacpac.md)
- [Consulta de columnas mediante Always Encrypted con SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Exportación e importación de bases de datos con Always Encrypted](always-encrypted-migrate-using-bacpac.md)
- [Copia de seguridad y restauración de bases de datos con Always Encrypted](always-encrypted-migrate-using-backup-restore.md)
- [Migración de datos a o desde columnas mediante Always Encrypted con el Asistente para importación y exportación de SQL Server](always-encrypted-migrate-using-import-export-wizard.md)

## <a name="see-also"></a>Consulte también
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Información general sobre la administración de claves de Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [Configurar Always Encrypted con PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)
- [Desarrollo de aplicaciones con Always Encrypted](always-encrypted-client-development.md)
