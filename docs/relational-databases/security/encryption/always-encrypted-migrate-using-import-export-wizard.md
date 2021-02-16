---
description: Migración de datos a o desde columnas mediante Always Encrypted con el Asistente para importación y exportación de SQL Server
title: Migración de datos a o desde columnas mediante Always Encrypted con el Asistente para importación y exportación de SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 10/31/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.COLUMNMASTERKEY.ROTATION.F1
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 234dabee531f85b4e3f8a687067dee9f91c96b33
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345492"
---
# <a name="migrate-data-to-or-from-columns-using-always-encrypted-with-sql-server-import-and-export-wizard"></a>Migración de datos a o desde columnas mediante Always Encrypted con el Asistente para importación y exportación de SQL Server 
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

El [Asistente para importación y exportación de SQL Server](../../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md) es una herramienta que permite copiar datos desde un origen a un destino. En este documento se describe cómo usar el Asistente para importación y exportación de SQL Server si un origen o un destino es una base de datos de SQL Server que contiene columnas protegidas con [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md).

## <a name="migration-scenarios"></a>Escenarios de migración
Con el Asistente para importación y exportación de SQL Server, puede implementar los escenarios siguientes para migrar datos hacia o desde columnas cifradas.

### <a name="encrypt-plaintext-data-on-migration"></a>Cifrar datos de texto no cifrado durante la migración
Si el origen de datos contiene datos de texto no cifrado y el destino es una base de datos de SQL Server que contiene columnas cifradas, puede usar el Asistente para importación y exportación de SQL Server para recuperar los datos de texto no cifrado del origen, cifrarlos y copiar los datos cifrados (texto cifrado) a las columnas cifradas en la base de datos de destino. En este escenario de migración, el origen de datos puede ser cualquier almacén de datos que el Asistente para importación y exportación de SQL Server admita. Por ejemplo, un archivo, una base de datos de SQL Server o una base de datos en otro sistema de base de datos.

Para asegurarse de que el Asistente para importación y exportación de SQL Server puede cifrar los datos, debe habilitar Always Encrypted para la conexión de base de datos de destino y debe tener acceso a las claves que protegen los datos en las columnas de la base de datos de destino. Para obtener más información, vea [Habilitar y deshabilitar Always Encrypted para una conexión de base de datos](#enable-and-disable-always-encrypted-for-a-database-connection) y [Permisos para cifrar o descifrar datos durante la migración](#permissions-for-encrypting-or-decrypting-data-during-migration).

### <a name="decrypt-encrypted-data-on-migration"></a>Descifrar datos cifrados durante la migración
Si va a migrar datos almacenados en columnas de base de datos cifradas en una base de datos de SQL Server, puede configurar el Asistente para importación y exportación de SQL Server para descifrar los datos y copiar los datos descifrados (texto no cifrado) en un destino, que puede ser cualquier almacén de datos que el Asistente para importación y exportación de SQL Server admita, por ejemplo, un archivo, una base de datos de SQL Server o una base de datos en otro sistema de base de datos.

Para asegurarse de que el Asistente para importación y exportación de SQL Server puede descifrar los datos, debe habilitar Always Encrypted para la conexión de base de datos de origen y debe tener acceso a las claves que protegen los datos en las columnas de la base de datos de origen. Para obtener más información, vea [Habilitar y deshabilitar Always Encrypted para una conexión de base de datos](#enable-and-disable-always-encrypted-for-a-database-connection) y [Permisos para cifrar o descifrar datos durante la migración](#permissions-for-encrypting-or-decrypting-data-during-migration).

### <a name="re-encrypt-data-on-migration"></a>Volver a cifrar los datos durante la migración
Si va a copiar los datos de las columnas cifradas en una base de datos de SQL Server de origen en columnas cifradas de la misma base de datos de SQL Server o en otra, puede configurar el Asistente para importación y exportación de SQL Server para descifrar los datos después de recuperarlos del origen y volver a cifrarlos antes de insertarlos en las columnas cifradas en la base de datos de destino. Use este método si el esquema de las columnas de destino (por ejemplo, los tipos de datos de columna, los tipos de cifrado y las claves de cifrado de columna) es diferente del esquema de las columnas de origen.

Para asegurarse de que el Asistente para importación y exportación de SQL Server puede cifrar y descifrar los datos, debe habilitar Always Encrypted para la conexión de base de datos de origen y de destino, y debe tener acceso a las claves que protegen los datos tanto en las columnas de la base de datos de origen como en las de destino. Para obtener más información, vea [Habilitar y deshabilitar Always Encrypted para una conexión de base de datos](#enable-and-disable-always-encrypted-for-a-database-connection) y [Permisos para cifrar o descifrar datos durante la migración](#permissions-for-encrypting-or-decrypting-data-during-migration).

### <a name="keep-data-encrypted-during-migration"></a>Mantener los datos cifrados durante la migración
Si va a copiar los datos de las columnas cifradas en una base de datos de SQL Server de origen en columnas cifradas de la misma base de datos de SQL Server o en otra, y las columnas de destino utilizan **exactamente** el esquema (incluidos los mismos tipos de datos, tipos de cifrado y claves de cifrado de columnas) como columnas de origen, puede configurar el Asistente para importación y exportación de SQL Server para recuperar texto cifrado de las columnas de origen e insertar los datos cifrados (texto cifrado) en una columna cifrada en la base de datos de SQL Server de destino. 

En este escenario, puede usar cualquier proveedor de datos que admita SQL Server para conectarse a la base de datos de origen o de destino de SQL Server. Si usa un proveedor que admite Always Encrypted para conectarse a la base de datos de destino, debe asegurarse de que Always Encrypted esté deshabilitado para la conexión de base de datos. Para obtener más información, vea [habilitar y deshabilitar Always Encrypted para una conexión de base de datos](#enable-and-disable-always-encrypted-for-a-database-connection).

También debe asegurarse de que la entidad de seguridad de base de datos (usuario) que usa el Asistente para importación y exportación de SQL Server para conectarse a la base de datos de destino esté configurada con la opción `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` establecida en `ON`. Esta opción suprime las comprobaciones de metadatos criptográficos del servidor en operaciones de copia masiva, lo que permite al asistente insertar los datos cifrados de forma masiva en la base de datos de destino sin descifrar los datos. Para obtener más información, vea [Carga masiva de datos cifrados a columnas protegidas por Always Encrypted](migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="enable-and-disable-always-encrypted-for-a-database-connection"></a>Habilitar y deshabilitar Always Encrypted para una conexión de base de datos
Si el escenario de migración requiere que el Asistente para importación y exportación de SQL Server pueda cifrar o descifrar los datos, debe configurar la conexión de base de datos de SQL Server de origen y/o la conexión de base de datos de SQL Server de destino mediante un proveedor de datos que admita Always Encrypted. También debe habilitar Always Encrypted para la conexión de base de datos de origen o de destino.

Puede usar cualquier proveedor de datos para una conexión si no necesita que el asistente cifre o descifre los datos en esa conexión.

Los siguientes proveedores de datos del Asistente para importación y exportación de SQL Server admiten Always Encrypted.

- Proveedor de datos .NET Framework para SQL Server
  - Asegúrese de que la máquina en la que se ejecuta el asistente use .NET Framework 4.6.1 o posterior.
  - Si desea habilitar Always Encrypted para una conexión, establezca `Column Encryption Setting` en `Enabled` en las propiedades de conexión. Para deshabilitar Always Encrypted, establezca `Column Encryption Setting` en `Disabled`. Para obtener más información, vea [Conectarse a SQL Server con el Proveedor de datos de .NET Framework para SQL Server](../../../integration-services/import-export-data/connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard.md#connect-to-sql-server-with-the-net-framework-data-provider-for-sql-server) y [Habilitar Always Encrypted para consultas de la aplicación](develop-using-always-encrypted-with-net-framework-data-provider.md#enabling-always-encrypted-for-application-queries).
- Proveedor de datos .NET Framework para ODBC.
  - Instale Microsoft ODBC driver 13.1 o posterior.
    - Si desea habilitar Always Encrypted para una conexión, establezca `Column Encryption` en `Enabled` en las propiedades de conexión. Para deshabilitar Always Encrypted, establezca `Column Encryption` en `Disabled`. Para obtener más información, vea [Conectarse a SQL Server con el controlador ODBC para SQL Server](../../../integration-services/import-export-data/connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard.md#connect-to-sql-server-with-the-odbc-driver-for-sql-server) y [Habilitar Always Encrypted en una aplicación ODBC](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#enabling-always-encrypted-in-an-odbc-application).

## <a name="permissions-for-encrypting-or-decrypting-data-during-migration"></a>Permisos para cifrar o descifrar datos durante la migración

Para cifrar o descifrar datos almacenados en la base de datos de origen o de destino de SQL Server, necesita tener los permisos *VIEW ANY COLUMN MASTER KEY DEFINITION* y *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION* en la base de datos de origen.

También necesita tener acceso a las claves maestras de columna configuradas para las columnas que almacenan los datos que se cifran o se descifran:

- **Almacén de certificados: equipo local**: debe tener acceso de lectura al certificado que se usa como clave maestra de columna, o bien ser el administrador del equipo.
- **Azure Key Vault**: necesita los permisos _get_, _unwrapKey_ y _verify_ en el almacén que contiene la clave maestra de columna.
- **Proveedor del almacén de claves (CNG)**: el permiso y las credenciales necesarios; se le podrían solicitar al usar una clave o un almacén de claves, en función de la configuración del almacén o del proveedor de almacenamiento de claves (KSP).
- **Proveedor de servicios criptográficos (CAPI)**: el permiso y las credenciales necesarios; se le podrían solicitar al usar una clave o un almacén de claves, en función del almacén y de la configuración del proveedor de servicios criptográficos (CSP).
Para obtener más información, vea [Create and Store Column Master Keys (Always Encrypted) (Crear y almacenar claves maestras de columna (Always Encrypted))](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

## <a name="next-steps"></a>Pasos siguientes
- [Consulta de columnas mediante Always Encrypted con SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Desarrollo de aplicaciones con Always Encrypted](always-encrypted-client-development.md)

## <a name="see-also"></a>Consulte también
- [Always Encrypted](always-encrypted-database-engine.md)
- [Exportación e importación de bases de datos con Always Encrypted](always-encrypted-migrate-using-bacpac.md)
- [Copia de seguridad y restauración de bases de datos con Always Encrypted](always-encrypted-migrate-using-backup-restore.md)
- [Carga masiva de datos cifrados a columnas mediante Always Encrypted](migrate-sensitive-data-protected-by-always-encrypted.md)
