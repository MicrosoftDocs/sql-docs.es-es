---
title: Configuración del cifrado de columnas mediante el asistente para Always Encrypted | Microsoft Docs
description: Obtenga información sobre cómo establecer la configuración de Always Encrypted para las columnas de las bases de datos mediante el Asistente para Always Encrypted en SQL Server.
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.alwaysencryptedwizard.encryption.f1
- sql13.swb.alwaysencryptedwizard.f1
- sql13.swb.alwaysencryptedwizard.masterkey.f1
helpviewer_keywords:
- Wizard, Always Encrypted
ms.assetid: 68daddc9-ce48-49aa-917f-6dec86ad5af5
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bedf502949e206b45bf7e1e332962c8c093d1bcb
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236694"
---
# <a name="configure-column-encryption-using-always-encrypted-wizard"></a>Configuración del cifrado de columnas mediante el asistente para Always Encrypted
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

El asistente para Always Encrypted es una herramienta eficaz que permite establecer la configuración de [Always Encrypted](always-encrypted-database-engine.md) elegida para las columnas de la base de datos seleccionadas. Según la configuración actual y la configuración de destino elegida, el asistente puede cifrar una columna, descifrarla (quitar el cifrado) o volver a cifrarla (por ejemplo, con una nueva clave de cifrado de columnas o con un tipo de cifrado diferente del actual configurado para la columna). Es posible configurar varias columnas en una misma ejecución del asistente.

El asistente le permite cifrar columnas con claves de cifrado de columnas existentes, aunque también puede generar una nueva clave de cifrado de columnas o, además de esta, una nueva clave maestra de columna. 

El asistente sirve para sacar datos de la base de datos y realizar operaciones criptográficas en el proceso de SSMS. El asistente crea una tabla (o varias) con la configuración de cifrado deseada en la base de datos, carga todos los datos de las tablas originales, realiza las operaciones criptográficas solicitadas, carga los datos en las nuevas tablas y, después, intercambia las tablas originales por las nuevas.

> [!NOTE]
> La ejecución de operaciones criptográficas puede llevar mucho tiempo. Durante ese tiempo, la base de datos no está disponible para escribir transacciones. PowerShell es una herramienta recomendada para realizar operaciones criptográficas en tablas de mayor tamaño. Vea [Configuración del cifrado de columnas mediante Always Encrypted con PowerShell](configure-column-encryption-using-powershell.md).

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> Si usa [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] y su instancia de SQL Server está configurada con un enclave seguro, puede ejecutar las operaciones criptográficas en contexto, sin sacar los datos de la base de datos. Vea [Configuración del cifrado de columnas en contexto mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-configure-encryption.md). Tenga en cuenta que el asistente no admite el cifrado en contexto.

::: moniker-end

Se recomienda el uso de PowerShell: 

 - Para ver una introducción completa en la que se muestra cómo configurar Always Encrypted con el asistente y cómo usarlo en una aplicación cliente, consulte los siguientes tutoriales de Azure SQL Database:
    - [Protección de la información confidencial en Azure SQL Database con Always Encrypted y con claves maestras de columna en el almacén de certificados de Windows](/azure/azure-sql/database/always-encrypted-certificate-store-configure)
    - [Protección de la información confidencial en Azure SQL Database con Always Encrypted y con claves maestras de columna en Azure Key Vault](/azure/sql-database/sql-database-always-encrypted-azure-key-vault)

 - Para ver un vídeo que incluye el uso del asistente, vea [Keeping Sensitive Data Secure with Always Encrypted](https://channel9.msdn.com/events/DataDriven/SQLServer2016/AlwaysEncrypted)(Protección de la información confidencial con Always Encrypted). Consulte también el blog del equipo de seguridad de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)][SSMS Encryption Wizard - Enabling Always Encrypted in a Few Easy Steps (Asistente para cifrado de SSMS: habilitación de Always Encrypted en unos sencillos pasos)](https://techcommunity.microsoft.com/t5/SQL-Server/SSMS-Encryption-Wizard-Enabling-Always-Encrypted-in-a-Few-Easy/ba-p/384545).  
 - Para obtener más información sobre las claves de Always Encrypted, vea [Información general de administración de claves de Always Encrypted](overview-of-key-management-for-always-encrypted.md).
 - Para obtener más información sobre los tipos de cifrado admitidos en Always Encrypted, vea [Selección del cifrado determinista o aleatorio](always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).
 
 ## <a name="permissions"></a>Permisos
Para realizar operaciones criptográficas con el asistente, necesita tener los permisos **VIEW ANY COLUMN MASTER KEY DEFINITION** y **VIEW ANY COLUMN ENCRYPTION KEY DEFINITION**. También debe tener permisos para acceder a las claves maestras de columna que usa en los almacenes de claves que las contienen:
- **Almacén de certificados, equipo local**: debe tener acceso de lectura al certificado que se usa como clave maestra de columna o ser el administrador del equipo.
- **Azure Key Vault**: necesita los permisos get, unwrapKey y verify en el almacén que contiene la clave maestra de columna.
- **Proveedor de almacén de claves (CNG)** : se le podrían solicitar el permiso y las credenciales necesarios al usar una clave o un almacén de claves, en función del almacén y de la configuración del KSP.
- **Proveedor de servicios criptográficos (CAPI)** : se le podrían solicitar el permiso y las credenciales necesarios al usar una clave o un almacén de claves, en función del almacén y de la configuración del CSP.

Además, si va a crear nuevas claves mediante el asistente, necesita los permisos adicionales enumerados en [Aprovisionamiento de claves maestras de columna con el cuadro de diálogo Nueva clave maestra de columna](configure-always-encrypted-keys-using-ssms.md#provision-column-master-keys-with-the-new-column-master-key-dialog) y en [Aprovisionamiento de claves de cifrado de columnas con el cuadro de diálogo Nueva clave de cifrado de columnas](configure-always-encrypted-keys-using-ssms.md#provision-column-encryption-keys-with-the-new-column-encryption-key-dialog).

## <a name="open-the-always-encrypted-wizard"></a>Apertura del asistente para Always Encrypted
Puede iniciar el asistente en tres niveles distintos: 
- En un nivel de base de datos: si quiere cifrar varias columnas ubicadas en diferentes tablas.
- En un nivel de tabla: si quiere cifrar varias columnas ubicadas en la misma tabla.
- En un nivel de columna: si quiere cifrar una columna específica.
 
 1. Conéctese a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] con el componente del Explorador de objetos de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
   
 2. Para cifrar:
     1. Varias columnas ubicadas en diferentes tablas en una base de datos, haga clic con el botón derecho en la base de datos, seleccione **Tareas** y, después, seleccione **Cifrar columnas**.
     1. Varias columnas ubicadas en la misma tabla, vaya a la tabla, haga clic en ella con el botón derecho y, después, seleccione **Cifrar columnas**.
     1. Una columna individual, vaya a la columna, haga clic en ella con el botón derecho y, después, seleccione **Cifrar columnas**.


   
 ## <a name="column-selection-page"></a>Página de selección de columnas
En esta página, puede seleccionar las columnas que quiere cifrar, volver a cifrar o descifrar y puede definir la configuración de cifrado de destino para las columnas seleccionadas.

Para cifrar una columna de texto no cifrado, seleccione un tipo de cifrado (**determinista** o **aleatorio**) y una clave de cifrado para la columna. 

Para cambiar un tipo de cifrado o rotar (cambiar) una clave de cifrado de columnas por una columna ya cifrada, seleccione el tipo de cifrado que quiere aplicar y la clave. 

Si quiere que el asistente cifre o vuelva a cifrar una o más columnas con una nueva clave de cifrado de columnas, seleccione una clave que contenga **(New)** en su nombre. El asistente generará la clave.

Para descifrar una columna que actualmente está cifrada, seleccione **Texto no cifrado** para el tipo de cifrado.


> [!NOTE]
> El asistente no admite operaciones criptográficas en tablas temporales y en memoria. Puede crear tablas temporales o en memoria vacías con Transact-SQL, e insertar datos mediante su aplicación.

## <a name="master-key-configuration-page"></a>Página de configuración de la clave maestra
Si ha seleccionado una clave de cifrado de columnas generada automáticamente para cualquier columna de la página anterior, en esta página deberá seleccionar una clave maestra de columna existente o configurar una nueva clave maestra de columna que cifre la clave de cifrado de columnas. 

A la hora de configurar una nueva clave maestra de columna, puede elegir una clave existente en el almacén de certificados de Windows o en Azure Key Vault y hacer que el asistente cree un objeto de metadatos para la clave en la base de datos, o puede generar la clave y el objeto de metadatos que describe la clave en la base de datos. 

Para obtener más información acerca de cómo crear y almacenar claves maestras de columna en el almacén de certificados de Windows, en Azure Key Vault o en otros almacenes de claves, vea [Creación y almacenamiento de claves maestras de columna para Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

> [!TIP]
> El asistente solo le permite buscar y crear claves en el almacén de certificados de Windows y en Azure Key Vault. También genera automáticamente los nombres de las nuevas claves y los objetos de metadatos de la base de datos que describen las claves. Si necesita más control sobre la manera en que se aprovisionan las claves (y más opciones para un almacén de claves que contenga su clave maestra de columna), puede usar los cuadros de diálogo **Nueva clave maestra de columna** y **Nueva clave de cifrado de columnas** para crear las claves y, después, ejecute el asistente y seleccione las claves que ha generado. Vea [Aprovisionamiento de claves maestras de columna con el cuadro de diálogo Nueva clave maestra de columna](configure-always-encrypted-keys-using-ssms.md#provision-column-master-keys-with-the-new-column-master-key-dialog) y [Aprovisionamiento de claves de cifrado de columnas con el cuadro de diálogo Nueva clave de cifrado de columnas](configure-always-encrypted-keys-using-ssms.md#provision-column-encryption-keys-with-the-new-column-encryption-key-dialog). 

## <a name="next-steps"></a>Pasos siguientes
- [Consulta de columnas mediante Always Encrypted con SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Desarrollo de aplicaciones con Always Encrypted](always-encrypted-client-development.md)

## <a name="see-also"></a>Consulte también  
 - [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
 - [Información general sobre la administración de claves de Always Encrypted](overview-of-key-management-for-always-encrypted.md) 
 - [Configuración de Always Encrypted con SQL Server Management Studio](configure-always-encrypted-using-sql-server-management-studio.md)
 - [Aprovisionamiento de claves de Always Encrypted mediante PowerShell](configure-always-encrypted-keys-using-powershell.md)
 - [Configuración del cifrado de columnas mediante Always Encrypted con PowerShell](configure-column-encryption-using-powershell.md)
 - [Configuración del cifrado de columnas mediante Always Encrypted con un paquete DAC](configure-always-encrypted-using-dacpac.md)