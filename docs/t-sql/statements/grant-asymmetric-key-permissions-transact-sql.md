---
description: GRANT (permisos de clave asimétrica de Transact-SQL)
title: GRANT (permisos de clave asimétrica de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], asymmetric keys
- permissions [SQL Server], asymmetric keys
- asymmetric keys [SQL Server], permissions
- GRANT statement, asymmetric keys
ms.assetid: a70e2ee6-59b0-4543-b883-e9cbae6199be
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: dde6b49c05c1d0e7b45b5eee851bd094423fe3aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159628"
---
# <a name="grant-asymmetric-key-permissions-transact-sql"></a>GRANT (permisos de clave asimétrica de Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Concede permisos para una clave asimétrica.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
GRANT { permission  [ ,...n ] }   
    ON ASYMMETRIC KEY :: asymmetric_key_name   
       TO database_principal [ ,...n ]  
    [ WITH GRANT OPTION ]  
    [ AS granting_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *permission*  
 Especifica un permiso que se puede conceder para una clave asimétrica. Se muestra a continuación.  
  
 ON ASYMMETRIC KEY **::** _asymmetric_key_name_  
 Especifica la clave asimétrica en la que se va a conceder el permiso. Es preciso utilizar el calificador de ámbito "::".  
  
 *database_principal*  
 Especifica la entidad de seguridad para la que se concede el permiso. Uno de los siguientes:  
  
-   usuario de base de datos  
-   rol de base de datos  
-   rol de aplicación  
-   usuario de base de datos asignado a un inicio de sesión de Windows  
-   usuario de base de datos asignado a un grupo de Windows  
-   usuario de base de datos asignado a un certificado  
-   usuario de base de datos asignado a una clave asimétrica  
-   usuario de base de datos no asignado a una entidad de seguridad del servidor  
  
GRANT OPTION  
 Indica que la entidad de seguridad también podrá conceder el permiso especificado a otras entidades de seguridad.  
  
AS *granting_principal*  
 Especifica una entidad de seguridad de la que la entidad de seguridad que ejecuta esta consulta deriva su derecho de conceder el permiso. Uno de los siguientes:  
  
-   usuario de base de datos  
-   rol de base de datos  
-   rol de aplicación  
-   usuario de base de datos asignado a un inicio de sesión de Windows  
-   usuario de base de datos asignado a un grupo de Windows  
-   usuario de base de datos asignado a un certificado  
-   usuario de base de datos asignado a una clave asimétrica  
-   usuario de base de datos no asignado a una entidad de seguridad del servidor  
  
## <a name="remarks"></a>Observaciones  
 Una clave asimétrica es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. A continuación, se enumeran los permisos más específicos y limitados que se pueden conceder para una clave asimétrica, además de los permisos más generales que los incluyen implícitamente.  
  
|Permiso de clave asimétrica|Implícito en el permiso de clave asimétrica|Implícito en el permiso de base de datos|  
|-------------------------------|------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ASYMMETRIC KEY|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Permisos  
 El otorgante del permiso (o la entidad de seguridad especificada con la opción AS) debe tener el permiso con GRANT OPTION, o un permiso superior que implique el permiso que se va a conceder.  
  
 Si se utiliza la opción AS, debe tener en cuenta los requisitos adicionales siguientes.  
  
|AS *granting_principal*|Permiso adicional necesario|  
|------------------------------|------------------------------------|  
|Usuario de la base de datos|Permiso IMPERSONATE en el usuario, pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Usuario de la base de datos asignado a un inicio de sesión de Windows|Permiso IMPERSONATE en el usuario, pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Usuario de la base de datos asignado a un grupo de Windows|Pertenencia al grupo de Windows, pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Usuario de la base de datos asignado a un certificado|Pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Usuario de la base de datos asignado a una clave asimétrica|Pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Usuario de la base de datos no asignado a una entidad de seguridad del servidor|Permiso IMPERSONATE en el usuario, pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Rol de base de datos|Permiso ALTER para el rol, pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Rol de aplicación|Permiso ALTER para el rol, pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
  
 Los propietarios de objetos pueden conceder permisos para los objetos que poseen. Las entidades de seguridad con permiso CONTROL sobre un elemento protegible pueden conceder permisos para ese elemento.  
  
 Los beneficiarios del permiso CONTROL SERVER, como por ejemplo, los miembros del rol fijo de servidor **sysadmin**, pueden conceder cualquier permiso para cualquier elemento protegible en el servidor. Los beneficiarios del permiso CONTROL para una base de datos, como por ejemplo, los miembros del rol fijo de base de datos **db_owner**, pueden conceder cualquier permiso para cualquier elemento protegible en la base de datos. Los receptores del permiso CONTROL en un esquema pueden conceder los permisos en cualquier objeto del esquema.  
  
## <a name="see-also"></a>Consulte también  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)  
  
  
