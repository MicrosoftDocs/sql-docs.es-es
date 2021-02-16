---
title: Información general de administración de claves de Always Encrypted | Microsoft Docs
description: 'Aprenda a administrar los dos tipos de claves criptográficas que Always Encrypted usa para proteger los datos en SQL Server: clave de cifrado de columna y clave maestra de columna.'
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.prod_service: security, sql-database"
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 07a305b1-4110-42f0-b7aa-28a4e32e912a
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 234f6233e2f6b024e6ad74a82f403e3df7db9a05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340180"
---
# <a name="overview-of-key-management-for-always-encrypted"></a>Información general de administración de claves de Always Encrypted
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]


[Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) usa dos tipos de claves criptográficas para proteger los datos: una clave para cifrar los datos y otra para cifrar la clave que cifra los datos. La clave de cifrado de columnas cifra los datos, mientras que la clave maestra de columna cifra la clave de cifrado de columnas. En este artículo se proporciona información general detallada para administrar estas claves de cifrado.  

Cuando se habla de las claves de Always Encrypted y la administración de claves, es importante comprender la diferencia entre las claves criptográficas reales y los objetos de metadatos que *describen* las claves. Los términos **clave de cifrado de columnas** y **clave maestra de columna** hacen referencia a las claves criptográficas reales, mientras que los términos **metadatos de clave de cifrado de columnas** y **metadatos de clave maestra de columna** hacen referencia a las *descripciones* de las claves de Always Encrypted en la base de datos.

- Las ***claves de cifrado de columnas*** son las claves de cifrado de contenido que se usan para cifrar datos. Como su nombre indica, las claves de cifrado de columnas se usan para cifrar los datos de las columnas de la base de datos. Puede cifrar una o más columnas con la misma clave de cifrado de columnas, o puede usar varias claves de cifrado de columnas en función de los requisitos de la aplicación. Las claves de cifrado de columnas están a su vez cifradas, y en la base de datos solo se almacenan los valores cifrados de las claves de cifrado de columnas (como parte de los metadatos de clave de cifrado de columnas). Los metadatos de clave de cifrado de columnas se almacenan en las vistas de catálogo [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md) y [sys.column_encryption_key_values (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md) . Las claves de cifrado de columnas que se usan con el algoritmo AES-256 tienen una longitud de 256 bits.


- Las ***claves maestras de columna*** protegen las claves usadas para cifrar las claves de cifrado de columnas. Las claves maestras de columna deben almacenarse en un almacén de claves de confianza, como el Almacén de certificados de Windows, el Almacén de claves de Azure o un módulo de seguridad de hardware. La base de datos solo contiene metadatos sobre las claves maestras de columna (el tipo de almacén de claves y la ubicación). Los metadatos de clave maestra de columna se almacenan en la vista de catálogo [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md) .  

Es importante tener en cuenta que los metadatos de clave del sistema de base de datos no contienen claves maestras de columna de texto no cifrado ni claves de cifrado de columnas de texto no cifrado. La base de datos solo contiene información sobre el tipo y la ubicación de las claves maestras de columna y valores cifrados de las claves de cifrado de columnas. Esto significa que nunca se exponen claves de texto no cifrado al sistema de base de datos, lo que garantiza la seguridad de los datos protegidos mediante Always Encrypted, incluso si el sistema de base de datos está en peligro. Para asegurarse de que el sistema de base de datos no pueda obtener acceso a las claves de texto no cifrado, asegúrese de ejecutar las herramientas de administración de claves en una máquina distinta de la que hospeda la base de datos. Revise a continuación la sección [Consideraciones de seguridad para la administración de claves](#security-considerations-for-key-management) para obtener más información.

Dado que la base de datos solo contiene datos cifrados (en columnas protegidas con Always Encrypted) y no puede obtener acceso a las claves de texto no cifrado, no puede descifrar los datos. Esto significa que la consulta de columnas Always Encrypted solo devolverá valores cifrados, por lo que las aplicaciones cliente que necesitan cifrar o descifrar datos protegidos deben tener acceso a la clave maestra de columna y a las correspondientes claves de cifrado de columnas. Para obtener detalles, vea [Desarrollo de aplicaciones con Always Encrypted](always-encrypted-client-development.md).



## <a name="key-management-tasks"></a>Tareas de administración de claves

El proceso de administración de claves se puede dividir en las siguientes tareas de alto nivel:

- **Aprovisionamiento de claves** : creación de las claves físicas en un almacén de claves de confianza (por ejemplo, el Almacén de certificados de Windows, el Almacén de claves de Azure o un módulo de seguridad de hardware), cifrado de las claves de cifrado de columnas con claves maestras de columna y creación de metadatos para ambos tipos de claves en la base de datos.

- **Rotación de claves** : reemplazo periódico de una clave existente por una clave nueva. Puede que necesite rotar una clave si está en peligro, o bien para cumplir las directivas o los reglamentos de la organización que exigen que se roten las claves criptográficas. 


## <a name="key-management-roles"></a><a name="KeyManagementRoles"></a> Roles de administración de claves

Hay dos roles de usuarios que administran las claves de Always Encrypted, los administradores de seguridad y los administradores de bases de datos (DBA):

- **Administrador de seguridad** : genera claves de cifrado de columnas y claves maestras de columna y administra los almacenes de claves que contienen las claves maestras de columna. Para realizar estas tareas, el administrador de seguridad debe tener acceso a las claves y al almacén de claves, pero no necesita tener acceso a la base de datos.
- **DBA**: administra los metadatos de las claves en la base de datos. Para realizar tareas de administración de claves, el DBA debe poder administrar los metadatos de clave en la base de datos, pero no necesita tener acceso a las claves o al almacén de claves que contiene las claves maestras de columna.

Teniendo en cuenta los roles anteriores, hay dos maneras de realizar tareas de administración de claves para Always Encrypted: *con separación de roles* y *sin separación de roles*. Según las necesidades de la organización, puede seleccionar el proceso de administración de claves que mejor se adapte a sus requisitos.

## <a name="managing-keys-with-role-separation"></a>Administración de claves con separación de roles
Cuando las claves de Always Encrypted se administran con separación de roles, los roles de administrador de seguridad y DBA están asignados a personas diferentes de una organización. Un proceso de administración de claves con separación de roles garantiza que los DBA no tengan acceso a las claves o a los almacenes de claves que contienen las claves reales y que los administradores de seguridad no tengan acceso a la base de datos que contiene información confidencial. Se recomienda que use la administración de claves con separación de roles si su objetivo es asegurarse de que los DBA de la organización no tengan acceso a información confidencial. 

**Nota:** Los administradores de seguridad generan y usan claves de texto no cifrado, de modo que nunca deben realizar sus tareas en los mismos equipos que hospedan un sistema de base de datos ni en equipos a los que puedan tener acceso los DBA o posibles adversarios. 

## <a name="managing-keys-without-role-separation"></a>Administración de claves sin separación de roles
Cuando las claves de Always Encrypted se administran sin separación de roles, una misma persona puede asumir los roles de administrador de seguridad y DBA, lo que implica que dicha persona debe tener acceso a las claves, los almacenes de claves y los metadatos de clave y debe poder administrarlos. La administración de claves sin separación de roles está recomendada para organizaciones que usen el modelo DevOps, o si la base de datos está hospedada en la nube y el objetivo principal es impedir que los administradores de la nube (pero no los DBA locales) accedan a información confidencial.



## <a name="tools-for-managing-always-encrypted-keys"></a>Herramientas para administrar claves de Always Encrypted

Las claves de Always Encrypted se pueden administrar mediante [SQL Server Management Studio (SSMS)](../../../ssms/sql-server-management-studio-ssms.md) y [PowerShell](../../../powershell/sql-server-powershell.md):

- **SQL Server Management Studio (SSMS)** : proporciona cuadros de diálogo y asistentes que combinan tareas relacionadas con el acceso al almacén de claves y a la base de datos, por lo que SSMS no admite la separación de roles, pero facilita la configuración de las claves. Para obtener más información sobre la administración de claves con SSMS, vea:
    - [Aprovisionamiento de claves de Always Encrypted mediante SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md)
    - [Rotación de claves de Always Encrypted mediante SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md)

- **SQL Server PowerShell**: incluye cmdlets para administrar las claves de Always Encrypted con y sin separación de roles. Para más información, consulte:
    - [Configuración de claves de Always Encrypted con PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell.md)
    - [Rotación de claves de Always Encrypted con PowerShell](../../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell.md)


## <a name="security-considerations-for-key-management"></a>Consideraciones de seguridad para la administración de claves

El objetivo principal de Always Encrypted es garantizar la seguridad de la información confidencial almacenada en una base de datos, incluso si el sistema de base de datos o su entorno de hospedaje están en peligro. A continuación se incluyen algunos ejemplos de ataques de seguridad en los que Always Encrypted puede ayudar a impedir pérdidas de información confidencial:

- Un usuario malintencionado de base de datos con un alto nivel de privilegios, como un DBA, consulta columnas con información confidencial.
- Un administrador no autorizado de un equipo que hospeda una instancia de SQL Server examina la memoria de un proceso de SQL Server o los archivos de volcado de memoria del proceso de SQL Server.
- Un operador malintencionado de un centro de datos consulta una base de datos de clientes, examina los archivos de volcado de memoria de SQL Server o examina la memoria de un equipo que hospeda los datos de clientes en la nube.
- Un software malicioso se ejecuta en un equipo que hospeda la base de datos.

Para asegurarse de que Always Encrypted impida eficazmente estos ataques, el proceso de administración de claves debe garantizar que nunca se revelen a un posible atacante las claves maestras de columna y las claves de cifrado de columnas, así como las credenciales de un almacén de claves que contiene las claves maestras de columna. Estas son algunas instrucciones que debe seguir:

- No genere nunca claves maestras de columna ni claves de cifrado de columnas en un equipo que hospede la base de datos. En su lugar, genere las claves en un equipo independiente que sea exclusivo para la administración de claves o en un equipo que hospede las aplicaciones que necesitarán tener acceso a las claves. Esto significa que **nunca debe ejecutar las herramientas usadas para generar las claves en el equipo en el que se hospeda la base de datos**, porque si un atacante accede a un equipo que se usa para aprovisionar o mantener las claves de Always Encrypted podrá obtener las claves, incluso si estas solo aparecen brevemente en la memoria de la herramienta.
- Para asegurarse de que el proceso de administración de claves no revele accidentalmente las claves maestras de columna o las claves de cifrado de columnas, es fundamental que identifique los posibles adversarios y las amenazas de seguridad antes de definir e implementar un proceso de administración de claves. Por ejemplo, si su objetivo es garantizar que los DBA no tengan acceso a información confidencial, el responsable de generar las claves no puede ser un DBA. Pero un DBA *puede* administrar los metadatos de clave de la base de datos, ya que los metadatos no contienen las claves de texto no cifrado.

## <a name="next-steps"></a>Pasos siguientes
- [Configuración del cifrado de columna mediante el asistente para Always Encrypted](always-encrypted-wizard.md)
- [Creación y almacenamiento de claves maestras de columna para Always Encrypted](create-and-store-column-master-keys-always-encrypted.md)
- [Aprovisionamiento de claves de Always Encrypted mediante SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md)
- [Aprovisionamiento de claves de Always Encrypted mediante PowerShell](configure-always-encrypted-keys-using-powershell.md)

## <a name="see-also"></a>Consulte también
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Tutorial de Asistente de Always Encrypted (Almacén de claves de Azure)](/azure/azure-sql/database/always-encrypted-azure-key-vault-configure)
- [Tutorial de Asistente de Always Encrypted (Almacén de certificados de Windows)](/azure/azure-sql/database/always-encrypted-certificate-store-configure)