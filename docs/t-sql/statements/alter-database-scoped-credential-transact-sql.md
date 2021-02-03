---
description: ALTER DATABASE SCOPED CREDENTIAL (Transact-SQL)
title: ALTER DATABASE SCOPED CREDENTIAL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/27/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER DATABASE SCOPED CREDENTIAL
- ALTER_DATABASE_SCOPED_CREDENTIAL_TSQL
helpviewer_keywords:
- ALTER DATABASE SCOPED CREDENTIAL statement
- credentials [SQL Server], ALTER DATABASE SCOPED CREDENTIAL statement
ms.assetid: 966b75b5-ca87-4203-8bf9-95c4e00cb0b5
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: 245e99b6a377b1026c6865ac16989daf8080960c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179211"
---
# <a name="alter-database-scoped-credential-transact-sql"></a>ALTER DATABASE SCOPED CREDENTIAL (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Cambia las propiedades de una credencial de ámbito de base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER DATABASE SCOPED CREDENTIAL credential_name WITH IDENTITY = 'identity_name'  
    [ , SECRET = 'secret' ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *credential_name*  
 Especifica el nombre de la credencial de ámbito de base de datos que se va a modificar.  
  
 IDENTITY **="** _identity_name_*_"_*  
 Especifica el nombre de la cuenta que se utilizará para conectarse fuera del servidor. Para importar un archivo de Azure Blob Storage, el nombre de identidad debe ser `SHARED ACCESS SIGNATURE`.  Para saber más sobre las firmas de acceso compartido, vea [Uso de firmas de acceso compartido (SAS)](/azure/storage/storage-dotnet-shared-access-signature-part-1).  
    
  
 SECRET **="** _secret_*_"_*  
 Especifica el secreto necesario para la autenticación de salida. *secret* es necesario para importar un archivo de Azure Blob Storage. *secret* puede ser opcional para otros fines.   
> [!WARNING]
>  El valor de clave SAS debe empezar con un signo de interrogación (“?”). Cuando use la clave SAS, debe quitar el símbolo “?” inicial. Si no lo hace, puede que se bloquee su trabajo.    
  
## <a name="remarks"></a>Observaciones  
 Cuando se cambia una credencial de ámbito de base de datos, se restablecen los valores de *identity_name* y *secret*. Si no se especifica el argumento opcional SECRET, el valor del secreto almacenado se establecerá en NULL.  
  
 El secreto está cifrado mediante la clave maestra de servicio. Si se vuelve a generar la clave maestra de servicio, el secreto se vuelve a cifrar utilizando la nueva clave maestra de servicio.  
  
 Encontrará información sobre las credenciales de ámbito de base de datos en la vista de catálogo [sys.database_scoped_credentials](../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso `ALTER` en la credencial.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-changing-the-password-of-a-database-scoped-credential"></a>A. Cambiar la contraseña de una credencial de ámbito de base de datos  
 En este ejemplo se cambia el secreto almacenado en una credencial de ámbito de base de datos denominada `Saddles`. La credencial de ámbito de base de datos contiene el inicio de sesión de Windows `RettigB` y su contraseña. La nueva contraseña se agrega a la credencial de ámbito de base de datos mediante la cláusula SECRET.  
  
```sql  
ALTER DATABASE SCOPED CREDENTIAL AppCred WITH IDENTITY = 'RettigB',   
    SECRET = 'sdrlk8$40-dksli87nNN8';  
GO  
```  
  
### <a name="b-removing-the-password-from-a-credential"></a>B. Quitar la contraseña de una credencial  
 En este ejemplo se quita la contraseña de una credencial de ámbito de base de datos denominada `Frames`. La credencial de ámbito de base de datos contiene un inicio de sesión de Windows `Aboulrus8` y una contraseña. Después de ejecutar la instrucción, la credencial de ámbito de base de datos tendrá una contraseña NULL porque no se especifica la opción SECRET.  
  
```sql  
ALTER DATABASE SCOPED CREDENTIAL Frames WITH IDENTITY = 'Aboulrus8';  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [Credenciales &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)   
 [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)   
 [DROP DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-scoped-credential-transact-sql.md)   
 [sys.database_scoped_credentials](../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md)   
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)   
 [sys.credentials &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)  
  
