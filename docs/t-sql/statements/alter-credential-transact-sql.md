---
description: ALTER CREDENTIAL (Transact-SQL)
title: ALTER CREDENTIAL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER CREDENTIAL
- ALTER_CREDENTIAL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- passwords [SQL Server], credentials
- credentials [SQL Server], ALTER CREDENTIAL statement
- modifying credentials
- authentication [SQL Server], credentials
- ALTER CREDENTIAL statement
ms.assetid: b08899a6-c09e-4af4-91aa-a978ada79264
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 3ae2deff1638c7316fdaef9c3923fa5e2108404f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191055"
---
# <a name="alter-credential-transact-sql"></a>ALTER CREDENTIAL (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Cambia las propiedades de una credencial.  

> [!IMPORTANT]
> Información "debe hacer" como procedimiento recomendado; "tiene que hacer" para completar la tarea ![Icono del vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql 
ALTER CREDENTIAL credential_name WITH IDENTITY = 'identity_name'  
    [ , SECRET = 'secret' ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *credential_name*  
 Especifica el nombre de la credencial que se va a modificar.  
  
 IDENTITY **="** _identity_name_*_"_*  
 Especifica el nombre de la cuenta que se utilizará para conectarse fuera del servidor.  
  
 SECRET **="** _secret_*_"_*  
 Especifica el secreto necesario para la autenticación de salida. *secret* es opcional.
  
> [!IMPORTANT]
> Azure SQL Database solo admite las identidades de Azure Key Vault y de Firma de acceso compartido. No se admiten las identidades de usuario de Windows.
  
## <a name="remarks"></a>Comentarios  
 Cuando se cambia una credencial, se restablecen los valores de *identity_name* y *secret*. Si no se especifica el argumento opcional SECRET, el valor del secreto almacenado se establecerá en NULL.  
  
 El secreto está cifrado mediante la clave maestra de servicio. Si se vuelve a generar la clave maestra de servicio, el secreto se vuelve a cifrar utilizando la nueva clave maestra de servicio.  
  
 Encontrará más información sobre las credenciales en la vista de catálogo **sys.credentials**.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER ANY CREDENTIAL. Si la credencial es una credencial del sistema, requiere el permiso CONTROL SERVER.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-changing-the-password-of-a-credential"></a>A. Cambiar la contraseña de una credencial  
 En el siguiente ejemplo se cambia el secreto almacenado en una credencial denominada `Saddles`. La credencial contiene el inicio de sesión de Windows `RettigB` y su contraseña. La nueva contraseña se agrega a la credencial mediante la cláusula SECRET.  
  
```sql  
ALTER CREDENTIAL Saddles WITH IDENTITY = 'RettigB',   
    SECRET = 'sdrlk8$40-dksli87nNN8';  
GO  
```  
  
### <a name="b-removing-the-password-from-a-credential"></a>B. Quitar la contraseña de una credencial  
 En el ejemplo siguiente se quita la contraseña de una credencial denominada `Frames`. La credencial contiene el inicio de sesión de Windows `Aboulrus8` y una contraseña. Después de ejecutar la instrucción, la credencial tendrá una contraseña NULL porque no se especifica la opción SECRET.  
  
```sql  
ALTER CREDENTIAL Frames WITH IDENTITY = 'Aboulrus8';  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [Credenciales &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)   
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)   
 [DROP CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-credential-transact-sql.md)   
 [ALTER DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-credential-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sys.credentials &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)  
  
  
