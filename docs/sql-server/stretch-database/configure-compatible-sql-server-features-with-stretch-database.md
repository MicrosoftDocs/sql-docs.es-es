---
description: Configurar características compatibles de SQL Server con Stretch Database
title: Configuración de características de SQL Server compatibles
ms.date: 03/14/2017
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: c8121ede-1aec-459b-b7b0-1408bb3e62fb
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: c1f2389ea412b1cdc542f87ee25f3a95c34fa256
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080216"
---
# <a name="configure-compatible-sql-server-features-with-stretch-database"></a>Configurar características compatibles de SQL Server con Stretch Database
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


Realice pasos simples para configurar las siguientes características de SQL Server para trabajar con Stretch Database.
-   Always On
-   Always Encrypted
-   Cifrado de datos transparente (TDE)
-   Tablas temporales

## <a name="configure-always-on-with-stretch-database"></a>Configurar AlwaysOn con Stretch Database
Si usa AlwaysOn con Stretch Database, debe asegurarse de que la clave maestra de base de datos esté disponible en las réplicas secundarias. Stretch Database usa la clave maestra de base de datos para proteger las credenciales que usa para conectarse a la base de datos remota de Azure.

Una vez que configure el grupo de disponibilidad AlwaysOn, ejecute el procedimiento almacenado **sp_control_dbmasterkey_password** en cada réplica secundaria y proporcione la contraseña de la base de datos habilitada para Stretch. Para más información, consulte [sp_control_dbmasterkey_password](../../relational-databases/system-stored-procedures/sp-control-dbmasterkey-password-transact-sql.md). 

## <a name="configure-always-encrypted-with-stretch-database"></a>Configurar Always Encrypted con Stretch Database
Si desea usar Always Encrypted en conjunto con Stretch Database, debe configurar el cifrado de las columnas seleccionadas antes de habilitar Stretch Database en la tabla.

Si ya habilitó Stretch Database en la tabla y desea usar columnas de Always Encrypted, deberá hacer lo siguiente.
1.   Deshabilite Stretch Database en la tabla y devuelva los datos remotos desde Azure. Para obtener más información, vea [Deshabilitar Stretch Database y recuperar datos remotos](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).
2.   Configure Always Encrypted en las columnas seleccionadas.
3. Vuelva a habilitar Stretch Database en la tabla. Para obtener más información, vea [Enable Stretch Database for a database](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md).

## <a name="configure-transparent-data-encryption-tde-with-stretch-database"></a>Configurar Cifrado de datos transparente (TDE) con Stretch Database

Si TDE está habilitado en la base de datos local, no se habilitará automáticamente en el extremo remoto de Stretch Database. Recuerde habilitar TDE en el extremo remoto después de habilitar Stretch en la base de datos.

## <a name="configure-temporal-tables-with-stretch-database"></a>Configurar tablas temporales con Stretch Database
Si usa tablas temporales, puede habilitar Stretch Database en la tabla de historial, pero no en la tabla actual.
-   Para instrucciones sobre cómo usar tablas temporales con Stretch Database, consulte [Administración de la retención de datos históricos en las tablas temporales con versiones del sistema](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md).
-   Para filtrar las filas que se van a migrar desde la tabla de historial con una ventana deslizante, consulte [Seleccionar las filas que se van a migrar mediante una función de filtro](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).
-   No se puede habilitar Stretch Database en la tabla temporal de historial si la tabla está optimizada para memoria. No se admiten las tablas optimizadas para memoria.
