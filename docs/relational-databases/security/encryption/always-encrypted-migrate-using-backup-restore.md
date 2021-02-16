---
description: Copia de seguridad y restauración de bases de datos con Always Encrypted
title: Copia de seguridad y restauración de bases de datos con Always Encrypted | Microsoft Docs
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1b5f491846b51b1d70b36c73029ad96706cb1b3e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345487"
---
# <a name="backup-and-restore-databases-using-always-encrypted"></a>Copia de seguridad y restauración de bases de datos con Always Encrypted 
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

En este artículo se describe cómo realizar una copia de seguridad y restaurar una base de datos que contiene columnas protegidas con [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md).

Cuando se realiza una copia de seguridad de una base de datos, el archivo de copia de seguridad resultante contiene cifrado almacenado en columnas cifradas y todos los metadatos de las claves de Always Encrypted.

Al restaurar una base de datos, se restauran todos los datos cifrados y todos los metadatos de las claves de Always Encrypted. 

Si ha restaurado la base de datos en un servidor diferente o con un nombre diferente, no es necesario hacer nada especial para permitir que la aplicación consulte los datos cifrados en la base de datos de destino, ya que las claves de ambas bases de datos son las mismas.

Para obtener más información sobre cómo realizar una copia de seguridad de una base de datos y cómo restaurarla, vea:
- [Backup Overview (SQL Server)](../../backup-restore/backup-overview-sql-server.md)
- [Restauración de una base de datos en una instancia administrada](/azure/sql-database/sql-database-managed-instance-get-started-restore)

## <a name="next-steps"></a>Pasos siguientes
- [Consulta de columnas mediante Always Encrypted con SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Desarrollo de aplicaciones mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-client-development.md) 

## <a name="see-also"></a>Consulte también
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Exportación e importación de bases de datos con Always Encrypted](always-encrypted-migrate-using-bacpac.md)
- [Migración de datos a o desde columnas mediante Always Encrypted con el Asistente para importación y exportación de SQL Server](always-encrypted-migrate-using-import-export-wizard.md)
- [Carga masiva de datos cifrados a columnas mediante Always Encrypted](migrate-sensitive-data-protected-by-always-encrypted.md)