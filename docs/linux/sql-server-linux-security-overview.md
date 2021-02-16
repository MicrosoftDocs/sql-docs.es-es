---
title: Limitaciones de seguridad para SQL Server en Linux
description: Obtenga información sobre las restricciones de SQL Server en Linux, incluida la incompatibilidad con el uso de claves almacenadas en Azure Key Vault y la administración extensible de claves.
author: VanMSFT
ms.author: vanto
ms.date: 09/12/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 64da74cc-14bf-4636-a55e-8cc1fce2aaff
ms.openlocfilehash: 68aa25c93a500b90b778be1563b0d373b07653cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346381"
---
# <a name="security-limitations-for-sql-server-on-linux"></a>Limitaciones de seguridad para SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Actualmente, SQL Server en Linux tiene las siguientes limitaciones:

* Se proporciona una directiva de contraseñas estándar. MUST_CHANGE es la única opción que se puede configurar. No se admite la opción CHECK_POLICY.
* No se admite la Administración extensible de claves. 
* No se admite el uso de claves almacenadas en Azure Key Vault.
* SQL Server genera su propio certificado autofirmado para cifrar las conexiones. SQL Server puede configurarse para usar un certificado proporcionado por el usuario para TLS. 

Para obtener más información sobre las características de seguridad disponibles en SQL Server, consulte [Centro de seguridad para el motor de base de datos SQL Server y Azure SQL Database](../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md).

## <a name="next-steps"></a>Pasos siguientes

Para conocer las tareas comunes de seguridad, consulte una [introducción a las características de seguridad de SQL Server en Linux](sql-server-linux-security-get-started.md). Si quiere obtener un script que le permita cambiar el número del puerto TCP y los directorios de SQL Server, así como configurar las marcas de seguimiento o la intercalación, consulte [Configurar SQL Server en Linux con la herramienta mssql-conf](sql-server-linux-configure-mssql-conf.md).
