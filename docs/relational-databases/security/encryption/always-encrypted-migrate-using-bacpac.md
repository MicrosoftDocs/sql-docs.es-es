---
description: Exportación e importación de bases de datos con Always Encrypted
title: Exportación e importación de bases de datos con Always Encrypted | Microsoft Docs
ms.custom: ''
ms.date: 10/30/2019
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
ms.openlocfilehash: 674ad450fc1cd2ef45aad45333463632dafcf523
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345472"
---
# <a name="export-and-import-databases-using-always-encrypted"></a>Exportación e importación de bases de datos con Always Encrypted 
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

En este artículo se describe cómo exportar e importar bases de datos que contienen columnas protegidas con [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md).

Al exportar una base de datos, todos los datos almacenados en las columnas cifradas de esa base de datos se recuperan en formato cifrado (texto cifrado) y se colocan en el [BACPAC](../../data-tier-applications/data-tier-applications.md) resultante. El BACPAC resultante también contiene los metadatos de las claves de Always Encrypted.

Al importar el BACPAC en una base de datos, los datos cifrados del BACPAC se cargan en la base de datos y se vuelven a crear los metadatos de clave de Always Encrypted. 

Si tiene una aplicación que está configurada para consultar columnas cifradas almacenadas en la base de datos de origen (la que exportó), no tendrá que hacer nada para permitir que la aplicación consulte los datos cifrados en la base de datos de destino, ya que las claves de ambas bases de datos son las mismas.

Para más información sobre cómo exportar e importar una base de datos, vea:
- [Exportar una aplicación de la capa de datos](../../data-tier-applications/export-a-data-tier-application.md)
- [Importar un archivo de bacpac para crear una nueva base de datos de usuario](../../data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database.md)
- [Exportación de una base de datos de Azure SQL Database a un archivo BACPAC](/azure/sql-database/sql-database-export)
- [Importación de un archivo BACPAC en una base de datos de Azure SQL Database](/azure/sql-database/sql-database-import)
- [SqlPackage.exe](../../../tools/sqlpackage/sqlpackage.md)

## <a name="permissions-for-migrating-databases-with-encrypted-columns"></a>Permisos para migrar las bases de datos con columnas cifradas

Necesita los permisos *ALTER ANY COLUMN MASTER KEY* y *ALTER ANY COLUMN ENCRYPTION KEY* en la base de datos de origen. Necesita los permisos *ALTER ANY COLUMN MASTER KEY*, *ALTER ANY COLUMN ENCRYPTION KEY*, *VIEW ANY COLUMN MASTER KEY DEFINITION* y *VIEW ANY COLUMN ENCRYPTION DEFINITION* en la base de datos de destino.

No es necesario tener acceso a las claves maestras de columna configuradas para las columnas cifradas, ya que los datos permanecen cifrados durante las operaciones de exportación e importación.

## <a name="next-steps"></a>Pasos siguientes
- [Desarrollo de aplicaciones con Always Encrypted](always-encrypted-client-development.md)

## <a name="see-also"></a>Consulte también
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Copia de seguridad y restauración de bases de datos con Always Encrypted](always-encrypted-migrate-using-backup-restore.md)
- [Migración de datos a o desde columnas mediante Always Encrypted con el Asistente para importación y exportación de SQL Server](always-encrypted-migrate-using-import-export-wizard.md)
- [Carga masiva de datos cifrados a columnas mediante Always Encrypted](migrate-sensitive-data-protected-by-always-encrypted.md)