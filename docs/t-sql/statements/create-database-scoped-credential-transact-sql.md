---
description: CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)
title: CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATABASE SCOPED CREDENTIAL
- DATABASE_SCOPED_CREDENTIAL_TSQL
- SCOPED_TSQL
- CREATE_DATABASE_SCOPED_CREDENTIAL
- CREATE_DATABASE_SCOPED_CREDENTIAL_TSQL
- SCOPED_CREDENTIAL_TSQL
- SCOPED_CREDENTIAL
helpviewer_keywords:
- DATABASE SCOPED CREDENTIAL statement
- credentials [SQL Server], DATABASE SCOPED CREDENTIAL statement
ms.assetid: fe830577-11ca-44e5-953b-2d589d54d045
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=aps-pdw-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1b9a9c27b80d702706d0b6bf1d7e7b3cb7d1d09b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192724"
---
# <a name="create-database-scoped-credential-transact-sql"></a>CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Crea una credencial de base de datos. Una credencial de base de datos no está asignada a un usuario de base de datos o de inicio de sesión de servidor. La base de datos utiliza la credencial para acceder a la ubicación externa siempre que la base de datos realice una operación que requiera acceso.

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintaxis

```syntaxsql
CREATE DATABASE SCOPED CREDENTIAL credential_name
WITH IDENTITY = 'identity_name'
    [ , SECRET = 'secret' ]

```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

*credential_name* Especifica el nombre de la credencial con ámbito de base de datos que se va a crear. *credential_name* no puede comenzar por el signo de almohadilla (#). Las credenciales del sistema comienzan por ##.

IDENTITY **="** _nombre\_identidad_ **"** Especifica el nombre de la cuenta que se va a usar para conectarse fuera del servidor. Para importar un archivo desde Azure Blob Storage usando una clave compartida, el nombre de identidad debe ser `SHARED ACCESS SIGNATURE`. Para cargar datos en Azure Synapse Analytics, se puede usar cualquier valor válido para la identidad. Para saber más sobre las firmas de acceso compartido, vea [Uso de firmas de acceso compartido (SAS)](/azure/storage/storage-dotnet-shared-access-signature-part-1). Cuando use Kerberos (Windows Active Directory o MIT KDC), no use el nombre de dominio en el argumento IDENTITY. Solamente debe usar el nombre de la cuenta.

> [!IMPORTANT]
> Los conectores ODBC de SQL, Oracle, Teradata y MongoDB para PolyBase solo admiten la autenticación básica, no la autenticación Kerberos.

> [!NOTE]
> WITH IDENTITY no es necesario si el contenedor de Azure Blob Storage está habilitado para el acceso anónimo. Para ver un ejemplo en el que se consulta Azure Blob Storage, consulte [Importación en una tabla desde un archivo almacenado en Azure Blob Storage](../functions/openrowset-transact-sql.md#j-importing-into-a-table-from-a-file-stored-on-azure-blob-storage).

SECRET **="** _secreto_ **"** Especifica el secreto necesario para la autenticación de salida. `SECRET` es necesario para importar un archivo del almacenamiento de blobs de Azure. Para cargar desde Azure Blob Storage a Azure Synapse Analytics o Almacenamiento de datos paralelos, el secreto debe ser la clave de Azure Storage.
> [!WARNING]
> El valor de clave SAS debe empezar con un signo de interrogación (“?”). Cuando use la clave SAS, debe quitar el símbolo “?” inicial. Si no lo hace, puede que se bloquee su trabajo.

## <a name="remarks"></a>Observaciones

Una credencial con ámbito de base de datos es un registro que contiene la información de autenticación necesaria para conectarse a un recurso fuera de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La mayoría de las credenciales incluyen un usuario y una contraseña de Windows.

Para poder crear una credencial con ámbito de base de datos, la base de datos debe tener una clave maestra para proteger la credencial. Para obtener más información, vea [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md).

Si IDENTITY es un usuario de Windows, el secreto puede ser la contraseña. El secreto se cifra usando la clave maestra de servicio. Si se vuelve a generar la clave maestra de servicio, el secreto se vuelve a cifrar con la nueva clave maestra de servicio.

Encontrará información sobre las credenciales de ámbito de base de datos en la vista de catálogo [sys.database_scoped_credentials](../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md).

Estas son algunas aplicaciones de credenciales con ámbito de base de datos:

- [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] utiliza una credencial con ámbito de base de datos para acceder al almacenamiento de blobs no público de Azure o a los clústeres de Hadoop protegidos con Kerberos mediante PolyBase. Para obtener más información, consulte [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md).

- [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] utiliza una credencial con ámbito de base de datos para acceder al almacenamiento de blobs no público de Azure mediante PolyBase. Para obtener más información, consulte [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md).

- [!INCLUDE[ssSDS](../../includes/sssds-md.md)] usa las credenciales con ámbito de base de datos para su característica de consulta global. Se trata de la capacidad de hacer consultas en varias particiones de base de datos.

- [!INCLUDE[ssSDS](../../includes/sssds-md.md)] usa las credenciales con ámbito de base de datos para escribir archivos de eventos extendidos en el almacenamiento de blobs de Azure.

- [!INCLUDE[ssSDS](../../includes/sssds-md.md)] usa credenciales con ámbito de base de datos para grupos elásticos. Para obtener más información, vea [Tame explosive growth with elastic databases](/azure/azure-sql/database/elastic-pool-overview) (Control del crecimiento explosivo con bases de datos elásticas).

- [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) y [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) utilizan credenciales con ámbito de base de datos para acceder a los datos desde el almacenamiento de blobs de Azure. Para obtener más información, consulte [Ejemplos de acceso masivo a datos en Azure Blob Storage](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md). 

## <a name="permissions"></a>Permisos

Necesita el permiso **CONTROL** en la base de datos.

## <a name="examples"></a>Ejemplos

### <a name="a-creating-a-database-scoped-credential-for-your-application"></a>A. Creación de una credencial con ámbito de base de datos para la aplicación

En el ejemplo siguiente se crea una credencial con ámbito de base de datos denominada `AppCred`. Esta credencial contiene el usuario de Windows `Mary5` y una contraseña.

```sql
-- Create a db master key if one does not already exist, using your own password.
CREATE MASTER KEY ENCRYPTION BY PASSWORD='<EnterStrongPasswordHere>';

-- Create a database scoped credential.
CREATE DATABASE SCOPED CREDENTIAL AppCred WITH IDENTITY = 'Mary5',
    SECRET = '<EnterStrongPasswordHere>';
```

### <a name="b-creating-a-database-scoped-credential-for-a-shared-access-signature"></a>B. Creación de una credencial con ámbito de base de datos para una firma de acceso compartido

En el ejemplo siguiente se crea una credencial con ámbito de base de datos que se puede usar para crear un [origen de datos externo](../../t-sql/statements/create-external-data-source-transact-sql.md), que puede realizar operaciones masivas, como [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) y [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md). Las firmas de acceso compartido no se pueden usar con PolyBase en SQL Server, APS ni Azure Synapse Analytics.

```sql
-- Create a db master key if one does not already exist, using your own password.
CREATE MASTER KEY ENCRYPTION BY PASSWORD='<EnterStrongPasswordHere>';

-- Create a database scoped credential.
CREATE DATABASE SCOPED CREDENTIAL MyCredentials
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
SECRET = 'QLYMgmSXMklt%2FI1U6DcVrQixnlU5Sgbtk1qDRakUBGs%3D';
```

### <a name="c-creating-a-database-scoped-credential-for-polybase-connectivity-to-azure-data-lake-store"></a>C. Creación de una credencial con ámbito de base de datos para establecer conectividad de PolyBase con Azure Data Lake Store

En el ejemplo siguiente se genera una credencial con ámbito de base de datos que se puede usar para crear un [origen de datos externo](../../t-sql/statements/create-external-data-source-transact-sql.md), que PolyBase puede usar en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].

Azure Data Lake Store usa una aplicación de Azure Active Directory para la autenticación de servicio a servicio.
[Cree una aplicación AAD](/azure/data-lake-store/data-lake-store-authenticate-using-active-directory) y documente su client_id, OAuth_2.0_Token_EndPoint y la clave antes de intentar crear una credencial con ámbito de base de datos.

```sql
-- Create a db master key if one does not already exist, using your own password.
CREATE MASTER KEY ENCRYPTION BY PASSWORD='<EnterStrongPasswordHere>';

-- Create a database scoped credential.
CREATE DATABASE SCOPED CREDENTIAL ADL_User
WITH
    IDENTITY = '<client_id>@\<OAuth_2.0_Token_EndPoint>',
    SECRET = '<key>'
;
```

## <a name="more-information"></a>Más información

- [Credenciales &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [ALTER DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-credential-transact-sql.md)
- [DROP DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-scoped-credential-transact-sql.md)
- [sys.database_scoped_credentials](../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md)
- [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)
- [sys.credentials &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)
