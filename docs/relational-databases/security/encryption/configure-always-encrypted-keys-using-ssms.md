---
title: Aprovisionamiento de claves de Always Encrypted mediante SQL Server Management Studio | Microsoft Docs
description: Obtenga información sobre cómo aprovisionar claves maestras de columna y claves de cifrado de columna para Always Encrypted mediante SQL Server Management Studio.
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.COLUMNMASTERKEY.PAGE.F1
- SQL13.SWB.COLUMNENCRYPTIONKEY.PAGE.F1
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f31a17d3f8bf2285bc41c7462deec4e53008efd1
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237079"
---
# <a name="provision-always-encrypted-keys-using-sql-server-management-studio"></a>Aprovisionamiento de claves de Always Encrypted mediante SQL Server Management Studio
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

En este artículo se indican los pasos para aprovisionar claves maestras de columna y claves de cifrado de columna para Always Encrypted mediante [SQL Server Management Studio (SSMS)](../../../ssms/download-sql-server-management-studio-ssms.md).

Para obtener información general sobre la administración de claves de Always Encrypted, incluidos algunos procedimientos recomendados y consideraciones de seguridad importantes, vea [Información general de administración de claves de Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).

<a name="provisioncmk"></a>
## <a name="provision-column-master-keys-with-the-new-column-master-key-dialog"></a>Aprovisionamiento de claves maestras de columna con el cuadro de diálogo Nueva clave maestra de columna

El cuadro de diálogo **Nueva clave maestra de columna** permite generar una clave maestra de columna o elegir una clave existente en un almacén de claves y crear metadatos de clave maestra de columna para la clave creada o seleccionada en la base de datos.

1.  En el **Explorador de objetos**, vaya a la carpeta **Security > Always Encrypted Keys** de la base de datos.
2.  Haga clic con el botón derecho en la carpeta **Claves maestras de columna** y seleccione **Nueva clave maestra de columna...** . 
3.  En el cuadro de diálogo **Nueva clave maestra de columna** , escriba el nombre del objeto de metadatos de la clave maestra de columna.
4.  Seleccione un almacén de claves:
    - **Almacén de certificados: usuario actual**: indica la ubicación del almacén de certificados del usuario actual en el Almacén de certificados de Windows, que es el almacén personal. 
    - **Almacén de certificados: equipo local**: indica la ubicación del almacén de certificados del equipo local en el Almacén de certificados de Windows. 
    - **Azure Key Vault**: deberá iniciar sesión en Azure (haga clic en **Iniciar sesión**). Una vez que haya iniciado sesión, podrá elegir una de las suscripciones de Azure y un almacén de claves.
    - **Proveedor de almacén de claves (KSP)** : indica un almacén de claves accesible a través de un proveedor de almacén de claves (KSP) que implementa la API Cryptography Next Generation (CNG). Normalmente, este tipo de almacén es un módulo de seguridad de hardware (HSM). Después de seleccionar esta opción, debe elegir un KSP. De forma predeterminada está seleccionado el **proveedor de almacén de claves de software de Microsoft** . Si quiere usar una clave maestra de columna almacenada en un HSM, seleccione un KSP para el dispositivo (debe estar instalado y configurado en el equipo antes de que abra el cuadro de diálogo).
    -   **Proveedor de servicios de cifrado (CAPI)** : almacén de claves accesible mediante un proveedor de servicios de cifrado (CSP) que implementa Cryptography API (CAPI). Normalmente, este almacén es un módulo de seguridad de hardware (HSM). Después de seleccionar esta opción, debe elegir un CSP.  Si quiere usar una clave maestra de columna almacenada en un HSM, seleccione un CSP para el dispositivo (debe estar instalado y configurado en el equipo antes de que abra el cuadro de diálogo).
    
    > [!NOTE]
    > Dado que CAPI es una API en desuso, la opción Proveedor de servicios criptográficos (CAPI) está deshabilitada de forma predeterminada. Puede habilitarla creando el valor DWORD CAPI Provider Enabled en la clave **[HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\sql13\Tools\Client\Always Encrypted]** del Registro de Windows y estableciéndolo en 1. Debe usar CNG en lugar de CAPI, a menos que el almacén de claves no admita CNG.
   
    Para obtener más información sobre los anteriores almacenes de claves, vea [Creación y almacenamiento de claves maestras de columna (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

5. Si usa [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] y su instancia de SQL Server está configurada con un enclave seguro, puede activar la casilla **Permitir cálculos de enclave** para que la clave maestra esté habilitada para el enclave. Para obtener más información, vea [Always Encrypted con enclaves seguros](always-encrypted-enclaves.md). 

    > [!NOTE]
    > La casilla **Permitir cálculos de enclave** no aparece si la instancia de SQL Server no se ha configurado correctamente con un enclave seguro.

6.  Elija una clave existente en el almacén de claves, o bien haga clic en el botón **Generar clave** o **Generar certificado** para crear una clave en el almacén de claves. 
7.  Haga clic en **Aceptar** y la nueva clave aparecerá en la lista. 

Una vez que haya completado el cuadro de diálogo, SQL Server Management Studio creará los metadatos para la clave maestra de columna en la base de datos. El cuadro de diálogo consigue esto generando y emitiendo una instrucción [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) .

::: moniker range=">=sql-server-ver15"

Si está configurando una clave maestra de columna habilitada para el enclave, SSMS también firma los metadatos mediante la clave maestra de columna. 

::: moniker-end

### <a name="permissions-for-provisioning-a-column-master-key"></a>Permisos para el aprovisionamiento de una clave maestra de columna

Necesita tener el permiso *ALTER ANY COLUMN MASTER KEY* en la base de datos para que el cuadro de diálogo cree la clave maestra de columna. Para usar el cuadro de diálogo para crear una clave maestra de columna o usar una clave existente en la creación de un almacén de claves, es posible que necesite permisos en el almacén de claves o en la clave:
- **Almacén de certificados: equipo local**: debe tener acceso de lectura al certificado que se usa como una clave maestra de columna, o bien ser el administrador del equipo.
- **Azure Key Vault**: necesita los permisos *get* y *list* para seleccionar y usar una clave y el permiso *create* para crear una clave. Para configurar una clave maestra de columna habilitada para el enclave, también necesita el permiso *sign* para generar una firma de los metadatos de clave.
- **Proveedor de almacén de claves (CNG)** : se le podrían solicitar el permiso y las credenciales necesarios al usar una clave o un almacén de claves, en función del almacén y de la configuración del KSP.
- **Proveedor de servicios criptográficos (CAPI)** : se le podrían solicitar el permiso y las credenciales necesarios al usar una clave o un almacén de claves, en función del almacén y de la configuración del CSP.

Para más información, vea [Creación y almacenamiento de claves maestras de columna (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

<a name="provisioncek"></a> 
## <a name="provision-column-encryption-keys-with-the-new-column-encryption-key-dialog"></a>Aprovisionamiento de claves de cifrado de columna con el cuadro de diálogo Nueva clave de cifrado de columnas

El cuadro de diálogo **Nueva clave de cifrado de columnas** permite generar una clave de cifrado de columnas, cifrarla con una clave maestra de columna y crear los metadatos de clave de cifrado de columnas en la base de datos.

1.  En el **Explorador de objetos**, vaya a la carpeta **Security &gt; Always Encrypted Keys** de la base de datos.
2.  Haga clic con el botón derecho en la carpeta **Claves de cifrado de columna** y seleccione **Nueva clave de cifrado de columnas...** . 
3.  En el cuadro de diálogo **Nueva clave de cifrado de columnas** , escriba el nombre del objeto de metadatos de la clave de cifrado de columnas.
4.  Seleccione un objeto de metadatos que represente la clave maestra de columna de la base de datos.
5.  Haga clic en **OK**. 

Una vez que complete el cuadro de diálogo, SQL Server Management Studio generará una clave de cifrado de columna y luego recuperará los metadatos para la clave maestra de columna seleccionada en la base de datos. Después, SSMS usará los metadatos de la clave maestra de columna para ponerse en contacto con el almacén de claves que contiene la clave maestra de columna y cifrar la clave de cifrado de columna. Por último, SSMS crea los datos de metadatos para el nuevo cifrado de columnas en la base de datos. Para ello, genera y emite una instrucción [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md).

### <a name="permissions-for-provisioning-a-column-encryption-key"></a>Permisos para el aprovisionamiento de una clave de cifrado de columna

Necesita tener los permisos de base de datos *ALTER ANY COLUMN ENCRYPTION KEY* y *VIEW ANY COLUMN MASTER KEY DEFINITION* en la base de datos para que el cuadro de diálogo cree los metadatos de la clave de cifrado de columnas y tenga acceso a los metadatos de la clave maestra de columna.
Para tener acceso a un almacén de claves y usar la clave maestra de columna, es posible que necesite permisos en el almacén de claves o en la clave:
- **Almacén de certificados: equipo local**: debe tener acceso de lectura al certificado que se usa como una clave maestra de columna, o bien ser el administrador del equipo.
- **Azure Key Vault**: necesita los permisos *get*, *unwrapKey*, *wrapKey*, *sign* y *verify* en el almacén que contiene la clave maestra de columna.
- **Proveedor de almacén de claves (CNG)** : se le podrían solicitar el permiso y las credenciales necesarios al usar una clave o un almacén de claves, en función del almacén y de la configuración del KSP.
- **Proveedor de servicios criptográficos (CAPI)** : se le podrían solicitar el permiso y las credenciales necesarios al usar una clave o un almacén de claves, en función del almacén y de la configuración del CSP.

Para obtener más información, vea [Creación y almacenamiento de claves maestras de columna (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

## <a name="provision-always-encrypted-keys-using-the-always-encrypted-wizard"></a>Aprovisionamiento de claves de Always Encrypted con el Asistente para Always Encrypted

El [Asistente para Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-wizard.md) es una herramienta para cifrar, descifrar y volver a cifrar las columnas seleccionadas de una base de datos. Aunque puede usar claves ya configuradas, también permite generar una nueva clave maestra de columna y un nuevo cifrado de columna. 

## <a name="next-steps"></a>Pasos siguientes
- [Configuración del cifrado de columna mediante el asistente para Always Encrypted](always-encrypted-wizard.md)
- [Configuración del cifrado de columnas mediante Always Encrypted con un paquete DAC](configure-always-encrypted-using-dacpac.md)
- [Rotación de claves de Always Encrypted mediante SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md)
- [Desarrollo de aplicaciones con Always Encrypted](always-encrypted-client-development.md)
- [Migración de datos a o desde columnas mediante Always Encrypted con el Asistente para importación y exportación de SQL Server](always-encrypted-migrate-using-import-export-wizard.md)

## <a name="see-also"></a>Consulte también
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Información general sobre la administración de claves de Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [Creación y almacenamiento de claves maestras de columna para Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)
- [Configuración de Always Encrypted con SQL Server Management Studio](configure-always-encrypted-using-sql-server-management-studio.md)
- [Aprovisionamiento de claves de Always Encrypted mediante PowerShell](configure-always-encrypted-keys-using-powershell.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)
