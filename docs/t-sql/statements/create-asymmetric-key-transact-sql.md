---
description: CREATE ASYMMETRIC KEY (Transact-SQL)
title: CREATE ASYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ASYMMETRIC_KEY_TSQL
- CREATE ASYMMETRIC KEY
- CREATE_ASYMMETRIC_KEY_TSQL
- ASYMMETRIC KEY
dev_langs:
- TSQL
helpviewer_keywords:
- asymmetric keys [SQL Server], creating
- encryption [SQL Server], asymmetric keys
- CREATE ASYMMETRIC KEY statement
- asymmetric keys [SQL Server]
- cryptography [SQL Server], asymmetric keys
ms.assetid: 141bc976-7631-49f6-82bd-a235028645b1
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 8945797d67574e1c2b8ffb60b2d74e7b5b9462f3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205002"
---
# <a name="create-asymmetric-key-transact-sql"></a>CREATE ASYMMETRIC KEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crea una clave asimétrica en la base de datos.  
  
 Esta función no es compatible con la exportación de la base de datos con el Marco de trabajo de la aplicación de capa de datos (DACFx). Debe quitar todas las claves asimétricas antes de exportar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE ASYMMETRIC KEY asym_key_name   
   [ AUTHORIZATION database_principal_name ]  
   [ FROM <asym_key_source> ]  
   [ WITH <key_option> ] 
   [ ENCRYPTION BY <encrypting_mechanism> ] 
   [ ; ]
  
<asym_key_source>::=  
     FILE = 'path_to_strong-name_file'  
   | EXECUTABLE FILE = 'path_to_executable_file'  
   | ASSEMBLY assembly_name  
   | PROVIDER provider_name  
  
<key_option> ::=  
   ALGORITHM = <algorithm>  
      |  
   PROVIDER_KEY_NAME = 'key_name_in_provider'  
      |  
      CREATION_DISPOSITION = { CREATE_NEW | OPEN_EXISTING }  
  
<algorithm> ::=  
      { RSA_4096 | RSA_3072 | RSA_2048 | RSA_1024 | RSA_512 }   
  
<encrypting_mechanism> ::=  
    PASSWORD = 'password'   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *asym_key_name*  
 Nombre de una clave asimétrica en la base de datos. Los nombres de clave asimétrica deben cumplir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md) y ser exclusivos en el esquema.  

 AUTHORIZATION *database_principal_name*  
 Especifica el propietario de la clave asimétrica. El propietario no puede ser un rol ni un grupo. Si se omite esta opción, el propietario será el usuario actual.  
  
 FROM *asym_key_source*  
 Especifica el origen desde el que se carga el par de claves asimétricas.  
  
 FILE = '*path_to_strong-name_file*'  
 Especifica la ruta de acceso a un archivo de nombre seguro desde el que se carga el par de claves. Limitado a 260 caracteres por MAX_PATH de la API de Windows.  
  
> [!NOTE]  
>  Esta opción no está disponible en las bases de datos independientes.  
  
 EXECUTABLE FILE = '*path_to_executable_file*'  
 Especifica la ruta de acceso de un archivo de ensamblado del que se carga la clave pública. Limitado a 260 caracteres por MAX_PATH de la API de Windows.  
  
> [!NOTE]  
>  Esta opción no está disponible en las bases de datos independientes.  
  
 ASSEMBLY *assembly_name*  
 Especifica el nombre de un ensamblado firmado que ya se ha cargado en la base de datos de la que se carga la clave pública.  
  
 PROVIDER *provider_name*  
 Especifica el nombre de un proveedor de Administración extensible de claves (EKM). El proveedor debe definirse antes mediante la instrucción CREATE PROVIDER. Para obtener más información sobre la Administración extensible de claves, vea [Administración extensible de claves &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md).  
  
 ALGORITHM = \<algorithm>  
 Se pueden proporcionar cinco algoritmos: RSA_4096, RSA_3072, RSA_2048, RSA_1024 y RSA_512.  
  
 RSA_1024 y RSA_512 están en desuso. Para usar los algoritmos RSA_1024 o RSA_512 (no se recomienda), debe establecer la base de datos en el nivel de compatibilidad de base de datos 120 o inferior.  
  
 PROVIDER_KEY_NAME = '*key_name_in_provider*'  
 Especifica el nombre de la clave del proveedor externo.  
  
 CREATION_DISPOSITION = CREATE_NEW  
 Crea una nueva clave en el dispositivo de Administración extensible de claves. Debe utilizarse PROVIDER_KEY_NAME para especificar el nombre de clave en el dispositivo. Si ya existe una clave en el dispositivo, se producirá un error en la instrucción.  
  
 CREATION_DISPOSITION = OPEN_EXISTING  
 Asigna una clave asimétrica de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a una clave de Administración extensible de claves. Debe utilizarse PROVIDER_KEY_NAME para especificar el nombre de clave en el dispositivo. Si no se proporciona CREATION_DISPOSITION = OPEN_EXISTING, el valor predeterminado es CREATE_NEW.  
  
 ENCRYPTION BY PASSWORD = '*password*'  
 Especifica la contraseña con la que se cifra la clave privada. Si no está presente esta cláusula, la clave privada se cifrará con la clave maestra de la base de datos. *password* tiene un máximo de 128 caracteres. *password* debe cumplir los requisitos de la directiva de contraseñas de Windows del equipo que ejecuta la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="remarks"></a>Observaciones  
 Una *clave asimétrica* es una entidad protegible en el nivel de base de datos. De manera predeterminada, esta entidad contiene una clave pública y otra privada. Si se ejecuta sin la cláusula FROM, CREATE ASYMMETRIC KEY genera un nuevo par de claves. Si se ejecuta con la cláusula FROM, CREATE ASYMMETRIC KEY importa un par de claves desde un archivo o importa una clave pública desde un ensamblado o un archivo DLL.  
  
 De manera predeterminada, la clave privada está protegida por la clave maestra de la base de datos. Si no se ha creado una clave maestra de base de datos, se necesita una contraseña para proteger la clave privada.  
  
 La clave privada puede ser de 512, 1.024 o 2.048 bits.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CREATE ASYMMETRIC KEY en la base de datos. Si se especifica la cláusula AUTHORIZATION, es necesario el permiso IMPERSONATE en la entidad de seguridad de la base de datos o el permiso ALTER en el rol de aplicación. Solo los inicios de sesión de Windows, los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y los roles de aplicación pueden poseer claves asimétricas. Los grupos y roles no pueden poseer claves asimétricas.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-an-asymmetric-key"></a>A. Crear una clave asimétrica  
 En el siguiente ejemplo se crea una clave asimétrica con el nombre `PacificSales09` mediante el algoritmo `RSA_2048` y se protege la clave privada con una contraseña.  
  
```sql  
CREATE ASYMMETRIC KEY PacificSales09   
    WITH ALGORITHM = RSA_2048   
    ENCRYPTION BY PASSWORD = '<enterStrongPasswordHere>';   
GO  
```  
  
### <a name="b-creating-an-asymmetric-key-from-a-file-giving-authorization-to-a-user"></a>B. Crear una clave asimétrica desde un archivo, concediendo autorización a un usuario  
 En el siguiente ejemplo se crea la clave asimétrica `PacificSales19` a partir de un par de claves almacenado en un archivo y asigna la propiedad de la clave asimétrica al usuario `Christina`. La clave privada está protegida por la clave maestra de base de datos, que debe crearse antes de crear la clave asimétrica.  
  
```sql  
CREATE ASYMMETRIC KEY PacificSales19  
    AUTHORIZATION Christina  
    FROM FILE = 'c:\PacSales\Managers\ChristinaCerts.tmp';  
GO  
```  
  
### <a name="c-creating-an-asymmetric-key-from-an-ekm-provider"></a>C. Crear una clave asimétrica de un proveedor de EKM  
 En el ejemplo siguiente se crea la clave asimétrica `EKM_askey1` a partir de un par de claves almacenado en un proveedor de Administración extensible de claves llamado `EKM_Provider1` y una clave de ese proveedor denominada `key10_user1`.  
  
```sql  
CREATE ASYMMETRIC KEY EKM_askey1   
    FROM PROVIDER EKM_Provider1  
    WITH   
        ALGORITHM = RSA_2048,   
        CREATION_DISPOSITION = CREATE_NEW  
        , PROVIDER_KEY_NAME  = 'key10_user1' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-asymmetric-key-transact-sql.md)  
 [DROP ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-asymmetric-key-transact-sql.md)  
 [ASYMKEYPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/asymkeyproperty-transact-sql.md)  
 [ASYMKEY_ID &#40;Transact-SQL&#41;](../../t-sql/functions/asymkey-id-transact-sql.md)  
 [Elegir un algoritmo de cifrado](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)  
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)  
 [Administración extensible de claves con el Almacén de claves de Azure &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
  
