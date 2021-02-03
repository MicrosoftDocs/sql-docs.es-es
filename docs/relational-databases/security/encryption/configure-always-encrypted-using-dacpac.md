---
description: Configuración del cifrado de columna mediante Always Encrypted con un paquete DAC
title: Configuración del cifrado de columna mediante Always Encrypted con un paquete DAC | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
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
ms.openlocfilehash: 7a42972d4a0cd5e6b456d72ccdba6786ad66c734
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237112"
---
# <a name="configure-column-encryption-using-always-encrypted-with-a-dac-package"></a>Configuración del cifrado de columna mediante Always Encrypted con un paquete DAC 
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Un [paquete de aplicación de capa de datos (DAC)](../../data-tier-applications/data-tier-applications.md), también conocido como DACPAC, es una unidad portátil de implementación de bases de datos de SQL Server que define todos los objetos de SQL Server, incluidas las tablas y las columnas que contienen. Al publicar un DACPAC en una base de datos (cuando se actualiza una base de datos mediante un DACPAC), el esquema de la base de datos de destino se actualiza para que coincida con el esquema del DACPAC. Puede publicar un DACPAC mediante el [Asistente para actualizar la aplicación de capa de datos](../../data-tier-applications/upgrade-a-data-tier-application.md#UsingDACUpgradeWizard) en SQL Server Management Studio, [PowerShell](../../data-tier-applications/upgrade-a-data-tier-application.md#UpgradeDACPowerShell) o [sqlpackage](../../../tools/sqlpackage/sqlpackage-publish.md).

En este artículo se abordan las consideraciones especiales que hay que tener en cuenta para actualizar una base de datos cuando el DACPAC o la base de datos de destino contienen columnas protegidas con [Always Encrypted](always-encrypted-database-engine.md). Si el esquema de cifrado de una columna del DACPAC difiere del esquema de cifrado de una columna existente en la base de datos de destino, cuando se publica el DACPAC, se cifran, se descifran o se vuelven a cifrar los datos almacenados en la columna. Consulte la tabla siguiente para obtener más información.

| Condición|Acción|
|:---|:---|
|La columna está cifrada en el DACPAC y no está cifrada en la base de datos.| Los datos de la columna se cifrarán.|
|La columna no está cifrada en el DACPAC y está cifrada en la base de datos.| Los datos de la columna se descifrarán (se quitará el cifrado de la columna).|
| La columna está cifrada en el DACPAC y en la base de datos, pero la columna del DACPAC usa un tipo de cifrado diferente o una clave de cifrado de columnas diferentes de los que usa la columna correspondiente de la base de datos.|Los datos de la columna se descifrarán y luego se volverán a cifrar para que coincida con la configuración de cifrado del DACPAC.|

La implementación de un paquete DAC también puede dar lugar a la creación o la eliminación de objetos de metadatos para las claves maestras de columna o las claves de cifrado de columna para Always Encrypted.

## <a name="performance-considerations"></a>Consideraciones de rendimiento
Para realizar operaciones criptográficas, la herramienta que se use para implementar un DACPAC debe trasladar los datos fuera de la base de datos. La herramienta crea una tabla (o varias) con la configuración de cifrado deseada en la base de datos, carga todos los datos de las tablas originales, realiza las operaciones criptográficas solicitadas, carga los datos en las nuevas tablas y, después, intercambia las tablas originales por las nuevas. La ejecución de operaciones criptográficas puede llevar mucho tiempo. Durante ese tiempo, la base de datos no está disponible para escribir transacciones. 

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> Si usa [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] y su instancia de SQL Server está configurada con un enclave seguro, puede ejecutar las operaciones criptográficas en contexto, sin sacar los datos de la base de datos. Vea [Configuración del cifrado de columnas en contexto mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-configure-encryption.md). Tenga en cuenta que el cifrado en contexto no está disponible para las implementaciones de DACPAC.

::: moniker-end

## <a name="permissions-for-publishing-a-dac-package-if-always-encrypted-is-set-up"></a>Permisos para publicar un paquete DAC si Always Encrypted está configurado

Para publicar un paquete DAC si Always Encrypted está configurado en el DACPAC o en la base de datos de destino, podría necesitar algunos de los permisos siguientes o todos, en función de las diferencias que existan entre el esquema del DACPAC y el esquema de la base de datos de destino.

*ALTER ANY COLUMN MASTER KEY*, *ALTER ANY COLUMN ENCRYPTION KEY*, *VIEW ANY COLUMN MASTER KEY DEFINITION*, *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION*

Si la operación de actualización desencadena una operación de cifrado de datos, también debe tener acceso a las claves maestras de columna configuradas para las columnas afectadas:

- **Almacén de certificados: equipo local**: debe tener acceso de lectura al certificado que se usa como clave maestra de columna, o bien ser el administrador del equipo.
- **Azure Key Vault**: necesita los permisos *create*, *get*, *unwrapKey*, *wrapKey*, *sign* y *verify* en el almacén que contiene la clave maestra de columna.
- **Proveedor de almacén de claves (CNG)** : se le podrían solicitar el permiso y las credenciales necesarios al usar una clave o un almacén de claves, en función del almacén y de la configuración del KSP.
- **Proveedor de servicios criptográficos (CAPI)** : se le podrían solicitar el permiso y las credenciales necesarios al usar una clave o un almacén de claves, en función del almacén y de la configuración del CSP.

Para obtener más información, vea [Create and Store Column Master Keys (Always Encrypted) (Crear y almacenar claves maestras de columna (Always Encrypted))](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md). 

 
## <a name="next-steps"></a>Pasos siguientes
- [Desarrollo de aplicaciones con Always Encrypted](always-encrypted-client-development.md)
- [Consulta de columnas mediante Always Encrypted con SQL Server Management Studio](always-encrypted-query-columns-ssms.md)

## <a name="see-also"></a>Consulte también  
 - [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
 - [Información general sobre la administración de claves de Always Encrypted](overview-of-key-management-for-always-encrypted.md) 
 - [Configuración de Always Encrypted con SQL Server Management Studio](configure-always-encrypted-using-sql-server-management-studio.md)
 - [Configuración del cifrado de columna mediante el asistente para Always Encrypted](always-encrypted-wizard.md)
 - [Configuración del cifrado de columnas mediante Always Encrypted con PowerShell](configure-column-encryption-using-powershell.md)
 
